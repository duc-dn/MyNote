- Tạo môi trường ảo dùng conda or python virtualvenv
```
python -n venv <tên kernel>
```


- Active kernel
```
source ./bin/activate
```

- Cài đặt các requirements (pyspark, pandas, ...)
- Cài đặt ipykernel
```
pip install ipykernel
```

- List kernel
```
jupyter kernelspec list
```


- Active kernel
```
python -m ipykernel install --user --name <tên kernel>
```

>Câu lệnh "python -m ipykernel install --user --name <tên kernel>" được sử dụng để cài đặt một kernel IPython mới vào Jupyter Notebook của bạn. 
  Dưới đây là ý nghĩa của các cờ và tham số trong câu lệnh:

```
- "python": Đây là từ khóa để chạy một tệp mã Python hoặc một module Python từ dòng lệnh.
- "-m ipykernel": Đây là một tùy chọn của Python để chạy module ipykernel. Module này cung cấp các công cụ để cài đặt và quản lý các kernel IPython.
- "install": Đây là một tùy chọn cho module ipykernel để yêu cầu cài đặt một kernel mới.
- "--user": Đây là một tùy chọn để chỉ định cài đặt kernel cho người dùng hiện tại thay vì toàn hệ thống. Nghĩa là kernel này chỉ khả dụng cho người dùng hiện tại.
- "--name <tên kernel>": Đây là một tùy chọn để đặt tên cho kernel mới. Bạn cần thay thế "<tên kernel>" bằng tên bạn muốn đặt cho kernel.
```

### Setup sparkmagic
Link: https://github.com/jupyter-incubator/sparkmagic

```
pip install sparkmagic
```
#### Fix error when installing 
- Install sparkmagic
```python
apt-get install libkrb5-dev
```

- nbextension does not appear
```
This error can cause version. Therefore downgrade version notebook
```
```
pip install notebook==6.1.5
```
Link: https://stackoverflow.com/questions/49647705/jupyter-nbextensions-does-not-appear

### Test with notebook

```python
import pyspark
from pyspark.sql import SparkSession
spark = SparkSession.builder
                    .appName('test') \
                    .getOrCreate()
```

```python
simpleData = [("James","Sales","NY",90000,34,10000),
    ("Michael","Sales","NY",86000,56,20000),
    ("Robert","Sales","CA",81000,30,23000),
    ("Maria","Finance","CA",90000,24,23000),
    ("Raman","Finance","CA",99000,40,24000),
    ("Scott","Finance","NY",83000,36,19000),
    ("Jen","Finance","NY",79000,53,15000),
    ("Jeff","Marketing","CA",80000,25,18000),
    ("Kumar","Marketing","NY",91000,50,21000)
  ]

schema = ["employee_name","department","state","salary","age","bonus"]
df = spark.createDataFrame(data=simpleData, schema = schema)
df.printSchema()
df.show(truncate=False)
```


```python
df.groupBy("department").sum("salary").show(truncate=False)
```

```python
df.groupBy("department").count()
```