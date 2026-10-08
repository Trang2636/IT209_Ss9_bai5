# Bài 5 – Khởi chạy dịch vụ đơn giản với Docker Compose

## Mô tả

Sử dụng Docker Compose CLI v2 để quản lý một dịch vụ Nginx đơn giản, thay thế lệnh `docker run` thủ công.

## Cấu trúc thư mục

```
ex5/
├── docker-compose.yml   # Tệp cấu hình Docker Compose
└── README.md            # Báo cáo bài tập
```

## Nội dung `docker-compose.yml`

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8082:80"
```

**Giải thích:**

| Trường | Mô tả |
|--------|-------|
| `services` | Khai báo danh sách các dịch vụ |
| `web` | Tên dịch vụ |
| `image: nginx:alpine` | Sử dụng image Nginx phiên bản Alpine |
| `ports: "8082:80"` | Map cổng 8082 trên host tới cổng 80 trong container |

> **Lưu ý:** File YAML dùng 2 dấu khoảng trắng (space) để thụt lùi dòng, **không** dùng phím Tab.

## Các bước thực hiện

### 1. Khởi chạy dịch vụ

```bash
docker compose up -d
```

**Kết quả:**

```
[+] Running 1/1
 ✔ Container ex5-web-1  Started
```

### 2. Kiểm tra trạng thái dịch vụ

```bash
docker compose ps
```

**Kết quả mong đợi:**

```
NAME         IMAGE          COMMAND                  SERVICE   CREATED          STATUS          PORTS
ex5-web-1    nginx:alpine   "/docker-entrypoint.…"   web       10 seconds ago   Up 9 seconds    0.0.0.0:8082->80/tcp
```

### 3. Kiểm tra phản hồi web

```bash
curl http://localhost:8082
```

**Kết quả mong đợi:**

```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and working.</p>
...
</body>
</html>
```

### 4. Hạ dịch vụ xuống

```bash
docker compose down
```

**Kết quả:**

```
[+] Running 2/2
 ✔ Container ex5-web-1  Removed
 ✔ Network ex5_default  Removed
```

## So sánh Docker Run vs Docker Compose

| Tiêu chí | `docker run` | `docker compose` |
|-----------|-------------|-------------------|
| Cách dùng | Gõ lệnh dài với nhiều flag | Khai báo trong file YAML |
| Quản lý nhiều service | Phải chạy từng lệnh riêng | Quản lý tất cả trong 1 file |
| Tái sử dụng | Phải nhớ/ghi lại lệnh | Lưu file, chạy lại bất cứ lúc nào |
| Khởi động lại | `docker start <name>` | `docker compose up -d` |
| Dọn dẹp | `docker stop` + `docker rm` | `docker compose down` |

> ✅ Service `web` đang chạy trên port 8082, lệnh `curl` trả về trang chào mừng của Nginx – xác nhận Docker Compose hoạt động thành công.
