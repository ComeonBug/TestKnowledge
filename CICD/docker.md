`docker run -d --name influxdb --network grafana -p 8086:8086 -v ${pwd}:/var/lib/influxdb/ influxdb`





docker ps

docker start ***

docker exec -it *** bash（进入容器）

docker pull *** （pull下来镜像）

不同的镜像文件系统是怎么隔离的？——Linux的namespace机制

docker run -it -p 3306:3306 -e **传参**





pstree -p 1 

ps -ef