# BOUNDED CONTEXT 2: COURSE CATALOG & CONTENT CONTEXT
## Phân Tích & Thiết Kế Chi Tiết Phân Hệ Danh Mục Khóa Học & Mã Kích Hoạt

---

## I. MỤC TIÊU & PHẠM VI NGHIỆP VỤ
*   **Mục tiêu:** Quản lý cấu trúc danh mục khóa học, nội dung bài giảng, danh sách mã kích hoạt khóa học (`ActivationCode`), và tải lên học liệu đính kèm (PDF/DOCX) để tự động nạp cơ sở tri thức RAG Vector Store.
*   **Phạm vi nghiệp vụ:** Tạo/sửa khóa học, tạo mã kích hoạt, quản lý danh mục bài học, nạp học liệu RAG (Chunking + Embedding).

---

## II. THIẾT KẾ STRATEGIC & TACTICAL DDD

### 2.1 Aggregates

#### Aggregate 1: `Course`
*   **Aggregate Root:** `Course`
    *   *Value Objects:* `CourseId`, `InstructorId`, `Title`, `Description`, `Category`.

#### Aggregate 2: `ActivationCode`
*   **Aggregate Root:** `ActivationCode`
    *   *Value Objects:* `CodeId`, `CodeString`, `CourseId`, `IsUsed`, `UsedByLearnerId`.

---

## III. DDL CƠ SỞ DỮ LIỆU (POSTGRESQL SCHEMA)

```sql
-- Bảng Khóa học (Catalog Context)
CREATE TABLE courses (
    course_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title VARCHAR(255) NOT NULL,
    description TEXT,
    instructor_id UUID NOT NULL, -- FK logic sang users table
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Bảng Mã kích hoạt khóa học
CREATE TABLE activation_codes (
    code_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    code VARCHAR(50) UNIQUE NOT NULL,
    course_id UUID REFERENCES courses(course_id) ON DELETE CASCADE,
    is_used BOOLEAN DEFAULT FALSE,
    used_by UUID,
    used_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_activation_codes_code ON activation_codes(code);
```

---

## IV. ĐẶC TẢ API SPECIFICATION

| Method | Endpoint | Description | Auth Required | Payload / Response |
| :--- | :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/courses` | Tạo khóa học mới | Yes (INSTRUCTOR/ADMIN) | `{ title, description }` |
| `GET` | `/api/v1/courses` | Lấy danh sách tất cả khóa học | No | Returns Array of `CourseDTO` |
| `GET` | `/api/v1/courses/{course_id}` | Xem thông tin chi tiết & cây bài học của khóa | No | Returns `CourseDetailDTO` |
| `POST` | `/api/v1/courses/{course_id}/materials` | Tải lên tài liệu đính kèm (PDF/DOCX) nạp RAG Vector DB | Yes (INSTRUCTOR) | `multipart/form-data (file)` |
| `POST` | `/api/v1/courses/{course_id}/activation-codes` | Sinh danh sách mã kích hoạt theo lô | Yes (ADMIN) | `{ count: 50 }` |
| `POST` | `/api/v1/activation-codes/redeem` | Nhập mã kích hoạt để tham gia khóa học | Yes (LEARNER) | `{ code: "ACT-8899-XXXX" }` |

---

## V. QUY TRÌNH NẠP HỌC LIỆU RAG (VECTOR DB INGESTION)

```
[Giảng viên tải lên PDF/DOCX] ──► [Trích xuất Văn bản] ──► [Chunking (500 tokens)] 
                                                                │
                                                                ▼
[Qdrant Vector DB] ◄── [Tạo Embedding (text-embedding-004)] ◄───┤
```

---

## VI. SỰ KIỆN MIỀN (DOMAIN EVENTS)
*   `CoursePublishedEvent`: Phát ra khi khóa học mới được công bố.
*   `ActivationCodeUsedEvent`: Phát ra khi người học nhập mã kích hoạt khóa học thành công.
    *   *Consumer:* Learning Progression Context (để tạo bản ghi `Enrollment`).
