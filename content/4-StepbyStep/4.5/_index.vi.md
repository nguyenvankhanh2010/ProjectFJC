---
title: "Bước 2.5: Tự Động Hóa với CloudWatch/EventBridge"
date: 2025-07-07
weight: 5
chapter: false
pre: " <b> 4.5 </b> "
---

# Bước 2.5: Tự Động Hóa với CloudWatch/EventBridge

#### Tại Sao Quan Trọng

Tự động hóa hàm Lambda với CloudWatch/EventBridge đảm bảo kiểm tra định kỳ các snapshot không sử dụng, giảm công sức thủ công. Tuy nhiên, điều này làm tăng chi phí do các lần gọi Lambda thường xuyên, vì vậy hãy cân nhắc kích hoạt thủ công để tiết kiệm chi phí.

---

## Hướng Dẫn

1. **Điều Hướng đến CloudWatch Console**:
   - Từ AWS Console, nhấp **Services** → **CloudWatch** để truy cập CloudWatch Console.

2. **Tạo Quy Tắc**:
   - Vào **Rules** → Nhấp **Create Rule**.
   - Chọn **Schedule** làm nguồn sự kiện.
   - Đặt lịch để chạy mỗi giờ (ví dụ: nhập `rate(1 hour)`).
   - Thêm **Target**:
     - Chọn **Hàm Lambda**.
     - Chọn `StaleSnapshotCleaner` từ danh sách thả xuống.
   - Nhấp **Next** → Chọn **None** cho **Action after Schedule**.
   - Cung cấp **Tên Quy Tắc** (ví dụ: `HourlySnapshotCleanup`) → Nhấp **Create Rule**.

![Tạo Lịch CloudWatch](../images/cloudwatch_schedule.png?featherlight=false&width=90pc)

3. **Theo Dõi Chi Phí**:
   - Lưu ý rằng các lần gọi Lambda thường xuyên (ví dụ: hàng giờ) sẽ phát sinh chi phí bổ sung.
   - Để tối ưu hóa chi phí, cân nhắc điều chỉnh lịch trình thành hàng ngày hoặc hàng tuần, hoặc kích hoạt hàm thủ công khi cần.

---

> **Bạn Có Biết Không?** Lịch trình CloudWatch/EventBridge có thể được tùy chỉnh để chạy vào các thời điểm hoặc ngày cụ thể, mang lại sự linh hoạt trong việc cân bằng tự động hóa và chi phí.