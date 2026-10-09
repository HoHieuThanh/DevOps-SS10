# Bài 3: Tối ưu hóa dung lượng Image và Layer Caching với Multi-stage Build

## 1. Mục tiêu

Tái cấu trúc Dockerfile của ứng dụng Node.js/Express từ Single-stage sang Multi-stage Build, tận dụng Docker layer caching và so sánh dung lượng image.

## 2. Cấu trúc dự án

```text
ex3/
├── Dockerfile
├── Dockerfile.single
├── package.json
├── server.js
└── README.md
```

## 3. Single-stage Build

File `Dockerfile.single` sử dụng `node:18-alpine`, cài đặt dependencies và sao chép mã nguồn vào image.

Lệnh build:

```bash
docker build -f Dockerfile.single -t hw-backend:single .
```

## 4. Multi-stage Build

File `Dockerfile` có hai giai đoạn:

- **Builder:** cài đặt dependencies và chuẩn bị mã nguồn ứng dụng.
- **Runtime:** sử dụng `node:18-alpine`, chỉ sao chép `package.json`, `node_modules` và `server.js` từ builder.

Lệnh build:

```bash
docker build -t hw-backend:multistage .
```

Việc sao chép file khai báo dependencies trước khi cài đặt giúp Docker tái sử dụng cache khi mã nguồn thay đổi nhưng dependencies không thay đổi.

## 5. So sánh dung lượng image

Kết quả đo thực tế bằng `docker images hw-backend`:

| Tiêu chí | Single-stage | Multi-stage |
|---|---|---|
| Image | `hw-backend:single` | `hw-backend:multistage` |
| Base image | `node:18-alpine` | `node:18-alpine` |
| Dung lượng | [Điền số đo thực tế] | [Điền số đo thực tế] |
| Số mục lịch sử | [Đếm từ docker history] | [Đếm từ docker history] |
| Có tách builder và runtime | Không | Có |

Tỷ lệ giảm dung lượng:

```text
(Size Single-stage - Size Multi-stage) / Size Single-stage * 100%
```

Lưu ý: Hai image cùng sử dụng `node:18-alpine` và đều chứa dependencies cần thiết để chạy Express. Vì vậy, mức giảm dung lượng phụ thuộc vào nội dung các layer thực tế và có thể thấp hơn mục tiêu 50–70%.

## 6. Kiểm tra container

Lệnh chạy:

```bash
docker run -d --name app-multistage -p 8080:3000 hw-backend:multistage
```

Kiểm tra trạng thái:

```bash
docker ps
```

Kiểm tra log:

```bash
docker logs app-multistage
```

Kiểm tra API:

```bash
curl.exe -i http://localhost:8080/
```

Kết quả mong đợi: HTTP `200 OK` và JSON:

```json
{
  "status": "success",
  "message": "Hello from Docker Single-stage!"
}
```

## 7. Kết luận

Đã xây dựng hai image Single-stage và Multi-stage cho ứng dụng Node.js/Express. Multi-stage tách biệt giai đoạn chuẩn bị ứng dụng với giai đoạn runtime, đồng thời hỗ trợ tối ưu layer caching. Dung lượng image và số mục lịch sử được ghi nhận dựa trên kết quả đo thực tế.