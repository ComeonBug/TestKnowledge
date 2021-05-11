jmeter基于java的



# 关于性能测试

对性能指标的测试

功能测试关注能不能用，性能测试关注好不好用

开始点：功能测试基本完成之后

关注指标：接口响应时间、吞吐量、TPS（每秒处理的事务数）

性能测试的期望值数据从哪里来？

​	如果上线前，参考竞品的数据

​	上线后，运营数据



Jmeter：

下载一个zip包直接解压即可



可以录制：Thread Group——》Recording Controller

   配合：HTTP(S) Test Script Recorder（这个就是开启代理）

录制时想要开启浏览器的代理功能

view result tree：看结果

响应断言：Assertion——》Response Assertion

Aggregate Report：聚合报告

​	samples：实际产生的样本数

​	avage：所有的响应时间的平局之（会受到极端值的影响）

​	media：中位数（也就是50%line，较为客观），从小到大排序，取中间的那个值

​	99%line：，从小到大排序，取中间的第99%位置的那个值





Jmeter静默压测

脱离UI进行压测

先配一个jmeter_path，这样就能直接输入jmeter这个命令压测了

静默压测命令：

$jmeter -n -t xxx.jmx -l result.jtl

result.jtl	其实就是聚合报告和结果树





jmeter 

Debug sampler：一般只打开Jmeter vriables





Post Processors——》JSON Extractor 来提取json中的值，匹配方式【$.】代表根目录

ps：可以在View result tree中验证

Jmeter中获取变量值的方法：【${变量名}】



全局参数设置：

Config element——》User Define Variables



CSV数据导入：

Config Element——》CSV Data Set Config



建议：变量设置（全局变量、http header、csv等）类的sampler放到线程组的最前面、debug sampler放最后





压测的时候：

1.发压机和被压测机器不要是一台机器

2.发压机和被压测机器在同一网段

3.使用静默压测:

jmeter -n -t $jmx_file -l $jtl_file



4.禁用debug sampler





压测报告中统计的数据：

并发数、90%line、95%line、Error率



错误率很高的时候thoughoutput也是可能会很高的，因为Jmeter统计TPS的时候是不分这个请求的对错的

TPS：每秒处理的事务｜请求数



自动化压测的流程图：

1.UI界面调case——》生成jmx文件

2.修改jms中的变量

3.自动化的shell脚本

4.执行脚本

5.导入jtl查看报告｜监控实时数据





JDBC请求压测：

1.JDBC Connection Configuration

要填的几个地方：

1）pool：jdbc request的时候会用到

2）Validation Query选select 1即可

3）Database URL：你要链接的db的url

4）JDBC Driver class：com.mysql.jdbc.Driver

5）Username：链接DB的用户名

6）password：链接db的密码

![image-20210512010252608](jmeter%E5%9F%BA%E7%A1%80.assets/image-20210512010252608.png)

