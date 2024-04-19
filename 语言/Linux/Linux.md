Linux：一切皆文件



/dev目录：device，存放设备（硬件、键盘、鼠标）相关权限

/dev/sda：第1块硬盘

​	（sd or hd）+（a第一块、b第二块、c）

​	hd：IDE设备

​	sd：SCSI设备

```shell
$ hostname # 查看主机名
liyangdembp
$ ifconfig # 查看ip：  lo0：回环测试结构，固定127.0.0.1，指代自己   etho：第一张网卡
$ mkdir -p ./test1/test2  # -p（parent） 创建集联目录，父目录不存在则创建
$ head -5 人经验.md
$ tail -2 个人经验.md
```



tail

小tips：

```bash
$ find . -name \*.properties
./saveservice.properties
./system.properties
./jmeter.properties
./reportgenerator.properties
./upgrade.properties
./user.properties
$ ls | grep properties
jmeter.properties
reportgenerator.properties
saveservice.properties
system.properties
upgrade.properties
user.properties
```

