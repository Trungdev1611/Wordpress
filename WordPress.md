# WordPress: 12 bảng dữ liệu mặc định trong database

Khi bạn cài đặt một website WordPress mới, trước khi thêm bất kỳ plugin hay theme nào, hệ thống sẽ tự động tạo ra đúng 12 bảng cơ bản trong cơ sở dữ liệu.

Dưới đây là danh sách các bảng gốc và chức năng chính của từng bảng.

---

## 1) Nhóm quản lý nội dung (Core Content)

| Bảng | Chức năng |
|------|-----------|
| `wp_posts` | Bảng trung tâm của WordPress, lưu trữ tất cả bài viết, trang, sản phẩm, file đính kèm, menu, bản nháp và revision. |
| `wp_postmeta` | Lưu trữ các thông tin mở rộng cho bài viết như custom fields, ảnh đại diện, cấu hình riêng của bài viết. |
| `wp_comments` | Lưu toàn bộ bình luận, đánh giá, phản hồi của người dùng trên bài viết. |
| `wp_commentmeta` | Lưu thông tin phụ cho từng bình luận như trạng thái duyệt, điểm đánh giá, dữ liệu bổ sung. |

---

## 2) Nhóm quản lý phân loại (Taxonomy)

| Bảng | Chức năng |
|------|-----------|
| `wp_terms` | Lưu tên và slug của các danh mục, tag, nhãn phân loại. |
| `wp_term_taxonomy` | Xác định kiểu của từng term: category, post_tag, product_cat, product_tag,... |
| `wp_term_relationships` | Liên kết giữa bài viết và các danh mục/tag mà bài đó thuộc về. |
| `wp_termmeta` | Lưu dữ liệu bổ sung cho từng danh mục hoặc tag như icon, màu sắc, cấu hình riêng. |

---

## 3) Nhóm quản lý người dùng (Users)

| Bảng | Chức năng |
|------|-----------|
| `wp_users` | Lưu thông tin tài khoản người dùng: username, mật khẩu mã hóa, email, ngày đăng ký. |
| `wp_usermeta` | Lưu các dữ liệu bổ sung của người dùng như vai trò, quyền hạn, họ tên, avatar, cấu hình admin, session token. |

---

## 4) Nhóm cấu hình hệ thống (System Config)

| Bảng | Chức năng |
|------|-----------|
| `wp_options` | Lưu toàn bộ cấu hình của website: tên site, URL, trang chủ, cài đặt plugin, theme, quyền hạn, thiết lập chung. |
| `wp_links` | Bảng cũ còn sót lại từ các phiên bản WordPress trước, dùng cho quản lý bookmark/liên kết giới thiệu. |

---

## Kết luận

Bất kỳ tính năng mới nào xuất hiện sau này như:
- quyền hạn,
- sản phẩm WooCommerce,
- dữ liệu plugin,
- custom fields,

đều chỉ là dữ liệu được lưu thêm vào các bảng gốc này, hoặc được plugin tự tạo thêm bảng riêng nếu cần.

> Nói cách khác: WordPress không cần plugin mới để có `postmeta` hoặc `usermeta`; những bảng này đã có sẵn từ đầu.

---

# `postmeta` và `usermeta` có sẵn từ đầu hay không?

Câu trả lời là: **có, đã có sẵn từ lúc WordPress mới cài xong**.

Không cần cài ACF mới có `wp_postmeta` hay `wp_usermeta`.

---

## 1) `wp_postmeta` đã có sẵn từ đầu

WordPress dùng `wp_postmeta` để lưu các dữ liệu mở rộng của bài viết, ví dụ:

- `_thumbnail_id` (ảnh đại diện bài viết)
- trạng thái cho phép bình luận
- cấu hình riêng cho bài viết
- custom fields đơn giản
- dữ liệu mà plugin/theme cần lưu

---

## 2) `wp_usermeta` đã có sẵn từ đầu

`wp_usermeta` cũng được WordPress dùng ngay từ đầu để lưu:

- vai trò và quyền hạn của user (`wp_capabilities`)
- tên người dùng (`first_name`, `last_name`)
- màu giao diện admin (`admin_color`)
- session token đăng nhập
- thông tin cấu hình của người dùng

---

# ACF làm gì?

ACF không tạo ra bảng mới trong database.

ACF chỉ là một công cụ giúp bạn tạo giao diện nhập liệu dễ dùng hơn, ví dụ:

- chọn file
- chọn màu
- chọn ngày
- nhập văn bản dài
- repeater / group field
- layout phức tạp hơn

Khi bạn lưu dữ liệu trong ACF, thực chất ACF đang lưu vào các bảng có sẵn của WordPress, chủ yếu là:

- `wp_postmeta`
- `wp_usermeta`
- `wp_options` (đối với cấu hình của field group, theme options,...)

Nói ngắn gọn:

> ACF không tạo mới database, nó chỉ là một lớp giao diện để thao tác dữ liệu trên các bảng sẵn có của WordPress.

---

## Ví dụ đơn giản

Nếu bạn tạo một Custom Field trong ACF cho bài viết:

- tên field: `location`
- giá trị: `Hà Nội`

Thì dữ liệu này sẽ được lưu vào:

```sql
wp_postmeta
