Refer link: https://readmedium.com/practical-notes-in-change-data-capture-with-debezium-and-postgres-fe31bb11ab78

![[postgres_write_log_flow.png]]

Flow của hình ảnh này mô tả quá trình sao chép các sự kiện thay đổi từ PostgreSQL đến một khách hàng sao chép (như Debezium) thông qua các bước chính sau:

1. **XLog Records**: Các thay đổi trong cơ sở dữ liệu được ghi vào Write-Ahead Log (WAL) dưới dạng các bản ghi XLog (XLog Recordn, XLog Recordn-1,..., XLog Record1).
    
2. **WAL Writer Process**: Quá trình này chịu trách nhiệm ghi các bản ghi XLog vào WAL.
    
3. **Flush WAL**: Khi WAL được làm sạch (flush), các bản ghi XLog sẽ được chuyển sang quá trình tiếp theo.
    
4. **WAL Sender**: WAL Sender là một quá trình trong PostgreSQL để gửi các bản ghi WAL tới các điểm nhận từ xa (như khách hàng sao chép).
    
5. **Reorder Buffer**: Trước khi gửi các bản ghi WAL ra ngoài, chúng được xếp lại thứ tự trong một bộ đệm (Reorder Buffer).
    
6. **Replication Slot**: Một replication slot được tạo ra để đảm bảo rằng các bản ghi WAL được giữ lại cho đến khi chúng được sao chép thành công. Điều này giúp ngăn ngừa việc WAL bị xóa trước khi chúng được xử lý.
    
7. **Publication**: Để sao chép dữ liệu, một publication được tạo ra trong PostgreSQL. Publication này xác định các bảng và dữ liệu nào cần được sao chép.
    
8. **Output Plugin (pgoutput)**: Output Plugin (như pgoutput) chịu trách nhiệm chuyển đổi các bản ghi WAL thành một định dạng mà khách hàng sao chép có thể hiểu được.
    
9. **Network Communication**: Các bản ghi đã được chuyển đổi sẽ được gửi qua mạng đến các khách hàng sao chép (Replication Client).
    
10. **Replication Client (Debezium)**: Khách hàng sao chép (như Debezium) nhận các sự kiện thay đổi và xử lý chúng. Debezium có thể chuyển các sự kiện này thành các sự kiện thay đổi cấu trúc dữ liệu (Change Events).
    
11. **Change Events**: Các sự kiện thay đổi này có thể được đẩy tới các hệ thống khác để xử lý thêm hoặc lưu trữ.
    
12. **Heartbeat**: Để đảm bảo rằng kết nối sao chép vẫn còn hoạt động và không bị mất các sự kiện, một heartbeat được gửi định kỳ. Heartbeat này chứa thông tin như flush_lsn để theo dõi tiến trình của việc sao chép.
    

### Các bước chi tiết:

1. **XLog Records**: Các thay đổi được ghi vào các bản ghi WAL.
2. **WAL Writer Process**: Ghi các bản ghi này vào WAL.
3. **Flush WAL**: Làm sạch WAL và chuyển các bản ghi đến WAL Sender.
4. **WAL Sender**: Gửi các bản ghi WAL đến các điểm nhận.
5. **Reorder Buffer**: Xếp lại thứ tự các bản ghi trước khi gửi ra ngoài.
6. **Replication Slot**: Đảm bảo rằng các bản ghi WAL không bị xóa cho đến khi được sao chép.
7. **Publication**: Xác định các dữ liệu cần được sao chép.
8. **Output Plugin (pgoutput)**: Chuyển đổi các bản ghi WAL thành định dạng hiểu được bởi khách hàng sao chép.
9. **Network Communication**: Gửi các bản ghi đã chuyển đổi qua mạng.
10. **Replication Client (Debezium)**: Nhận và xử lý các sự kiện thay đổi.
11. **Change Events**: Các sự kiện thay đổi được xử lý và đẩy tới các hệ thống khác.
12. **Heartbeat**: Gửi định kỳ để duy trì kết nối và theo dõi tiến trình.

#### Nó lọc các log cho các table ở publication trước rồi mới đẩy log vào replication slot?
Đúng vậy. Quy trình lọc các log cho các bảng đã được xác định trong publication xảy ra trước khi đẩy các log vào replication slot. Cụ thể, các bước diễn ra như sau:

1. **Publication**: Khi bạn tạo một publication trong PostgreSQL, bạn xác định các bảng nào cần được sao chép. Publication này lọc các bản ghi WAL để chỉ bao gồm các thay đổi liên quan đến các bảng được chỉ định.
    
2. **Reorder Buffer**: Các bản ghi WAL được đưa vào Reorder Buffer. Bộ đệm này sắp xếp lại thứ tự các bản ghi để đảm bảo rằng chúng có thể được xử lý một cách chính xác theo thứ tự xảy ra.
    
3. **Replication Slot**: Sau khi các log đã được lọc bởi publication, chúng được giữ lại trong replication slot. Replication slot đảm bảo rằng các bản ghi WAL không bị xóa cho đến khi chúng được sao chép thành công bởi khách hàng sao chép.
    
4. **Output Plugin (pgoutput)**: Output plugin như pgoutput chuyển đổi các bản ghi WAL đã được lọc thành định dạng mà khách hàng sao chép có thể hiểu được.
    
5. **Network Communication**: Cuối cùng, các bản ghi đã chuyển đổi được gửi qua mạng đến khách hàng sao chép (Replication Client).
    
6. **Replication Client (Debezium)**: Debezium nhận các sự kiện thay đổi và xử lý chúng, biến chúng thành các sự kiện thay đổi cấu trúc dữ liệu (Change Events) để sử dụng trong các hệ thống khác.