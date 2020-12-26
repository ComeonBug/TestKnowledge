如何把自动化测试和jenkins结合起来？

1.测试也要建一个jenkins item

​	测试的这个jenkins任务是要干什么呢？

​	定时跑？全量跑case？跑完发邮件？

2.肯定有一个代码构建的jenkins任务，然后在这个代码构建的任务中增加



我觉得：

只要把自动化脚本所在的git地址有了，然后在jenkins的execute shell里加入pytest的运行、allure报告的生成命令就行

再结合jenkins的邮件插件，到时候把结果发出来

ps:jenkins不能放在docker里



jenkins怎么和docker结合？

jenkins启动一个容器

这个容器里有环境、clone代码、运行脚本



首先，开发代码是开发代码，这一部分是独立的，测试代码是测试代码，这部分不能一起合并到主分支上，要保证线上业务代码的纯净

而且，对于开发和测试代码的配置、环境是不一样的，不能放一起

所以：测试代码放在独立的代码仓库，有独立的jenkins管理

那只需要在开发的jenkins构建时也触发测试的jenkins构建就行，使用这个【Parameterized Trigger plugin】插件，设置build触发器：比如【push events】的时候触发

那测试的jenkins如何设置？

1.关联git项目仓库-代码分支

2.设置build触发器：比如【push events】的时候触发

3.设置build脚本：

git clone ***

pytest **  或者 sh ***.sh

4.设置email

5.gitlab设置中配置好jenkins相关的webhooks的触发



那上面的步骤如何和docker结合呢？

在execute shell中即可，放入dockerfile的执行脚本，启动2个docker，一个编译docker，一个执行docker

这2个docker里包含了代码+所需的运行的环境

这样其实，测试也可以自己搞一个docker镜像，只要提供给devops这个镜像，放到开发的build execute shell中就行，都不需要单独写脚本





python-docker基础镜像：

dockerfile

```bash
FROM ubuntu:16.04
MAINTAINER cc-man
#添加python的安装包
ADD  Python-3.5.0.tar.xz /opt 
#更新apt
RUN  apt-get update && apt-get install -y 
#安装依赖
RUN  apt-get install gcc -y && apt-get install make -y \
		&& apt-get install vim -y && apt-get install openssl -y \
		&& apt-get install libssl-dev -y && apt-get install python3-pip -y
RUN  ./opt/Python-3.5.0/configure --prefix=/usr/local/python3.5 \
		&& make && make install
RUN mkdir /opt/myApp/
VOLUME ["/opt/myApp/"]
CMD [""]
```







问个问题，我们一般的CI/CD是不是可以简化为下面的这个流程呀：

CI：java的话 jar 打一个镜像

run容器的时候尽量比较轻量

maven打包不要放到镜像里，



测试过的包直接发布到prod

一个包，这个包没有环境区分的？

host、数据库

打包的时候不能把配置文件打包进去的，不然镜像就没法放到其他环境了

配置要单独放进CD中





开发代码的编译是一个镜像、开发代码的执行是一个docker，QA自动化测试代码是一个docker
然后我们的jenkins的话可能有多个，比如开发代码构建的jenkins、自动化测试代码构建的jenkins，
然后开发代码构建的jenkins里可能需要
1）安装git插件，在比如git push的时候出发jenkins构建
2）jenkins构建脚本中启动对应的docker服务（代码编译的docker、代码执行的docker）
3）需要关联自动化测试的jenkins





CI/CD

CI的参数：名字、地址、分支、

打包、部署

配置文件的管理、怎么发布配置文件





**一处打包，处处运行**

配置文件是拎出来





CD之后测试如何介入的？调接口执行脚本、命令行直接执行脚本

jenkins job是怎么搭的：



docker image版本

可以发布了

开发给我docker image，开发才知道打包哪个分支

docker image给测试，测试发布到测试环境，发布完要自动化测试

触发我自动化job jenkins 的触发



核心概念：在哪里切入发布之后切入进行，发布之后掉我的自动化接口就行





打tag的目的：目的是：为了能回滚

项目版本管理的方式





各种环节下要干什么，为什么干？









