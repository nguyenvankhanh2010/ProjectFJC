---
title: "Các Bước Chuẩn Bị"
date: 2025-07-03
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## Tổng Quan

Trước khi chạy ứng dụng **Project Management Dashboard**, bạn cần chuẩn bị đầy đủ công cụ, thư viện và cấu hình môi trường cần thiết cho cả frontend và backend. Mục này hướng dẫn từng bước cài đặt và cấu hình trên máy tính cá nhân.

---

## Danh Sách Từng Bước

### ✅ **4.1 – Clone Mã Nguồn**

- Sử dụng lệnh `git clone` để tải toàn bộ mã nguồn từ GitHub.
- Kho lưu trữ: 👉 [https://github.com/DyyyPhatt/project-management](https://github.com/DyyyPhatt/project-management)

### ✅ **4.2 – Cài Đặt Node.js**

- Cần thiết để chạy frontend (Next.js) và backend (Express).
- Tự động cài đặt kèm công cụ quản lý thư viện `npm`.

### ✅ **4.3 – Cài Đặt Visual Studio Code**

- IDE được khuyến nghị để viết và quản lý mã nguồn.
- Cài thêm các tiện ích mở rộng như Markdown, JavaScript/TypeScript, Prisma,...

### ✅ **4.4 – Cài Đặt PostgreSQL**

- Hệ quản trị cơ sở dữ liệu quan hệ chính của hệ thống.
- Lưu thông tin người dùng, dự án, nhiệm vụ, nhóm, phân quyền,...

### ✅ **4.5 – Cài Đặt PgAdmin**

- Công cụ giao diện đồ họa để quản lý cơ sở dữ liệu PostgreSQL.
- Hỗ trợ xem bảng, dữ liệu mẫu, viết truy vấn SQL.

### ✅ **4.6 – Cài Đặt Postman**

- Công cụ kiểm thử các API (GET, POST, PUT, DELETE).
- Cho phép gửi token xác thực để kiểm tra quyền truy cập API.

### ✅ **4.7 – Cài Đặt AWS CLI**

- Công cụ dòng lệnh để tương tác với các dịch vụ AWS như EC2, RDS, S3, Cognito.
- Dùng để triển khai backend và quản lý tài nguyên trên AWS.

### ✅ **4.8 – Cài Đặt Thư Viện Cần Thiết**

- Cài thư viện frontend:
  - Next.js, Tailwind CSS, MUI Data Grid, Redux Toolkit, RTK Query, React DnD, Recharts, Gantt Chart,...
- Cài thư viện backend:
  - Express.js, Prisma ORM và các công cụ liên quan.

---

👉 Sau khi hoàn tất các bước trên, bạn có thể tiếp tục với phần **cấu hình dự án, thiết lập cơ sở dữ liệu và triển khai hệ thống**.