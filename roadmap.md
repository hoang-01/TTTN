# LỘ TRÌNH THỰC TẬP TỐT NGHIỆP (TTTN)
**Sinh viên thực hiện:** Phạm Tiến Đạt  
**Mã sinh viên:** N22DCCN119  
**Lớp:** C31  
**Đề tài:** Xây dựng hệ thống quản lý học tập trực tuyến tích hợp trợ lý AI hỗ trợ học tập  

---

## BẢNG TỔNG HỢP LỘ TRÌNH 6 TUẦN

| Tuần | Thời gian | Nội dung công việc chính | Kết quả / Sản phẩm dự kiến |
| :--- | :--- | :--- | :--- |
| **Tuần 1** | 22/06 - 28/06 | Khảo sát nhu cầu, nghiên cứu tài liệu các hệ thống LMS lớn (Moodle, Blackboard...) và các tiêu chuẩn e-learning (SCORM, OpenBadges). Nghiên cứu lý thuyết về kiến trúc RAG, LLM và Microservices. | Báo cáo khảo sát công nghệ và đặc tả yêu cầu nghiệp vụ sơ bộ. |
| **Tuần 2** | 29/06 - 05/07 | Phân tích và Thiết kế hệ thống: Vẽ sơ đồ Use Case, sơ đồ lớp, thiết kế cơ sở dữ liệu (ERD), thiết kế luồng xử lý RAG và thiết kế giao diện (UI Mockup) cho LMS và Trợ lý AI. | Tài liệu Thiết kế Hệ thống chi tiết (System Design Document). |
| **Tuần 3** | 06/07 - 12/07 | Phát triển phân hệ LMS cốt lõi: Khởi tạo dự án, xây dựng các chức năng Quản lý tài khoản, Quản lý khóa học/danh mục, Quản lý bài học và tài liệu học tập. | Mã nguồn phân hệ LMS cơ bản và giao diện trang quản trị Admin/Teacher. |
| **Tuần 4** | 13/07 - 19/07 | Hoàn thiện chức năng LMS & Tích hợp Trợ lý AI: Xây dựng tính năng theo dõi tiến độ, lịch sử học tập. Thiết lập Vector Database từ tài liệu khóa học và tích hợp API LLM với cơ chế RAG để sinh câu trả lời. | Module theo dõi học tập và Nguyên mẫu (Prototype) Trợ lý AI phản hồi dựa trên tài liệu học tập. |
| **Tuần 5** | 20/07 - 26/07 | Xây dựng cơ chế kiểm soát phản hồi & Kiểm thử: Thiết kế các bộ lọc Guardrails để kiểm tra tính hợp lệ và giới hạn phạm vi trả lời của AI. Tiến hành Unit Test và Integration Test toàn hệ thống. | Module Guardrail hoạt động ổn định, Kịch bản kiểm thử (Test Cases) và Báo cáo kiểm thử. |
| **Tuần 6** | 27/07 - 02/08 | Tối ưu hóa hệ thống & Hoàn thiện báo cáo TTTN: Sửa lỗi phát sinh, tối ưu hóa giao diện/hiệu năng hệ thống, viết và định dạng báo cáo thực tập tốt nghiệp chi tiết. | Hệ thống LMS tích hợp AI hoàn chỉnh và Báo cáo TTTN (Word/PDF). |

---

## CHI TIẾT CÔNG VIỆC TỪNG TUẦN

### TUẦN 1: Nghiên cứu tài liệu và Khảo sát yêu cầu (22/06 - 28/06)
*   **Nhiệm vụ:**
    *   Tìm hiểu hiện trạng, ưu-nhược điểm của các hệ thống LMS lớn trên thế giới và trong nước (Moodle, Blackboard, Lạc Việt...).
    *   Nghiên cứu các tiêu chuẩn bài giảng điện tử tương tác **SCORM** (1.2, 2004) và chuẩn huy hiệu kỹ thuật số **OpenBadges**.
    *   Tìm hiểu lý thuyết nền tảng về **AI tạo sinh**, mô hình ngôn ngữ lớn (LLM), kiến trúc **RAG** (Retrieval-Augmented Generation) để truy xuất dữ liệu từ học liệu.
    *   Xác định các tác nhân (Học viên, Giảng viên, Quản trị viên) và các yêu cầu chức năng cho hệ thống LMS.
*   **Sản phẩm đạt được:** Tài liệu khảo sát công nghệ và Báo cáo phân tích yêu cầu sơ bộ.

### TUẦN 2: Phân tích & Thiết kế hệ thống (29/06 - 05/07)
*   **Nhiệm vụ:**
    *   Xây dựng đặc tả yêu cầu chi tiết (Use Case Diagram, Use Case Specification).
    *   Thiết kế kiến trúc hệ thống (lựa chọn công nghệ, thiết kế luồng trao đổi dữ liệu giữa LMS, Vector Database và LLM).
    *   Thiết kế Cơ sở dữ liệu (Database Schema / ERD) gồm các bảng quản lý người dùng, khóa học, bài học, tiến độ, và lịch sử hỏi đáp AI.
    *   Thiết kế giao diện người dùng (UI/UX Mockups) cho trang chủ, trang khóa học và khung chat của Trợ lý AI.
*   **Sản phẩm đạt được:** Tài liệu Thiết kế hệ thống (System Design Document) hoàn chỉnh.

### TUẦN 3: Xây dựng hệ thống LMS cốt lõi (06/07 - 12/07)
*   **Nhiệm vụ:**
    *   Thiết lập môi trường phát triển (Setup project backend/frontend, database).
    *   Hiện thực hóa chức năng Quản lý tài khoản (Đăng ký, Đăng nhập, Phân quyền người dùng Student/Teacher/Admin).
    *   Hiện thực hóa chức năng Quản lý khóa học, chương mục, bài học và tài liệu đi kèm (cho phép upload PDF, slide, video).
    *   Xây dựng giao diện quản trị Admin để quản lý danh sách người dùng và các danh mục khóa học.
*   **Sản phẩm đạt được:** Mã nguồn phân hệ LMS cơ bản (giao diện quản lý khóa học và học liệu tĩnh).

### TUẦN 4: Theo dõi tiến trình & Xây dựng Trợ lý AI tích hợp RAG (13/07 - 19/7)
*   **Nhiệm vụ:**
    *   Xây dựng module theo dõi tiến độ học tập (ghi nhận học viên đã học xong bài nào, xem bao nhiêu % tài liệu).
    *   Xây dựng module quản lý lịch sử học tập và kết quả điểm số.
    *   Xây dựng Trợ lý AI: Tích hợp API của mô hình ngôn ngữ lớn (LLM).
    *   Xây dựng cơ chế **RAG**: Chuyển đổi tài liệu khóa học thành các vector embeddings, lưu vào Vector Database (như Pinecone, Chroma, hoặc PGVector) để hỗ trợ tìm kiếm ngữ nghĩa khi học viên hỏi.
*   **Sản phẩm đạt được:** Module theo dõi tiến độ hoạt động tốt và demo Trợ lý AI có thể trả lời câu hỏi dựa trên nội dung khóa học.

### TUẦN 5: Xây dựng cơ chế kiểm soát (Guardrails) & Kiểm thử hệ thống (20/07 - 26/07)
*   **Nhiệm vụ:**
    *   Thiết kế cơ chế **Guardrails** cho Trợ lý AI: 
        *   Kiểm soát tính hợp lệ của câu hỏi đầu vào (chỉ cho phép hỏi trong phạm vi môn học).
        *   Kiểm duyệt câu trả lời đầu ra của LLM (tránh câu trả lời sai lệch - hallucination, hoặc chứa nội dung không phù hợp).
    *   Xây dựng kịch bản kiểm thử (Test Cases) cho các tính năng LMS và các kịch bản hội thoại với AI.
    *   Thực hiện kiểm thử chức năng (Functional Testing) và sửa các lỗi (bugs) phát sinh.
*   **Sản phẩm đạt được:** Module Guardrail hoạt động ổn định và Bảng kết quả kiểm thử hệ thống.

### TUẦN 6: Tối ưu hóa & Hoàn thiện báo cáo TTTN (27/07 - 02/08)
*   **Nhiệm vụ:**
    *   Tối ưu hiệu năng tải trang và thời gian phản hồi của trợ lý AI (áp dụng cơ chế caching câu hỏi thường gặp).
    *   Đóng gói mã nguồn hệ thống.
    *   Tổng hợp kết quả đạt được, viết báo cáo Thực tập tốt nghiệp chi tiết theo đúng form mẫu của trường (mô tả lý thuyết, phân tích thiết kế, kết quả xây dựng chương trình và đánh giá).
    *   Chuẩn bị slide thuyết trình và demo hệ thống trước hội đồng chấm thực tập.
*   **Sản phẩm đạt được:** Mã nguồn dự án hoàn thiện trên GitHub, Báo cáo TTTN bản mềm và Slide báo cáo.
