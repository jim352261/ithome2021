## kind安裝
從上一章了解各種K8s的特點,在這章將會教學如何安裝Kind,由於其利用docker的特性,會比使用VM的minikube和Kubeadm適合在個人電腦上測試(例如在筆電上跑Kubeadm就會聽到筆電起飛的聲音).
而既然要使用的是KIND(Kubernetes In Docker),那最優先的必定是安裝docker,在安裝Kind前,會先從安裝docker說明起.

## docker 安裝
這邊會介紹如何在linux和mac OS上安裝docker

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
