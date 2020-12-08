1.jmeter可以设置代理，录制

<img src="../Untitled.assets/image-20201130145508603.png" alt="image-20201130145508603" style="zoom:33%;" />![image-20201130145541850](../Untitled.assets/image-20201130145541850.png)

![image-20201130145541850](../Untitled.assets/image-20201130145541850.png)

2.Jmeter监控情况

输入命令：`jconsole`

<img src="../Untitled.assets/image-20201130145807038.png" alt="image-20201130145807038" style="zoom:33%;" />

![image-20201130145827893](../Untitled.assets/image-20201130145827893.png)

![image-20201130145842864](../Untitled.assets/image-20201130145842864.png)

![image-20201130150003600](../Untitled.assets/image-20201130150003600.png)

![image-20201130150234959](../Untitled.assets/image-20201130150234959.png)

修改之后重启jmeter、jconsole

![image-20201130150347751](../Untitled.assets/image-20201130150347751.png)



3.[用户定义的变量]里的变量是全局变量



4.jmeter plugin manage

在https://jmeter-plugins.org/install/Install/下载jra包

<img src="Jmeter%E5%B0%8F%E5%B7%A5%E5%85%B7.assets/image-20201202150854493.png" alt="image-20201202150854493" style="zoom:33%;" />![image-20201202151041597](Jmeter%E5%B0%8F%E5%B7%A5%E5%85%B7.assets/image-20201202151041597.png)

然后把jar包放到jmeter的lib/ext目录下，之后重启jmeter，就有这个插件了

<img src="Jmeter%E5%B0%8F%E5%B7%A5%E5%85%B7.assets/image-20201202151438935.png" alt="image-20201202151438935" style="zoom:33%;" />

什么时候需要事务控制元件？

一套的操作具有原子性，类似于数据库的【事务】的概念