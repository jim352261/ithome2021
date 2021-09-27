在前一章提到
## 建立Deployment
首先建立一個nginx-example.yaml，並將下面內容複製進去。
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

並在該yaml檔所在的目錄執行 apply
```
kubectl apply -f nginx-example.yaml
```

這樣就可以建立Deployment，如果要確認有沒有建立成功，則可以使用get deployments
```
kubectl get deployments
```
可以看到下圖有成功建立，如果你這時get pods的話，可以看到有三個pod正在建立。

![https://ithelp.ithome.com.tw/upload/images/20210922/201296072X5z603fko.png](https://ithelp.ithome.com.tw/upload/images/20210922/201296072X5z603fko.png)

## 更新Deployment內image 版本
透過下面的指令，可以更新Deployment內image，以這個範例來說，可以將pod內的image從1.14.2改成1.16.1
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --record
```
這樣可以將image更新，這時去get pods可以看到pods有重啟，但是Deployment並沒有重啟。
![https://ithelp.ithome.com.tw/upload/images/20210922/20129607DEA73DzHkt.png](https://ithelp.ithome.com.tw/upload/images/20210922/20129607DEA73DzHkt.png)


## 刪除Deployment
如果已經不再需要Deployment，則可以用下面的指令刪除Deployment。
```
 kubectl delete deployment nginx-deployment
```
由於前面的metadata.name為nginx-deployment，所以這邊刪除要指定刪nginx-deployment。
刪除後就會像下面這樣：
![https://ithelp.ithome.com.tw/upload/images/20210922/20129607hIZ2p0UmSZ.png](https://ithelp.ithome.com.tw/upload/images/20210922/20129607hIZ2p0UmSZ.png)