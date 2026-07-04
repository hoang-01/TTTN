# BOUNDED CONTEXT 1: IDENTITY & ACCESS MANAGEMENT (IAM) CONTEXT
## Phân Tích & Thiết Kế Chi Tiết Phân Hệ Xác Thực & Phân Quyền

---

## I. MỤC TIÊU & PHẠM VI NGHIỆP VỤ
*   **Mục tiêu:** Quản lý toàn bộ vòng đời người dùng, xác thực danh tính bảo mật qua giao thức JWT Token, phân quyền người dùng theo các vai trò (`LEARNER`, `INSTRUCTOR`, `ADMIN`).
*   **Phạm vi nghiệp vụ:** Đăng ký tài khoản, Đăng nhập, Làm mới token (Refresh Token), Quản lý hồ sơ (Profile), Đổi mật khẩu, Phân quyền truy cập tài nguyên.

---

## II. THIẾT KẾ STRATEGIC & TACTICAL DDD

### 2.1 Aggregate Root: `User`

```
[User Aggregate Root]
  ├── Entities:
  │    └── UserProfile
  └── Value Objects:
       ├── UserId (UUID)
       ├── Email
       ├── PasswordHash
       ├── FullName
       └── UserRole (LEARNER, INSTRUCTOR, ADMIN)
```

### 2.2 Quy tắc Đóng gói (Invariants):
1. Email phải là duy nhất trên toàn hệ thống.
2. Mật khẩu không bao giờ được lưu dưới dạng văn bản thuần (Plaintext), phải mã hóa qua thuật toán BCrypt với Salt Factor $\ge 10$.
3. Vai trò người dùng (`UserRole`) quyết định phạm vi truy cập các API thuộc các Bounded Contexts khác.

---

## III. DDL CƠ SỞ DỮ LIỆU (POSTGRESQL SCHEMA)

```sql
-- Bảng Người dùng (IAM Context)
CREATE TABLE users (
    user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    full_name VARCHAR(100) NOT NULL,
    role VARCHAR(20) NOT NULL CHECK (role IN ('LEARNER', 'INSTRUCTOR', 'ADMIN')),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

---

## IV. ĐẶC TẢ API SPECIFICATION

| Method | Endpoint | Description | Auth Required | Payload / Response |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/auth/register` | Đăng ký tài khoản mới | No | `{ email, password, full_name, role }` |
| `POST` | `/api/v1/auth/login` | Đăng nhập hệ thống | No | Returns JWT Access Token & Refresh Token |
| `GET` | `/api/v1/users/me` | Lấy thông tin tài khoản hiện tại | Yes (JWT) | Returns `UserDTO` |
| `PUT` | `/api/v1/users/me/password` | Đổi mật khẩu | Yes (JWT) | `{ old_password, new_password }` |

---

## V. SỰ KIỆN MIỀN (DOMAIN EVENTS)
*   `UserRegisteredEvent`: Phát ra khi người dùng mới đăng ký thành công.
    *   *Payload:* `{ user_id, email, role, registered_at }`
    *   *Consumer:* Quota Management Context (để khởi tạo hạn ngạch mặc định `UserQuota`).
