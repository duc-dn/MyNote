```
def test(n):
	for i in range(n):
		yield i

  
num = test(10)
print(num) # <generator object test at 0x7f8b1c1b3f90>

for i in num:
	print(i, type(i))
```


- Generator như một blueprint chứa các phép biến đổi trong hàm và chỉ khi gọi nó thì mới bắt đầu thực hiện tính các phép biến đổi trong hàm 
- Khi bạn gọi một generator, nó không thực sự chạy ngay lập tức. Thay vào đó, nó trả về một iterator, và chỉ khi bạn yêu cầu giá trị tiếp theo từ iterator (thông qua vòng lặp `for`, hoặc sử dụng `next()`), generator mới bắt đầu thực hiện tính toán và trả về giá trị.
 
- Trong mã mẫu của bạn, `num` không phải là một iterator mà thực chất là một generator object. Khi bạn gọi hàm `test(10)`, nó trả về một generator object, chứ không phải một iterator trực tiếp.

- Tuy nhiên, bạn có thể sử dụng hàm `iter()` để chuyển đổi generator object thành một iterator. Điều này là cần thiết nếu bạn muốn sử dụng các phương thức như `next()` để lấy giá trị từ generator.

```
def test(n):
	for i in range(n):
		yield i

  
num = test(10)
print(num) # <generator object test at 0x7f8b1c1b3f90>

for i in num:
	print(i, type(i))
```

- Vòng lặp `for` tự động gọi hàm `next()` trên generator object cho đến khi generator không sinh ra giá trị nữa
- Python tự động gọi hàm `next()` cho generator object trả về từ `test(10)`. Mỗi lần vòng lặp lấy giá trị từ generator object, generator function sẽ được thực thi cho đến khi gặp phát biểu `yield`, tạo ra một giá trị mới để trả về. Sau đó, vòng lặp in giá trị đó và tiếp tục lặp lại quá trình cho đến khi generator không sinh ra giá trị nữa. Điều này giúp việc sử dụng generator trở nên tiện lợi và tự nhiên khi làm việc với vòng lặp.