# 📘 Bộ Bài Tập SQL Thực Hành

## **1. Phân Tích Thiết Kế Database (1NF, 2NF, 3NF)**

### 🎯 Yêu cầu:

- Cho bảng `Orders` ban đầu:

```sql
CREATE TABLE Orders (
    order_id INT,
    customer_name VARCHAR(255),
    customer_address VARCHAR(255),
    product_name VARCHAR(255),
    product_category VARCHAR(255),
    quantity INT,
    order_date DATE
);
```

1. Chuẩn hóa bảng này lên **3NF**.
2. Viết câu truy vấn tạo bảng mới sau khi chuẩn hóa.

---

## **2. Quan Hệ 1-1, 1-N, N-N**

### 🎯 Yêu cầu:

- Thiết kế các bảng và tạo khóa ngoại cho quan hệ sau:
  - **1-1**: Bảng `Users` và `UserProfiles`.
  - **1-N**: Bảng `Customers` và `Orders`.
  - **N-N**: Bảng `Students` và `Courses`.

---

## **3. Hiệu Suất & Thiết Kế Vật Lý (Indexing, Partitioning)**

### 🎯 Yêu cầu:

1. Tạo **index** để tăng tốc câu truy vấn sau:

```sql
SELECT * FROM Orders WHERE order_date BETWEEN '2024-01-01' AND '2024-02-01';
```

2. Chia bảng `Orders` thành **partition** theo tháng.

---

## **4. Quản Trị Database**

### 🎯 Yêu cầu:

1. Tạo user `dev_user` chỉ có quyền SELECT trên bảng `Orders`.
2. Thực hiện backup và restore database `sales_db`.

---

## **5. Phân Biệt Indexing, Procedure, Function**

### 🎯 Yêu cầu:

1. Tạo **index** trên cột `customer_name` của bảng `Customers`.
2. Viết **Stored Procedure** để cập nhật trạng thái đơn hàng.
3. Viết **Function** tính tổng doanh thu theo tháng.

---

## **6. SQL Queries (SELECT, JOIN, GROUP BY, etc.)**

### 🎯 Yêu cầu:

1. Lấy danh sách khách hàng có tổng số đơn hàng lớn hơn 10.
2. Lấy danh sách sản phẩm chưa có đơn hàng nào.
3. Tính tổng doanh thu theo tháng.

---

## **7. Tối Ưu Truy Vấn & Quản Lý Kết Nối Database**

### 🎯 Yêu cầu:

1. Dùng `EXPLAIN ANALYZE` để tối ưu truy vấn:

```sql
SELECT * FROM Orders WHERE customer_id = 1001;
```

2. Thử nghiệm Connection Pooling với PostgreSQL.

---

## **8. Xử Lý Dữ Liệu Lớn (2 Triệu+ Records)**

### 🎯 Yêu cầu:

1. Tạo bảng `LargeOrders` và chèn 2 triệu dòng dữ liệu giả lập.
2. Thực hiện truy vấn tối ưu trên bảng này.

---

## **9. Quản Lý Kết Nối Đồng Thời**

### 🎯 Yêu cầu:

1. Tạo 2 session update cùng một đơn hàng để kiểm tra deadlock.
2. Kiểm tra isolation levels trong PostgreSQL.

---

## **10. Transaction & Rollback**

### 🎯 Yêu cầu:

1. Viết transaction cập nhật số dư tài khoản ngân hàng và rollback nếu có lỗi.

```sql
BEGIN;
UPDATE Accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE Accounts SET balance = balance + 500 WHERE account_id = 2;
COMMIT;
```

2. Thử rollback nếu có lỗi.

---

## **11. Performance Tuning**

### 🎯 Yêu cầu:

1. Dùng `EXPLAIN ANALYZE` kiểm tra hiệu suất truy vấn.
2. So sánh tốc độ truy vấn trước và sau khi tạo index.

---

💾 **Lưu ý**: Bạn có thể chạy các bài tập trên PostgreSQL, MySQL hoặc SQL Server tuỳ vào môi trường của bạn! 🚀
