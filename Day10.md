在前一章我們介紹完kind常用的指令，既然已經知道指令了，接下來我們就來試看看部署

## 建立測試環境
首先，去官網找到並複製kind-example-config(https://raw.githubusercontent.com/kubernetes-sigs/kind/main/site/content/docs/user/kind-example-config.yaml)
或是在本地產生一個kind-example-config.yaml並將下面內容複製上去。
```
# this config file contains all config fields with comments
# NOTE: this is not a particularly useful config file
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
# patch the generated kubeadm config with some extra settings
kubeadmConfigPatches:
- |
  apiVersion: kubelet.config.k8s.io/v1beta1
  kind: KubeletConfiguration
  evictionHard:
    nodefs.available: "0%"
# patch it further using a JSON 6902 patch
kubeadmConfigPatchesJSON6902:
- group: kubeadm.k8s.io
  version: v1beta2
  kind: ClusterConfiguration
  patch: |
    - op: add
      path: /apiServer/certSANs/-
      value: my-hostname
# 1 control plane node and 3 workers
nodes:
# the control plane node config
- role: control-plane
# the three workers
- role: worker
- role: worker
- role: worker
```
再來在yaml檔所在目錄下
```
kind create cluster --config kind-example-config.yaml
```
透過這種方式，即可用剛剛建立的設定yaml檔去建立cluster，可以使用下面指令檢查是否有正確建立nodes

```
kind get nodes
```
成功的話會看到有一個control-plane的nodes以及三個worker的node，就像下面的圖片一樣：


在前面有提到，kind是用docker的pod當作節點，因此當你現在使用查詢docker 運作中的pod的節點，就可以看到剛剛產生出來的nodes，就像下圖

