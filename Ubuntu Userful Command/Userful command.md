## Commands List
#### 1. netstat -tnlp
- Được dùng để hiện thị danh sách các kết nối mạng đang hoạt động và các port trên PC.
```
- "netstat": Là tên của câu lệnh để xem thông tin về kết nối mạng và cổng lắng nghe.
- "-t": Tùy chọn này chỉ định rằng chỉ hiển thị các kết nối TCP (Transmission Control Protocol).
- "-n": Tùy chọn này chỉ định rằng địa chỉ IP và số cổng sẽ được hiển thị dưới dạng số, không được chuyển đổi thành tên miền hoặc tên dịch vụ.
- "-l": Tùy chọn này chỉ định rằng chỉ hiển thị các cổng lắng nghe (listening ports).
- "-p": Tùy chọn này chỉ định rằng hiển thị thông tin về quy trình (process) đang lắng nghe hoặc đã tạo ra kết nối.
```
#### 2. ln -sf
- "ln -sf" được sử dụng trong hệ điều hành Unix và các phiên bản tương đương để tạo liên kết mềm (symbolic link) từ một tệp hoặc thư mục đến một đích khác
```
- "ln": Là tên của câu lệnh tạo liên kết, ngắn gọn cho từ "link".
- "-s": Tùy chọn này chỉ định rằng liên kết được tạo ra sẽ là liên kết mềm (symbolic link). Một liên kết mềm là một tham chiếu đến một tệp hoặc thư mục khác.
- "-f": Tùy chọn này chỉ định rằng nếu liên kết đã tồn tại, nó sẽ bị ghi đè mà không cần xác nhận từ người dùng.
```

#### 3. Ncdu
- Được sử dụng để phân tích và hiển thị thông tin về sử dụng disk (disk usage) trên Linux File System
- Quét qua các thư mục và tệp tin trên `File System` và show dung lượng disk được sử dụng với từng thư mục và file.
- Command: 
```
sudo su
ncdu /
```