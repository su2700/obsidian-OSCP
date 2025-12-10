
```
virtualenv env

```

```
source env/bin/activate
```

```
# 用标准库 venv
python3 -m venv .venv
source .venv/bin/activate

# 用 virtualenv（先安装 virtualenv）
pip install --user virtualenv
virtualenv env
source env/bin/activate

# 指定解释器（virtualenv 更灵活）
virtualenv -p /usr/bin/python3.11 env311

```

 ```
 python3 -m venv myvenv
 ```
➜  143 
```
source myvenv/bin/activate
```
(myvenv) ➜  143 
```
python -m pip install aerospike
```

Collecting aerospike
  Downloading aerospike-18.0.0-cp313-cp313-manylinux_2_28_x86_64.whl.metadata (5.4 kB)
Downloading aerospike-18.0.0-cp313-cp313-manylinux_2_28_x86_64.whl (6.1 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.1/6.1 MB 8.2 MB/s  0:00:00
Installing collected packages: aerospike
Successfully installed aerospike-18.0.0
(myvenv) ➜  143 python cve2020-13151.py