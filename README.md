# Trung Dev Test — Ghi chú WordPress cho người mới

Site Local: `trung-dev-test`  
Thư mục code: `app/public` (workspace Git nằm ở đây)

---

## Đây có phải WordPress không?

**Có.** Đây là WordPress chạy trên **Local** (Local WP). Cấu trúc điển hình:

| Thư mục / file | Vai trò | Có đẩy lên Git? |
|---|---|---|
| `wp-admin/` | Dashboard quản trị | Không (core) |
| `wp-includes/` | Thư viện core | Không (core) |
| `wp-content/` | Theme, plugin, uploads — **nơi bạn làm việc** | Có (theme/plugin của bạn) |
| `wp-config.php` | Kết nối DB + cấu hình máy | **Không** (có mật khẩu) |
| `index.php` + file `wp-*.php` gốc | Điểm vào / core | Không |

**Quy tắc vàng:** Git chỉ chứa mã nguồn của bạn. Core + DB + config máy = mỗi người tự dựng (giống `node_modules` / `vendor` + `.env` + database ở backend).

---

## Git & WordPress — vì sao mới init đã 1000+ file?

Vì bạn đang đứng trong cả bản WordPress đầy đủ (~3900 file). Phần lớn là `wp-admin` + `wp-includes`.

**Không** đẩy cả core lên Git. Dùng `.gitignore` để loại core, `wp-config.php`, `uploads/`, cache…

### Khi người khác clone repo thì lấy core thế nào?

Không thiếu thông tin — core không nằm trong Git mà lấy từ chỗ khác:

1. Tạo site Local **cùng version WordPress**
2. Clone / copy code từ Git vào `app/public` (chủ yếu `wp-content`)
3. Local đã tạo sẵn `wp-config.php` và database

Hoặc dùng Composer/Bedrock (team chuyên nghiệp hơn): `composer install` tải đúng version core.

| Thứ | Nguồn |
|---|---|
| `wp-admin`, `wp-includes` | Local / WP-CLI / Composer |
| Theme / plugin của bạn | **Git** |
| `wp-config.php` | Mỗi máy tự có |
| Bài viết, user, media | Database + `uploads/` (export/import riêng nếu cần) |

---

## `wp-config.php` và Database trên Local

- File **có trên máy**, thường bị ẩn khỏi Git (và đôi khi khỏi Source Control) vì `.gitignore`.
- Tab **Database** trên Local và các hằng `DB_*` trong `wp-config.php` là **cùng một bộ thông tin** (Local thường: DB `local`, user/pass `root`).

### Đổi password trong `wp-config.php` có đổi DB trên GUI không?

**Không.** `wp-config.php` chỉ bảo WordPress *dùng* mật khẩu nào để kết nối. Đổi sai → site lỗi kết nối; MySQL trên Local và AdminNeo vẫn dùng mật khẩu thật cũ. Muốn đổi thật phải đổi trên MySQL rồi mới sửa `wp-config.php` cho khớp.

---

## Theme Twenty Twenty-Five (block theme)

Theme hiện tại: `wp-content/themes/twentytwentyfive/` — **Full Site Editing** (template HTML + patterns, không phải PHP template cổ điển).

### `style.css` sửa mà không thấy, `functions.php` thì thấy?

Trong `functions.php`, theme enqueue CSS như sau:

- `SCRIPT_DEBUG` **tắt** (mặc định) → load **`style.min.css`**
- `SCRIPT_DEBUG` **bật** → load `style.css`

Bạn sửa `style.css` nhưng trình duyệt vẫn đọc `.min` → không đổi.  
`echo` trong `functions.php` chạy mỗi request PHP → hiện ngay.

Có thể bật trong `wp-config.php` (chỉ môi trường local):

```php
define( 'SCRIPT_DEBUG', true );
```

### Template Home đã chỉnh thử

File: `wp-content/themes/twentytwentyfive/templates/home.html`

Cấu trúc học thử:

1. Header (`template-part`)
2. Hero
3. Các section (giới thiệu, services, banner, CTA)
4. Comments
5. Footer

**Lưu ý:**

- Nếu đã sửa Home trong **Appearance → Editor** (Site Editor), bản trong DB có thể **đè** file trên disk → cần Reset template mới thấy thay đổi từ file.
- Block **Comments** trên trang “latest posts” thường trống / ít ý nghĩa; form comment rõ nhất trên **single post / page**.

---

## Checklist khi bắt đầu dự án WP mới

1. Cài Local → tạo site → ghi nhớ URL admin + version WP/PHP  
2. `git init` trong `app/public` + `.gitignore` loại core & secrets  
3. **Không** commit `wp-config.php`  
4. Làm việc trong `wp-content/themes/...` hoặc `plugins/...`  
5. Nên tạo **child theme** khi customize lâu dài (đừng sửa theme mặc định nếu có thể cập nhật)  
6. README ghi: version WordPress, PHP, cách chạy local  
7. DB/content: backup/export khi cần chia sẻ dữ liệu mẫu  

---

## So sánh nhanh với backend quen thuộc

| Backend | WordPress |
|---|---|
| `node_modules` / `vendor` | `wp-admin` + `wp-includes` |
| Code app (`src/`) | Theme / plugin trong `wp-content` |
| `.env` | `wp-config.php` |
| Database | MySQL (Local tab Database / AdminNeo) |
| Docker / runtime | Local (PHP + MySQL + Nginx/Apache) |

---

## Repo này chứa gì?

- `.gitignore` — loại core & secrets  
- `README.md` — ghi chú học WordPress (file này)  
- `wp-content/themes/twentytwentyfive/` — theme đang chỉnh (learning)  
- `.htaccess` — rewrite Local  

**Không** chứa: core WP, `wp-config.php`, uploads, database dump.
