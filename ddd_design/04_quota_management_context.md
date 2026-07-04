# BOUNDED CONTEXT 4: QUOTA & RESOURCE MANAGEMENT CONTEXT
## Phân Tích & Thiết Kế Chi Tiết Cổng Quản Lý Hạn Ngạch AI (Quota Gateway)

---

## I. MỤC TIÊU & PHẠM VI NGHIỆP VỤ
*   **Mục tiêu:** Kiểm soát và phân bổ tài nguyên truy vấn LLM API của học viên, ngăn chặn rủi ro quá tải hệ thống và bảo đảm chi phí hạ tầng không vượt định mức.
*   **Phạm vi nghiệp vụ:** Khởi tạo hạn ngạch tin nhắn AI (`UserQuota` - mặc định 100 câu/tháng), kiểm tra lượt khả dụng ở cấp API Gateway trước khi chuyển tiếp yêu cầu đến LLM, tự động khóa nút gửi câu hỏi khi đạt trần.

---

## II. THIẾT KẾ STRATEGIC & TACTICAL DDD

### 2.1 Aggregate Root: `UserQuota`

```
[UserQuota Aggregate Root]
  ├── Entities:
  │    └── QuotaConsumptionLog
  └── Value Objects:
       ├── QuotaId (UUID)
       ├── LearnerId (UUID)
       ├── MonthlyLimit (100 msgs)
       ├── UsedCount (0..100)
       ├── BillingMonth / BillingYear
       └── QuotaStatus (ACTIVE, EXHAUSTED)
```

### 2.2 Quy tắc Đóng gói (Invariants):
1. **Ràng buộc Hạn ngạch AI (BR-COM-01):** Tài khoản học viên tiêu chuẩn có hạn ngạch tối đa 100 lượt chat/tháng. Khi `UsedCount` $\ge$ `MonthlyLimit`, từ chối mọi yêu cầu chat AI và trả về mã lỗi `HTTP 429 Too Many Requests`.

---

## III. DDL CƠ SỞ DỮ LIỆU (POSTGRESQL SCHEMA)

```sql
-- Bảng Hạn ngạch AI (Quota Context)
CREATE TABLE user_quotas (
    quota_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    learner_id UUID UNIQUE NOT NULL,
    monthly_limit INT DEFAULT 100,
    used_count INT DEFAULT 0,
    billing_month INT NOT NULL,
    billing_year INT NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_user_quotas_learner ON user_quotas(learner_id);
```

---

## IV. ĐẶC TẢ API SPECIFICATION

| Method | Endpoint | Description | Auth Required | Payload / Response |
| :--- | :--- | :--- | :--- | :--- |
| `GET` | `/api/v1/quotas/me` | Học viên xem số lượt tin nhắn AI còn lại trong tháng | Yes (LEARNER) | Returns `{ monthly_limit: 100, used_count: 35, remaining: 65, status: "ACTIVE" }` |
| `GET` | `/api/v1/admin/quotas` | Admin xem báo cáo tổng quan tiêu thụ API token | Yes (ADMIN) | Returns Array of `UserQuotaSummaryDTO` |
| `PUT` | `/api/v1/admin/quotas/{learner_id}` | Admin nâng trần định mức Quota cho tài khoản cá biệt | Yes (ADMIN) | `{ new_monthly_limit: 200 }` |
| `POST` | `/api/v1/internal/quotas/verify-and-consume` | Microservice nội bộ kiểm tra & trừ lượt Quota | Yes (INTERNAL API KEY) | `{ learner_id }` -> Returns `200 OK` hoặc `429 Too Many Requests` |

---

## V. THIẾT KẾ LUỒNG XỬ LÝ QUOTA GATEWAY (REDIS CACHE)

```python
# Logic kiểm tra hạn ngạch siêu nhanh qua Redis Cache tại API Gateway
def check_and_increment_quota(learner_id: str) -> bool:
    quota_key = f"quota:{learner_id}:{current_year_month()}"
    used_count = redis_client.get(quota_key) or 0
    monthly_limit = 100 # Cấu hình mặc định
    
    if int(used_count) >= monthly_limit:
        return False # Từ chối yêu cầu
        
    redis_client.incr(quota_key)
    return True # Hợp lệ, chuyển tiếp sang AI Engine
```

---

## VI. SỰ KIỆN MIỀN (DOMAIN EVENTS)
*   `AIQuotaExhaustedEvent`: Phát ra khi học viên dùng hết 100 lượt tin nhắn trong chu kỳ.
    *   *Consumer:* Presentation Layer (để tạm khóa giao diện ô nhập chat và gửi thông báo).
