[TOC]





## 弱网测试：

弱网会产生的问题：

​	1）丢包

​	2）数据无法加载

​	3）消息更新不及时

使用charles模拟弱网

web页面的话：

1）charles配置：开启【Throttle Setting】

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125151705352.png" alt="image-20201125151705352" style="zoom:33%;" />

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125165328874.png" alt="image-20201125165328874" style="zoom:33%;" />

确认节流开启成功——小乌龟图标亮了

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125165818190.png" alt="image-20201125165818190" style="zoom:33%;" />

2）浏览器中开启使用代理：可以配合chrome插件【[SwitchyOmega](chrome-extension://padekgcemlokbadohgkifijomclgjgif/options.html#!/about)】来用

SwitchyOmega可以方便的帮我们切换到直接连接还是代理连接，还可以设置不同的代理，然后使用这个插件来回切

在SwitchyOmega里设置：

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125165615288.png" alt="image-20201125165615288" style="zoom:33%;" />

然后chrome中选择代理就行：

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125165708987.png" alt="image-20201125165708987" style="zoom:33%;" />

如果是app弱网：

1）手机连接代理

​	tips：统一局域网、charles 

2）手机设置为3G



## 健壮性测试

用于测试系统在出现故障时，是否能自动恢复或者忽略故障继续运行

操作过程：

1.对应用进行盲点——monkey、maxim、appcrawler

2.网络不佳——charles（Throttle Setting）

3.数据不通——charles（）



## 兼容性测试

移动设备、型号

工具：appcrawler综合性能：



## webview性能

底层原理：使用Linux性能相关的命令

ps：侧面说明安卓是借用Linux内核的

```bash
$ adb shell vmstat
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 5  9      0 2396280  1340 229668    0    0  1425    18    0  795  2  4 94  1
```

工具1：vmstat

```bash
while true; do adb shell vmstat | tail -1 | awk '{print $5}'; done
```

工具2：python库：subprocess

```python
import subprocess

cmd = "adb shell vmstat | tail -1 | awk '{print $5}'"
res = subprocess.Popen(cmd, shell=True, stdout=subprocess.PIPE)
print(res.stdout.read())
```



### 方法1：python+js简直是一个利器

使用的注意点：

​	1.测webview相关的 【caps['chromedriverExecutable']='***'】一定要写

​	2.找不到元素的话，试试先打印一下driver的context

​	3.报这个错的话【AttributeError: 'WebDriver' object has no attribute 'contexts'】，把webdriver的导包由【from selenium import webdriver】改为【from **appium** import webdriver】，这里也提醒了webdriver不要随便导

​	4.如何打印webview的context：

```python
# driver的contexts属性返回一个context列表
contexts_list = driver.contexts
print(contexts_list)
# 打印结果为：['NATIVE_APP', 'WEBVIEW_cn.com.weilaihui3']
# 一般webview在最后一个
```

​	5.切换到webview

```python
driver.switch_to.context(contexts_list[-1])
```

​	6.常用的js：

```js
window.location.href="https://baidu.com"
window.location.href
window.performance.navigation.type
window.location.reload()
```

然后url放到chrome浏览器总分析

蓝线：dom加载完成时间

红线：页面所有资源加载完成时间

![image-20201124180907743](19%E8%AF%BE.assets/image-20201124180907743.png)

感觉chrome的F12里【Capture screenshots】和app启动性能分析的拆帧很像

<img src="app-web%E6%80%A7%E8%83%BD.assets/image-20201125022909214.png" alt="image-20201125022909214" style="zoom:33%;" />



输入过滤条件可以过滤资源：

表达式举例：

`status-code:404`

`domain:*.baidu.com`

`larger-than:1M`

`larger-than:1M domain:*.cn`  （ps：表达式只有and逻辑，没有or的逻辑，and用空格隔开就行）

![image-20201124182421230](19%E8%AF%BE.assets/image-20201124182421230.png)



performance可以看到调用链已经各自消耗的时间：

<img src="app-web%E6%80%A7%E8%83%BD.assets/image-20201125023220902.png" alt="image-20201125023220902" style="zoom:33%;" />

<img src="app-web%E6%80%A7%E8%83%BD.assets/image-20201124193307582.png" alt="image-20201124193307582" style="zoom:33%;" />



### 方法2：chrome respect工具

app webview ——》chrome respect工具

chrome://inspect/#devices

前提必须要手机当前activity是一个webview的才会在下面这里显示，而且得翻墙

mumu模拟器点击了【inspect】以后连接不到

### 上实例：

```python
import subprocess
from appium import webdriver
from selenium.webdriver.common.by import By


class TestWebTiming:
    def test_navigation(self):
  			caps = {}
        caps['platform'] = 'android'
        caps['platfromVersion'] = '6.0'
        caps['noReset'] = 'true'
        caps['dontStopAppOnReset'] = 'true'
        caps['appPackage'] = 'cn.com.weilaihui3'
        caps['appActivity'] = '.app.ui.activity.HomeActivity'
        caps['chromedriverExecutable'] = '/Users/liyang/chromedriver/chromedriver52'
        self.driver = webdriver.Remote("http://localhost:4723/wd/hub", caps)
        self.driver.implicitly_wait(20)
        self.driver.find_element(By.XPATH,'//*[@text="爱车"]').click()
        webview = self.driver.contexts[-1]
        # print(webview)
        self.driver.switch_to.context(webview)
        # url = self.driver.execute_script('return window.location.href')
        url = self.driver.execute_script('window.location.href="https://baidu.com"')
        # print(url)
```



侧面印证了：webview本质上用的就是chrome内核

不过，`caps['chromedriverExecuteable']='***'`这个选项不也是印证了？



小tips：

遇到webview可以先获取到webview的url，然后在chrome浏览器中进行元素定位，这个方法不需要开发帮忙开启webview的开关

```python
driver.switch_to.context(driver.contexts[-1])
url = driver.execute_script('return window.location.href')
# 拿到这个url在电脑端的chrome中获取到元素，接下来就可以用seleniu的一套了，比如可以用css selector定位了，移动端本来是没有css定位的哦
driver.find_element_by_css_selector('****')
```

或者通过adb logcat也能过滤出url的日志：

```bash
adb logcat | grep http
```



### 总结：

还是老样子，上底层原理：【webview就是一个放在app里，内核为chrome的html页面】

所以分析webview的所有的东西，比如定位、性能都是分析html一样，所以我们要分析一个webview的指标就像分析一个html的指标一样，那两种办法：第一种抓到url用万能的chrome F12的各种神级分析工具，第二种办法使用js代码获取w3c帮我们硬埋点的各种指标或者js获取页面属性

总之：强调一句，就时一个html页面，所有web的html的东西它都适合，不要被它的“包装”给吓住



## 耗电量

拓展思考：有哪些app或者功能明显耗电：

底层原理：

耗电杀手之一：唤醒锁

​	1.关闭屏幕显示，让CPU在后台运行

​	2.app长期获取唤醒锁，不释放

​		1）阻止设备进入低电量模式

指标：（电池会话：再次充满电之间）

​	1）消耗整个电池会话的0.7%

​	2）在后台运行时，消耗整个电池会话的0.1%

对于开发来说，修改方法：try-catch-finally，finally中关闭唤醒锁

测试来说，获取数据的工具：

​	1）battery history

​		基于：go、python2.7、adb

​		首先一定要清除历史的电池数据，然后开启开关、接着操作app就行

​		其实底层原理还是adb

     ####  		待补充：battery history命令

​	2）dumpsys

```bash
adb shell dumpsys power
```

其中【Wake Locks: size=0】就是唤醒锁的个数，size=0说明现在没有

## dumpsys

dumpsys查看所有的服务的



```bash
$ adb shell dumpsys -l | wc -l
     103
```



## 卡顿检测

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125122846011.png" alt="image-20201125122846011" style="zoom:33%;" />

卡与不卡的界限时什么？

12fps（帧）比较卡

24fps：电影常用，比较连贯

人眼最大感知分辨为60fps

安卓以60为基准，1000/60=16ms

引申：视频用udp。快，丢一帧没啥，但只要快人眼就感受不出来

分析问题：

systree



## 崩溃问题

 崩溃问题类型：

1.ANR：主线程5s没有响应

​	为什么会出现ANR：UI线程被阻塞太长时间

​	1）当活动处于前台，app对输入事件、广播接受器无反应5s

​	2）当活动不在前台，广播接收器长时间执行

​	什么是【广播接收器】？

​	两个应用、设备 间通信是通过广播机制

​	ANR的常见情况（都是发生在主线程）——所以，直接去主线程找就行：

​	1）app在主线程进行缓慢的I/O操作

​	2）app在主线程进行长时间计算

​	3）主线程同步绑定其他线程，其他进程长时间无响应

​	4）主线程被阻塞，等待同步锁

​	5）主线程处于死锁状态

方法&工具：

1）play console（花钱、检查自己的应用）

2）开发代码埋点：严苛模式

严苛模式会捕捉主线程偶然磁盘或者网络访问，系统检测出主线程违例的情况

3）开发者模式中打开【show all ANRS】

<img src="app%E4%B8%93%E9%A1%B9%EF%BC%88%E5%85%A8%EF%BC%89.assets/image-20201125144009951.png" alt="image-20201125144009951" style="zoom:33%;" />



4）查看trace日志（新版本android）

​	`adb pull /data/anr/traces.txt`

`adb bugreport 本地保存文件的路径 `

2.java Crash：未捕获的Android vm异常（一般是java语言写的）

3.Native Crash：未处理的native异常（一般是C语言写的）



## Amdahl定律

