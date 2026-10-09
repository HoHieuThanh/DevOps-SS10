# Bài 3: Đóng gói ứng dụng Web Backend bằng Single-stage Dockerfile

## 1. Mục tiêu

Xây dựng ứng dụng REST API đơn giản bằng Node.js và Express, sau đó đóng gói ứng dụng thành Docker Image bằng Single-stage Dockerfile và chạy container trên môi trường Docker.

## 2. Công nghệ sử dụng

- Node.js 18 Alpine
- Express.js 4
- Docker Desktop
- Docker Engine

## 3. Cấu trúc dự án

```text
ex2/
├── Dockerfile
├── package.json
├── server.js
└── README.md
```

## 4. Xây dựng Dockerfile

Dockerfile sử dụng image nền `node:18-alpine` và khai báo các chỉ thị:

- `FROM`: Chọn image nền Node.js.
- `WORKDIR /app`: Thiết lập thư mục làm việc.
- `COPY`: Sao chép mã nguồn và file cấu hình vào image.
- `RUN`: Cài đặt các thư viện cần thiết bằng npm.
- `ENV PORT=3000`: Khai báo cổng ứng dụng.
- `EXPOSE 3000`: Khai báo cổng ứng dụng sử dụng.
- `CMD ["node", "server.js"]`: Khởi chạy ứng dụng bằng exec form.

## 5. Build Docker Image

Lệnh thực hiện:

```bash
docker build -t hw-backend:v1 .
```

Kết quả: Docker Image `hw-backend:v1` được tạo thành công.

## 6. Khởi chạy Container

Lệnh thực hiện:

```bash
docker run -d --name app-single-stage -p 8080:3000 hw-backend:v1
```

Kiểm tra container:

```bash
docker ps
```

Kết quả thực tế: Container `app-single-stage` có trạng thái `Up` và ánh xạ cổng `8080` trên máy host tới cổng `3000` trong container.

## 7. Kiểm tra log ứng dụng

Lệnh thực hiện:

```bash
docker logs app-single-stage
```

Kết quả:

```text
Server listening on port 3000
```

Kết quả xác nhận ứng dụng Node.js đã khởi động và đang lắng nghe trên cổng 3000.

## 8. Kiểm tra REST API

Lệnh thực hiện:

```bash
curl.exe http://localhost:8080/
```

Kết quả thực tế:

```json
{"status":"success","message":"Hello from Docker Single-stage!"}
```

API trả về dữ liệu JSON đúng như yêu cầu. Container hoạt động bình thường và có thể truy cập thông qua cổng 8080.

## 9. Kết luận

Đã xây dựng và đóng gói thành công ứng dụng REST API Node.js/Express bằng Single-stage Dockerfile. Docker Image `hw-backend:v1` được tạo thành công, container `app-single-stage` đang chạy và API trả về dữ liệu JSON theo yêu cầu.