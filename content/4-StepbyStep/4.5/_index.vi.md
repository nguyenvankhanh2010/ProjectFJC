---
title: "Bước 4.5: Tự Động Hóa với CloudWatch/EventBridge"
date: 2025-07-07
weight: 4.5
chapter: false
pre: "<b>4.5 </b>"
---

# Bước 4.5: Tự Động Hóa với CloudWatch/EventBridge

#### Tại Sao Quan Trọng

Tự động hóa hàm Lambda bằng CloudWatch/EventBridge đảm bảo dọn dẹp snapshot không sử dụng định kỳ, nhưng các lần gọi thường xuyên có thể tăng chi phí. Bước này thiết lập lịch trình để kích hoạt hàm định kỳ.

---

## Hướng Dẫn

1. **Điều Hướng đến CloudWatch Console**:
   - Từ trang chủ AWS Console, nhấp **Services** → **CloudWatch** để vào CloudWatch Console.

   ![CloudWatch Console](/images/cloudwatch_console.png?featherlight=false&width=90pc)

2. **Tạo Quy Tắc**:
   - Trong CloudWatch Console, nhấp **Rules** ở thanh bên trái dưới mục **Events**.
   - Nhấp **Create Rule**.
   - Chọn **Schedule** làm nguồn sự kiện.
   - Cấu hình lịch trình:
     - Chọn **Fixed rate** và đặt thành `1 hour` (ví dụ: `rate(1 hour)`).
   - Nhấp **Add Target** và chọn **Lambda function**.
   - Trong danh sách **Function**, chọn `StaleSnapshotCleaner`.

   ![Quy Tắc CloudWatch](/images/cloudwatch_rules.png?featherlight=false&width=90pc)
   ![Lịch CloudWatch](/images/cloudwatch_schedule.png?featherlight=false&width=90pc)
   ![Mục Tiêu CloudWatch](/images/cloudwatch_target.png?featherlight=false&width=90pc)
   ![Cấu Hình Lịch CloudWatch](/images/cloudwatch_schedule_config.png?featherlight=false&width=90pc)

3. **Cấu Hình Chi Tiết Quy Tắc**:
   - Nhấp **Next**.
   - Trong phần **Targets**, đảm bảo hàm `StaleSnapshotCleaner` được chọn.
   - Nhấp **Next** lần nữa.
   - Đối với **Action after Schedule**, chọn **None**.

   ![CloudWatch Tiếp Theo](/images/cloudwatch_next.png?featherlight=false&width=90pc)
   ![Chọn Mục Tiêu CloudWatch](/images/cloudwatch_target_selection.png?featherlight=false&width=90pc)
   ![CloudWatch Hành Động None](/images/cloudwatch_action_none.png?featherlight=false&width=90pc)

4. **Hoàn Thiện Quy Tắc**:
   - Đặt **Tên Quy Tắc** là `HourlySnapshotCleanup`.
   - Nhấp **Create Rule** để kích hoạt lịch trình.

   ![Tên Quy Tắc CloudWatch](/images/cloudwatch_rule_name.png?featherlight=false&width=90pc)
   ![Quy Tắc CloudWatch Đã Tạo](/images/cloudwatch_rule_created.png?featherlight=false&width=90pc)

5. **Theo Dõi Chi Phí**:
   - Lịch trình kích hoạt hàm Lambda mỗi giờ, có thể tăng chi phí do thực thi thường xuyên.
   - Cân nhắc điều chỉnh thành lịch hàng ngày hoặc hàng tuần (ví dụ: `rate(1 day)`) hoặc kích hoạt thủ công để tối ưu chi phí.

---

> **Bạn Có Biết Không?** Lịch trình CloudWatch/EventBridge có thể được tùy chỉnh để chạy vào các thời điểm hoặc ngày cụ thể, mang lại sự linh hoạt trong quản lý chi phí.