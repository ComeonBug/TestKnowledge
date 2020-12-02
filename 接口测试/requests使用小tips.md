1.开启代理后requests如何能访问到url

如果在python requests不想挂代理，不想下载证书，就可以使用

```python
requests.get('https://www.baidu.com', verify=False)
```



2.http basic基本认证——Basic access authentication

比如请求头中有这样的信息：

`'Authorization': 'Basic bHk6MTIzNDU2'`

代理请求时，提供用户名和密码的一种方式

```python
import requests
from requests.auth import HTTPBasicAuth

class TestHttp:
    def test_auto(self):
        username = 'ly'
        pwd = '123456'
        url = f'https://httpbin.org/basic-auth/{username}/{pwd}'
        res = requests.get(url, auth=HTTPBasicAuth(username, pwd))
        print(res.text)
```

