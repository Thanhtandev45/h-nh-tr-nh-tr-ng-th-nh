# PROJECT TÂN × TRÂN — HÀNH TRÌNH TRƯỞNG THÀNH (TxT)
## Tagline: "Hai Cuộc Đời, Một Câu Chuyện."

> **Kiến trúc:** Một file HTML duy nhất (`index.html`) · Firebase Backend (Auth, Firestore, Storage) · Triển khai Vercel.

---

## 📌 MỤC LỤC
1. [Giới thiệu dự án](#01-giới-thiệu-dự-án)
2. [Cấu trúc mã nguồn](#02-cấu-trúc-mã-nguồn)
3. [Hướng dẫn cấu hình Firebase từ A đến Z](#03-hướng-dẫn-cấu-hình-firebase-từ-a-đến-z)
4. [Cấu hình Security Rules (Firestore & Storage)](#04-cấu-hình-security-rules)
5. [Tài khoản kiểm thử & Ma trận kiểm thử 15 bước](#05-ma-trận-kiểm-thử-chi-tiết)
6. [Hướng dẫn triển khai lên Vercel](#06-hướng-dẫn-triển-khai-lên-vercel)
7. [Hướng dẫn sao lưu & khôi phục dữ liệu](#07-hướng-dẫn-sao-lưu--khôi-phục)
8. [Giới hạn kiến trúc & Khuyến nghị kỹ thuật](#08-giới-hạn-kiến-trúc--khuyến-nghị)

---

## 01. GIỚI THIỆU DỰ ÁN
**TxT (Tân × Trân)** là một website không gian riêng tư được thiết kế theo phong cách **Cinematic Luxury & Cosmic Aesthetic**, dành riêng cho Tân và Trân để cùng nhau:
- Đếm và lưu giữ từng cột mốc trưởng thành (khởi đầu từ mốc gặp nhau trong Free Fire **22/10/2025** và chính thức yêu **15/09/2026**).
- Viết nhật ký, tâm sự, gửi những bức thư cho tương lai.
- Quản lý mục tiêu cá nhân và mục tiêu chung với danh sách công việc nhỏ (subtasks) và thanh tiến độ.
- Lên danh sách những điều muốn cùng trải nghiệm trong đời (Bucket List).
- Lưu giữ hình ảnh, video, âm thanh trên Firebase Storage.
- Tận hưởng giai điệu Cosmic Ambient du dương được tổng hợp trực tiếp bằng Web Audio API mà không sợ link nhạc chết.

---

## 02. CẤU TRÚC MÃ NGUỒN
Dự án tuân thủ nghiêm ngặt nguyên tắc **Single Frontend File**:
```
d:\Hành trình trưởng thành\
├── index.html        # Toàn bộ giao diện HTML, CSS, JavaScript, Firebase Web SDK v11
├── firestore.rules   # Quy tắc bảo mật Firestore (sao chép vào Firebase Console)
├── storage.rules     # Quy tắc bảo mật Firebase Storage (sao chép vào Firebase Console)
└── README.md         # Tài liệu hướng dẫn bàn giao toàn diện
```

---

## 03. HƯỚNG DẪN CẤU HÌNH FIREBASE TỪ A ĐẾN Z

Dành cho người mới học Software Engineering, các bước thực hiện vô cùng đơn giản:

### Bước 1: Tạo Firebase Project
1. Truy cập [Firebase Console](https://console.firebase.google.com/) bằng tài khoản Google.
2. Bấm **Add project** (Thêm dự án) -> Đặt tên dự án: `project-txt-journey` (hoặc tên tùy thích).
3. Bỏ chọn Google Analytics nếu không cần thiết -> Bấm **Create project**.

### Bước 2: Tạo Web App & Lấy Cấu hình (Firebase Config)
1. Tại trang **Project Overview**, bấm vào biểu tượng Web (`</>`).
2. Đặt App nickname: `TxT Web` -> Bỏ chọn Firebase Hosting (chúng ta sẽ dùng Vercel) -> Bấm **Register app**.
3. Màn hình sẽ hiển thị đoạn mã `firebaseConfig`. Sao chép các thông số:
   ```javascript
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "project-txt-journey.firebaseapp.com",
     projectId: "project-txt-journey",
     storageBucket: "project-txt-journey.firebasestorage.app",
     messagingSenderId: "123456789...",
     appId: "1:123456789:web:abcdef..."
   };
   ```
4. Mở file `index.html`, tìm đến dòng comment:
   `// ===== BẮT ĐẦU CẤU HÌNH FIREBASE VÀ TÀI KHOẢN (BẮT BUỘC ĐIỀN THẬT) =====`
   và dán đè vào vị trí tương ứng.

### Bước 3: Kích hoạt Firebase Authentication
1. Tại menu bên trái Firebase Console, chọn **Build** -> **Authentication** -> Bấm **Get started**.
2. Tại thẻ **Sign-in method**, bấm chọn **Email/Password**.
3. Bật công tắc **Enable** đầu tiên -> Bấm **Save**.
4. Chuyển sang thẻ **Users**:
   - Bấm **Add user** -> Tạo tài khoản của Tân:
     - Email: ví dụ `tan@txt.local` hoặc email cá nhân thật.
     - Password: Mật khẩu bảo mật do Tân chọn.
   - Bấm **Add user** lần nữa -> Tạo tài khoản của Trân:
     - Email: ví dụ `tran@txt.local` hoặc email cá nhân thật.
     - Password: Mật khẩu bảo mật do Trân chọn.
5. Sau khi tạo xong, nhìn vào cột **User UID**:
   - Sao chép chuỗi UID của Tân (ví dụ: `kL98fG2...`) -> Dán vào biến `const TAN_UID = "..."` trong `index.html`.
   - Sao chép chuỗi UID của Trân (ví dụ: `mN34pQ7...`) -> Dán vào biến `const TRAN_UID = "..."` trong `index.html`.

### Bước 4: Tạo Cloud Firestore Database
1. Chọn **Build** -> **Firestore Database** -> Bấm **Create database**.
2. Chọn vị trí máy chủ (Location): `asia-southeast1` (Singapore) để tốc độ tải tại Việt Nam nhanh nhất.
3. Chọn chế độ bảo mật: **Start in production mode** -> Bấm **Create**.

### Bước 5: Kích hoạt Firebase Storage
1. Chọn **Build** -> **Storage** -> Bấm **Get started**.
2. Chọn **Start in production mode** -> Chọn vùng máy chủ tương đồng -> Bấm **Done**.

---

## 04. CẤU HÌNH SECURITY RULES

### 4.1. Firestore Security Rules
Mở file `firestore.rules` có sẵn trong thư mục dự án:
1. Thay thế `TAN_UID_PLACEHOLDER` bằng UID thật của Tân.
2. Thay thế `TRAN_UID_PLACEHOLDER` bằng UID thật của Trân.
3. Vào Firebase Console -> **Firestore Database** -> Thẻ **Rules** -> Dán toàn bộ nội dung vào -> Bấm **Publish**.

### 4.2. Storage Security Rules
Mở file `storage.rules`:
1. Tương tự, thay thế 2 UID của Tân và Trân.
2. Vào Firebase Console -> **Storage** -> Thẻ **Rules** -> Dán nội dung vào -> Bấm **Publish**.

---

## 05. MA TRẬN KIỂM THỬ CHI TIẾT (15 BƯỚC)

| STT | Kịch bản kiểm thử | Hành động & Kỳ vọng |
|---|---|---|
| 1 | Tân đăng nhập | Nhập email/mật khẩu của Tân -> Đăng nhập thành công, hiển thị badge Tân 👨‍💻. |
| 2 | Trân đăng nhập | Nhập email/mật khẩu của Trân -> Đăng nhập thành công, hiển thị badge Trân 🌸. |
| 3 | Đăng nhập sai mật khẩu | Nhập sai pass -> Hệ thống báo lỗi "Email hoặc mật khẩu không chính xác". |
| 4 | Người lạ chưa đăng nhập | Không thể can thiệp dữ liệu, chỉ có thể xem chế độ Guest. |
| 5 | Tài khoản lạ cố tình đăng ký/đăng nhập | Firebase từ chối truy cập hoặc hệ thống tự động đăng xuất do UID không trùng `TAN_UID` hay `TRAN_UID`. |
| 6 | Tân quản lý dữ liệu cá nhân của mình | Tân thêm, sửa, xóa kỷ niệm cá nhân của Tân -> Thành công. |
| 7 | Trân xem dữ liệu cá nhân của Tân | Trân xem được kỷ niệm của Tân, nhưng nút Sửa/Xóa bị ẩn; nếu cố tình gọi hàm thì Firestore Rules từ chối. |
| 8 | Trân quản lý dữ liệu cá nhân của mình | Trân thêm, sửa, xóa kỷ niệm cá nhân của Trân -> Thành công. |
| 9 | Tân xem dữ liệu cá nhân của Trân | Tân xem được kỷ niệm của Trân nhưng không được quyền sửa hoặc xóa. |
| 10 | Quản lý dữ liệu chung | Cột mốc chung, mục tiêu chung, điều ước chung: cả hai đều có thể xem, sửa và đánh dấu hoàn thành. |
| 11 | Thử nghiệm tấn công trực tiếp Firestore | Dùng browser console gọi lệnh ghi trái phép -> Bị Rules chặn với mã lỗi `permission-denied`. |
| 12 | Tải lên file Media & Phân quyền | Upload ảnh/video vào Storage -> Metadata lưu Firestore -> Phân quyền xem/xóa chuẩn xác. |
| 13 | Giữ phiên đăng nhập | F5 tải lại trang hoặc tắt trình duyệt mở lại -> Vẫn giữ nguyên phiên đăng nhập. |
| 14 | Hiển thị đa thiết bị | Đăng nhập trên điện thoại iPhone/Android -> Giao diện co giãn chuẩn mượt, không vỡ layout. |
| 15 | Xuất & Khôi phục dữ liệu | Bấm tải bản sao lưu JSON/CSV -> Tải về file thật -> Thử nhập lại bản sao lưu -> Hệ thống xem trước và khôi phục an toàn. |

---

## 06. HƯỚNG DẪN TRIỂN KHAI LÊN VERCEL

Website hoàn toàn ở dạng tĩnh (Static Single-file) nên việc đưa lên Vercel chỉ mất 2 phút:

1. **Đưa mã nguồn lên GitHub:**
   - Tạo một kho lưu trữ (repository) mới trên [GitHub](https://github.com), đặt tên ví dụ `txt-journey`.
   - Tải file `index.html` lên thư mục gốc của repository.
2. **Kết nối Vercel:**
   - Đăng nhập vào [Vercel](https://vercel.com/) (đăng nhập bằng tài khoản GitHub).
   - Bấm **Add New...** -> **Project**.
   - Chọn repository `txt-journey` -> Bấm **Import**.
   - Ở mục **Framework Preset**: Chọn **Other**.
   - Không cần điền Build Command hay Output Directory (để mặc định).
   - Bấm **Deploy**.
3. **Cấu hình Authorized Domains trên Firebase:**
   - Sau khi Vercel deploy xong, bạn sẽ nhận được một đường link (ví dụ: `txt-journey.vercel.app`).
   - Vào **Firebase Console** -> **Authentication** -> Thẻ **Settings** -> Mục **Authorized domains**.
   - Bấm **Add domain** -> Điền domain Vercel của bạn (ví dụ: `txt-journey.vercel.app`) -> Bấm **Done**.
   - Giờ đây hai bạn có thể truy cập website bất kỳ lúc nào từ bất kỳ đâu trên thế giới!

---

## 07. HƯỚNG DẪN SAO LƯU & KHÔI PHỤC

- **Bản sao lưu JSON:** Chứa toàn bộ các bài viết nhật ký, cột mốc, danh mục mục tiêu, checklist và link media.
- **Bản sao lưu CSV:** Mở dễ dàng trên Microsoft Excel hoặc Google Sheets để đọc lại như một cuốn sách nhật ký.
- **Lưu ý về Media:** File hình ảnh, video và âm thanh thực tế được lưu trên đám mây của Google Firebase Storage. Nếu muốn sao lưu ảnh gốc về máy tính cá nhân hàng năm, bạn chỉ cần vào mục Firebase Storage trên console và bấm tải về thư mục `uploads/`.

---

## 08. GIỚI HẠN KIẾN TRÚC & KHUYẾN NGHỊ

1. **Bảo mật phía Client:**
   - Web App Config (`apiKey`, `projectId`) là thông tin công khai theo thiết kế của Firebase. Tính bảo mật hoàn toàn được bảo vệ bởi **Firebase Security Rules** (kiểm tra `request.auth.uid`).
   - Tuyệt đối không bao giờ đưa Firebase Admin SDK Service Account Key vào mã nguồn frontend.
2. **Âm thanh tự động (Autoplay Policy):**
   - Theo tiêu chuẩn trình duyệt hiện đại (Chrome/Safari), âm thanh chỉ phát sau khi người dùng có ít nhất một tương tác (click vào nút phát nhạc). TxT đã tích hợp sẵn trình phát nhạc synthesizer không gian êm dịu, hoạt động mượt mà ngay khi bấm nút.
