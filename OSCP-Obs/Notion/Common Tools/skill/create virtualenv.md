
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