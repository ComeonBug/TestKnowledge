# CI/CD的具体步骤：

先自己打包——》CI阶段：生成镜像——〉CD阶段：部署到环境，自动化测试，或者发布

然后我们把整个代码的交付的每个步骤划分一下：

打包阶段——【生成部署镜像——这个就是CI过程】——【部署环境——这个是CD阶段】

## 打包

java的话，maven打包，pyhton的话就不需要这一步了

## CI

生成部署镜像：docker image、push到docker 仓库（CI阶段）

​	这个部署镜像做的事情是：构建运行环境、clone代码（java不clone代码，java把war包打进去，python类的脚脚本语言把代码clone进去）

​	这个镜像FROM基础镜像就行

​	只是构建运行环境，不进行代码的拉取、运行

​	这样的目的是：**一次打包处处运行**

再次强调：CI做的最好的就是一次打包处处运行

把配置文件（服务器信息、数据库信息等）拆出来，在发布到prod的时候进行注入

这个时候开发的单元测试代码会运行

## CD

部署代码（CD阶段）：

​	第二步CI只是完成一个docker image 的生成，但是具体要发布到哪个环境、发布什么应用、发布什么分支、发布完成之后执行自动化测试代码 是部署的时候就是CD的阶段来做的

要解决这三个问题，CD 的jenkins job，起码要拥有以下三个属性：
1：应用名字

建CD job的时候，就已经知道应用名字了，把这个当作一个参数写死在cd job里，就相当于你获取了应用名字信息，这样就解决了

2：部署环境

这里的环境不是指代码的运行环境，而是只比如服务器环境、数据库环境等底层环境，因为这个我们是不在镜像里打包进去的

蔚来的做法是，也把它写到job名字里，然后固化到cd job的参数里，几个环境就建几个CD jenkins job

3：代码版本（docker image）

每次手动填写docker images的版本号（因为每次CI生成的docker iamge版本不一样，比如这个是V1.0版本，下一次发布的时候的版本是V1.1，总之每次发布的docker image版本不一样）

命令比如：CI_Box_website、CD_Box_website_dev

说明：

CI是一个jenkins job，CD是一个jenkins job

CI就是为了解决CD的代码版本、运行环境问题的

如果这时候需要CI完成后自动进行CD，可以在CI的job中，在docker iamge生成的这个步骤之后调CD jenkins job的build接口，调用这个接口要把docker image的版本号一起传过去，这样就能自动构建CD了

ps：调jenkins build一个类似下面的这样的一个接口

```bash
POST http://localhost:8080/job/demo/build，要求表单格式（application/x-www-form-urlencoded），请求消息体有特殊格式要求
以 name+value 键值对集合作为请求参数，再进行序列化，形如 {"parameter":[{"name":"branch","value":"test"}]}
将请求参数转义，以表单格式（application/x-www-form-urlencoded）发送，键为固定值 json。

$ curl -i http://localhost:8080/job/rdc-pipline/build -H 'Authorization: Basic YWRtaW46MTFiNDY0NGYwMDRkYWM3MWU0YjI4ODMyNmQwZWUwNmFhMw==' -d "json=%7B%22parameter%22%3A%5B%7B%22name%22%3A%22branch%22%2C%22value%22%3A%22test%22%7D%5D%7D"
HTTP/1.1 302 Found
Date: Wed, 14 Oct 2020 09:23:36 GMT
X-Content-Type-Options: nosniff
Location: http://localhost:8080/job/rdc-pipline/
Content-Length: 0
Server: Jetty(9.4.30.v20200611)
```

这个时候调用的当然要么是dev环境的CD 接口要么是qa环境的CD 接口

当然开发同学可以先构建dev环境的CD，这样可以自己先在dev环境测试，自测通过以后就通知QA提测，然后测试同学构建qa环境的jenkins CD job，这时候的qa 环境的jenkins CD job里在部署代码以后就可以调用自动化测试的jenkins 的接口来进行自动化测试，当然调自动化测试的jenkins的时候要传递环境的参数过去，这样自动化测试才知道到底跑哪个环境

其实这个过程中，自动化测试还是有一点点的滞后，如果能在dev CD阶段介入的话，就能让开发在自测的时候就发现一些问题，而不必要等到提测的时候了，这时候我们可能要更好的规划的一下我们的自动化测试case的运行，比如dev环境冒烟的时候运行哪些case，qa环境冒烟的时候运行哪些case。而且由于dev环境的数据库连接的时候dev环境的，所以一些数据上准备可能需要进行一些处理，比如在case总都有准备数据的setup case，这样的话就能避免因为数据的问题导致无法跑通



下图为网络上一个一张图，写的CICD过程已经挺清晰了，贴到这里可以参考一下

![image-20201208111243262](CICD.assets/image-20201208111243262.png)