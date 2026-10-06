**vậy là khi tạo mới wp, họ cho sẵn những bảng nào rồi**

Khi bạn cài đặt mới tinh một website WordPress (chưa cài thêm bất kỳ plugin hay theme phụ nào), WordPress sẽ tự động khởi tạo đúng 12 bảng tiêu chuẩn trong CSDL.

Dưới đây là danh sách 12 bảng "nguyên bản" kèm nhiệm vụ chính của từng bảng:

1. Nhóm Quản Lý Nội Dung (Core Content)
wp_posts: Bảng trung tâm lớn nhất, lưu tất cả bài viết, trang, sản phẩm, file đính kèm, menu, bản lưu nháp/revision...

wp_postmeta: Lưu các thông tin phụ, thuộc tính mở rộng (Custom Fields/ACF) đi kèm với từng bài viết trong wp_posts.

wp_comments: Lưu toàn bộ bình luận, đánh giá của người dùng trên bài viết.

wp_commentmeta: Lưu các thông tin bổ sung cho bình luận (ví dụ: điểm đánh giá 5 sao, trạng thái duyệt...).

2. Nhóm Quản Lý Phân Loại (Taxonomy)
wp_terms: Lưu tên và đường dẫn (slug) của các danh mục, chuyên mục, thẻ (Tag), nhãn phân quyền.

wp_term_taxonomy: Xác định loại cho từng term (đây là Danh mục bài viết category, Thẻ post_tag, hay Nhãn sản phẩm phan_quyen_san_pham...).

wp_term_relationships: Bảng trung gian nối bài viết (wp_posts) với các nhãn/danh mục (wp_terms) xem bài nào thuộc nhãn nào.

wp_termmeta: Lưu thông tin bổ sung cho từng nhãn/danh mục (ví dụ: màu sắc của nhãn, icon chuyên mục...).

3. Nhóm Quản Lý Người Dùng (Users)
wp_users: Lưu thông tin tài khoản cơ bản (Username, Mật khẩu mã hóa, Email, Ngày đăng ký...).

wp_usermeta: Lưu các thông tin chi tiết và Vai trò (Role/Capabilities), Tên hiển thị, Token phiên đăng nhập của người dùng.

4. Nhóm Cấu Hình Hệ Thống (System Config)
wp_options: Lưu toàn bộ cài đặt chung của website (Tên web, Địa chỉ URL, Trang chủ là trang nào, các bảng cấu hình phân quyền dùng chung...).

wp_links: Bảng cũ còn sót lại từ các phiên bản WordPress trước (dùng để quản lý các liên kết giới thiệu/Bookmark, hiện tại gần như ít sử dụng).

Tóm lại:
Bất kỳ tính năng mới nào (như Phân quyền VAA DATA, Sản phẩm, Tài liệu) xuất hiện sau này đều chỉ là dữ liệu nhét thêm vào 12 bảng này hoặc do các Plugin tự tạo thêm bảng mới (ví dụ bảng wp_user_role_cat_pro mà bạn thấy ở dự án VAA DATA là bảng tùy biến tạo thêm).




**mấy cái postmeta, usermeta là sau khi cài AFC mưới có hay ban đầu cũng có rồi**

Ban đầu cài WordPress xong là đã có sẵn rồi, không cần cài ACF mới có!

Bản thân WordPress nguyên bản (Core WordPress) đã được thiết kế sẵn các bảng meta này để phục vụ các chức năng mặc định:

1. Khi chưa cài ACF, WordPress dùng postmeta và usermeta để làm gì?
wp_postmeta (có sẵn):

Lưu ảnh đại diện của bài viết (_thumbnail_id)

Lưu trạng thái cho phép bình luận

Lưu cấu hình giao diện riêng của bài đó

Lưu các "Trường tùy chỉnh" (Custom Fields) mặc định đơn sơ của WordPress

wp_usermeta (có sẵn):

Lưu Vai trò / Quyền hạn của user (wp_capabilities)

Lưu Họ và Tên người dùng (first_name, last_name)

Lưu cấu hình màu giao diện Admin mà user đó chọn (admin_color)

Lưu Token phiên đăng nhập (session_tokens)

2. Vậy ACF xuất hiện để làm gì?
ACF không tạo ra bảng mới trong Database.

ACF chỉ đóng vai trò là một giao diện đẹp và tiện lợi giúp bạn tạo ra các ô nhập liệu nâng cao (như chọn File CAD, chọn Màu, chọn Ngày, bảng lặp Repeater...).

Khi bạn điền dữ liệu vào các ô ACF đó và bấm Lưu, ACF chỉ đơn giản là mượn lại bảng wp_postmeta có sẵn của WordPress để cất dữ liệu vào dưới dạng các cặp meta_key và meta_value.
