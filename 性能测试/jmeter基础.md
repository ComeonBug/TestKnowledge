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

下载一个source 的zip包直接解压即可（ps：启动的时候需要系统有java环境）



http://www.jmeter.com.cn/





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



文件上传的接口：







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



2.JDBC Request

1）配置参数区：

pool写JDBC Connection Configuration中配置的pool名

2）sql语句区：

insert ***

3）变量配置区

sql中如果有?的变量，就在这里配置

变量的值用逗号隔开

变量的类型

![image-20210512011746752](jmeter%E5%9F%BA%E7%A1%80.assets/image-20210512011746752.png)

<<<<<<< HEAD
选query type为：select statement

可以提取变量为count

后续如果在其他的sample中要使用的话，就可以用【${count_1}】取第一个，【${count_2}】取第二个，这样的方式来取

![](jmeter%E5%9F%BA%E7%A1%80.assets/image-20210512013054575.png)

delete语句：

使用的Query Type为：update statement

sql例如：delete from `jmeter_class`.`user` where `username`=testsuer





Jmeter的线程组：

1.取样器结束后要做什么动作

2.线程属性

线程数：就是要模拟的用户数

每秒启动的线程数

3.调度配置

![image-20210512231915940](jmeter%E5%9F%BA%E7%A1%80.assets/image-20210512231915940.png)



setup线程组、teardown线程组





jmx本质上其实是配置文件，不是程序代码

Java Request是什么？

1.纯java程序，实现了Jmeter中提供的接口——JavaSamplerClient

2.将java程序集成到JMeter中，通过Java Request实现调度

3.java程序实现与压测‘目标’的交互

4.Jmeter来控制java程序的生命周期、并发调度、收集结果报告等处理



Java request实现知识点：

1.java request执行类必须继承AbstractJavaSamplerClient抽象类

2.各种post、get、delete请求需要使用java的HTTPClient来实现

3.接口请求之间的交互在java代码中来控制

4.HttpResponse中的json返回值解析使用的java中的json解析库来进行解析——》java jars下、jmeter ext下

5.除来最终的运行和数据交互需要进入jmeter，基本上已经俨然变成了java程序开发活动



java编辑器：Intellij Idea

打包工具：Maven

jar包存放的位置；《jmeter path》/lib/ext



java request环境配置：

1.jdk1.8

2.maven

3.export jmeter_path=**

4.Intellij Idea





其实，java request是提供了一个思路：Jmeter提供了接口，其他程序继承、实现、调用 接口
=======






Csv loader是在初始化的时候就加载到内存里的，jmeter的内存是有限的

实际场景中，Jemter做压测的时候很少加复杂的as

sert校验，复杂的assert很影响性能



Jmeter原理介绍

实际上所有界面的配置都会存在一个jmx文件中





Jmeter最重要的几个配置文件：jmeter.properties（这个用的最多，里面是jmeter的一些默认值）、user.properties（用户自定义的一些，如果重复，就替换jmeter.properties）、system.properties





jmeter分布式：rmi方式

具体的配置在user.properties里



-Jmeter代码里其实也有setupXXX、teardownXXX之类功能的方法



Lesson4-01

jmeter核心配置元件

Jmeter运行机制

Jmeter时序关系

Jmeter核心模块
>>>>>>> 32527d3857fd1763c7b035a60c2ef068f210b582
