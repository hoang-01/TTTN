# LỘ TRÌNH THỰC TẬP TỐT NGHIỆP (TTTN)
**Sinh viên thực hiện:** Phạm Tiến Đạt  
**Mã sinh viên:** N22DCCN119  
**Lớp:** C31  
**Đề tài:** Xây dựng hệ thống quản lý học tập trực tuyến tích hợp trợ lý AI hỗ trợ học tập  

---

## BẢNG TỔNG HỢP LỘ TRÌNH 7 TUẦN

| Tuần | Thời gian | Nội dung công việc chính | Kết quả / Sản phẩm dự kiến |
| :--- | :--- | :--- | :--- |
| **Tuần 1** | 22/06 - 28/06 | Khảo sát nhu cầu, nghiên cứu tài liệu các hệ thống LMS lớn (Moodle, Blackboard...) và các tiêu chuẩn e-learning (SCORM, OpenBadges). Nghiên cứu lý thuyết về kiến trúc RAG, LLM và Microservices. | Báo cáo khảo sát công nghệ và đặc tả yêu cầu nghiệp vụ sơ bộ. |
| **Tuần 2** | 29/06 - 05/07 | Tiếp tục khảo sát, nghiên cứu tài liệu chuyên sâu và phân tích yêu cầu nghiệp vụ chi tiết cho hệ thống quản lý học tập trực tuyến và trợ lý AI. | Báo cáo khảo sát chi tiết và Tài liệu Phân tích yêu cầu nghiệp vụ. |
| **Tuần 3** | 06/07 - 12/07 | Phân tích và Thiết kế hệ thống: Vẽ sơ đồ Use Case, sơ đồ lớp, thiết kế cơ sở dữ liệu (ERD), thiết kế luồng xử lý RAG và thiết kế giao diện (UI Mockup) cho LMS và Trợ lý AI. | Tài liệu Thiết kế Hệ thống chi tiết (System Design Document). |
| **Tuần 4** | 13/07 - 19/07 | Khởi tạo dự án & Phát triển phân hệ LMS cốt lõi: Thiết lập môi trường, xây dựng các chức năng Quản lý tài khoản, Quản lý khóa học/danh mục, bài học và tài liệu học tập. | Khung dự án (Project setup) và mã nguồn phân hệ LMS cơ bản (giao diện Admin/Teacher). |
| **Tuần 5** | 20/07 - 26/07 | Hoàn thiện chức năng LMS & Xây dựng Trợ lý AI tích hợp RAG: Xây dựng tính năng theo dõi tiến độ học tập. Tích hợp API LLM với cơ chế RAG (Vector DB) để hỗ trợ phản hồi dựa trên tài liệu học tập. | Module theo dõi học tập và Nguyên mẫu (Prototype) Trợ lý AI phản hồi dựa trên tài liệu khóa học. |
| **Tuần 6** | 27/07 - 02/08 | Xây dựng cơ chế kiểm soát phản hồi của trợ lý AI: Thiết kế và phát triển bộ lọc Guardrails để kiểm tra tính hợp lệ và giới hạn phạm vi trả lời của AI trong nội dung khóa học. | Module Guardrail hoạt động ổn định và tích hợp thành công vào Trợ lý AI. |
| **Tuần 7** | 03/08 - 09/08 | Kiểm thử, Tối ưu hóa hệ thống & Hoàn thiện báo cáo TTTN: Thực hiện Unit Test/Integration Test, sửa lỗi, tối ưu hiệu năng/giao diện và hoàn thiện cuốn báo cáo thực tập tốt nghiệp. | Hệ thống LMS tích hợp AI hoàn chỉnh và Báo cáo TTTN hoàn thiện (Word/PDF). |

---

## CHI TIẾT CÔNG VIỆC TỪNG TUẦN

### TUẦN 1: Nghiên cứu tài liệu và Khảo sát yêu cầu (22/06 - 28/06)
*   **Nhiệm vụ:**
    *   Tìm hiểu hiện trạng, ưu-nhược điểm của các hệ thống LMS lớn trên thế giới và trong nước (Moodle, Blackboard, Lạc Việt...).
    *   Nghiên cứu các tiêu chuẩn bài giảng điện tử tương tác **SCORM** (1.2, 2004) và chuẩn huy hiệu kỹ thuật số **OpenBadges**.
    *   Tìm hiểu lý thuyết nền tảng về **AI tạo sinh**, mô hình ngôn ngữ lớn (LLM), kiến trúc **RAG** (Retrieval-Augmented Generation) để truy xuất dữ liệu từ học liệu.
    *   Xác định các tác nhân (Học viên, Giảng viên, Quản trị viên) và các yêu cầu chức năng sơ bộ cho hệ thống LMS.
*   **Sản phẩm đạt được:** Báo cáo khảo sát công nghệ và đặc tả yêu cầu nghiệp vụ sơ bộ.

### TUẦN 2: Nghiên cứu sâu & Phân tích yêu cầu nghiệp vụ chi tiết (29/06 - 05/07)
*   **Nhiệm vụ:**
    *   Tiếp tục nghiên cứu chuyên sâu về tài liệu kỹ thuật của các thư viện hỗ trợ SCORM, OpenBadges.
    *   Khảo sát thực tế nhu cầu đào tạo chuyên sâu và các luồng quy trình nghiệp vụ cần giải quyết trong một LMS tích hợp AI.
    *   Nghiên cứu sâu các giải pháp công nghệ để triển khai RAG (chọn Vector Database phù hợp, tìm hiểu API của mô hình ngôn ngữ lớn).
    *   Phân tích chi tiết các yêu cầu nghiệp vụ (yêu cầu chức năng và phi chức năng) của hệ thống.
*   **Sản phẩm đạt được:** Báo cáo khảo sát chi tiết và Tài liệu Phân tích yêu cầu nghiệp vụ hoàn thiện.

### TUẦN 3: Phân tích & Thiết kế hệ thống (06/07 - 12/07)
*   **Nhiệm vụ:**
    *   Xây dựng đặc tả yêu cầu chi tiết (Use Case Diagram, Use Case Specification).
    *   Thiết kế kiến trúc hệ thống (lựa chọn công nghệ, thiết kế luồng trao đổi dữ liệu giữa LMS, Vector Database và LLM).
    *   Thiết kế Cơ sở dữ liệu (Database Schema / ERD) gồm các bảng quản lý người dùng, khóa học, bài học, tiến độ, và lịch sử hỏi đáp AI.
    *   Thiết kế giao diện người dùng (UI/UX Mockups) cho trang chủ, trang khóa học và khung chat của Trợ lý AI.
*   **Sản phẩm đạt được:** Tài liệu Thiết kế hệ thống (System Design Document) hoàn chỉnh.

### TUẦN 4: Khởi tạo dự án & Xây dựng hệ thống LMS cốt lõi (13/07 - 19/07)
*   **Nhiệm vụ:**
    *   Thiết lập môi trường phát triển (Setup project backend/frontend, database).
    *   Hiện thực hóa chức năng Quản lý tài khoản (Đăng ký, Đăng nhập, Phân quyền người dùng Student/Teacher/Admin).
    *   Hiện thực hóa chức năng Quản lý khóa học, chương mục, bài học và tài liệu đi kèm (cho phép upload PDF, slide, video).
    *   Xây dựng giao diện quản trị Admin để quản lý danh sách người dùng và các danh mục khóa học.
*   **Sản phẩm đạt được:** Khung dự án (Project setup) và mã nguồn phân hệ LMS cơ bản (giao diện quản lý khóa học và học liệu tĩnh).

### TUẦN 5: Theo dõi tiến trình & Xây dựng Trợ lý AI tích hợp RAG (20/07 - 26/07)
*   **Nhiệm vụ:**
    *   Xây dựng module theo dõi tiến độ học tập (ghi nhận học viên đã học xong bài nào, xem bao nhiêu % tài liệu).
    *   Xây dựng Trợ lý AI: Tích hợp API của mô hình ngôn ngữ lớn (LLM).
    *   Xây dựng cơ chế **RAG**: Chuyển đổi tài liệu khóa học thành các vector embeddings, lưu vào Vector Database (như Pinecone, Chroma, hoặc PGVector) để hỗ trợ tìm kiếm ngữ nghĩa khi học viên hỏi.
*   **Sản phẩm đạt được:** Module theo dõi tiến độ hoạt động tốt và Nguyên mẫu (Prototype) Trợ lý AI phản hồi dựa trên tài liệu học tập.

### TUẦN 6: Xây dựng cơ chế kiểm soát phản hồi (Guardrails) (27/07 - 02/08)
*   **Nhiệm vụ:**
    *   Thiết kế và xây dựng cơ chế **Guardrails** cho Trợ lý AI.
    *   Kiểm soát tính hợp lệ của câu hỏi đầu vào (chỉ cho phép hỏi các chủ đề liên quan đến nội dung khóa học).
    *   Kiểm duyệt câu trả lời đầu ra của LLM (ngăn chặn thông tin sai lệch - hallucination, lọc ngôn từ không phù hợp, giới hạn phạm vi kiến thức).
    *   Tích hợp bộ lọc Guardrails trực tiếp vào luồng xử lý RAG đã xây dựng ở Tuần 5.
*   **Sản phẩm đạt được:** Module Guardrail hoạt động ổn định và được liên kết thành công với Trợ lý AI.

### TUẦN 7: Kiểm thử, Tối ưu hóa & Hoàn thiện báo cáo TTTN (03/08 - 09/08)
*   **Nhiệm vụ:**
    *   Lập kịch bản kiểm thử (Test Cases), tiến hành Unit Test và Integration Test toàn bộ hệ thống để phát hiện và sửa các lỗi phát sinh.
    *   Tối ưu hiệu năng tải trang và thời gian phản hồi của trợ lý AI.
    *   Tổng hợp kết quả đạt được, viết báo cáo Thực tập tốt nghiệp chi tiết theo đúng form mẫu của trường (mô tả lý thuyết, phân tích thiết kế, kết quả xây dựng chương trình và đánh giá).
    *   Chuẩn bị slide thuyết trình và demo hệ thống trước hội đồng chấm thực tập.
*   **Sản phẩm đạt được:** Mã nguồn dự án hoàn thiện trên GitHub, Báo cáo TTTN bản mềm và Slide báo cáo.
