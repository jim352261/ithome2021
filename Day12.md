前一章介紹如何建立，刪除pod之後，就要來談談如何對pod本身進行互動了。
當我們建立完有應用程式的pod，會需要跟pod內的應用程式進行互動。
但是在沒有特別設定的情況下，是沒辦法直接跟pod內應用程式交流的。
要跟pod內的應用程式交流，有兩種辦法，一個是利用port-forward，一個是用service。
service的部分會留到下一章說明，這邊先說明用port-forward跟pod溝通。

## 透過yaml建立範例pod
在上一章我們使用image建立了一個pod，不過用這種方式沒辦法詳細設定pod的內容。
因此這邊會講解如何用寫好的yaml來建立pod(之後的service Deployment等等也會用這種方式建立)
### 範例yaml
```
# redis.yaml
apiVersion: v1
kind: Pod
metadata:
  name: redis
spec:
  containers:
  - name: redis
    image: redis

```
首先將上面yaml內容複製起來，在本機開個檔案叫redis_example.yaml，並將內容貼上。
上面可以看到
* apiVersion
    這是目前k8s該元件的版本號，以這個範例的kind為pod，則他的版本號就是v1(代表第一個穩定釋出的版本)。
    如果要用其他元件，則版本號可以從官網的[Kubernetes API](https://kubernetes.io/docs/reference/kubernetes-api/)去查詢。
* Kind
    指定的元件類型，在這邊就是pod，k8s判斷你建立的是service還是pod也是看這邊。
* metadata
    下面會有name labels 以及 annotations， name就是這個pod叫什麼名字，label會在後面章節提到，主要是讓k8s去分配管理用，而annotations則是單純註解，不支援查找跟分類。
* spec
    定義容器內容，在這邊是指定名稱為redis，並使用redis image。

### 建立pod
要透過yaml建立pod，需要在yaml檔的目錄執行下面指令
```
kubectl create -f redis_example.yaml
```

這樣他就會建立出你yaml檔所設定的pod了，這時用get pod就可以看到下圖。
![https://ithelp.ithome.com.tw/upload/images/20210920/20129607A0uBflHovK.png](https://ithelp.ithome.com.tw/upload/images/20210920/20129607A0uBflHovK.png)

要測看看有沒有正常，則可以使用exec指令
```
kubectl exec -ti redis bash
```
指定要對redis這個pod操作，進去之後就可以用redis-cli來確認是否有正常運作
![https://ithelp.ithome.com.tw/upload/images/20210920/20129607US6wlil51t.png](https://ithelp.ithome.com.tw/upload/images/20210920/20129607US6wlil51t.png)

