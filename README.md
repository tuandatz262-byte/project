# 🔐 Auth Backend — Node.js + Express + MongoDB

## Cấu trúc thư mục

```
auth-backend/
├── config/
│   └── db.js              # Kết nối MongoDB
├── middleware/
│   └── auth.js            # JWT middleware
├── models/
│   └── User.js            # User schema
├── routes/
│   └── auth.js            # Auth routes
├── .env.example           # Mẫu biến môi trường
├── server.js              # Entry point
└── package.json
```

## Cài đặt

```bash
# 1. Cài dependencies
npm install

# 2. Tạo file .env từ mẫu
cp .env.example .env

# 3. Điền thông tin vào .env
# - MONGO_URI: lấy từ MongoDB Atlas
# - JWT_SECRET: chuỗi ngẫu nhiên khó đoán

# 4. Chạy server
npm run dev     # Development (có hot reload)
npm start       # Production
```

## API Endpoints

| Method | URL | Mô tả | Auth |
|--------|-----|--------|------|
| POST | `/api/auth/register` | Đăng ký | ❌ |
| POST | `/api/auth/login` | Đăng nhập | ❌ |
| GET | `/api/auth/me` | Lấy thông tin user | ✅ |
| PUT | `/api/auth/me` | Cập nhật profile | ✅ |
| PUT | `/api/auth/change-password` | Đổi mật khẩu | ✅ |
| POST | `/api/auth/logout` | Đăng xuất | ✅ |
| GET | `/api/health` | Kiểm tra server | ❌ |

## Cách dùng API

### Đăng ký
```bash
POST /api/auth/register
Content-Type: application/json

{
  "name": "Nguyễn Văn A",
  "email": "test@example.com",
  "password": "123456"
}
```

### Đăng nhập
```bash
POST /api/auth/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "123456"
}
```
→ Trả về `token`. Lưu token này ở localStorage hoặc cookie.

### Gọi API cần auth
```bash
GET /api/auth/me
Authorization: Bearer <token>
```

## Kết nối từ Portfolio (HTML)

Trong file portfolio.html, cập nhật form submit:

```js
// Đăng ký
const res = await fetch('http://localhost:5000/api/auth/register', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name, email, password })
});
const data = await res.json();
if (data.success) {
  localStorage.setItem('token', data.token);
  // Redirect hoặc cập nhật UI
}

// Đăng nhập
const res = await fetch('http://localhost:5000/api/auth/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ email, password })
});
```

## Lấy MongoDB URI miễn phí

1. Vào https://cloud.mongodb.com
2. Tạo tài khoản → Create Free Cluster
3. Database Access → Add user
4. Network Access → Allow from anywhere (0.0.0.0/0)
5. Connect → Drivers → Copy connection string
6. Dán vào MONGO_URI trong .env
