# Day7-aws或gcp 我選擇本機建立k8s

## 是否該用雲端平台
在正式使用k8s的時候，部署k8s最好的情況是使用雲端平台，一來機器規格可以依照需求調整，二來可以不必花錢雇用硬體工程師，讓平台商去處理即可。
最常見的雲端平台有亞馬遜(Amazon)提供的aws(Amazon Web Services)以及google提供的gcp(google cloud platform)
不過我們在測試時不會用到這兩個平台，除了學會部署到雲端平台上需要學習成本外，最重要的是使用雲端平台的各種功能都需要花錢，儘管初辦雲端平台的帳號都會有試用金額，但是拿來部署k8s以及測試的話，一下就用完了。
在這種限制下，比起使用雲端平台，會比較推薦在本機部署k8s，因此接下來會介紹幾種本機k8s，再依情況選擇適合自己需求的版本。

## 本機版k8s
在這邊會介紹三種官方推薦的工具， Minikube Kind以及Kubeadm：

### MiniKube
MiniKube為官方維護的工具之一,其特色為透過虛擬機器(VM virtual machine)來建立一個單節點的k8s叢集,由於其簡單安裝的特性,以及本身就有提供儀表板(dashboard),所以使用上非常方便
是目前最被廣泛使用的本機k8s測試工具,安裝完之後只要來一句minikube start,就可以啟動MiniKube.
順帶一提,儀表板並非k8s本身的功能,而是minikube包裹好的插件,在使用別種k8s工具時沒有儀表板是正常的.

### Kind
Kind是什麼，絕對不是仁慈或和藹可親，它的全名是Kubernetes in Docker,顧名思義是使用docker在本機運行kubernetes的工具,kind會將docker的容器當成節點(node)來運行,相較於MiniKube使用VM來建立節點,kind使用docker,因此kind的啟動速度會比MiniKube快上不少.



### Kubeadm
跟MiniKube一樣是官方維護的專案,和MiniKube很類似,Kubeadm會使用VM來建立節點,跟MiniKube不一樣在於,MiniKube會建立單節點cluster,Kubeadm會建立master node和slave-node,和MiniKube相比,Kubeadm更接近完整個k8s,但是相較之下實體資源吃的也更重,如果使用者想在筆電上測試,不是個好選擇

