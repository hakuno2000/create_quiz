# Hướng dẫn sử dụng công cụ tạo Trealet

Project sử dụng framework Vuejs, yêu cầu cần có Nodejs để chạy code Javascript và NPM để quản lý và cài đặt các thư viện của Nodejs, bao gồm cả framework Vuejs.

# Hướng dẫn cài đặt

## Nodejs

### Kiểm tra version Nodejs hiện tại tại terminal:
```
node -v
```
Nếu thiết bị hiện tại đã cài đặt Nodejs, hãy bỏ qua bước này và đến bước tiếp theo

### Cài đặt Nodejs:
- Đối với Windows: Download trực tiếp Nodejs bản LTS từ link https://nodejs.org/en/#home-downloadhead
- Đối với Debian và Ubuntu:
```
# Với tài khoản root
curl -fsSL https://rpm.nodesource.com/setup_lts.x | bash -

# Không có quyền root
curl -fsSL https://rpm.nodesource.com/setup_lts.x | sudo bash -
```

## NPM

### Kiểm tra version NPM hiện tại tại terminal:
```
npm -v
```
Nếu thiết bị hiện tại đã cài đặt NPM, hãy bỏ qua bước này và đến bước tiếp theo.

### Cài đặt NPM tại terminal:
```
npm install -g npm
```

# Hướng dẫn chạy chương trình

## Cài đặt thư viện
Sử dụng lệnh dưới đây tại terminal để cài đặt các thư viện cần thiết cho chương trình. Sau khi cài đặt xong, tại thư mục gốc của chương trình sẽ sinh ra một folder mới tên node_modules, chứa file chạy của các thư viện nói trên.
```
npm install
```

## Chạy chương trình
Chạy lệnh dưới đây tại terminal để bắt đầu chạy chương trình. 
```
npm run serve
```
Sau khi hoàn thành, terminal sẽ thông báo: 
```
DONE - Compiled successfully in xxxx ms
App running at:
- Local:   http://localhost:8080/ 
- Network: http://192.168.11.20:8080/
```
Truy cập 1 trong 2 đường link trên để bắt đầu sử dụng chương trình.

# Hướng dẫn sử dụng chương trình

## Cấu trúc streamline

Streamline bao gồm 1 grid và các slide con. 
Grid chung sẽ chứa hình ảnh và thông tin bao quát của các nội dung nhỏ để người dùng có thể chọn nội dung mình muốn xem.
Sau khi click để lựa chọn nội dung mình muốn xem, người dùng sẽ được dẫn sang trang slide của nội dung tương ứng. 

## Cấu trúc file trealet

File trealet gồm tiêu đề chung của bộ câu hỏi và thông tin cho từng câu hỏi riêng. Thông tin riêng cho từng câu hỏi được định nghĩa trong trường ```questions```, với mỗi object trong array ```questions``` tương ứng với 1 câu hỏi. 1 câu hỏi gồm:
- Tiêu đề câu hỏi
- Các đáp án
- Hình ảnh
- Đáp án chính xác

## Tạo file trealet
- Sửa tên file: Chỉnh sửa trường ```File Name``` để cố định tên file sẽ xuất ra. File sau khi xuất sẽ có dạng <tên_file>.trealet
- Sửa tên bộ câu hỏi: Chỉnh sửa trường ```Title``` để cố định tên bộ câu hỏi chung. Tiêu đề của bộ câu hỏi sẽ xuất hiện khi mở quiz.

Tại List of Questions, người tạo trealet có thể thêm câu hỏi (qua nút Add Question bên phải bảng), sửa câu hỏi cũ (qua nút cây bút ở cột Actions), xóa câu hỏi (qua nút thùng rác ở cột Actions) và xem demo câu hỏi nếu được hiển thị trên quiz (nút mũi tên quay xuống ở cột bên phải cùng của bảng)
- Thêm câu hỏi: Ấn nút Add Question để hiển thị modal thêm mới câu hỏi. Tại đây có các trường Question (nội dung câu hỏi), Id of question image (ID ảnh sẽ được dùng trong câu hỏi; Nếu câu hỏi không sử dụng ảnh, người dùng điền -1), các trường answer (nội dung các phương án) và trường correct answer (chọn 1 đáp án từ 4 phương án được đưa ra). Người dùng nhấn nút Save để lưu lại câu hỏi này, hoặc Cancel để dừng việc thêm mới
- Sửa câu hỏi: Ấn nút cây bút tại cột Actions để sửa câu hỏi tương ứng. Tương tự với thêm câu hỏi, người dùng có thể sửa các trường trong modal hiện lên, ấn Save để lưu thay đổi.
- Xóa câu hỏi: Ấn nút thùng rác tại cột Actions để hiện lên modal xác nhận. Đồng ý xóa câu hỏi hiện tại với nút Confirm và không đồng ý với nút Cancel.

Người dùng có thể chỉnh sửa số lượng câu hỏi hiện lên trên 1 bảng ở trường ```Rows per page``` tại cuối bảng, và nếu số lượng câu hỏi hiện tại đã vượt quá số lượng câu hỏi trên 1 trang, người dùng có thể ấn 2 mũi tên trái phải ở cuối bảng để lật trang.

- Xuất file: Ấn nút Export ở cuối trang để xuất file.
