# TRẠM TRĂNG TREO – Website tự đọc dữ liệu từ GitHub

Website tĩnh, không cần database và không cần sửa HTML mỗi khi thêm nhóm/thành viên. Trang đọc danh sách folder và ảnh trực tiếp từ GitHub Contents API.

## 1. Cấu trúc repository

```text
TRAM-TRANG-TREO/
├── index.html
└── groups/
    ├── V WORLD 019/
    │   ├── NINNIN.jpg          # Ảnh đại diện (không bắt buộc)
    │   ├── NINNIN_QR.png      # QR của NINNIN
    │   ├── YASUOO.jpg         # Ảnh đại diện (không bắt buộc)
    │   └── YASUOO_QR.png      # QR của YASUOO
    └── TRẠM TRĂNG TREO/
        ├── NINNIN_QR.png
        └── CHUT_QR.jpg
```

- Mỗi folder trực tiếp bên trong `groups/` là một nhóm.
- File có hậu tố `_QR` trước phần mở rộng là mã QR, ví dụ `NINNIN_QR.png`.
- Tên thành viên được lấy từ phần tên trước `_QR`, vì vậy chỉ cần có `NINNIN_QR.png` là thành viên NINNIN đã xuất hiện.
- Ảnh trùng tên nhưng không có `_QR` là ảnh đại diện tùy chọn: `NINNIN.jpg`.
- Hỗ trợ PNG, JPG/JPEG, WEBP, GIF và AVIF.
- Tên folder nhóm được dùng làm tên nhóm và dòng `TRẠM: ...` dưới mã QR.
- Dùng tên file dễ đọc; tránh đặt hai file QR khác nhau nhưng cùng tên thành viên trong một nhóm.

## 2. Cấu hình website

Mở `index.html`, tìm đoạn `CẤU HÌNH GITHUB` và sửa:

```js
const GITHUB_OWNER = "YOUR_GITHUB_USERNAME";
const GITHUB_REPO = "YOUR_REPOSITORY_NAME";
const GITHUB_BRANCH = "main";
const DATA_FOLDER = "groups";
```

Ví dụ tài khoản GitHub là `nguy123`, repository là `TRAM-TRANG-TREO`, branch là `main`:

```js
const GITHUB_OWNER = "nguy123";
const GITHUB_REPO = "TRAM-TRANG-TREO";
const GITHUB_BRANCH = "main";
const DATA_FOLDER = "groups";
```

Repository cần để **Public** để khách truy cập website có thể xem danh sách file qua GitHub API. Không cần tạo GitHub token và không đặt token trong HTML.

## 3. Đưa website lên GitHub Pages

1. Tạo một repository trên GitHub, ví dụ `TRAM-TRANG-TREO`.
2. Upload `index.html` vào thư mục gốc repository.
3. Tạo folder `groups` bằng cách upload ảnh theo đường dẫn `groups/TEN_NHOM/TEN_THANH_VIEN_QR.png`. Git không lưu folder rỗng, nên hãy tạo folder bằng cách upload ít nhất một file vào đó.
4. Điền `GITHUB_OWNER` và `GITHUB_REPO` trong `index.html`, sau đó commit.
5. Mở **Settings → Pages**.
6. Ở **Build and deployment**, chọn **Deploy from a branch**, branch `main`, folder `/(root)`, rồi Save.
7. Mở URL GitHub Pages mà GitHub cung cấp.

## 4. Cập nhật nội dung

Mỗi khi thêm/xóa/đổi tên folder hoặc ảnh trong `groups/`, hãy commit thay đổi lên GitHub. Website sẽ đọc dữ liệu mới khi tải trang, khi nhấn **Làm mới**, và tự kiểm tra lại mỗi 5 phút khi tab đang mở.

Chỉ cần upload ảnh theo quy ước tên file; không phải sửa `index.html` khi thêm nhóm hoặc thành viên.

## 5. Lưu ý

- Nếu repository để Private, website mẫu này không đọc được dữ liệu công khai bằng API không xác thực.
- GitHub API không xác thực có giới hạn lượt gọi. Website chỉ đọc thư mục đang cần hiển thị, không tải toàn bộ repository cùng lúc.
- GitHub Pages có thể mất một chút thời gian để publish lần đầu hoặc sau khi sửa `index.html`. Chỉ thêm ảnh vào `groups/` thường không cần build lại website; dữ liệu được lấy trực tiếp từ GitHub API.
