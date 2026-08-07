---
title: "Triển khai Frontend bằng AWS Amplify"
date: 2026-08-07
weight: 4
chapter: false
pre: " <b> 5.7.4. </b> "
---

---

Sau khi Backend Infrastructure được triển khai thành công bằng AWS CDK, bước tiếp theo là build và triển khai ứng dụng Frontend lên **AWS Amplify**.

Frontend của hệ thống được xây dựng bằng **React và Vite**. Trong quá trình build, Frontend cần biết địa chỉ API Gateway của Backend để có thể giao tiếp với các API được cung cấp bởi hệ thống.

Sau khi Backend được triển khai, thông tin `ApiGatewayUrl` được lưu trong file `cdk-outputs.json` và được sử dụng để cấu hình Frontend.

### Bước 1: Build mã nguồn React

Di chuyển đến thư mục Frontend:

```bash id="r4q6xw"
cd frontend/image-optimization-system/image-optimization-frontend
```

Cài đặt các dependency cần thiết:

```bash id="4v8m2a"
npm install
```

Sau khi Backend được deploy thành công, file `cdk-outputs.json` được tạo ở thư mục gốc của project.

Sử dụng giá trị `ApiGatewayUrl` từ file này để tạo biến môi trường cho Frontend:

```bash id="2c4p1j"
echo "VITE_API_GATEWAY_URL=$(jq -r '.ApiStack.ApiGatewayUrl' ../../../cdk-outputs.json)" > .env
```

Trong đó:

- `VITE_API_GATEWAY_URL`: Địa chỉ API Gateway mà Frontend sử dụng để gọi Backend.
- `cdk-outputs.json`: File chứa các giá trị output được AWS CDK tạo ra sau quá trình triển khai.
- `.env`: File cấu hình biến môi trường cho ứng dụng React/Vite.

Sau khi cấu hình biến môi trường, tiến hành build Frontend:

```bash id="k3w9hp"
npm run build
```

Nếu quá trình build thành công, Vite sẽ tạo thư mục:

```text id="q5z8bn"
dist/
```

Thư mục `dist` chứa các file tĩnh đã được build và sẵn sàng để triển khai lên môi trường hosting.

> **Lưu ý:** Lệnh sử dụng `jq` trong ví dụ trên phù hợp với môi trường Linux/macOS hoặc các môi trường có cài đặt `jq`. Trên Windows, có thể đọc giá trị `ApiGatewayUrl` từ `cdk-outputs.json` và cấu hình file `.env` theo cách phù hợp với môi trường đang sử dụng.

---

### Bước 2: Đóng gói Frontend

Sau khi build thành công, di chuyển vào thư mục `dist`:

```bash id="6g1fyt"
cd dist
```

Nén toàn bộ nội dung của thư mục:

```bash id="s9q4jk"
zip -r ../frontend-dist.zip .
```

Sau khi hoàn tất, file:

```text id="1v8m3z"
frontend-dist.zip
```

được tạo tại thư mục Frontend.

Quay lại thư mục Frontend:

```bash id="x4r2nc"
cd ..
```

File ZIP này sẽ được sử dụng làm source cho quá trình triển khai ứng dụng lên AWS Amplify.

---

### Bước 3: Triển khai Frontend lên AWS Amplify

Frontend có thể được triển khai lên AWS Amplify thông qua AWS CLI.

Sử dụng lệnh:

```bash id="8v2m6r"
aws amplify start-deployment \
  --app-id <AMPLIFY_APP_ID> \
  --branch-name main \
  --source-url s3://<BUCKET>/frontend-dist.zip
```

Trong đó:

- `<AMPLIFY_APP_ID>`: ID của Amplify App.
- `main`: Nhánh được sử dụng để triển khai ứng dụng.
- `<BUCKET>`: S3 Bucket chứa file `frontend-dist.zip`.

Sau khi deployment được tạo, AWS Amplify sẽ sử dụng source được cung cấp để cập nhật phiên bản Frontend.

Có thể kiểm tra trạng thái deployment trên **AWS Management Console → Amplify**.

---

### Quy trình triển khai Frontend

Toàn bộ quá trình có thể được tóm tắt như sau:

```text id="j8h4vx"
cdk-outputs.json
       │
       ▼
  ApiGatewayUrl
       │
       ▼
   Frontend .env
       │
       ▼
    npm run build
       │
       ▼
      dist/
       │
       ▼
frontend-dist.zip
       │
       ▼
   AWS Amplify
       │
       ▼
  Frontend Application
```

Trong môi trường triển khai thực tế, các bước build và deploy Frontend có thể được tự động hóa thông qua **GitHub Actions**. Quy trình CI/CD sẽ được trình bày trong phần tiếp theo.
