# python判断文件存在

## os模块

### 判断文件是否存在

```python
import os
os.path.exists(test_file.txt)
#True

os.path.exists(no_exist_file.txt)
#False
```

### 判断文件夹是否存在

```python
import os
os.path.exists(test_dir)
#True

os.path.exists(no_exist_dir)
#False
```

这种方法有个问题，假设你想检查文件“test_data”是否存在，但是当前路径下有个叫“test_data”的文件夹，这样就可能出现误判。为了避免这样的情况，可以只检查文件：

```python
import os
os.path.isfile("test_data")
```

### 判断文件是否可以进行读写操作

```python
import os
if os.access("/file/path/foo.txt", os.F_OK):
    print "Given file path is exist."

if os.access("/file/path/foo.txt", os.R_OK):
    print "File is accessible to read"

if os.access("/file/path/foo.txt", os.W_OK):
    print "File is accessible to write"

if os.access("/file/path/foo.txt", os.X_OK):
    print "File is accessible to execute"
```

## Try 语句

```python
try:
    f =open()
    f.close()
except IOError:
    print "File is not accessible."
```

## pathlib 模块

pathlib模块在Python3版本中是内建模块，但是在Python2中是需要单独安装三方模块。

### 检查路径是否存在

```python
path = pathlib.Path("path/file")
path.exist()
```

### 检查路径是否是文件

```python
path = pathlib.Path("path/file")
path.is_file()
```

