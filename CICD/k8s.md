helm

helm包管理工具

helm list查看chart list



chart



release





# 实例

## deployment

```shell
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  name: test1
spec:
  replicas: 5
  template:
    metadata:
      labels:
        app: test1
    spec:
      containers:
      - name: test1
        image: httpd
        ports:
            - containerPort: 80
```



![在这里插入图片描述](k8s.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NzAxOTAxNg==,size_16,color_FFFFFF,t_70.png)



## service

```shell
kind: Service
apiVersion: v1
metadata:
  name: test-svc
spec:
  type: NodePort  # 为了能让外网访问，需要设置service的类型是nodetport
  selector:
    app: test1
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 32034  #nodePort的有效范围是：30000-32767
```

