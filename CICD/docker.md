容器

`docker run -d --name influxdb --network grafana -p 8086:8086 -v ${pwd}:/var/lib/influxdb/ influxdb`



docker ps

docker start ***

docker exec -it *** bash（进入容器）

docker pull *** （pull下来镜像）

不同的镜像文件系统是怎么隔离的？——Linux的namespace机制

docker run -it -p 3306:3306 -e **传参**





pstree -p 1 

ps -ef



面试题：

https://zhuanlan.zhihu.com/p/571931032



# docker镜像构建必须要dockerfile吗？

不一定，可以先把一个容器export成一个tar文件，然后import这个tar也可以

然后可以直接把容器创建为一个镜像docker commit

但是都不建议，这样的方式没法追溯这个容器构建的步骤和过程，不好复用以及后期出现问题不好查哪一步的问题，就是一点【不透明】，我们的dockerfile就很清晰透明

