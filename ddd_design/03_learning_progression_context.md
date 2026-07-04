# BOUNDED CONTEXT 3: LEARNING PROGRESSION CONTEXT
## Phân Tích & Thiết Kế Chi Tiết Phân Hệ Tiến Trình Học Tuyến Tính (DAG)

---

## I. MỤC TIÊU & PHẠM VI NGHIỆP VỤ
*   **Mục tiêu:** Quản lý đăng ký tham gia khóa học (`Enrollment`), tính toán lộ trình học theo Đồ thị tuyến tính (Directed Acyclic Graph - DAG), ghi nhận phần trăm thời lượng xem video ($\ge 90\%$), và tính toán điểm trung bình tích lũy ($GPA$).
*   **Phạm vi nghiệp vụ:** Đăng ký khóa học, Mở khóa bài học theo thứ tự DAG, Cập nhật tiến độ video, Lưu lịch sử kết quả bài học.

---

## II. THIẾT KẾ STRATEGIC & TACTICAL DDD

### 2.1 Aggregate Root: `Enrollment`

```
[Enrollment Aggregate Root]
  ├── Entities:
  │    └── LessonProgress
  └── Value Objects:
       ├── EnrollmentId (UUID)
       ├── LearnerId (UUID)
       ├── CourseId (UUID)
       ├── ProgressPercentage (0 - 100%)
       ├── GPA (0.0 - 10.0)
       ├── LessonStatus (LOCKED, IN_PROGRESS, COMPLETED)
       └── CompletionCriteria (VideoDurationPct >= 90%, EssayApproved)
```

### 2.2 Quy tắc Đóng gói (Invariants):
1. **Ràng buộc Mở khóa tuyến tính (BR-LMS-03):** Bài học $N+1$ không thể chuyển sang trạng thái `IN_PROGRESS` trừ khi Bài học $N$ có trạng thái `COMPLETED`.
2. **Tiêu chuẩn hoàn thành video (BR-LMS-02):** Thời lượng xem video phải đạt tối thiểu $90\%$ tổng thời lượng video của bài học đó.

---

## III. DDL CƠ SỞ DỮ LIỆU (POSTGRESQL SCHEMA)

```sql
-- Bảng Bài học & Đồ thị Tuyến tính DAG
CREATE TABLE lessons (
    lesson_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    course_id UUID NOT NULL,
    title VARCHAR(255) NOT NULL,
    video_url VARCHAR(500),
    video_duration_seconds INT DEFAULT 0,
    prerequisite_lesson_id UUID REFERENCES lessons(lesson_id), -- Khóa phụ nối DAG
    order_index INT NOT NULL
);

-- Bảng Đăng ký khóa học của học viên
CREATE TABLE enrollments (
    enrollment_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    learner_id UUID NOT NULL,
    course_id UUID NOT NULL,
    progress_percentage DECIMAL(5,2) DEFAULT 0.00,
    gpa DECIMAL(3,2) DEFAULT 0.00,
    status VARCHAR(20) DEFAULT 'ACTIVE',
    enrolled_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(learner_id, course_id)
);

-- Bảng Tiến độ từng bài học của học viên
CREATE TABLE lesson_progress (
    progress_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    enrollment_id UUID REFERENCES enrollments(enrollment_id) ON DELETE CASCADE,
    lesson_id UUID REFERENCES lessons(lesson_id),
    status VARCHAR(20) DEFAULT 'LOCKED' CHECK (status IN ('LOCKED', 'IN_PROGRESS', 'COMPLETED')),
    video_watched_seconds INT DEFAULT 0,
    quiz_score DECIMAL(4,2),
    essay_status VARCHAR(20) DEFAULT 'NONE',
    completed_at TIMESTAMP WITH TIME ZONE,
    UNIQUE(enrollment_id, lesson_id)
);
```

---

## IV. ĐẶC TẢ API SPECIFICATION

| Method | Endpoint | Description | Auth Required | Payload / Response |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/enrollments/my-courses` | Xem danh sách các khóa học đang tham gia & % tiến độ | Yes (LEARNER) | Returns Array of `EnrolledCourseDTO` |
| `GET` | `/api/v1/enrollments/{enrollment_id}/lessons/{lesson_id}` | Truy cập bài học (Kiểm tra điều kiện mở khóa DAG) | Yes (LEARNER) | Returns `LessonContentDTO` nếu status $\ne$ `LOCKED` |
| `POST` | `/api/v1/lessons/{lesson_id}/video-progress` | Cập nhật thời lượng xem video | Yes (LEARNER) | `{ watched_seconds: 350 }` |
| `POST` | `/api/v1/lessons/{lesson_id}/submit-essay` | Nộp bài tập tự luận ngắn | Yes (LEARNER) | `{ student_answer: "Nội dung bài làm..." }` |
| `GET` | `/api/v1/enrollments/{enrollment_id}/summary` | Xem tổng quan kết quả học tập & bảng điểm GPA | Yes (LEARNER/INSTRUCTOR) | Returns `LearningSummaryDTO` |

---

## V. SỰ KIỆN MIỀN (DOMAIN EVENTS)
*   `LessonCompletedEvent`: Phát ra khi bài học đạt 100% điều kiện hoàn thành.
*   `EssaySubmittedEvent`: Phát ra khi học viên nộp bài tự luận ngắn.
    *   *Consumer:* AI Assistance Context (để thực hiện chấm nháp `Draft_Graded`).
*   `CourseCompletedEvent`: Phát ra khi tiến độ đạt 100% và $GPA \ge 7.0$.
    *   *Consumer:* Certification Context (để sinh chứng chỉ PDF).
