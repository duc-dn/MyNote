## Log Sequence Number (LSN)
Định nghĩa: LSN là một số tuần tự duy nhất đại diện cho vị trí của một bản ghi trong Write-Ahead Log (WAL). WAL là một nhật ký ghi lại mọi thay đổi đối với dữ liệu trong cơ sở dữ liệu trước khi những thay đổi đó được áp dụng chính thức.

### Vai trò của LSN:

- Ghi nhận thay đổi: Mỗi khi có một thay đổi nào đó xảy ra trong cơ sở dữ liệu, PostgreSQL ghi lại thay đổi này trong WAL với một LSN tương ứng. Điều này giúp đảm bảo rằng mọi thay đổi đều được lưu trữ một cách tuần tự và có thể được khôi phục khi cần.
- Phục hồi dữ liệu: Trong trường hợp hệ thống bị lỗi hoặc tắt đột ngột, LSN cho phép PostgreSQL phục hồi cơ sở dữ liệu đến trạng thái nhất quán cuối cùng bằng cách áp dụng lại các thay đổi đã được ghi trong WAL.
- Replication: Trong hệ thống replication, LSN giúp đồng bộ hóa dữ liệu giữa master và replica. Replica sử dụng LSN để biết được nó cần sao chép từ vị trí nào trong WAL của master.

### Cấu trúc của LSN
- LSN thường được biểu diễn dưới dạng một cặp số (log file sequence number, byte offset) hoặc dưới dạng một số thập lục phân duy nhất. Ví dụ, một LSN có thể trông như 0/16B3740.
- LSN thường được biểu diễn dưới dạng một giá trị thập lục phân duy nhất. Ví dụ: 0/16B3740.
- Trong đó:
    - Log File Sequence Number: Số thứ tự của tệp WAL.
    - Byte Offset: Vị trí byte cụ thể trong tệp WAL đó.
- Ví dụ
Giả sử chúng ta có một LSN như 0/16B3740:
    - 0 là số thứ tự của tệp WAL.
    - 16B3740 là byte offset trong tệp WAL đó.