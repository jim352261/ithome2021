## kind安裝
從上一章了解各種K8s的特點,在這章將會教學如何安裝Kind,由於其利用docker的特性,會比使用VM的minikube和Kubeadm適合在個人電腦上測試(例如在筆電上跑Kubeadm就會聽到筆電起飛的聲音).
而既然要使用的是KIND(Kubernetes In Docker),那最優先的必定是安裝docker,在安裝Kind前,會先從安裝docker說明起.

## docker 安裝
這邊會介紹如何在linux和macOS上安裝docker

### linux上透過儲存庫(repository)安裝docker

#### 準備好儲存庫(repository)
1. 更新apt套件的索引(index)以及安裝套件來讓apt能透過HTTPS使用儲存庫:

```
 sudo apt-get update
 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

2. 新增docker官方的GPG key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3. 準備穩定(stable)的儲存庫,可以在指令中的stable後面加上nightly 或 test,來使用這兩種儲存庫:

```
 echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 安裝Docker Engine
1. 更新apt套件的索引並更新最新版的docker
```
 sudo apt-get update
 sudo apt-get install docker-ce docker-ce-cli containerd.io
```
2. 確認是否有安裝成功
```
sudo docker run hello-world
```


### mac上安裝桌面板docker

1. 透過官網下載桌面板docker
https://www.docker.com/get-started

2. 將docker 移到app目錄

## 安裝KIND
在上面介紹完如何安裝docker後,接下來就是進入正題,如何安裝kind

1. 在linux上安裝kind
```
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64
chmod +x ./kind
mv ./kind /some-dir-in-your-PATH/kind
```

2. 如果是MacOs 可以透過mac brew安裝:
```
brew install kind
```

當安裝完後,可以透過下面指令測試是否有安裝成功
```
kind create cluster
```
成功的話會像下面這樣
