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

## III. SINGLE PAGE - Quy tắc xác định

### Câu hỏi: Khi tạo một post type mới, làm sao WordPress biết dùng file nào để hiển thị trang chi tiết của bài viết?

---

TRANG CHI TIẾT (SINGLE) TRONG WORDPRESS – TÊN FILE ĐI THEO POST_TYPE
======================================================================

1. QUY TẮC
----------
Trang chi tiết (xem một bài) có tên file:

    single-<post_type>.php

 - "single-" là phần CỐ ĐỊNH do WordPress quy định, nghĩa là "trang xem MỘT bài".
 - <post_type> là tên loại bài, đúng bằng giá trị cột post_type trong bảng wp_posts.
 - File này chạy cho MỌI bài thuộc loại đó (không phân biệt từng bài).
 - Slug của từng bài (cột post_name) chỉ quyết định URL, KHÔNG quyết định file.

2. LUỒNG TỪ LÚC TẠO ĐẾN LÚC HIỂN THỊ
------------------------------------
 a) Dev tạo loại bài (ACF -> Post Types, hoặc viết code). Tên khóa của loại bài
    chính là post_type, ví dụ: product, tai-lieu, doi-tac, test-posttypenew.
 b) Khi nhập liệu và bấm Đăng, bài được lưu thành MỘT DÒNG trong bảng wp_posts,
    cột post_type = tên loại bài đó.
 c) Người dùng mở URL của bài. WordPress tra trong DB: URL này là bài nào,
    post_type là gì.
 d) WordPress ghép tên file: "single-" + post_type + ".php" rồi tìm file đó trong theme.
 e) Có file thì dùng file đó. Không có thì dùng phương án dự phòng (mục 3).

3. THỨ TỰ WORDPRESS TÌM FILE (lấy đầu tiên tìm thấy)
----------------------------------------------------
    1. single-<post_type>-<slug của bài>.php   (làm riêng cho đúng một bài, ít dùng)
    2. single-<post_type>.php                  (cho mọi bài của loại đó)  <-- dùng cái này
    3. single.php                              (dự phòng chung cho mọi loại bài)
    4. index.php                               (dự phòng cuối cùng)

Vì vậy loại bài chưa có file riêng thì rơi về single.php.
Trong project này: loại "post" (tin tức) không có single-post.php nên dùng single.php.

4. VÍ DỤ TRONG PROJECT
----------------------
    post_type            URL của bài                          File được chạy
    -------------------  -----------------------------------  ------------------------
    product              /san-pham/<slug-bai>/                single-product.php
    tai-lieu             /tai-lieu/<slug-bai>/                single-tai-lieu.php
    doi-tac              /doi-tac/<slug-bai>/                 single-doi-tac.php
    test-posttypenew     /test-posttypenew/posttypenew1/      single-test-posttypenew.php
    post (tin tức)       /<slug-bai>/ (tùy cấu hình URL)      single.php (dự phòng)

Đã kiểm chứng ở local:
 - Tạo loại bài "test-posttypenew", đăng bài slug "posttypenew1" (ID 8806).
 - Tạo file single-test-posttypenew.php -> mở URL thấy đúng nội dung của file đó.
 - Đổi tên file thành single-document.php -> bài tài liệu rơi về single.php.

5. LƯU Ý QUAN TRỌNG
-------------------
 - Phần sau "single-" phải khớp ĐÚNG post_type, kể cả dấu gạch ngang
   (tai-lieu, không phải tai_lieu).
 - Phần đầu URL (ví dụ /san-pham/) là tiền tố "rewrite" đặt riêng. Nó THƯỜNG
   trùng post_type nhưng KHÔNG LUÔN: sản phẩm có URL /san-pham/ nhưng post_type là product.
   Tên file luôn theo post_type, không theo URL.
 - Người nhập liệu không đặt được post_type. Chỉ dev đặt một lần khi tạo loại bài.
 - Ba thứ dễ lẫn:
       post_type  = loại bài          (cột wp_posts.post_type)      -> quyết định tên file
       post_name  = slug của một bài  (cột wp_posts.post_name)      -> quyết định URL
       tiền tố URL = rewrite slug     (cài đặt khi tạo loại bài)    -> phần đầu của URL

6. CÁCH KIỂM TRA NHANH
----------------------
 a) Xem có những loại bài nào (chính là các "abc" trong single-abc.php):
        SELECT DISTINCT post_type FROM wp_posts;
 b) Mở trang bài bất kỳ, bấm F12, gõ trong Console:
        document.body.className
    Có class dạng "single-<post_type>" (ví dụ single-tai-lieu).
 c) Nếu trang chạy sai giao diện, kiểm tra tên file có khớp post_type chưa.

7. THÊM MỘT LOẠI BÀI MỚI VÀ GIAO DIỆN RIÊNG
--------------------------------------------
 1. ACF -> Post Types -> Add new, đặt khóa (post_type), ví dụ: tin-tuyen-dung.
 2. Nhập liệu và đăng ít nhất một bài.
 3. Tạo file single-tin-tuyen-dung.php trong theme VKT.
 4. Bên trong file thường có: get_header(); ... nội dung hoặc get_template_part(...); ... get_footer();
 5. Nếu cần CSS/JS riêng, thêm một nhánh nạp trong import_css_js/import_css_js.php
    (project này không tự nạp).

8. QUY TẮC TƯƠNG TỰ CHO NHÓM PHÂN LOẠI (TAXONOMY)
-------------------------------------------------
    taxonomy-<tên taxonomy>.php   chạy cho MỌI mục của nhóm đó.
    Ví dụ: taxonomy-linh-vuc.php (mọi lĩnh vực), taxonomy-bo-suu-tap.php (mọi nhãn hàng).
    Dự phòng: taxonomy.php, rồi archive.php, rồi index.php.

9. SO SÁNH VỚI template-pages/
------------------------------
    File ở gốc theme (single-*.php, taxonomy-*.php, front-page.php, search.php):
        WordPress TỰ chọn theo loại URL và tên file.
    File trong template-pages/ (có dòng "Template Name: ..."):
        ADMIN CHỌN TAY ở ô "Template" khi sửa một Page; lựa chọn lưu trong DB
        (wp_postmeta, khóa _wp_page_template).
