# Ubuntu 系统安装 Python 的 Redis 客户端

## 系统和软件版本
1. 操作系统：Lubuntu 16.04 / 18.04
2. Redis 版本：4.0.2
3. Python 版本：2.7.12

## 问题描述
按照书中的第一章的简单方法和附录 A 中的详细方法，安装Python 的 redis 和 hiredis 均失败。

在网上搜到的安装方法解决了这个问题，以下是整理后的具体安装步骤。


## 安装步骤
假定文件下载的保存目录是 ```/usr/local/src```

第一步：下载 get-pip.py 文件（用来代替easy_setup.py）
```
$ sudo wget https://bootstrap.pypa.io/get-pip.py
```


第二步：运行 get-pip.py ，安装 setuptools 工具
```
$ sudo python get-pip.py
```

当执行完成，看到了屏幕上输出的最后两行类似下面的提示，则说明成功安装了 setuptools 工具。
```
Installing collected packages: pip, setuptools, wheel
Successfully installed pip-19.2.2 setuptools-41.1.0 wheel-0.33.4
```


第三步：安装 redis, hiredis 客户端
```
$ sudo python -m easy_install redis hiredis
$ sudo python -m easy_install redis hiredis
```

仔细看输出可以看到类似如下的提示：
```
...

Installed /usr/local/lib/python2.7/dist-packages/redis-3.3.7-py2.7.egg
Processing dependencies for redis
Finished processing dependencies for redis

...

Installed /usr/local/lib/python2.7/dist-packages/hiredis-1.0.0-py2.7-linux-x86_64.egg
Processing dependencies for hiredis
Finished processing dependencies for hiredis
```


## 测试 Python 的 redis 客户端
第一步：命令行中启动 Python
```
$ python
Python 2.7.12 (default, Nov 12 2018, 14:36:49)
[GCC 5.4.0 20160609] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

第二步：输入 import redis
```
>>> import redis
```

如果这一步没有报错，则说明上面的安装是成功的。

第三步：使用 Python 操作 redis (数据库可选择 0~15 中的任意一个)
```
>>> conn = redis.Redis(db=8)            # 创建一个指向Redis的连接
>>> conn.set('str-key', 'hello world')  # 往 redis 写入
True
>>> conn.get('str-key')                 # 从 redis 读取
'hello world'
>>>
```


## 按书中的方法有以下报错信息
```
$ sudo python ez_setup.py
Downloading http://pypi.python.org/packages/2.7/s/setuptools/setuptools-0.6c11-py2.7.egg
Traceback (most recent call last):
  File "ez_setup.py", line 278, in <module>
    main(sys.argv[1:])
  File "ez_setup.py", line 210, in main
    egg = download_setuptools(version, delay=0)
  File "ez_setup.py", line 158, in download_setuptools
    src = urllib2.urlopen(url)
  File "/usr/lib/python2.7/urllib2.py", line 154, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/lib/python2.7/urllib2.py", line 435, in open
    response = meth(req, response)
  File "/usr/lib/python2.7/urllib2.py", line 548, in http_response
    'http', request, response, code, msg, hdrs)
  File "/usr/lib/python2.7/urllib2.py", line 473, in error
    return self._call_chain(*args)
  File "/usr/lib/python2.7/urllib2.py", line 407, in _call_chain
    result = func(*args)
  File "/usr/lib/python2.7/urllib2.py", line 556, in http_error_default
    raise HTTPError(req.get_full_url(), code, msg, hdrs, fp)
urllib2.HTTPError: HTTP Error 403: SSL is required
```
