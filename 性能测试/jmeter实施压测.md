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