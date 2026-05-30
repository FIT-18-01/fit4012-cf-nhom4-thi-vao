[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/JLI_HvmF)
Mô tả đề tài, công cụ, cách chạy demo, tài khoản giả lập nếu có.


# CF10 - File Upload Vulnerability Demo

## 1. Thông tin nhóm

- Tên nhóm: Nhóm 5
- Lớp/Học phần: FIT4012
- Đề tài: CF10 - File Upload Vulnerability
- Thành viên:
  - Đỗ Trung Kiên
  - Nguyễn Trung Kiên
  - Trương Triệu Việt Hoàng
  - Đinh Mạnh Tú

## 2. Giới thiệu đề tài

Project này là bài demo cho chủ đề **CF10 - File Upload Vulnerability** trong học phần/hoạt động Cyber Fortress - FIT4012.

Demo mô phỏng chức năng **upload ảnh đại diện** trên website và so sánh giữa hai cách xử lý:

- **Bản lỗi:** nhận file gần như không kiểm tra an toàn.
- **Bản vá:** kiểm tra nhiều lớp trước khi chấp nhận file upload.

Project chỉ chạy ở môi trường local trên trình duyệt, phục vụ mục đích học tập và phòng thủ bảo mật.

---

## 3. Mục tiêu bài demo

- Hiểu File Upload Vulnerability là gì.
- Thấy được rủi ro khi hệ thống chỉ tin vào tên file hoặc đuôi file.
- Biết vì sao file text đổi đuôi thành ảnh vẫn có thể đánh lừa hệ thống kiểm tra yếu.
- Hiểu cách phòng chống bằng validate nhiều lớp.
- Có evidence và test case rõ ràng để nộp bài/thuyết trình.

---

## 4. Công nghệ sử dụng

Project này không cần backend và không cần cài thư viện ngoài.

Sử dụng:

- HTML
- CSS
- JavaScript thuần
- FileReader API
- Web Crypto API `crypto.getRandomValues()`
- Trình duyệt Chrome, Edge hoặc Firefox
- Visual Studio Code và Live Server nếu muốn chạy đẹp hơn

---

## 5. Chức năng chính

File demo chính nằm tại:

```text
demo/index.html
```

Website gồm 2 phần upload:

### 5.1. Bản lỗi - Upload không an toàn

Bản lỗi cố ý mô phỏng hệ thống upload yếu:

- Không kiểm tra whitelist extension.
- Không kiểm tra MIME type.
- Không kiểm tra chữ ký file.
- Không giới hạn kích thước.
- Dùng tên file gốc của người dùng.
- Có thể chấp nhận file sai loại hoặc file quá lớn.

Mục đích của bản lỗi là để chứng minh rằng upload file không kiểm soát có thể tạo rủi ro bảo mật.

### 5.2. Bản vá - Upload an toàn hơn

Bản vá áp dụng các bước kiểm tra:

- Chỉ cho phép file `.jpg`, `.jpeg`, `.png`.
- Chỉ cho phép MIME type `image/jpeg` và `image/png`.
- Kiểm tra chữ ký file cơ bản của PNG/JPEG.
- Giới hạn kích thước file tối đa 1MB.
- Đổi tên file khi lưu bằng tên ngẫu nhiên.
- Không tin tưởng tên file gốc do người dùng tải lên.

---

## 6. Cấu trúc thư mục

```text
CF10_File_Upload_Vulnerability
├── README.md
├── threat-model.md
├── ethics-safe-use.md
├── demo/
│   ├── index.html
│   └── test-files/
│       ├── valid-avatar.png
│       ├── fake-image.jpg
│       ├── normal-text.txt
│       └── big-file-demo.jpg
├── evidence/
│   ├── before-vulnerable.png
│   ├── before-vulnerable.txt
│   ├── after-secure.png
│   ├── after-secure.txt
│   ├── flow-upload.png
│   ├── large-file-rejected.png
│   ├── test-results.txt
│   └── valid-file-success.png
└── slides/
    └── noi-dung-slide.md
```

## 7. Cách chạy project

### Cách 1: Chạy bằng VS code 

1. Giải nén project.
2. Vào thư mục:

```text
demo/
```

3. Mở file:

```text
index.html
```
4. Run -> Start debugging
5. Chọn trình duyệt Chrome, Edge hoặc Firefox để chạy.

### Cách 2: Chạy bằng VS Code Live Server

1. Mở thư mục project bằng Visual Studio Code.
2. Cài extension **Live Server** nếu chưa có.
3. Chuột phải vào file:

```text
demo/index.html
```

4. Chọn **Open with Live Server**.
5. Trình duyệt sẽ tự mở trang demo.

---

## 8. Cách test demo

Các file test nằm trong thư mục:

```text
demo/test-files/
```

### TC01 - Upload ảnh hợp lệ

File dùng để test:

```text
valid-avatar.png
```

Kết quả mong đợi:

- Bản lỗi: chấp nhận.
- Bản vá: chấp nhận.
- Hệ thống hiển thị preview ảnh.

### TC02 - Upload file text đổi đuôi thành ảnh

File dùng để test:

```text
fake-image.jpg
```

Kết quả mong đợi:

- Bản lỗi: có thể chấp nhận sai.
- Bản vá: từ chối vì chữ ký file không khớp với ảnh JPG thật.

### TC03 - Upload file quá lớn

File dùng để test:

```text
big-file-demo.jpg
```

Kết quả mong đợi:

- Bản lỗi: có thể chấp nhận.
- Bản vá: từ chối vì vượt giới hạn dung lượng 1MB.

### TC04 - Upload file sai định dạng

File dùng để test:

```text
normal-text.txt
```

Kết quả mong đợi:

- Bản lỗi: có thể chấp nhận.
- Bản vá: từ chối vì không nằm trong whitelist extension và MIME type không hợp lệ.

---

## 9. Bảng test case

| Mã test | File test | Mục tiêu kiểm thử | Kết quả ở bản lỗi | Kết quả ở bản vá |
|---|---|---|---|---|
| TC01 | `valid-avatar.png` | Upload ảnh hợp lệ | Chấp nhận | Chấp nhận |
| TC02 | `fake-image.jpg` | File text đổi đuôi thành `.jpg` | Có thể chấp nhận sai | Từ chối |
| TC03 | `big-file-demo.jpg` | File vượt dung lượng cho phép | Có thể chấp nhận | Từ chối |
| TC04 | `normal-text.txt` | File sai định dạng | Có thể chấp nhận | Từ chối |

---

## 10. Luồng xử lý upload an toàn

Luồng xử lý ở bản vá:

1. Người dùng chọn file.
2. Hệ thống lấy tên file, MIME type và kích thước.
3. Kiểm tra extension có nằm trong whitelist không.
4. Kiểm tra MIME type có phải ảnh PNG/JPEG không.
5. Kiểm tra dung lượng có vượt quá 1MB không.
6. Đọc các byte đầu tiên của file.
7. Kiểm tra chữ ký file PNG/JPEG.
8. Nếu có lỗi, hệ thống từ chối file và ghi log.
9. Nếu hợp lệ, hệ thống đổi tên file bằng tên ngẫu nhiên.
10. Hiển thị kết quả upload thành công.

---

## 11. Giải thích lỗ hổng

File Upload Vulnerability xảy ra khi website cho phép người dùng tải file lên nhưng không kiểm tra đầy đủ.

Ví dụ:

- Người dùng đổi tên file text thành `.jpg`.
- Hệ thống chỉ nhìn đuôi file nên tưởng đó là ảnh thật.
- File sai loại vẫn được lưu vào hệ thống.
- Nếu server cấu hình yếu, file upload có thể gây rủi ro bảo mật.

Trong demo này, bản lỗi được thiết kế để minh họa vấn đề trên trong môi trường an toàn, không tấn công hệ thống thật.

---

## 12. Biện pháp phòng chống

Các biện pháp được áp dụng trong bản vá:

- Dùng whitelist extension thay vì blacklist.
- Kiểm tra MIME type.
- Kiểm tra chữ ký file/magic bytes.
- Giới hạn dung lượng file.
- Đổi tên file khi lưu.
- Không sử dụng tên file gốc.
- Không lưu file trong thư mục có quyền thực thi.
- Ghi log kết quả upload.
- Chỉ cho phép loại file thật sự cần thiết.

---

## 13. Evidence trong project

Thư mục `evidence/` chứa bằng chứng kết quả demo:

- `before-vulnerable.png`: ảnh minh họa bản lỗi.
- `before-vulnerable.txt`: mô tả kết quả bản lỗi.
- `after-secure.png`: ảnh minh họa bản vá.
- `after-secure.txt`: mô tả kết quả bản vá.
- `large-file-rejected.png`: bằng chứng file quá lớn bị từ chối.
- `valid-file-success.png`: bằng chứng file hợp lệ được chấp nhận.
- `flow-upload.png`: luồng xử lý upload an toàn.
- `test-results.txt`: tổng hợp kết quả test.

---

## 14. Nội dung thuyết trình gợi ý

Khi demo, nhóm có thể trình bày theo thứ tự:

1. Giới thiệu File Upload Vulnerability.
2. Mở `demo/index.html`.
3. Upload `fake-image.jpg` ở bản lỗi để cho thấy hệ thống nhận sai.
4. Upload lại `fake-image.jpg` ở bản vá để cho thấy hệ thống từ chối.
5. Upload `big-file-demo.jpg` để kiểm tra giới hạn dung lượng.
6. Upload `valid-avatar.png` để chứng minh file hợp lệ vẫn được chấp nhận.
7. Kết luận các biện pháp phòng chống.

---

## 15. Cam kết an toàn

Project này chỉ dùng cho học tập và demo phòng thủ.

Nhóm cam kết:

- Không dùng mã độc.
- Không dùng webshell.
- Không tấn công website thật.
- Không thu thập dữ liệu cá nhân.
- Không upload file nguy hiểm lên hệ thống thật.
- Chỉ demo trong môi trường local/lab.

---

## 16. Kết luận

Qua demo, nhóm rút ra rằng chức năng upload file cần được kiểm soát chặt chẽ. Chỉ kiểm tra đuôi file là chưa đủ. Một hệ thống an toàn hơn cần kết hợp nhiều lớp kiểm tra như extension, MIME type, chữ ký file, dung lượng và đổi tên file khi lưu.

Project giúp minh họa rõ sự khác biệt giữa upload không an toàn và upload được kiểm soát an toàn hơn.



