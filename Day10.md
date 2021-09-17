在前一章我們介紹完kind常用的指令，既然已經知道指令了，接下來我們就來試看看部署

## 建立環境
首先，去官網找到kind-example-config(https://raw.githubusercontent.com/kubernetes-sigs/kind/main/site/content/docs/user/kind-example-config.yaml)，看是要直接下載或是複製裡面內容然後在本地產生一個kind-example-config.yaml並將內容複製上去。
再來在yaml檔所在目錄下
```
kind create cluster --config kind-example-config.yaml
```
透過這種方式，即可用剛剛建立的設定yaml檔去建立cluster，可以使用
