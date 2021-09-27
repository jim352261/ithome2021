在前一章介紹完如何安裝kubectl以及利用kind建立node之後，接下來要開始介紹如何建立一個pod。
在前面的章節有提到k8s官方不建議直接對pod進行操作，會比較建議採用 Deployment 或是 Job，
間接控制pod，這兩個介紹會放在後面，為了測試，這邊還是會先直接對pod進行操作。

## 建立pod
上一章我們學了如何安裝kubectl這個東西，接下來就會直接用到他了。
首先讓我們用最簡單的方法：
```
kubectl run nginx --image=nginx --restart=Never
```
透過這個指令，我們可以告訴k8s建立出單個pod，名稱為nginx，使用Docker Hub上的nginx映像檔
且flag為--restart=Never。

### 查看pod資訊
而建立出來的pod則可以透過get pods查看：
```
kubectl get pods
```
結果會像下面這樣：


如果要看這個pod的詳細內容，則可以使用describe來查看詳細內容：
```
kubectl describe pod nginx
```
結果會像下圖：

會像這樣在終端顯示這個pod的yaml，可以看出這個pod的名稱，哪個節點以及建立時間等等。

### 刪除pod
如果要刪除pod的話，則可以透過delete pod來刪除pod：
```
kubectl delete pod nginx
```
結果會像下面這樣：

看得出來nginx被刪除後，get pod 和 describe pod都無法查看了。

這樣就瞭解了如何直接透過指令產生pod以及查看pod資訊跟刪除pod了
下一章會介紹如何透過yaml建立pod以及如何跟pod互動