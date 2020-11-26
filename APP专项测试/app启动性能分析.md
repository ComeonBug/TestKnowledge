

分析app启动的时间：

方法1——adb + logcat

`package=com.xueqiu.android`

`adb shell pm clear $package`

`adb shell am force-stop $package`

`adb shell am start -S -W $package/.view.WelcomeActivityAlias`

`adb logcat | grep -i displayed`

方法2——拆帧：adb录屏 + ffmpeg

`adb shell screenrecord --bugreport --time-limit 30 /data/local/tmp/weilai.mp4`

`adb pull /data/local/tmp/weilai.mp4 .`

`ffmpeg -i weilai.mp4 -r 10 frames_%03d.jpg`



ps:Mac上直接用brew安装ffmpeg就行：

`brew install ffmpeg`



分析webview加载一个新页面的启动过程：

https://www.w3.org/TR/navigation-timing/

参考：[app专项](/Users/liyang/TestingKnowledge/APP专项测试/app专项（全）.md)

```python
from selenium import webdriver


class TestWebTiming:
    def test_web_timing(self):
        driver = webdriver.Chrome()
        driver.get('https://www.nio.cn/')
        web_timing = driver.execute_script('return JSON.stringify(window.performance.timing)')
        # 返回的web_timing是一个字符串类型
        print(type(web_timing))
```

也可以直接在浏览器console控制台输入：

```js
window.performance.timing
window.performance.timing.domComplete-window.performance.timing.domLoading
```

