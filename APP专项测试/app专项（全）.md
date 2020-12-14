[TOC]

## app特性

要测APP端的专项，要先了解移动端的一些特性，下面的这些特性都是指Android：

Android的4大组件5大存储6大布局

### 组件

4大组件：activity、service、content provider、broadcast receiver

#### activity

activity：一个页面就是一个activity，可以通过adb logcat来获取，这个activity就是我们appium启动的cap里配置的【appActivity】，其实就是启动到哪个页面的意思，所以给我们测试用例又提供了一个思路：有些页面没必要一层一层通过各种操作来进入，直接拿到这个页面的activity进入就行

```python
driver.start_activity(package,activity)
```

activity的生命周期：话说，只要涉及到【生命周期】这个名词，其实顾名思义，不就是由生到死的过程吗，到那时要把“主人公”（这里是activity）套进去，而我们工作中遇到很多其他的生命周期，比如：【serverlet生命周期】、【线程生命周期】、【Vue生命周期】都是一个道理，就是让你说说这“主人公”怎么出生、经历了什么、然后消亡的过程，这样我们就很好说明activity的生命周期了：

1）用户参与下的正常的生命周期：

 - 调用OnCreate，创建activity，activity的状态为【已创建】
 - 调用OnStart，此时的activity用户可见，activity的状态为【已开始】
 - 调用onResume，此时activity一直与用户交互，activity的状态为【已恢复】
 - 调用onPause，此时activity不在前台，activity的状态为【已暂停】
    - 如果activity恢复到前台，会调用onResume
 - 调用onStop，此时activity已结束运行，activity状态为【已停止】
    - 完成一些重量级的资源的回收工作
 - 调用onDestory，此时activity彻底结束，activity状态为【已销毁】
    - 资源回收、释放

#### service

实现程序的后台运行

#### content provider

内容数据提供者

#### broadcast receiver

广播接收器：不同的应用、设备间都是通过broadcast receiver来通信

这里要注意：如果activity长时间不在前台，而broadcast reveiver又长时间接收消息的话，容易发生ANR，ANR是指【主线程5S没有反应】，而如果broadcast receiver长时间执行，导致UI线程被阻塞，就会发生ANR

### 布局

android通过容器的布局属性来管理控件位置，而ios通过变量之间的相对关系完成控件的位置计算

android的层级结构是一个定制的xml，叫【app source】，app source类似于dom树，表示app层级，代表界面里面所有控件树的结构，每个控件都有各种属性，比如：resource_id、content_desc，而由于他是类dom的，所以可以通过xpath定位

引申点：

dom（document object model：文档对象模型）用于表示界面的控件层级，界面的结构化描述，常见的格式为html、xml，核心元素为节点和属性

xpath（xml path：xml路径语言）：用于xml的节点定位

css selector（css选择器）：用于根据元素的属性或属性值选择元素

举例：

xpath定位：

//* 任意元素、/ 孩子、//子子孙孙、.当前节点、..父亲节点

```python
driver.find_element_by_xpath("//div[@id='C']/../..")
driver.find_element_by_xpath("//div[@id='B']/div")
driver.find_element_by_xpath("//*[@text='登陆']")
```

css selector定位：

#id、. class、>孩子、空格 子子孙孙、:nth-child(n) 第几个孩子

```python
driver.find_element_by_css_selector('#id_value')
driver.find_element_by_css_selector('[id=id_value]')
driver.find_element_by_css_selector('.class_value')   
driver.find_element_by_css_selector("input[type='password']").send_keys('test')
driver.find_element_by_css_selector('div#B>div')
driver.find_element_by_css_selector('div#B div:nth-child(1)')
```

## 网络

移动端切换4G、wifi、弱网

### 弱网测试：

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

如果是app测试弱网：

1）手机连接代理，charles设置【Throttle setting】

​	tips：同一局域网、手机开启代理、charles 允许远程调试、安装证书、信任证书

2）或者直接设置为3G



## 健壮性测试

用于测试系统在出现故障时，是否能自动恢复或者忽略故障继续运行

操作过程：

1.对应用进行盲点——monkey、maxim

2.网络不佳——charles（Throttle Setting）

3.数据不通——charles（Throttle Setting 或者打上断点篡改返回，让返回缺斤少两）



## 兼容性测试

**兼容性测试是app测试的重点**

需要兼容不同的系统版本：android版本、ios版本

需要兼容不同的厂商机型：华为（pro、mate）、三星、小米、OPPO、vivo

不同的屏幕：屏幕大小、头帘、曲面屏、横竖屏

可以使用商业的云测平台：testin之类，或者是搭建自己公司的设备实验室，通过OpenSTF这样的设备平台进行管理

引申：现在的兼容性、自动化都把大力气放在安卓上，可能的原因有几点：

1.安卓由于其开放性，导致了不同的厂商都很有自己的特性，不同的开发也有自己的特性，而且系统版本也有很多人没有升级过，很乱，所以问题就多，这样当然是主要去适配了

2.ios app在代码开发上很规范，不然都没法通过app store的审核，然后用户的ios系统也一般保持在新版本，毕竟老版本很多功能不支持用户不得不升级，再加上项目组的人员大多数用的都是ios系统，在测试、开发、适用的过程其实已经把ios兼容的差不多了

3.ios的自动化需要mac电脑，而有些公司······



## 性能

### 内存

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



CPU｜GPU



### webview



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

