---
title: "Triển khai Backend bằng AWS CDK"
date: 2026-08-07
weight: 3
chapter: false
pre: " <b> 5.7.3. </b> "
---

---

Sau khi hoàn tất việc chuẩn bị môi trường, bước tiếp theo là triển khai hạ tầng Backend bằng **AWS CDK**.

Toàn bộ hạ tầng Backend được định nghĩa dưới dạng **IaC** và được tổ chức thành 5 CDK Stack:

- `StorageStack`: Triển khai các tài nguyên lưu trữ của hệ thống.

```
from aws_cdk import (
    Stack,
    RemovalPolicy,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct


class StorageStack(Stack):
    def __init__(self, scope: Construct, construct_id: str, **kwargs) -> None:
        super().__init__(scope, construct_id, **kwargs)

        self.original_bucket = s3.Bucket(
            self,
            "OriginalImageBucket",
            bucket_name="aws-img-opt-original-bucket",
            versioned=True,
            encryption=s3.BucketEncryption.S3_MANAGED,
            block_public_access=s3.BlockPublicAccess.BLOCK_ALL,
            removal_policy=RemovalPolicy.DESTROY,
            auto_delete_objects=True,
            event_bridge_enabled=True,  # bắt buộc để ProcessingStack's EventBridge Rule nhận được sự kiện
            cors=[
                s3.CorsRule(
                    allowed_methods=[s3.HttpMethods.GET, s3.HttpMethods.PUT, s3.HttpMethods.POST, s3.HttpMethods.HEAD],
                    allowed_origins=["*"],
                    allowed_headers=["*"],
                    max_age=3000
                )
            ]
        )

        self.optimized_bucket = s3.Bucket(
            self,
            "OptimizedImageBucket",
            bucket_name="aws-img-opt-optimized-bucket",
            versioned=True,
            encryption=s3.BucketEncryption.S3_MANAGED,
            block_public_access=s3.BlockPublicAccess.BLOCK_ALL,
            removal_policy=RemovalPolicy.DESTROY,
            auto_delete_objects=True,
        )


        self.table = _dynamodb.Table(
            self,
            "ImageMetadata",
            table_name="aws-img-opt-metadata-table",
            partition_key=_dynamodb.Attribute(
                name="batchId",
                type=_dynamodb.AttributeType.STRING,
            ),
            sort_key=_dynamodb.Attribute(
                name="processingId",
                type=_dynamodb.AttributeType.STRING,
            ),
            billing_mode=_dynamodb.BillingMode.PAY_PER_REQUEST,
            removal_policy=RemovalPolicy.DESTROY,
        )


        # Bảng UserMetadata: Lưu thông tin người dùng
        self.user_table = _dynamodb.Table(
            self,
            "UserMetadata",
            table_name="aws-img-opt-user-metadata-table",
            partition_key=_dynamodb.Attribute(
                name="userId",
                type=_dynamodb.AttributeType.STRING,
            ),
            billing_mode=_dynamodb.BillingMode.PAY_PER_REQUEST,
            removal_policy=RemovalPolicy.DESTROY,
        )
        self.table.add_global_secondary_index(
            index_name="UserImagesIndex",
            partition_key=_dynamodb.Attribute(
                name="userId",
                type=_dynamodb.AttributeType.STRING,
            ),
            sort_key=_dynamodb.Attribute(
                name="processedAt",
                type=_dynamodb.AttributeType.STRING,
            ),
            projection_type=_dynamodb.ProjectionType.ALL,
        )

        from aws_cdk import CfnOutput

        CfnOutput(
            self,
            "OriginalImageBucketName",
            value=self.original_bucket.bucket_name,
            description="Name of the original image bucket"
        )

        CfnOutput(
            self,
            "OptimizedImageBucketName",
            value=self.optimized_bucket.bucket_name,
            description="Name of the optimized image bucket"
        )

        CfnOutput(
            self,
            "ImageMetadataTableName",
            value=self.table.table_name,
            description="Name of the DynamoDB table for image metadata"
        )

        CfnOutput(
            self,
            "UserMetadataTableName",
            value=self.user_table.table_name,
            description="Name of the DynamoDB table for user metadata"
        )
```

- `ProcessingStack`: Triển khai các thành phần phục vụ quá trình xử lý hình ảnh.

```
import aws_cdk as cdk
from aws_cdk import (
    Stack,
    Duration,
    aws_lambda as _lambda,
    aws_events as events,
    aws_events_targets as targets,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct


class ProcessingStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        optimized_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        self.processor = _lambda.Function(
            self,
            "ImageProcessor",
            runtime=_lambda.Runtime.PYTHON_3_13,
            handler="handler.lambda_handler",
            code=_lambda.Code.from_asset("lambda",
                bundling=cdk.BundlingOptions(
                    image=_lambda.Runtime.PYTHON_3_13.bundling_image,
                    command=["bash", "-c", "pip install -r requirements.txt -t /asset-output && cp -r . /asset-output"],
                ),
            ),
            timeout=Duration.seconds(30),
            memory_size=512,
            environment={
                "METADATA_TABLE": metadata_table.table_name,
                "OUTPUT_BUCKET": optimized_bucket.bucket_name
            }
        )

        # Grant permissions
        original_bucket.grant_read(self.processor)
        optimized_bucket.grant_write(self.processor)
        metadata_table.grant_read_write_data(self.processor)


        rule = events.Rule(
            self, "S3ObjectCreatedRule",
            event_pattern=events.EventPattern(
                source=["aws.s3"],
                detail_type=["Object Created"],
                detail={
                    "bucket": {
                        "name": [original_bucket.bucket_name]
                    }
                }
            )
        )
        rule.add_target(targets.LambdaFunction(self.processor))
```

- `BackendStack`: Triển khai các thành phần Backend của ứng dụng.

```
from aws_cdk import (
    Stack,
    Duration,
    aws_lambda as _lambda,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct
import os

class BackendStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        optimized_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        user_table: _dynamodb.Table,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # Deploy Spring Boot as a Lambda function using AWS Lambda Web Adapter
        # Đọc file .env để lấy Secret (trong thực tế CI/CD nên dùng AWS Secrets Manager)
        env_file_path = os.path.join("backend", "image-optimizer", ".env")
        google_client_id = "default_client_id"
        jwt_secret = "default_jwt_secret_must_be_at_least_32_characters_long_for_hmac_sha256"

        if os.path.exists(env_file_path):
            with open(env_file_path, "r") as f:
                for line in f:
                    if line.startswith("GOOGLE_CLIENT_ID="):
                        google_client_id = line.strip().split("=", 1)[1]
                    elif line.startswith("JWT_SECRET="):
                        jwt_secret = line.strip().split("=", 1)[1]

        self.spring_boot_lambda = _lambda.DockerImageFunction(
            self,
            "SpringBootApiLambda",
            code=_lambda.DockerImageCode.from_image_asset(
                directory="backend/image-optimizer"
            ),
            memory_size=512,  # Set to 512 to satisfy AWS Lab constraints
            timeout=Duration.seconds(30), # API Gateway has 29s timeout anyway
            environment={
                "S3_INPUT_BUCKET": original_bucket.bucket_name,
                "S3_OUTPUT_BUCKET": optimized_bucket.bucket_name,
                "DYNAMODB_IMAGE_TABLE": metadata_table.table_name,
                "DYNAMODB_USER_TABLE": user_table.table_name,
                "GOOGLE_CLIENT_ID": google_client_id,
                "JWT_SECRET": jwt_secret
            }
        )

        # Grant necessary permissions
        original_bucket.grant_read_write(self.spring_boot_lambda)
        optimized_bucket.grant_read_write(self.spring_boot_lambda)
        metadata_table.grant_read_write_data(self.spring_boot_lambda)
        user_table.grant_read_write_data(self.spring_boot_lambda)

```

- `ApiStack`: Triển khai API Gateway và cấu hình kết nối đến Backend.

```
from aws_cdk import (
    Stack,
    Duration,
    CfnOutput,
    aws_lambda as _lambda,
    aws_apigateway as apigw,
    aws_s3 as s3,
    aws_dynamodb as _dynamodb,
)
from constructs import Construct

class ApiStack(Stack):
    def __init__(
        self, scope: Construct, construct_id: str,
        original_bucket: s3.Bucket,
        metadata_table: _dynamodb.Table,
        backend_lambda: _lambda.Function,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        # Upload Lambda for generating pre-signed URLs
        self.upload_api_lambda = _lambda.Function(
            self,
            "UploadApiLambda",
            runtime=_lambda.Runtime.PYTHON_3_13,
            handler="api_handler.lambda_handler",
            code=_lambda.Code.from_asset("lambda"),
            timeout=Duration.seconds(10),
            memory_size=256,
            environment={
                "ORIGINAL_BUCKET": original_bucket.bucket_name,
                "METADATA_TABLE": metadata_table.table_name
            }
        )

        # Grant permissions
        original_bucket.grant_put(self.upload_api_lambda)
        metadata_table.grant_read_write_data(self.upload_api_lambda)

        # API Gateway REST API
        self.api = apigw.RestApi(
            self, "ImageUploadApi",
            rest_api_name="Image Upload Service",
            description="This service handles uploading images to S3."
        )

        # Integration
        upload_integration = apigw.LambdaIntegration(
        self.upload_api_lambda
        )

        # Resource and Method for Upload Lambda (Python)
        upload_resource = self.api.root.add_resource("upload")
        upload_resource.add_method("POST", upload_integration)
        upload_resource.add_method("OPTIONS", upload_integration)

        # Integration for Spring Boot Lambda (Java)
        spring_boot_integration = apigw.LambdaIntegration(backend_lambda)

        # Resource and Method for Spring Boot APIs: /api/{proxy+}
        api_resource = self.api.root.add_resource("api")
        api_resource.add_proxy(
            default_integration=spring_boot_integration,
            any_method=True
        )

        # Export API Gateway URL so sync_backend_env.py can read it from cdk-outputs.json
        CfnOutput(
            self,
            "ApiGatewayUrl",
            value=self.api.url,
            description="API Gateway endpoint URL (e.g. https://xxx.execute-api.region.amazonaws.com/prod/)"
        )


```

- `AmplifyHostingStack`: Cấu hình tài nguyên phục vụ việc hosting Frontend trên AWS Amplify.

```
from aws_cdk import (
    Stack,
    CfnOutput,
    aws_amplify as amplify,
)
from constructs import Construct


class AmplifyHostingStack(Stack):
    def __init__(
        self,
        scope: Construct,
        construct_id: str,
        api_url: str,
        **kwargs
    ) -> None:
        super().__init__(scope, construct_id, **kwargs)

        app = amplify.CfnApp(
            self,
            "FrontendAmplifyApp",
            name="image-optimization-frontend",
            platform="WEB",
            enable_branch_auto_deletion=False,
            environment_variables=[
                amplify.CfnApp.EnvironmentVariableProperty(
                    name="VITE_API_GATEWAY_URL",
                    value=api_url.rstrip("/")
                ),
                amplify.CfnApp.EnvironmentVariableProperty(
                    name="VITE_GOOGLE_CLIENT_ID",
                    value="{{resolve:secretsmanager:amplify/google-client-id:SecretString}}"
                ),
            ],
            build_spec="""version: 1
applications:
  - appRoot: frontend/image-optimization-system/image-optimization-frontend
    frontend:
      phases:
        preBuild:
          commands:
            - npm ci
        build:
          commands:
            - npm run build
      artifacts:
        baseDirectory: dist
        files:
          - '**/*'
      cache:
        paths:
          - node_modules/**/*
""",
            custom_rules=[
                amplify.CfnApp.CustomRuleProperty(
                    source="</^[^.]+$|\\.(?!(css|gif|ico|jpg|jpeg|js|png|svg|txt|webp|woff|woff2)$)([^.]+$)/>",
                    target="/index.html",
                    status="200",
                )
            ],
        )

        branch = amplify.CfnBranch(
            self,
            "MainBranch",
            app_id=app.attr_app_id,
            branch_name="main",
            stage="PRODUCTION",
            enable_auto_build=False,
        )

        CfnOutput(
            self,
            "AmplifyAppId",
            value=app.attr_app_id,
            description="Amplify app ID for the frontend",
        )

        CfnOutput(
            self,
            "FrontendUrl",
            value=f"https://{branch.branch_name}.{app.attr_default_domain}",
            description="Amplify HTTPS URL for the frontend",
        )

```

Các Stack được triển khai thông qua AWS CDK, giúp tự động tạo và cấu hình các tài nguyên AWS thay vì phải thực hiện thủ công trên AWS Management Console.

### Bước 1: Clone source code và tạo Virtual Environment

Clone source code của dự án từ repository:

```bash id="5p4d0w"
git clone https://github.com/MyPhungg/aws-automatic-image-optimization-system.git
```

Sau khi clone thành công, di chuyển vào thư mục dự án:

```bash id="m2yxde"
cd aws-automatic-image-optimization-system
```

Tạo Python Virtual Environment:

```bash id="6v8nqj"
python -m venv .venv
```

Kích hoạt Virtual Environment.

Trên **Windows**:

```bash id="qg1g7m"
.venv\Scripts\activate
```

Trên **Linux/macOS**:

```bash id="qz0jaf"
source .venv/bin/activate
```

Sau khi kích hoạt môi trường ảo, tiến hành cài đặt các thư viện cần thiết:

```bash id="a6rj4x"
pip install -r requirements.txt
```

Các thư viện này bao gồm những dependency cần thiết cho AWS CDK và các thành phần liên quan của dự án.

---

### Bước 2: Cấu hình Environment Variables

Một số thông tin cấu hình được sử dụng trong quá trình triển khai Backend. Tạo file `.env` tại thư mục:

```text id="2x2cwu"
backend/image-optimizer/.env
```

Nội dung mẫu:

```env id="86h2ol"
GOOGLE_CLIENT_ID=your-google-oauth-client-id
JWT_SECRET=your-secret-key-at-least-32-chars-long
```

Trong đó:

- `GOOGLE_CLIENT_ID`: Google OAuth Client ID được sử dụng cho chức năng đăng nhập bằng Google.
- `JWT_SECRET`: Secret key được sử dụng cho quá trình tạo và xác thực JWT.

Thay các giá trị mẫu bằng thông tin cấu hình thực tế của hệ thống.

> **Lưu ý:** File `.env` có thể chứa thông tin nhạy cảm. Không nên commit file này lên GitHub hoặc chia sẻ các giá trị secret thực tế.

---

### Bước 3: Bootstrap AWS CDK

Trước khi triển khai, AWS CDK cần được **bootstrap** trên AWS account và region sử dụng.

Nếu tài khoản AWS chưa được bootstrap, thực hiện:

```bash id="0f4j1x"
cdk bootstrap aws://<ACCOUNT_ID>/<REGION>
```

Trong đó:

- `<ACCOUNT_ID>` là AWS Account ID.
- `<REGION>` là AWS Region dùng để triển khai hệ thống.

Ví dụ:

```bash id="z8n7rc"
cdk bootstrap aws://123456789012/ap-southeast-1
```

Quá trình bootstrap tạo các tài nguyên cần thiết để AWS CDK có thể triển khai ứng dụng vào AWS environment.

> **Lưu ý:** Thông thường, mỗi AWS account và region chỉ cần bootstrap một lần. Nếu environment đã được bootstrap trước đó, có thể chuyển sang bước triển khai.

---

### Bước 4: Triển khai Backend Infrastructure

Sau khi hoàn tất bootstrap, thực hiện triển khai toàn bộ các CDK Stack bằng lệnh:

```bash id="75r1ba"
cdk deploy --all --require-approval never --outputs-file cdk-outputs.json
```

Trong đó:

- `--all`: Triển khai tất cả các CDK Stack trong project.
- `--require-approval never`: Không yêu cầu xác nhận thủ công đối với các thay đổi cần approval trong quá trình deploy.
- `--outputs-file cdk-outputs.json`: Lưu các giá trị output của các Stack vào file `cdk-outputs.json`.

AWS CDK sẽ đọc cấu hình từ source code, xác định các tài nguyên cần tạo hoặc cập nhật và thực hiện triển khai lên AWS.

Sau khi quá trình triển khai hoàn tất, file `cdk-outputs.json` sẽ chứa các thông tin được xuất ra từ các Stack. Các giá trị này có thể được sử dụng ở các bước triển khai Frontend và kiểm tra hệ thống.

---

### Bước 5: Build Docker Image cho Backend

Trong quá trình triển khai, một số Lambda của Backend được đóng gói dưới dạng **Docker Image** thông qua cấu hình `DockerImageFunction`.

AWS CDK sẽ sử dụng Docker trên máy local để build image tương ứng.

Quy trình tổng quát:

```text id="8fy2lv"
AWS CDK
   │
   ▼
Docker Build
   │
   ▼
Docker Image
   │
   ▼
Amazon ECR
   │
   ▼
AWS Lambda
```

Do đó, cần đảm bảo **Docker Desktop đang chạy** trước khi thực hiện lệnh `cdk deploy`.

Sau khi Docker image được build thành công, image sẽ được đưa vào **Amazon ECR** để Lambda sử dụng trong quá trình thực thi.

> **Lưu ý:** Quá trình build Docker image có thể mất thêm thời gian tùy thuộc vào kích thước source code, các dependency và tốc độ máy tính.
