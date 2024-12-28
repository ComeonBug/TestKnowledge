# python进程、线程、协程的区别



# python进程之间的通信方法



# python进程、线程的原理



# 工作中用到的测试方法以及具体例子

正交表法的具体用处

我当时回答的是在搜索的时候，不同的条件用正交表



# 接口测试如何做？

首先必须要有清晰的接口文档

1、正向用例

正向用例有一点需要注意，就是接口的实际效果需要关注，就是比如一个增加信息的接口，不能光看这个接口返回了200，返回的code里也有一些id什么的东西，需要实际的去库里确认，真的增加了，以及返回的数据也确实对

正向参数类型、非必填参数传部分｜组合｜全部、验证返回的结果无误

2、异常用例

异常用例关注请求的异常要和返回的code匹配，以及真的没有作用于系统，比如增加信息的接口返回异常了，去数据库查一下这个数据真的没有增加才算case通过

异常参数类型、参数个数有误、不传必填参数、重复传参、服务异常返回的code无误、一些接口的参数有固定组合不遵守组合规则、

3、安全性校验

权限校验（访问不该访问到的资源接口、直接url访问没有加权限校验）、密码｜key敏感信息明文、token没有过期机制、session和token欺骗（session和token不匹配是否能校验通过）、url的sql注入、DDOS攻击（疯狂发错误的请求，就是让服务停止的攻击方式）

4、接口性能测试

确保接口的响应在预期范围内



# dockerfile里ADD、COPY、WORKDIR的区别

首先，他们都是dockerfile里的命令

我们99%情况都是用dockerfile构建build容器的配置文件，剩余一种是（commit、export+import，不够透明，一千个不提倡）



add：添加本地文件、目录、远程文件（url可访问到的文件）、jar 这些文件到镜像里，添加到的目录就是根目录就是workdir设置的，后面第一个参数是要add的路径，可以一个或者多个，第二个参数为镜像里的路径，可以是workdir的相对路径，也可以是符合workdir设置的绝对路径

copy：只能复制本地文件到镜像里，后面第一个参数是要copy的文件的路径，第二个参数为镜像里的路径，可以是workdir的相对路径，也可以是符合workdir设置的绝对路径

workdir：为Dockerfile设置工作目录，后续dockerfile里所有的命令都在这个目录下运行，必须是绝对路径，比如上面的add和copy的文件都会已这个目录为根目录操作



如果是复制文件到镜像里，建议用copy，因为add可以通过访问url的方式，不确定下载的内容，不够透明，如果一定想要从远程获取文件，考虑使用【RUN + wget】、【RUN + curl】方式



举例，下面是一个dockerfile文件

```shell
# 使用官方的 Python 3.7 镜像作为基础镜像
FROM python:3.7

# 设置环境变量，确保 Python 输出直接打印到控制台，不会被缓存
ENV PYTHONUNBUFFERED 1
ENV MYPATH /usr/local
# 设置工作目录为 /usr/local，那我们以后--it进入容器后默认就在这个目录下了，dockerfile后面所有的文件操作都是相对于这个目录的，这里的workdir后面的路径必须是绝对路径
WORKDIR $MYPATH

# COPY将当前目录下的所有文件复制到工作目录 /usr/local 中
COPY readme.txt /usr/local/readme.txt
# 也可以写出
# COPY readme.txt .

# ADD将当前目录下的文件复制到工作目录 /usr/local 中
ADD jdk-17_linux-x64_bin.tar.gz /usr/local
ADD apache-tomcat-9.0.54.tar.gz /usr/local

# 使用 pip 安装依赖，requirements.txt 包含所需的 Python 包
RUN pip install --no-cache-dir -r requirements.txt

# 容器运行时监听的端口
EXPOSE 8000

# 定义容器启动时执行的命令，这里使用 gunicorn 作为 Web 服务器
CMD ["gunicorn", "-b", "0.0.0.0:8000", "myapp.wsgi:application"]

```



# dockerfile中CMD、RUN、ENTRYPOINT的区别

RUN：用于执行命令，并创建一个新的镜像层，比如【RUN pip install --no-cache-dir -r requirements.txt】

CMD：容器启动时执行的命令，比如构建一个mysql容器，那启动命令【*systemctl start mysqld.service*】



# docker怎么挂在本地文件



# docker容器间怎么通信的，通信原理是什么



# python垃圾回收机制



# jenkins如何调通接口



# 登陆cookie怎么做



# 性能测试怎么做



# 性能测试关注哪些指标



# 有没有性能监控平台



# 知道哪些算法

不使用高级语言封装好的方法，数组里有2个重复的元素，如何筛选出这个数





# 敏捷对于测试的影响



# 说一个成长最大的项目



