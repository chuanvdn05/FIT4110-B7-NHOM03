```markdown
# FIT4110 – B7 Notification Service

Notification Service được phát triển trong khuôn khổ học phần **Dịch vụ kết nối và Công nghệ nền tảng (FIT4110)** – Trường Đại học Đại Nam.

Dự án là một thành phần của hệ thống **Smart Campus Operations Platform**, có nhiệm vụ tiếp nhận các cảnh báo từ Core Business Service (B6), gửi thông báo đến người dùng thông qua Telegram Bot API, lưu trữ lịch sử xử lý vào PostgreSQL và cung cấp dữ liệu cho Analytics Service (B5).

---

# Kiến trúc hệ thống

```

B6 Core Business
│
▼
Notification Service (B7)
│
├────────► Telegram Bot
│
▼
PostgreSQL
│
▼
B5 Analytics

```

---

# Chức năng chính

- Tiếp nhận Alert từ Core Business Service (B6).
- Xác thực Bearer Token.
- Kiểm tra dữ liệu đầu vào.
- Chống gửi trùng (Idempotency).
- Retry khi gửi thất bại.
- Gửi thông báo qua Telegram Bot API.
- Lưu lịch sử gửi thông báo vào PostgreSQL.
- Cung cấp Logs API cho Analytics Service (B5).

---

# Công nghệ sử dụng

- Python 3.11
- FastAPI
- PostgreSQL
- Docker
- Docker Compose
- OpenAPI Specification
- Telegram Bot API
- Postman

---

# Cấu trúc dự án

```

FIT4110-B7-NHOM03/
│
├── contracts/
│   └── team-notify.openapi.yaml
│
├── postman/
│   ├── collections/
│   └── environments/
│
├── reports/
│
├── src/
│   ├── mock_channel/
│   │   └── main.py
│   │
│   └── notify_app/
│       ├── **init**.py
│       └── main.py
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── README.md
└── RUN_COMPOSE.md

```

---

# REST API

## Health Check

```

GET /health

```

---

## Receive Notification

```

POST /api/notifications/alerts

````

### Request

```json
{
  "alert_id": "ALT-20260617-001",
  "severity": "high",
  "message": "Temperature exceeded threshold",
  "target": "security_team",
  "channel": "telegram"
}
````

### Response

```json
{
  "alert_id": "ALT-20260617-001",
  "sent": true,
  "channel": "telegram",
  "status": "delivered",
  "attempts": 1,
  "detail": "telegram delivered"
}
```

---

## Logs API

```
GET /internal/notifications/logs
```

---

## Notification Status

```
GET /internal/notifications/{alert_id}/status
```

---

# Triển khai

## Clone project

```bash
git clone https://github.com/FIT-17-09/fit4110-lab5-nhom03.git
cd fit4110-b7-nhom03
```

---

## Khởi động

```bash
docker compose up -d --build
```

---

## Kiểm tra

```bash
docker compose ps
```

---

## Health

```
http://localhost:8000/health
```

---

# Tích hợp hệ thống

Notification Service đóng vai trò:

### Consumer

Nhận Alert từ:

* B6 – Core Business Service

### Provider

Cung cấp Logs API cho:

* B5 – Analytics Service

---

# Minh chứng

Dự án đã được kiểm thử thành công với:

* Docker Compose
* Health Check
* Telegram Notification
* PostgreSQL Logging
* Retry Mechanism
* Idempotency
* OpenAPI Contract
* Tích hợp B6
* Tích hợp B5

---

# Thành viên nhóm B7

* Nguyễn Xuân Anh
* Vũ Đức Nguyễn Chuẩn
* Tăng Tất Cương

---

# Giảng viên hướng dẫn

**ThS. Lê Thị Thùy Trang**

---

# Học phần

FIT4110 – Dịch vụ kết nối và Công nghệ nền tảng

Khoa Công nghệ Thông tin

Trường Đại học Đại Nam

2026

```
```
