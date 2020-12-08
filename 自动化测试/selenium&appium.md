# 简介

​	selenium、appium 完成UI自动化的两驾马车，而appium其实是基于selenium+adb的，下面来详细说明一下这俩的原理、常用方法、一些面试题和工作中问题的解决。

​	selenium：3大组件、2种等待方式、8种定位方式、16个元素方法、3种switch_to控件、

## selenium

​	我们常说的selenium是指框架，就是java或者python中导入的那个包，但是其实它不只是框架部分，selenium是一个**web自动化测试工具**，它里面包含了几个工具：**webdriver、IDE、Grid**。其中我们最长用的是webdriver，毕竟代码里实例化一个Driver就是用的webdriver；Grid是用来做分布式，我们可以让case运行到不同的电脑、不同的系统上，既可以节省运行时间，也能做兼容性测试；IDE的话，用的很少，毕竟这个功能挺鸡肋的，很多好用的工具可以代替，比如我们强大的Chrome F12就行，Firefox也有代替工具，所以这个就不需要了。

### 原理：

其实，就下面这么一串：

代码——〉webdriver——〉浏览器driver——〉浏览器—返回结果—〉浏览器driver——〉webdriver——〉代码

这个写的不错可以参考：https://www.cnblogs.com/jiang-cheng/p/9914803.html

webdriver其实是一无所知的，它不知道怎么找到对象、怎么操作这个对象、结果是否符合预期····

它只做一件事：完成代码和浏览器driver的通信

<img src="../Untitled.assets/image-20201207172417814.png" alt="image-20201207172417814" style="zoom:50%;" />

<img src="../Untitled.assets/image-20201207172506900.png" alt="image-20201207172506900" style="zoom:50%;" />

webdriver api就是我们导入的包，我们使用这个webdriver编辑测试代码，webdriver会调用浏览器的driver，比如ChromeDriver，把来驱动浏览器

```python
driver = webdriver.Chrome()
driver.get('https://www.baidu.com')
ele = driver.find_element(By.XPATH,"//input[@id='kw']")
ele.send_keys('selenium')
time.sleep(5)
driver.quit()
```

上面4个步骤，根据上面的原理来解释一下。

（1）第一步：会先实例化webdriver对象，并且访问http://localhost:4444/wd/hub创建session，然后ChromeDriver带着sessionid去调用浏览器api新建了窗口

所以其实第一句代码实际上是以下2个过程的简写形式：

第一个过程为启动selenium server

`$ java -jar selenium-server-standalone-3.141.59.jar`

```python
capabilities = {
            "browserName": "chrome",
            "seleniumProtocol": "WebDriver",
            "platform": "MAC",
            "version": "10"
        }
driver = webdriver.Remote('http://192.168.56.1:4444/wd/hub', capabilities)
```



（2）第二步：driver这个对象就等同于sessionid，当执行第二步这行代码，实际是去请求ChromeDriver的get接口，浏览器driver收到请求后，去调浏览器API输入url访问，因为有了sessionid，所以ChromeDriver、浏览器知道是在那个窗口打开url。

（3）第三步：请求webdriver的find_element接口，参数xpath、value值，拿到elementid

（4）第四步：去请求webdriver的element send keys接口，webdriver调用浏览器API输入值。



通过日志可以更清楚的看到整个过程：初始化webdriver、初始化一个ChromeDriverService实例、启动ChromeDriver、创建连接的session、最终销毁session：

```bash
$ java -jar selenium-server-standalone-3.141.59.jar
20:22:11.487 INFO [GridLauncherV3.parse] - Selenium server version: 3.141.59, revision: e82be7d358
20:22:11.586 INFO [GridLauncherV3.lambda$buildLaunchers$3] - Launching a standalone Selenium Server on port 4444
2020-12-07 20:22:11.640:INFO::main: Logging initialized @402ms to org.seleniumhq.jetty9.util.log.StdErrLog
20:22:11.893 INFO [WebDriverServlet.<init>] - Initialising WebDriverServlet
20:22:11.998 INFO [SeleniumServer.boot] - Selenium Server is up and running on port 4444
20:22:36.081 INFO [ActiveSessionFactory.apply] - Capabilities are: {
  "browserName": "chrome",
  "platform": "MAC",
  "seleniumProtocol": "WebDriver",
  "version": "10"
}
20:22:36.083 INFO [ActiveSessionFactory.lambda$apply$11] - Matched factory org.openqa.selenium.grid.session.remote.ServicedSession$Factory (provider: org.openqa.selenium.chrome.ChromeDriverService)
Starting ChromeDriver 86.0.4240.22 (398b0743353ff36fb1b82468f63a3a93b4e2e89e-refs/branch-heads/4240@{#378}) on port 43534
Only local connections are allowed.
Please see https://chromedriver.chromium.org/security-considerations for suggestions on keeping ChromeDriver safe.
[1607343756.172][WARNING]: FromSockAddr failed on netmask
ChromeDriver was started successfully.
20:22:39.205 INFO [ProtocolHandshake.createSession] - Detected dialect: W3C
20:22:39.262 INFO [RemoteSession$Factory.lambda$performHandshake$0] - Started new session be34abcf3c046921f04e7c13f3d16e19 (org.openqa.selenium.chrome.ChromeDriverService)
20:22:45.603 INFO [ActiveSessions$1.onStop] - Removing session be34abcf3c046921f04e7c13f3d16e19 (org.openqa.selenium.chrome.ChromeDriverService)
```



### 元素8大定位方法：

id、xpath、css selector、link text、name、class name、tag name、partial_link_text

最简单唯一的肯定id，但是由于开发的不规范不写id属性，还有时候id是动态变化的，这时候xpath和css selector就很好用了

但如果我们看下面selenium的源码find_element方法会发现，其实id、name、class name、tag name的本质上都是css selector定位，所以其实很多人说什么id定位最好····这种说法也不是那么准确

```python
def find_element_by_id(self, id_):
    return self.find_element(by=By.ID, value=id_)

def find_element(self, by=By.ID, value=None):
    if self.w3c:
        if by == By.ID:
            by = By.CSS_SELECTOR
            value = '[id="%s"]' % value
        elif by == By.TAG_NAME:
            by = By.CSS_SELECTOR
        elif by == By.CLASS_NAME:
            by = By.CSS_SELECTOR
            value = ".%s" % value
        elif by == By.NAME:
            by = By.CSS_SELECTOR
            value = '[name="%s"]' % value
    return self.execute(Command.FIND_ELEMENT, {
        'using': by,
        'value': value})['value']
```

而对于css和xpath我们该如何选择呢？毕竟用例的好坏有一部分原因是和定位方式的选择有关的

先来科普一下页面的加载方式：页面是自上而下解析的，元素呈现顺序为：title-dom树-css（此时元素可见了）-js（此时元素才是可点击的）

**引申点**：我们有时候页面白屏时间过长，可以考虑优化js代码的位置，因为页面是自上而下解析的，所以如果js放在html代码的开始位置，那会占用大量的时间去下载js，导致白屏时间过长，所以可以把js放在html代码的末尾，先加载页面让用户能看到

css selector是依赖于js和页面css的加载的，也就是会等js加载完毕才会查找元素，毕竟css selector定位是通过css的样式来定位的，xpath的话是解析HTML和xml的dom结构，自上而下遍历来找到元素，所以css相对xpath来说寻找元素的速度比较快，但xpath也有优点，xpath即可以在html页面中也可以用在原生页面中，所以如果用例设计的好是可以做到web和app共用一套代码的，不过，这个需要考虑的很多，还需要依赖于web和app端的业务，所以还是尽量分开写，不过有些需要相对定位的xpath就挺适合的

所以：**没有最好的定位方式只有最适合的定位方式**

这里可以把很多人吐槽UI自动化测试不稳定的问题拿出来说说了：UI测试不稳定的原因其实就一个：

#### case不稳定，元素有时候定位不到的原因分析

这个问题的可能原因就有很多：

1.网络原因

这个是最好解决的：合理使用隐式等待、显示等待、强制等待

那怎么个合理使用呢？

可以先大概统计一下你们的页面的平均打开时间是多长时间，根据这个时间设置全局隐式等待时间，然后在一些动态加载资源的页面、或者是什么前提条件的逻辑的时候加显示等待（只有达到什么条件才继续执行），这样2个等待配合一般就能解决了网络原因了，如果还是不行，那就加强制等待，但是这种情况也不多，加一两个对整个流程的速度也影响不大，这里要提醒：我们是先要保证case 的稳定，然后在稳定的基础上在考虑速度

```python
# 隐式等待
driver.implicitly_wait(5)
# 显示等待
WebDriverWait(driver,10).until(expected_conditions.element_to_be_clickable(ele))
# 强制等待
time.sleep(5)
```

2.就是单纯的没有定位到：

这个可能要看看当时的报错信息，看看为啥有时候能定位到有时候定位不到

看一下是因为元素的属性会变、还是说页面的动态加载的（一开始加载出来和最后呈现效果的位置不是同一个位置）、或者是元素在iframe里、或者是新打开的页面、或者是在一个alert窗口中。如果是属性会变的话，那就换个定位方式，换成相对定位；如果是动态加载的原因可以加入等待机制；如果是iframe或者窗口的原因，那就使用driver.swith_to切换进去后在进行操作,窗口的话使用driver.window_handles获取句柄

3.分布式运行时由于case之间相互影响导致的：

比如A case执行完之后，页面的信息发生了变化，B case就会出错，这是自动化方案设计的问题，梳理一下case的依赖关系，尽量借耦合

### 元素操作方法：

click、send_keys、clear、submit、is_enabled、is_displayed、is_selected、text、id、size、parent、location、tag_name、get_attribute、get_property、screenshot

从名称上也能猜出来这些方法都是干什么用的，这里就不赘述了

#### 特殊控件的操作方法

1.iframe：使用dirver.swith_to.frame('frame_id')切换iframe

2.alter：使用dirver.swith_to.alert()切换到alert中，然后accept确认、dismiss取消、send_keys输入、text获取内容

3.下拉框：这个要看下拉框的代码是怎么写的，可以使用ActionChains这个包里的鼠标的一些方法，或者是直接选择有时候也能选取到

4.上传文件：send_keys('文件路径')

5.时间控件：用js先去掉readonly属性，再send_keys

```python
driver.execute_script('arguments[0].removeAttribute(\"readonly\")', 要移除readonly的元素)
```

6.乱序密码键盘：先去掉readonly属性，再send_keys

如果一些普通方法没有效果，可以祭出大招：**driver.execute_script('任意js代码')**

#### 处理cookie的方法

方法1：把登陆case设置为setup module级别，使用pytest的fixture功能

方法2：先登陆driver.get_cookies()记录好cookie，然后再初始化dirver时候带上这个cookie：driver.add_cookies(cookie)

方法3：先登陆，然后复用打开的这个浏览器

#### 特别的方法

ActionChains库里有一些鼠标的动作，不具体举例了图片奉上

ps：appium有个类似的，叫TouchAction

分享一个记忆方法：一般web页面都是鼠标的动作链，所以叫ActionChain，而App端都是触摸式的，就是TouchAciton

<img src="selenium.assets/image-20201208011023312.png" alt="image-20201208011023312" style="zoom:50%;" />

excepted_conditions：和显示等待配合使用

<img src="selenium.assets/image-20201208011505149.png" alt="image-20201208011505149" style="zoom:50%;" />

## appium



### 原理



### 常用元素定位方法



**引申点**：对于原生app端，还有一种特殊的accessibility_id，这个一般都会有，而且ios和android通用，这个也很好用

所以：**没有最好的定位方式只有最适合的定位方式**

这里可以把很多人吐槽UI自动化测试不稳定的问题拿出来说说了：

UI测试不稳定的原因其实就一个：元素有时候定位不到

这个问题的可能原因就有很多：

1.网络原因

这个是最好解决的：合理使用隐式等待、显示等待、强制等待

那怎么个合理使用呢？

可以先大概统计一下你们的页面的平均打开时间是多长时间，根据这个时间设置全局隐式等待时间，然后在一些动态加载资源的页面、或者是什么前提条件的逻辑的时候加显示等待（只有达到什么条件才继续执行），这样2个等待配合一般就能解决了网络原因了，如果还是不行，那就加强制等待，但是这种情况也不多，加一两个对整个流程的速度也影响不大，这里要提醒：我们是先要保证case 的稳定，然后在稳定的基础上在考虑速度

2.就是单纯的没有定位到：

这个可能要看看当时的报错信息，看看为啥有时候能定位到有时候定位不到，是因为元素的属性会变、还是说页面的动态加载的（一开始加载出来和最后呈现效果的位置不是同一个位置）、如果是属性会变的话，那就换个定位方式，换成相对定位，如果是动态加载的原因可以加入等待机制

3.弹窗（主要体现在app端自动化上）

可以写一个黑名单方法（里面定义了很多弹框元素，比如包含内容为【同意】的空间，以及对应的操作，比如点击【同意】，这样就把这类的弹框取消掉了，然后定一个最多扫描的次数，比如黑名单扫描了3次以后就退出，不论有没有操作黑名单空间，超过3次就退出），try-except一下，定位不到元素的时候就调用黑名单方法，通过黑名单方法操作弹框空间，然后要合理设置每次case的setup方法，app启动时候的模式需要设置好，不需要每次case都要重装、重启、清除缓存这样子

**引申点**：下面列几个相关的caps设置（能看出来就是skip各种）：skipDevicesInitialization、skipServerInstallation、skipUnlock、skipLogcatCapture、dontStopAPPOnReset

ps：上面虽然web端和移动端有点混，但是原理是相同的，而且appium的底层就是selenium，所以一通百通

#### 特殊控件的操作方法

1.webview

对于app来说，只有一个webview我认为是特殊控件，其他的都直接定位就行，对于webview来说困难的是如何获取元素定位，只要能知道元素定位，那直接使用selenium那一套就行了，但是难点就在如何定位上，一下几点供参考：

1）最常用的一种方法：让开发打开webview开关，这样就能直接在+UIAutomator之类的工具里定位了

2）直接在chrome中访问webview对应的url，使用chrome 开发者工具定位即可

那如何获取这个url，一般自己公司的产品的话直接问开发就行，或者一般的webview都有分享的功能，分享一下看看url，最后实在不行就用adb logcat | grep抓一下

3）使用chrome 的inspect工具，但是这个工具得翻墙，还得手机的浏览器内核是chrome，如果能翻的话，那还是挺好用的，和Chrome的F12效果一样，只不过不需要抓url，只需要手机打开webview页面，adb devices能查到设备，直接在inspect中就可以看了

4）使用android6.0版本的模拟器+UIAutomator之类的工具，这是因为android有bug才能这样，其他的android版本是不行的

ps：原生和webview之间切换：

```python
# 获取页面所有的contexts对象，和selenium的句柄一样，返回一个list
contexts = driver.contexts
# 切换到webview
driver.switch_to.context(contexts[1])
# 也可以这样切
# driver.switch_to.context('WEBVIEW_com.tencent.mm:tools')
# 切回native
driver.switch_to.context(contexts[0])
# 这样也是可以的切回原生页面
# driver.switch_to.context("NATIVE_APP") 
```

2.toast，这种toast都是系统发出的，且页面一般只有一个toast（这个要和弹框分开，一些什么升级、定位、权限的那种弹框不是toast）

```python
driver.find_element(By.XPATH,'//*[@class="android.widget.Toast"]')
driver.find_element(By.XPATH,'//*[contains(@text,"点赞成功")]')
```



### 元素方法





### 特别的方法

TouchAction

### ios和android的区别



## 分布式部署运行

## 面试题（推荐）：

https://www.cnblogs.com/www-qcdwx-com/p/11520878.html

hidden或者是display ＝ none的元素如何定位到？

定位是可以定位到的，可以find到，可以打印出元素的属性，只是不能操作，比如send_keys、click会报错【ElementNotInteractableException】，如果时要操作的话，使用js直接操作就行：

```python
driver.execute_script("document.getElementById('baidu').click()")
```



# 提高篇

### PO模式

### 如何搭建UI自动化框架

本质就是封装：封装定位、封装动作、封装校验

封装元素定位：【find_element+黑名单+等待】封装为一个查找元素的方法

一些业务场景中长动作进行封装：比如下滑页面到某个元素进行点击，可以封装成一个动作