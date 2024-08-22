# Mô hình Cơ sở Dữ liệu Thư viện Sách

## 1. Bảng `Books` (Sách)
Bảng `Books` lưu trữ thông tin về các cuốn sách, bao gồm tiêu đề, tên tác giả, giá bán, mô tả, nhà xuất bản, ngày xuất bản, và thể loại.

```sql
CREATE TABLE Books (
  id INT AUTO_INCREMENT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  author VARCHAR(255) NOT NULL,
  price FLOAT NOT NULL,
  description TEXT,
  publisher VARCHAR(255),
  published_date DATE,
  genreId INT,
  FOREIGN KEY (genreId) REFERENCES Genres(id)
);
```

## 2. Bảng `Genres` (Thể loại)
Bảng `Genres` lưu trữ các thể loại sách khác nhau như hành động, lãng mạn, kinh dị, v.v.

```sql
CREATE TABLE Genres (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL
);
```

## 3. Bảng `Authors` (Tác giả)
Bảng `Authors` lưu trữ thông tin về tác giả của các cuốn sách, bao gồm tên, tiểu sử, và ngày sinh.

```sql
CREATE TABLE Authors (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  biography TEXT,
  birthdate DATE
);
```

## 4. Bảng `Books_Authors` (Quan hệ nhiều-nhiều giữa Sách và Tác giả)
Bảng `Books_Authors` thiết lập mối quan hệ nhiều-nhiều giữa bảng `Books` và bảng `Authors`, cho phép một cuốn sách có nhiều tác giả và một tác giả có thể viết nhiều cuốn sách.

```sql
CREATE TABLE Books_Authors (
  bookId INT,
  authorId INT,
  PRIMARY KEY (bookId, authorId),
  FOREIGN KEY (bookId) REFERENCES Books(id),
  FOREIGN KEY (authorId) REFERENCES Authors(id)
);
```

## 5. Bảng `Publishers` (Nhà xuất bản)
Bảng `Publishers` lưu trữ thông tin về các nhà xuất bản, bao gồm tên và địa chỉ.

```sql
CREATE TABLE Publishers (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  address TEXT
);
```

## 6. Bảng `Books_Publishers` (Quan hệ nhiều-nhiều giữa Sách và Nhà xuất bản)
Bảng `Books_Publishers` thiết lập mối quan hệ nhiều-nhiều giữa bảng `Books` và bảng `Publishers`, cho phép một cuốn sách có thể được xuất bản bởi nhiều nhà xuất bản.

```sql
CREATE TABLE Books_Publishers (
  bookId INT,
  publisherId INT,
  PRIMARY KEY (bookId, publisherId),
  FOREIGN KEY (bookId) REFERENCES Books(id),
  FOREIGN KEY (publisherId) REFERENCES Publishers(id)
);
```

## 7. Bảng `Reviews` (Đánh giá)
Bảng `Reviews` lưu trữ đánh giá của người dùng cho các cuốn sách, bao gồm rating, bình luận, và ngày đánh giá.

```sql
CREATE TABLE Reviews (
  id INT AUTO_INCREMENT PRIMARY KEY,
  bookId INT,
  userId INT,
  rating INT CHECK (rating >= 1 AND rating <= 5),
  comment TEXT,
  reviewDate DATE,
  FOREIGN KEY (bookId) REFERENCES Books(id)
);
```

## 8. Bảng `Users` (Người dùng)
Bảng `Users` lưu trữ thông tin về người dùng, bao gồm tên đăng nhập, mật khẩu, và email.
```sql
CREATE TABLE Users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL
);
```

## Mô hình Quan hệ (ERD)
Mô hình quan hệ này cho phép bạn:
- Lưu trữ thông tin chi tiết về các cuốn sách trong bảng `Books`.
- Liên kết thể loại sách với từng cuốn sách thông qua bảng `Genres`.
- Liên kết nhiều tác giả với nhiều cuốn sách thông qua bảng `Books_Authors`.
- Liên kết nhà xuất bản với các cuốn sách thông qua bảng `Books_Publishers`.
- Lưu trữ đánh giá của người dùng cho từng cuốn sách trong bảng `Reviews`.
- Lưu trữ thông tin về người dùng trong bảng `Users`.
