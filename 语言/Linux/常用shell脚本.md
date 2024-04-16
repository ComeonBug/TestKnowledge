1.自动化测试shell脚本：

自动在设备上跑monkey命令：

```bash
#! /bin/bash
devices=`adb devices | grep device | awk '{print $1}'`
for device in devices;do
	{nohup adb -s $device shell monkey -p com.xueqiu.cn --pct-touch 80 --pct-action20 -v --throttle 200 400 & }
done
```

自动跑test case：

```bash
[ -e /tmp/fifo_3 ] || mkfifo /tmp/fifo_3
exec 3 <> .tmp/fifo_3
rm -rf /tmp/fifo_3
# >&3 写入fd3中
adb devices | grep 'devices' | awk '{print $1}' >&3

find . -name 'test_xueqiu*.py' | {
	while read file;do
		# <&3 读取
		read udid <&3 && {
			echo udid=$udid
			udid=$udid pytest $file
			echo $udid >&3
		} &
		# & 放入后台执行
		done
		wait
	}
# 关闭fd3
exec 3<&-
exec 3<&-
```



2.性能测试shell脚本

ps：Linux下该脚本ok，macOS下不可

获取性能指标：

```bash
while true;do adb shell top -n 1 | grep xueqiu | awk '{print $3}';done
```



auto_stress.sh

```bash
echo 'stress testing start!'

thread_num_array=[10,50,100,150,200]
for num in ${thread_num_array}
do
	export jmx_filename="${jmx_template}_${num}_${suffix}"
	export jtl_filename="test_${num}.jtl"
	export web_report_path_name="web_${num}"
	
	rm -f ${jmx_filename} ${jtl_filename}
	rm -rf ${web_report_path_name}
	
	cp ${jmx_template_filename} ${jtl_filename}
	echo "stress script is ready"
	
	if [[ "${os_type}" == "Darwin" ]];then
		sed -i "" "s/thread_num/${num}/g" ${jmx_filename}
	else
		sed -i "s/thread_num/${num}g" ${jmx_filename}
	
	# jmeter静默压测
	nohup ${jmeter_path}/bin/jmeter -n -t ${jmx_filename} -l ${jtl_filename}
	
	# 生成jmeter压测报告
	${jmeter_path}/bin/jmeter -g ${jtl_filename} -e -o ${web_report_path_name}
	
	rm -f ${jmx_filename} ${jtl_filename}
done
echo "stress testing done!"
```



3.dockerfile脚本

dockerfile:

```bash
FROM centos:7
ADD entrypoint.sh /root
ADD requirements.txt /root

WORKDIR /root
USER root

RUN yum install -u python git python-34-setuptools python-34-devel.x86_64 
	&& easy_install-3.4 pip
	&& pip3 install -r requirements.txt
	
ENTRYPOINT ["/root/entrypoint/sh"]
```

entrypoint.sh

```bash
#! /bin/bash
mkdir -p /opt/web
cd /opt/web
git clone ***
cd holmes
pip3 install -r requirements.txt

whilt true
do
	sleep 10
done
```



4.jenkins构建脚本

```bash
. ~/.bash_profile

cd isSelenium_python
rm -rf pytests.xml
pytest -v test/web_ui.py -o junit_family=xuint2 --juint-xml=pytests.xml
```

app打包

```bash
. ~/.bash_profile
cd AndoridSampleApp
sh gradlew clean assemleDebug
```

安装、运行测试用例、生成报告：

```bash
. ~/.bash_profile

pwd=`pwd`
apk=$pwd/****/app_debug.apk

{
	adb uninstall com.***
} ||
{
	echo 'no apk found'
}

adb install $apk
cd AutoAppPytest
pytest -s -v test/app_test.py --alluredir=./report
allure serve /report
```

