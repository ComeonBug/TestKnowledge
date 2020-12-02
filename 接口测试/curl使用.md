curl使用

curl的作用：

1.把浏览器的请求真实的还原出来

2.附带了认证信息，所以可以脱离浏览器执行

3.可以方便开发者重放请求、修改参数调优、编写脚本

curl很强大的，也可以查看邮箱

curl是基于nc 的

curl优于postman的原因是：脚本化

脚本能做的事情太多了

curl的帮助文档直接键入：【man curl】即可

curl常见用法：

url = ''https://ww.baidu.com'

get请求：curl $curl

post请求（-d后面加post的data）：curl -d "***" $url

​	从浏览器中copy出来以后一般显示的是【--data-raw】这个选项，和【-d】选项是一个，这里其实隐含的一个知识点：linux命令一般--两个杠是命令的全称，-一个杠是命令的简写

proxy挂代理（可以和charles等抓包工具配合使用）：curl -x "代理服务器地址" $curl

使用curl很简单，不需要在额外的安装证书什么的

引申：如果在python requests不想挂代理，不想下载证书，就可以使用

```python
requests.get('https://www.baidu.com', verify=False)
```

## 举例：

ps:很多参数其实是没必要的，是浏览器主动帮我们加的

`curl 'https://httpbin.org/post' \
  -H 'authority: httpbin.org' \
  -H 'pragma: no-cache' \
  -H 'cache-control: no-cache' \
  -H 'upgrade-insecure-requests: 1' \
  -H 'origin: https://httpbin.org' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.198 Safari/537.36' \
  -H 'accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9' \
  -H 'sec-fetch-site: same-origin' \
  -H 'sec-fetch-mode: navigate' \
  -H 'sec-fetch-user: ?1' \
  -H 'sec-fetch-dest: document' \
  -H 'referer: https://httpbin.org/forms/post' \
  -H 'accept-language: zh-CN,zh;q=0.9' \
  -d 'custname=name&custtel=15812341234&custemail=123%40qq.com&size=small&topping=bacon&topping=cheese&delivery=&comments=test' \
  --compressed ;`