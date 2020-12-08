monkey：最简单的移动端压力测试工具，只能实现选择哪个包、动作的比例，但是无法控制操作范围，导致有时候会操作到顶部菜单栏或者底部的菜单，操作没有逻辑可言，精准度太低，很难能跑出什么问题

简单举例：

```bash
$ adb shell monkey -p com.xueqiu.android --throttle 200 --pct-motion 80 --pct-touch 20 -s 100 -vvv 5000
```

maxim：是基于monkey的一个压力测试工具，它补充了monkey的一些短板，比如点到菜单栏、没有遍历策略

简单举例：

```bash
$ adb shell CLASSPATH=/sdcard/monkey.jar:/sdcard/framework.jar exec app_process /system/bin tv.panda.test.monkey.Monkey -p com.panda.videoliveplatform --uiautomatormix --running-minutes 60 -v -v
```

appCrawler：是基于appium的自动遍历工具，把当前app页面dump为xml结构，然后获取待遍历元素（我们自己在配置文件中写的），在selectedList中找到要遍历的范围（没写就默认是全部），然后过滤掉黑名单（我们自己可以定义）、小控件、不可见控件等，再根据我们写的firstList、LastList中的元素重写排序（没写就默认正序）、然后再跳过已点击、配置了点击次数taglimit的控件，在根据匹配的规则执行action，然后每个activity执行上面的步骤——》所以appCrawler比较慢，但是人家毕竟可以深层次、自动的、可以根据你的个性化配置的方式来遍历的，所以，牺牲了速度但是提高的有效性，还是可以的

git地址：https://github.com/seveniruby/AppCrawler

执行命令为：

```bash
$ java -jar appcrawler.jar -c conf/xueqiu.yaml -a xueqiu.apk
```

配置文件为：

```bash
$ cat xueqiu.yml
--
pluginList: []
saveScreen: true
reportTitle: ""
resultDir: "20201120203504"
waitLoading: 500
waitLaunch: 6000
showCancel: true
maxTime: 10800
maxDepth: 10
capability:
  noReset: "true"
  fullReset: "false"
  appium: "http://127.0.0.1:4723/wd/hub"
  appPackage: "com.xueqiu.android"
  appActivity: ".main.view.MainActivity"
testcase:
  name: "TesterHome AppCrawler"
  steps:
  - given: []
    when: null
    then: []
    xpath: "/*"
    action: "Thread.sleep(5000)"
    actions: []
    times: 0
selectedList:
- given: []
  when: null
  then: []
  xpath: "//*[contains(name(), 'Button')]"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[contains(name(), 'Text') and @clickable='true' and string-length(@text)<10]"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[@clickable='true']/*[contains(name(), 'Text') and string-length(@text)<10]"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[contains(name(), 'Image') and @clickable='true']"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[@clickable='true']/*[contains(name(), 'Image')]"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[contains(name(), 'Image') and @name!='']"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[contains(name(), 'Text') and @name!='' and string-length(@label)<10]"
  action: null
  actions: []
  times: 0
firstList: []
lastList:
- given: []
  when: null
  then: []
  xpath: "//*[@selected='true']/..//*"
  action: null
  actions: []
  times: 0
- given: []
  when: null
  then: []
  xpath: "//*[@selected='true']/../..//*"
  action: null
  actions: []
  times: 0
backButton:
- given: []
  when: null
  then: []
  xpath: "Navigate up"
  action: null
  actions: []
  times: 0
triggerActions:
- given: []
  when: null
  then: []
  xpath: "share_comment_guide_btn"
  action: null
  actions: []
  times: 0
xpathAttributes:
- "name"
- "label"
- "value"
- "resource-id"
- "content-desc"
- "instance"
- "text"
sortByAttribute:
- "depth"
- "list"
- "selected"
findBy: "default"
defineUrl: []
baseUrl: []
appWhiteList: []
urlBlackList: []
urlWhiteList: []
blackList:
- given: []
  when: null
  then: []
  xpath: ".*[0-9]{2}.*"
  action: null
  actions: []
  times: 0
beforeRestart: []
beforeElement:
- given: []
  when: null
  then: []
  xpath: "/*"
  action: "Thread.sleep(500)"
  actions: []
  times: 0
afterElement: []
afterPage: []
afterPageMax: 2
tagLimitMax: 2
tagLimit:
- given: []
  when: null
  then: []
  xpath: "确定"
  action: null
  actions: []
  times: 1000
- given: []
  when: null
  then: []
  xpath: "取消"
  action: null
  actions: []
  times: 1000
- given: []
  when: null
  then: []
  xpath: "share_comment_guide_btn_name"
  action: null
  actions: []
  times: 1000
assertGlobal: []
```



也给我提供了一个点子：如果这个yaml文件玩的熟的话，那直接就可以做app端的UI自动化了，还省了自己的写case的时间，只不过可能是这个用来遍历的话每次的操作seed不一样，但是一个冒烟、探索性测试、自动遍历还是很不错的

