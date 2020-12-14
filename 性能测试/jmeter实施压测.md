# 压测准备

- 环境选择测试环境 or 线上环境

  1. 项目未上线——测试环境

  2. 有专门的性能测试环境——测试环静

     引申：如何搭建性能测试环境？

  3. 线上影响的系统比较多、影响的功能多——测试环境

  4. 真实还原线上现场，比如淘宝双11，人家就就是线上压的

  5. 线上：**多节点的集群，压单节点的集群，看单节点的性能如何，然后根据单节点的性能推测整体的性能**

- 如果有限制怎么办？比如用户登陆次数、验证码之类

  1. 准备足够做的用户，CSV文件导入

  2. 使用万能用户、万能验证码

  3. 关闭用户验证、验证码的开关

     ps：本来用户次数限制和验证码其中一个功能就是防止用户操作过快的，所以关闭了没有问题，开启后的性能指标肯定比关闭后的性能指标要好，所以关闭后性能指标都能达标的话那开启后肯定更美问题



# 脚本准备



一般都先在UI界面中调试好脚本，然后在命令行运行

```bash
$ docker network create grafana
$ docker run -d --name influxdb --network grafana -p 8086:8086 -v ${pwd}:/var/lib/influxdb/ influxdb
$ docker ps
$ docker exec -it influxdb influx
> show databases;
> create database jmeter;
$ docker run -d --name grafana --network grafana -p 3000:3000 grafana/grafana
```





http://localhost:3000/login

账户：admin/admin

<img src="jmeter%E5%AE%9E%E6%96%BD%E5%8E%8B%E6%B5%8B.assets/image-20201130182817103.png" alt="image-20201130182817103" style="zoom:33%;" />

https://grafana.com/grafana/dashboards/5496





代码：

https://github.com/princeqjzh/iJmeter



静默运行：

```bash
# 在 jmx 文件所在路径执行：
nohup /jmeter所在路径/bin/jmeter -n -t orderservice.jmx -l test.jtl
# 生成报告：
/jmeter所在路径/bin/jmeter -g test.jtl -e -o web_report
```



关键tips：

替换脚本的并发数:

MacOs：

```bash
$ sed -Ei "" "s#num_threads\"\>([0-9]*)\<\/stringProp#num_threads\"\>${num}\<\/stringProp#g" templete_order_auto.jmx
```

先摸底：看一下峰值

