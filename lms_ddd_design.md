# BÁO CÁO PHÂN TÍCH VÀ THIẾT KẾ HỆ THỐNG QUẢN LÝ HỌC TẬP TRỰC TUYẾN TỰ HỌC THEO TIẾN ĐỘ CÁ NHÂN TÍCH HỢP TRỢ LÝ AI (MODULAR DDD DESIGN INDEX)

---

## 📌 DANH SÁCH CÁC TỆP THIẾT KẾ THEO BOUNDED CONTEXT

Để tránh sự phức tạp của một tệp duy nhất và giúp thiết kế kiến trúc trở nên rõ ràng, sạch sẽ và chuẩn hóa theo đúng mô hình Microservices, hệ thống Phân tích & Thiết kế DDD đã được phân rã thành **8 tệp tài liệu chuyên biệt** nằm trong thư mục **[`ddd_design/`](file:///e:/TTTN/ddd_design/)**:

### 0. Tổng Quan Hệ Thống & Bản Đồ Ngữ Cảnh
- 📄 **[00_overview_and_context_map.md](file:///e:/TTTN/ddd_design/00_overview_and_context_map.md)**
  - Phân rã Miền nghiệp vụ (Core, Supporting, Generic Domains).
  - Từ điển Ngôn ngữ Chung (Ubiquitous Language Dictionary).
  - Sơ đồ **Context Map** tổng thể hệ thống.
  - Sơ đồ **C4 Model Container Diagram** (Microservices Infrastructure).

### 1. Phân Hệ Xác Thực & Phân Quyền (IAM Context)
- 📄 **[01_iam_context.md](file:///e:/TTTN/ddd_design/01_iam_context.md)**
  - Quản lý tài khoản, mật khẩu (BCrypt), đăng nhập JWT, phân quyền Role (`LEARNER`, `INSTRUCTOR`, `ADMIN`).
  - Aggregate Root `User`, DDL Table `users`, Đặc tả REST API & Domain Events.

### 2. Phân Hệ Danh Mục Khóa Học & Mã Kích Hoạt (Catalog Context)
- 📄 **[02_course_catalog_context.md](file:///e:/TTTN/ddd_design/02_course_catalog_context.md)**
  - Quản lý khóa học, cây bài học, mã kích hoạt (`ActivationCode`).
  - Quy trình Ingestion nạp học liệu RAG (Chunking + Vector Embedding `text-embedding-004`).
  - DDL Tables `courses`, `activation_codes`.

### 3. Phân Hệ Tiến Trình Học Tuyến Tính (Learning Progression Context)
- 📄 **[03_learning_progression_context.md](file:///e:/TTTN/ddd_design/03_learning_progression_context.md)**
  - Quản lý đăng ký (`Enrollment`), mở khóa tuyến tính theo Đồ thị có hướng (DAG).
  - Giám sát thời lượng video ($\ge 90\%$), tính toán điểm GPA tích lũy.
  - DDL Tables `lessons`, `enrollments`, `lesson_progress`.

### 4. Cổng Quản Lý Hạn Ngạch AI (Quota Management Context)
- 📄 **[04_quota_management_context.md](file:///e:/TTTN/ddd_design/04_quota_management_context.md)**
  - Cổng Quota Gateway kiểm soát lượt chat AI (mặc định 100 msgs/tháng).
  - Logic đếm và kiểm tra siêu nhanh bằng Redis Cache tại API Gateway.
  - DDL Table `user_quotas`.

### 5. Trợ Lý AI RAG & Chấm Nháp Bài Tự Luận (AI Assistance Context)
- 📄 **[05_ai_assistance_evaluation_context.md](file:///e:/TTTN/ddd_design/05_ai_assistance_evaluation_context.md)**
  - Tương tác AI Tutor (RAG search Qdrant Vector DB, Persona Switcher `Socratic`/`Direct`).
  - Tự động so khớp Rubric và chấm nháp bài tự luận ngắn (`Draft_Graded`).
  - Tầng kiểm soát an toàn AI Guardrails (Input/Output Guardrails, Faithfulness check).
  - Sequence Diagram luồng Chat SSE Streaming (TTFT $< 2.0$s).

### 6. Hàng Đợi Kiểm Duyệt Của Giảng Viên (Instructor Approval Queue Context)
- 📄 **[06_instructor_approval_queue_context.md](file:///e:/TTTN/ddd_design/06_instructor_approval_queue_context.md)**
  - Màn hình Approval Queue triển khai cơ chế **Teacher-in-the-loop**.
  - Giảng viên kiểm tra điểm/nhận xét nháp của AI, sửa đổi và chốt điểm chính thức (`Approved`).
  - DDL Table `grading_submissions` & Sequence Diagram luồng duyệt bài.

### 7. Phân Hệ Cấp Chứng Chỉ Số PDF (Certification Context)
- 📄 **[07_certification_context.md](file:///e:/TTTN/ddd_design/07_certification_context.md)**
  - Tự động cấp chứng chỉ PDF ký số và mã QR xác thực khi tiến độ = 100% & $GPA \ge 7.0/10.0$.
  - DDL Table `certificates` & Sequence Diagram luồng tự động cấp chứng chỉ.

---

## 🔗 THAM CHIẾU VÀ ĐỐI CHIẾU
- Báo cáo Phân tích Nghiệp vụ chuẩn BABOK® v3: [`lms_analysis_babok.md`](file:///e:/TTTN/lms_analysis_babok.md)
- Báo cáo Phân tích So sánh 2 Hướng: [`deep_business_analysis.md`](file:///e:/TTTN/deep_business_analysis.md)
