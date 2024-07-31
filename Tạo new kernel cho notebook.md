#### Tạo môi kernel by python virtualvenv
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

- install conda notebook
```
conda install nb_conda
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

#### Tạo kernel by conda
```
conda create -n yourenvname python=3.8 pyspark=3.5.1
conda active yourenvname
conda install nameofthelib

conda install ipykernel

## add kernel
python -m ipykernel install --user --name=yourenvname --display-name="test2"
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
spark = (
		 SparkSession
		.builder
		.master("spark://spark-master:7077")
		.appName('test')
		.getOrCreate()
	)
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



SELECT * FROM `radar-377104.operations.true_datastride_campaign_entity_master_union` where platform like "Facebook%";



{
	"argv": [
		"/opt/conda/bin/python",
		"-m",
		"ipykernel_launcher",
		"-f",
		"{connection_file}"
	],
		"display_name": "pyspark3.1.1",
		"language": "python",
		"metadata": {
		"debugger": true
	},
	"env": {
		"SPARK_HOME": "/usr/local/spark",
		"PYTHONPATH": "/opt/conda/bin/python",
		"PYTHONSTARTUP": "/usr/local/spark/python/pyspark/shell.py",
		"PYSPARK_SUBMIT_ARGS": "--master k8s://https://rancher-ic-dev.vnpt.vn --deploy-mode cluster --name spark-example  --conf spark.kubernetes.authenticate.submission.oauthTokenFile=/home/jovyan/token pyspark pyspark-shell"
	}
}



spark-submit \
    --master k8s://https://rancher-ic-dev.vnpt.vn \
    --deploy-mode cluster \
    --name spark-example \
    --conf spark.kubernetes.namespace=spark-jobs \
    --conf spark.executor.instances=2 \
    --conf spark.kubernetes.container.image=hub.vnpt.vn/uxdata/spark-3.3.0-hadoop3:latest \
    --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
    --conf spark.kubernetes.pyspark.pythonVersion=3 \
    --conf spark.kubernetes.container.image.pullPolicy=Always \
    --conf spark.kubernetes.container.image.pullSecrets=habour-registry \\ 
    --conf spark.kubernetes.authenticate.submission.oauthTokenFile=/home/ducdn/Desktop/spark-on-k8s/spark-docker/token \
    local:///app/bin/run.py
    
    
    
    
    /usr/local/spark/python/pyspark/shell.py
    
    
 "env": {
	"SPARK_HOME": "/usr/local/spark",
	"PYTHONPATH": /opt/conda/bin/python",
	"PYTHONSTARTUP": "/opt/spark/python/pyspark/shell.py",
	"PYSPARK_SUBMIT_ARGS": "--master k8s://https://rancher-ic-dev.vnpt.vn pyspark-shell"
}



```
from pyspark.sql import SparkSession
spark = (
    SparkSession.builder.appName("alo9")
    .master("k8s://https://kubernetes.default:443") # Your master address name
    .config("spark.kubernetes.container.image", "ducdn01/pyspark:latest") # Spark image name
    .config("spark.driver.port ", "8002")
    .config("spark.blockManager.port", "8001")
    .config("spark.driver.host", "jupyterhub-svc.jupyterhub") # Needs to match svc
    .config("spark.kubernetes.namespace", "spark")
    .config("spark.kubernetes.authenticate.driver.serviceAccountName", "spark")
    .config("spark.kubernetes.authenticate.serviceAccountName", "spark")
    .config("spark.executor.instances", "2")
    .config("spark.kubernetes.container.image.pullPolicy", "IfNotPresent")
    .getOrCreate()
)
```




```
from pyspark.sql import SparkSession
spark = (
    SparkSession.builder.appName("duc1")
    .master("k8s://https://kubernetes.default:443") # Your master address name
    .config("spark.kubernetes.container.image", "ducdn01/pyspark:latest") # Spark image name
    .config("spark.driver.port ", "8002")
    .config("spark.blockManager.port", "8001")
    .config("spark.driver.host", "spark-svc.test1") # Needs to match svc
    .config("spark.kubernetes.namespace", "test1")
    .config("spark.kubernetes.authenticate.driver.serviceAccountName", "spark1")
    .config("spark.kubernetes.authenticate.serviceAccountName", "spark1")
    .config("spark.kubernetes.authenticate.executor.serviceAccountName", "spark1")
    .config("spark.executor.instances", "2")
    .config("spark.kubernetes.container.image.pullPolicy", "IfNotPresent")
    .getOrCreate()
)


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

df.groupBy("department").sum("salary").show(truncate=False)

spark.stop()
```


```
spark = (
    SparkSession.builder
    .appName("ducdd")
    .master("k8s://https://kubernetes.default:443") # Your master address name
    .config("spark.kubernetes.container.image", "apache/spark:3.5.1") # Spark image name
    .config("spark.driver.port ", "29413")
    .config("spark.driver.bindAddress", "0.0.0.0")
    # .config("spark.blockManager.port", "8001")
    .config("spark.driver.host", "spark-jump-pod-svc.jupyter-spark") # Needs to match svc
    .config("spark.kubernetes.namespace", "jupyter-spark")
    .config("spark.kubernetes.authenticate.driver.serviceAccountName", "spark-sa")
    .config("spark.kubernetes.authenticate.serviceAccountName", "spark-sa")
    .config("spark.kubernetes.authenticate.executor.serviceAccountName", "spark-sa")
    .config("spark.executor.instances", "2")
    .config("spark.kubernetes.container.image.pullPolicy", "IfNotPresent")
    .getOrCreate()
)
```




```
pyspark --name sparksh-1                  
--master k8s://https://kubernetes.default.svc.cluster.local:443                  
--deploy-mode client                  
--conf spark.kubernetes.authenticate.driver.serviceAccountName=spark-sa                 
--conf spark.kubernetes.namespace=jupyter-spark                  
--conf spark.executor.instances=3                  
--conf spark.kubernetes.container.image=apache/spark:3.5.1                  
--conf spark.driver.host=jump-1-svc.jupyter-spark.svc.cluster.local                  
--conf spark.driver.port=30123
```


#### Get all config of Spark Session
```
config = spark.sparkContext.getConf().getAll()

# Print the configuration settings
for key, value in config:
    print(f"{key}: {value}")
```



```
from py4j.java_gateway import java_import
jvm = spark.sparkContext._jvm
java_import(jvm, 'org.apache.spark.sql.api.python.*')

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