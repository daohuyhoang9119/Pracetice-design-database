# Tài liệu ôn tập SQL và thiết kế database cho phỏng vấn Data Engineer

## Phân tích thiết kế database - Chuẩn hóa dữ liệu

### 1NF (First Normal Form)
- **Định nghĩa**: Mỗi cột chứa giá trị nguyên tử (không thể chia nhỏ hơn)
- **Yêu cầu**:
  - Không có dữ liệu lặp lại (repeating groups)
  - Mỗi cột chỉ chứa một giá trị duy nhất
  - Mỗi hàng phải có một khóa chính (primary key)
- **Ví dụ**:
  ```
  Thay vì: customer(id, name, phone1, phone2, phone3)
  Nên dùng: customer(id, name) và customer_phones(customer_id, phone)
  ```

### 2NF (Second Normal Form)
- **Định nghĩa**: Đã đạt 1NF và tất cả các thuộc tính không khóa phải phụ thuộc đầy đủ vào khóa chính
- **Yêu cầu**: 
  - Loại bỏ các phụ thuộc hàm một phần (partial dependency)
  - Nếu có khóa chính gồm nhiều cột, các cột không khóa phải phụ thuộc vào toàn bộ khóa, không phải một phần
- **Ví dụ**:
  ```
  Thay vì: order_items(order_id, product_id, quantity, product_name, price)
  Nên dùng: order_items(order_id, product_id, quantity) và products(product_id, product_name, price)
  ```

### 3NF (Third Normal Form)
- **Định nghĩa**: Đã đạt 2NF và các thuộc tính không khóa không phụ thuộc bắc cầu vào khóa chính
- **Yêu cầu**:
  - Loại bỏ các phụ thuộc hàm bắc cầu (transitive dependency)
  - Các thuộc tính không khóa không nên phụ thuộc vào thuộc tính không khóa khác
- **Ví dụ**:
  ```
  Thay vì: employees(emp_id, emp_name, dept_id, dept_name, dept_location)
  Nên dùng: employees(emp_id, emp_name, dept_id) và departments(dept_id, dept_name, dept_location)
  ```

## Các quan hệ trong database

### Quan hệ 1-1 (One-to-One)
- **Định nghĩa**: Một bản ghi trong bảng A liên kết với không quá một bản ghi trong bảng B và ngược lại
- **Cách triển khai**: 
  - Sử dụng khóa chính của một bảng làm khóa ngoại trong bảng kia
  - Thêm ràng buộc UNIQUE trên khóa ngoại
- **Ví dụ**: 
  ```sql
  CREATE TABLE users (user_id INT PRIMARY KEY, username VARCHAR(50));
  CREATE TABLE user_profiles (
      profile_id INT PRIMARY KEY,
      user_id INT UNIQUE,
      address VARCHAR(100),
      FOREIGN KEY (user_id) REFERENCES users(user_id)
  );
  ```

### Quan hệ 1-n (One-to-Many)
- **Định nghĩa**: Một bản ghi trong bảng A có thể liên kết với nhiều bản ghi trong bảng B, nhưng một bản ghi trong bảng B chỉ liên kết với một bản ghi trong bảng A
- **Cách triển khai**: Đặt khóa chính của bảng "một" làm khóa ngoại trong bảng "nhiều"
- **Ví dụ**:
  ```sql
  CREATE TABLE departments (dept_id INT PRIMARY KEY, dept_name VARCHAR(50));
  CREATE TABLE employees (
      emp_id INT PRIMARY KEY,
      emp_name VARCHAR(50),
      dept_id INT,
      FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
  );
  ```

### Quan hệ n-n (Many-to-Many)
- **Định nghĩa**: Nhiều bản ghi trong bảng A có thể liên kết với nhiều bản ghi trong bảng B
- **Cách triển khai**: Tạo bảng trung gian (junction table) chứa khóa ngoại tham chiếu đến cả hai bảng
- **Ví dụ**:
  ```sql
  CREATE TABLE students (student_id INT PRIMARY KEY, student_name VARCHAR(50));
  CREATE TABLE courses (course_id INT PRIMARY KEY, course_name VARCHAR(50));
  CREATE TABLE enrollments (
      student_id INT,
      course_id INT,
      enrollment_date DATE,
      PRIMARY KEY (student_id, course_id),
      FOREIGN KEY (student_id) REFERENCES students(student_id),
      FOREIGN KEY (course_id) REFERENCES courses(course_id)
  );
  ```

## Performance và Indexing

### Các loại Index

#### 1. B-Tree Index (Index thông thường)
- **Đặc điểm**: Cấu trúc cây cân bằng, thích hợp cho phép so sánh =, >, <, BETWEEN, LIKE 'abc%'
- **Khi sử dụng**: Cho các cột trong mệnh đề WHERE, ORDER BY, GROUP BY
- **Ví dụ**:
  ```sql
  CREATE INDEX idx_last_name ON employees(last_name);
  ```

#### 2. Unique Index
- **Đặc điểm**: Đảm bảo tính duy nhất của dữ liệu trên các cột được đánh index
- **Khi sử dụng**: Khi cần ràng buộc UNIQUE trên cột hoặc tổ hợp cột
- **Ví dụ**:
  ```sql
  CREATE UNIQUE INDEX idx_email ON users(email);
  ```

#### 3. Composite Index (Index phức hợp)
- **Đặc điểm**: Index trên nhiều cột, thứ tự cột là quan trọng
- **Khi sử dụng**: Khi truy vấn thường xuyên filter trên nhiều cột
- **Ví dụ**:
  ```sql
  CREATE INDEX idx_last_first_name ON employees(last_name, first_name);
  ```

#### 4. Clustered Index
- **Đặc điểm**: Xác định thứ tự vật lý của dữ liệu trong bảng, mỗi bảng chỉ có một
- **Khi sử dụng**: Mặc định được tạo trên khóa chính
- **Ví dụ**:
  ```sql
  -- SQL Server
  CREATE CLUSTERED INDEX idx_order_date ON orders(order_date);
  ```

#### 5. Non-Clustered Index
- **Đặc điểm**: Tạo cấu trúc dữ liệu tách biệt với dữ liệu bảng
- **Khi sử dụng**: Có thể tạo nhiều index non-clustered cho một bảng
- **Ví dụ**:
  ```sql
  -- SQL Server
  CREATE NONCLUSTERED INDEX idx_product_name ON products(product_name);
  ```

#### 6. Full-Text Index
- **Đặc điểm**: Hỗ trợ tìm kiếm văn bản toàn diện
- **Khi sử dụng**: Tìm kiếm trong các cột chứa văn bản dài
- **Ví dụ**:
  ```sql
  -- SQL Server
  CREATE FULLTEXT INDEX ON articles(content) KEY INDEX PK_articles;
  ```

#### 7. Bitmap Index
- **Đặc điểm**: Sử dụng mảng bit để tối ưu hóa cho các cột có số lượng giá trị phân biệt thấp
- **Khi sử dụng**: Cho các cột như gender, status có ít giá trị khác nhau
- **Ví dụ**:
  ```sql
  -- Oracle
  CREATE BITMAP INDEX idx_gender ON employees(gender);
  ```

### Stored Procedure, Function, Trigger

#### Stored Procedure
- **Định nghĩa**: Tập hợp các câu lệnh SQL có thể nhận tham số, thực hiện các thao tác và trả về kết quả
- **Ưu điểm**:
  - Giảm traffic mạng
  - Tăng bảo mật
  - Tái sử dụng code
  - Cải thiện hiệu suất (biên dịch sẵn)
- **Ví dụ**:
  ```sql
  CREATE PROCEDURE get_employees_by_dept(IN dept_id INT)
  BEGIN
      SELECT * FROM employees WHERE department_id = dept_id;
  END;
  ```

#### Function
- **Định nghĩa**: Tương tự stored procedure nhưng luôn phải trả về giá trị và có thể sử dụng trong các câu lệnh SQL
- **Phân biệt với procedure**:
  - Function phải trả về giá trị, procedure không bắt buộc
  - Function không thể thực hiện thao tác DML (INSERT, UPDATE, DELETE)
  - Function có thể gọi trong câu lệnh SELECT
- **Ví dụ**:
  ```sql
  CREATE FUNCTION calculate_age(birth_date DATE) 
  RETURNS INT
  BEGIN
      RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE());
  END;
  
  -- Sử dụng
  SELECT name, calculate_age(birth_date) AS age FROM employees;
  ```

#### Trigger
- **Định nghĩa**: Thủ tục tự động thực thi khi có sự kiện (INSERT, UPDATE, DELETE) xảy ra trên bảng
- **Loại**:
  - BEFORE trigger: Thực thi trước khi sự kiện xảy ra
  - AFTER trigger: Thực thi sau khi sự kiện xảy ra
- **Ví dụ**:
  ```sql
  CREATE TRIGGER before_employee_update
  BEFORE UPDATE ON employees
  FOR EACH ROW
  BEGIN
      SET NEW.last_updated = NOW();
  END;
  ```

## SQL Cơ bản và Nâng cao

### Các câu lệnh SELECT, UPDATE, DELETE, INSERT

#### SELECT
```sql
-- Cơ bản
SELECT column1, column2 FROM table_name WHERE condition;

-- Với JOIN
SELECT o.order_id, c.customer_name 
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
WHERE o.order_date > '2023-01-01';

-- Với GROUP BY và HAVING
SELECT department_id, COUNT(*) as employee_count
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 5;

-- Với CTE (Common Table Expression)
WITH high_salary_employees AS (
    SELECT * FROM employees WHERE salary > 100000
)
SELECT department_id, AVG(salary) 
FROM high_salary_employees
GROUP BY department_id;

-- Với Window Function
SELECT 
    employee_name,
    salary,
    AVG(salary) OVER (PARTITION BY department_id) as dept_avg_salary,
    RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) as salary_rank
FROM employees;
```

#### UPDATE
```sql
-- Cơ bản
UPDATE employees SET salary = salary * 1.1 WHERE department_id = 3;

-- Với JOIN
UPDATE orders o
JOIN customers c ON o.customer_id = c.customer_id
SET o.status = 'VIP'
WHERE c.customer_type = 'premium';

-- Với CTE
WITH old_orders AS (
    SELECT order_id FROM orders WHERE order_date < '2023-01-01'
)
UPDATE orders
SET status = 'archived'
WHERE order_id IN (SELECT order_id FROM old_orders);
```

#### DELETE
```sql
-- Cơ bản
DELETE FROM temporary_logs WHERE created_at < '2023-01-01';

-- Với JOIN
DELETE o
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
WHERE c.status = 'inactive' AND o.order_date < '2023-01-01';

-- Giới hạn số bản ghi xóa
DELETE FROM logs
WHERE created_at < '2023-01-01'
LIMIT 1000;
```

#### INSERT
```sql
-- Cơ bản
INSERT INTO employees (first_name, last_name, department_id, salary)
VALUES ('John', 'Doe', 3, 75000);

-- Nhiều bản ghi
INSERT INTO employees (first_name, last_name, department_id, salary)
VALUES 
    ('John', 'Doe', 3, 75000),
    ('Jane', 'Smith', 2, 82000);

-- Từ SELECT
INSERT INTO employee_archive (employee_id, first_name, last_name, termination_date)
SELECT employee_id, first_name, last_name, CURRENT_DATE
FROM employees
WHERE status = 'terminated';

-- INSERT ON DUPLICATE KEY UPDATE
INSERT INTO product_inventory (product_id, quantity)
VALUES (101, 50)
ON DUPLICATE KEY UPDATE quantity = quantity + 50;
```

### Các loại JOIN
- **INNER JOIN**: Trả về các bản ghi có giá trị khớp ở cả hai bảng
- **LEFT JOIN**: Trả về tất cả bản ghi từ bảng bên trái và các bản ghi khớp từ bảng bên phải
- **RIGHT JOIN**: Trả về tất cả bản ghi từ bảng bên phải và các bản ghi khớp từ bảng bên trái
- **FULL OUTER JOIN**: Trả về bản ghi khi có ít nhất một khớp ở bảng bên trái hoặc phải
- **CROSS JOIN**: Kết hợp mỗi hàng của bảng đầu tiên với mỗi hàng của bảng thứ hai
- **SELF JOIN**: Join một bảng với chính nó

```sql
-- INNER JOIN
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- LEFT JOIN
SELECT c.customer_name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;

-- SELF JOIN
SELECT e1.name as employee, e2.name as manager
FROM employees e1
LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
```

## Transaction Processing

### Nguyên tắc ACID
- **Atomicity (Tính nguyên tử)**: Tất cả các thao tác trong một giao dịch phải được thực hiện đầy đủ hoặc không thực hiện
- **Consistency (Tính nhất quán)**: Cơ sở dữ liệu phải chuyển từ trạng thái nhất quán này sang trạng thái nhất quán khác
- **Isolation (Tính độc lập)**: Các giao dịch đồng thời không ảnh hưởng lẫn nhau
- **Durability (Tính bền vững)**: Kết quả giao dịch phải được lưu trữ vĩnh viễn

### Quản lý Transaction
```sql
-- Bắt đầu transaction
BEGIN TRANSACTION;

-- Thực hiện các thao tác
UPDATE accounts SET balance = balance - 1000 WHERE account_id = 123;
UPDATE accounts SET balance = balance + 1000 WHERE account_id = 456;

-- Kiểm tra lỗi
IF @error = 0
    COMMIT TRANSACTION;
ELSE
    ROLLBACK TRANSACTION;
```

### Isolation Level
- **READ UNCOMMITTED**: Cho phép đọc dữ liệu chưa commit (dirty read)
- **READ COMMITTED**: Chỉ cho phép đọc dữ liệu đã commit
- **REPEATABLE READ**: Đảm bảo dữ liệu đọc lại không thay đổi trong suốt transaction
- **SERIALIZABLE**: Cấp độ cô lập cao nhất, ngăn các hiện tượng phantom read

```sql
-- Thiết lập isolation level
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
```

### Deadlock và cách xử lý
- **Định nghĩa**: Tình trạng hai transaction chờ đợi lẫn nhau để giải phóng khóa
- **Phòng tránh**:
  - Tuân thủ thứ tự khóa nhất quán
  - Hạn chế thời gian giữ khóa
  - Thiết lập thời gian chờ (timeout)
- **Xử lý**:
  - Phát hiện và hủy bỏ một transaction (victim)
  - Thiết lập deadlock priority

## Database Connection và Concurrency

### Quản lý Connection
- **Connection Pooling**: Kỹ thuật duy trì sẵn một tập các kết nối để tái sử dụng
- **Lợi ích**:
  - Giảm chi phí thiết lập kết nối mới
  - Quản lý hiệu quả số lượng kết nối đồng thời
  - Tối ưu hóa hiệu suất ứng dụng

### Keep vs Không Keep Connection
- **Keep Connection**:
  - **Ưu điểm**: Giảm overhead thiết lập kết nối, tăng hiệu suất cho ứng dụng với tần suất truy vấn cao
  - **Nhược điểm**: Lãng phí tài nguyên khi không sử dụng, hạn chế số lượng kết nối đồng thời
- **Không Keep Connection**:
  - **Ưu điểm**: Tiết kiệm tài nguyên, phù hợp cho ứng dụng có tần suất truy vấn thấp
  - **Nhược điểm**: Tốn chi phí thiết lập kết nối mới, giảm hiệu suất

### Xử lý đồng thời (Concurrency)
- **Hiện tượng**:
  - **Lost update**: Hai transaction cùng cập nhật một dữ liệu, update của transaction sau ghi đè lên transaction trước
  - **Dirty read**: Đọc dữ liệu chưa được commit
  - **Non-repeatable read**: Hai lần đọc trong cùng transaction cho kết quả khác nhau
  - **Phantom read**: Kết quả truy vấn bị thay đổi do có thêm dữ liệu mới
- **Kỹ thuật xử lý**:
  - Khóa (Locking): Pessimistic locking, Optimistic locking
  - MVCC (Multiversion Concurrency Control)
  - Row versioning

```sql
-- Pessimistic locking
SELECT * FROM accounts WHERE account_id = 123 FOR UPDATE;

-- Optimistic locking (sử dụng version)
UPDATE accounts SET 
    balance = balance - 1000, 
    version = version + 1
WHERE account_id = 123 AND version = @current_version;
```

## Performance Tuning

### Query Optimization
- **Sử dụng chỉ mục thích hợp**:
  - Đánh index cho các cột thường xuyên sử dụng trong WHERE, JOIN, ORDER BY
  - Cân nhắc trình tự của composite index
- **Viết truy vấn hiệu quả**:
  - Tránh SELECT *
  - Tránh hàm trong mệnh đề WHERE
  - Sử dụng JOIN thay vì subquery
  - Sử dụng EXISTS thay vì IN khi thích hợp
- **Phân tích và sử dụng Explain Plan**

### Optimization cho dữ liệu lớn
- **Phân vùng (Partitioning)**:
  - **Range partitioning**: Phân chia theo khoảng giá trị
  - **List partitioning**: Phân chia theo danh sách giá trị cụ thể
  - **Hash partitioning**: Phân chia theo hàm băm
- **Phân trang (Pagination)**:
  ```sql
  SELECT * FROM orders
  ORDER BY order_date DESC
  LIMIT 100 OFFSET 200;
  ```
- **Parallel processing**

### Database sharding
- **Định nghĩa**: Phân chia database thành nhiều phần nhỏ hơn trên nhiều server
- **Phương pháp**:
  - Vertical sharding: Phân chia theo cột
  - Horizontal sharding: Phân chia theo hàng

### Caching
- **Application-level cache**
- **Database-level cache**
- **Query cache**
- **Result set cache**

## Thực hành và Best Practices

### Xử lý dữ liệu lớn
- **Batch processing**: Xử lý theo lô thay vì từng bản ghi
  ```sql
  -- Thay vì nhiều INSERT đơn lẻ
  INSERT INTO target_table (col1, col2, col3)
  VALUES 
      (val1, val2, val3),
      (val4, val5, val6),
      ...
      (valN-2, valN-1, valN);
  ```
- **Chunking**: Chia dữ liệu thành các phần nhỏ hơn để xử lý
  ```sql
  -- Xử lý từng chunk 10,000 bản ghi
  DECLARE @offset INT = 0;
  WHILE @offset < @total_records
  BEGIN
      -- Xử lý chunk
      UPDATE large_table
      SET status = 'processed'
      WHERE id BETWEEN @offset AND @offset + 9999;
      
      SET @offset = @offset + 10000;
  END;
  ```
- **Bulk operations**:
  ```sql
  -- MySQL
  LOAD DATA INFILE '/path/to/file.csv' INTO TABLE target_table;
  
  -- SQL Server
  BULK INSERT target_table FROM '/path/to/file.csv';
  ```

### Database Monitoring
- **Các chỉ số cần theo dõi**:
  - Query execution time
  - Lock contention
  - Disk I/O
  - Memory usage
  - Connection count
  - Cache hit ratio
- **Công cụ**:
  - MySQL: Performance Schema, Slow Query Log
  - PostgreSQL: pg_stat_statements, Auto Vacuum
  - SQL Server: Dynamic Management Views (DMVs), Query Store

### Best Practices
- **Naming convention**: Đặt tên nhất quán cho các đối tượng database
- **Documentation**: Viết tài liệu cho schema, stored procedures
- **Schema migration**: Quản lý thay đổi schema một cách có hệ thống
- **Backup và recovery**: Thiết lập chiến lược sao lưu và phục hồi
- **Security**: Áp dụng least privilege principle, mã hóa dữ liệu nhạy cảm
