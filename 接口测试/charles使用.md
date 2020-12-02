# 使用

​	chrles抓电脑web页面浏览器包的时候可以配合chrome插件【[SwitchyOmega](chrome-extension://padekgcemlokbadohgkifijomclgjgif/options.html#!/about)】来用

​	SwitchyOmega可以方便的帮我们切换到直接连接还是代理连接，还可以设置不同的代理，然后使用这个插件来回切

​	在SwitchyOmega里设置：

![image-20201125165615288](charles%E4%BD%BF%E7%94%A8.assets/image-20201125165615288.png)

​	然后chrome中选择代理就行：

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201125165708987.png" alt="image-20201125165708987" style="zoom:33%;" />

​	如何确认浏览器成功的使用了charles代理：点击【🔒】的图标

<img src="/Users/liyang/Library/Application%20Support/typora-user-images/image-20201122020946822.png" alt="image-20201122020946822" style="zoom:33%;" />

# charles使用

​	

## 交互式拦截请求和响应

手工拦截——打断点	

​	篡改请求值、返回值：

​	1）把要篡改的请求右键-【Breakpoints】——打断点

​	2）重新发起一次请求

​	3）charles里这时会出现篡改的页面，第一个页面是篡改request的，如果不改request点击execut之后的页面是篡改response的，看情况你要改哪个，改request之后execut就直接返回了你改了以后的request对应的返回，如果是改的是response那就返回你改的response

测试中使用的场景：

构造前端数据：返回值：超长、特殊值

场景：app登陆的账户名下的车的车辆数据不全，而数据全的车没有挂到相应的测试账号下，就篡改app登陆用户的获取车辆数据的接口中的车辆vim码，改成数据全的vim码这样就能在前端展示了

Map-local也可以实现想通的效果，把预期中的response保存到本地，然后再想要篡改的请求上Map-local到保存的本地文件就可以了

## 自动化拦截

Mock 对请求和响应进行动态修改

Rewrite





## 篡改使用资源：

常用：Fake用本地cache代替线上环境

Map-local把要用到的资源比如js、png改成读取本地的

## 模拟弱网

charles配置：开启【Throttle Setting】

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201125151705352.png" alt="image-20201125151705352" style="zoom:33%;" />

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201125165328874.png" alt="image-20201125165328874" style="zoom:33%;" />

确认节流开启成功——小乌龟图标亮了

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201125165818190.png" alt="image-20201125165818190" style="zoom:33%;" />

## 重定向：

常用：Fake用测试环境代替线上环境：

比如：马山要上线的一个接口，想要用线上的app看一下，

通过设置断点的改动有点太具体了，具体到每个请求的期望的预期返回

但是是否可以把同一类请求都重定向到另一个host呢？

使用Map Remote Setting

可以把比如prod的域名Remote到stg环境来复现问题

不过前提是要在stg维护好和线上环境一样的数据

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201125191322750.png" alt="image-20201125191322750" style="zoom:33%;" />



map remote > map local



## charles基础功能：

1.导出为各式各样的文件，这些文件可以用于数据解析，导入到其他软件如postman里

2.charles是以session为一个单位了，可以打开不同的session

3.不同的http协议版本发起的请求方式不一样，可以通过chart里看到

而charles里的chart和chrome里的【waterfall】功能类似，都是可以看到资源的请求和响应的时间

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104036473.png" alt="image-20201126104036473" style="zoom:33%;" />

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104055901.png" alt="image-20201126104055901" style="zoom:33%;" />

4.高亮：

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104506248.png" alt="image-20201126104506248" style="zoom:33%;" /> 

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104618471.png" alt="image-20201126104618471" style="zoom:33%;" />

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104749483.png" alt="image-20201126104749483" style="zoom:33%;" />

5.只聚焦于某一个host主机的请求：

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126104942632.png" alt="image-20201126104942632" style="zoom:33%;" />

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126105011250.png" alt="image-20201126105011250" style="zoom:33%;" />

<img src="charles%E4%BD%BF%E7%94%A8.assets/image-20201126105105071.png" alt="image-20201126105105071" style="zoom:33%;" />



引申：跨语言调用：

[Protocol Buffers](https://developers.google.com/protocol-buffers)

charles的底层原理：

1）SSL

2）代理



SOCKS：一种技术，应用：

比如：代理，

websocked：应用层的一个协议

也是应用层的，所以如果要是抓了http就没法抓websocked



socket：七层模型的具体实现：socket编程

socket是一个抽象接口，不是协议