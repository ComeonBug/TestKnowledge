adb原理：

adb也是分为3部分：adb client、adb server、adb daemon

adb client是指我们电脑端，就是输入adb命令的地方，这些adb命令发起TCP请求，请求5037端口，请求发送到adb server端

adb server端也是在我们的电脑上运行的，只不过实在后台运行我们看不到而已，server端接受到client端的请求以后，管理client和daemon之间的通信

adb daemon简称adbd，是在设备上后台运行的守护进程，这些进程来运行指令

client《—tcp 5037—》server《—tcp ｜ usb —》daemon—》设备

常用命令：

```bash
$ adb kill-server
$ adb start-server
$ adb devices
$ adb shell pm list packages | grep xueqiu
$ adb install com.xueqiu.android
$ adb uninstall com.xueqiu.android
$ adb logcat | grep xueqiu
$ adb shell pm clear com.xueqiu.android
$ adb logcat | grep display
$ adb shell am start com.xueqiu.android/.view.WelcomeActivityAlias
$ adb shell ifconfig
$ adb shell cat /proc/cpuinfo
$ adb shell cat /proc/meminfo
$ adb push log.txt /sdcard/
# 导出ANR日志
$ adb pull /data/anr/traces.txt .
$ adb shell dumpsys battrty
# 显示这个包的所有的信息
$ adb shell pm dump com.xueqiu.android
# 获取当前窗口的activity
$ adb shell dumpsys window | grep mCurrent
$ adb shell dumpsys dropbox | grep 'data_app_crash'
$ adb logcat -d *:E 'com.xueqiu.android'
$ adb shell top
$ adb shell vmstat
# 查看有几个唤醒锁，Wake Locks：0代表是健康的，用来做耗电量测试的一个维度
$ adb shell dumpsys power | grep Locks
# 录屏30s，存储在/data/local/tmp/xueqiu.mp4
$ adb shell screenrecord --bugreport --time-limit 30 /data/local/tmp/xueqiu.mp4
$ adb pull /data/local/tmp/xueqiu.mp4 .
# 配套使用的拆帧命令,10毫秒拆一帧，命名规则为frame_数字.jpg
$ ffmpeg -i xueqiu.mp4 -r 10 frame_%03d.jpg
$ adb shell monkey -p com.xueqiu.android --throttle 200 --pct-motion 80 --pct-majornav 20 -s 100 -vv 150
```



由上面的adb shell部分的命令可以看出来，android的底层linux，linux的命令在android上同样适用

简单说明一下monkey：monkye可以支持每个动作的延迟时间、动作的比率、多少次动作、操作哪个包、打印日志的级别