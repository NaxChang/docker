# docker->Devops
下載地址  
https://www.docker.com/products/docker-desktop/  
下載hyper-v  
https://learn.microsoft.com/zh-tw/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v  
gitlab相關網站  
https://docs.gitlab.com/  


## 命令提示字元CMD
輸入 docker version,會出現clinet和server
## 查看有哪些映像檔可以安裝    
docker images  

### 拉取 GitLab Runner 映像  
docker pull gitlab/gitlab-runner:latest  

### 啟動 GitLab Runner 容器  
docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner:latest  
- -d：在後台運行容器。  
- --name gitlab-runner：為容器指定名稱為 gitlab-runner。
- --restart always：確保容器在 Docker 重啟時自動重新啟動。
- -v /srv/gitlab-runner/config:/etc/gitlab-runner：  
- 將宿主機的 /srv/gitlab-runner/config 目錄掛載到容器的  
-  /etc/gitlab-runner 目錄。  
- gitlab/gitlab-runner:latest：使用 gitlab/gitlab-runner 映像檔的最新版本。  

### 安裝好docker之後  
先查看有哪些容器? docker ps   
docker exec -it 名稱 /bin/bash  
### 啟動容器
docker exec -it 4082490b7a72 /bin/bash
docker exec -it 38d361ab9a26 /bin/bash



### 9/22 建立三格目錄
mkdir -p /data/gitlab/config
mkdir -p /data/gitlab/logs
mkdir -p /data/gitlab/data

### 9/22 最新的 GitLab 社區版映像(cmd內製作)
docker pull gitlab/gitlab-ce:latest

#### 9/22 啟動容器
docker run -d -p 443:443 -p 80:80 -p 222:22 --name gitlab --restart always -v /desktop-biltpq4/user/data/gitlab/config:/etc/gitlab -v /desktop-biltpq4/user/data/gitlab/logs:/var/log/gitlab -v /desktop-biltpq4/user/data/gitlab/data:/var/opt/gitlab gitlab/gitlab-ce:latest


####9/22 編輯
1. vim /etc/gitlab.rb
2. gitlab-ctl reconfigure 重新配置
3. docker restart gitlab

#### 以下這一段,到dokcer網站
https://hub.docker.com/r/gitlab/gitlab-runner

# 切記,先有doceker,安裝gitlab,再安裝runner

-------------------------------------------------

### 先檢查一下有沒有裝runner  
sc query gitlab-runner
### 如果有裝,要先註冊,在註冊前要有token,這點和gitHub很像
gitlab-runner register  
### 檢查一下狀態,這也跟Git很像,git status
gitlab-runner status
#### windows安裝git-lab running  
https://docs.gitlab.com/runner/install/windows.html  
#### 很重要,我嘗試裝了很多次
首先要下載到 C:\GitLab-Runner,然後cd 切換目錄,檔名改成gitlab-runner.exe,WIN EXIT_CODE為1077 表示
- SERVICE_NAME: gitlab-runner 服務的名稱。
- TYPE: 10 WIN32_OWN_PROCESS 表示該服務作為獨立的 Windows 進程運行。
- STATE: 1 STOPPED 表示服務當前已停止。
- WIN32_EXIT_CODE: 1077 (0x435) 表示「服務的指定控制器未被找到」，這通常意味著服務未能啟動或存在配置問題。
- SERVICE_EXIT_CODE: 0 (0x0) 表示服務正常退出。
- CHECKPOINT 和 WAIT_HINT: 0x0 表示沒有檢查點和等待提示
### 這一段很重要,因為檢查的服務可能不會啟動,所以要讓它啟動
- sc query gitlab-runner
在cmd輸入這一段,很重要
.\gitlab-runner.exe run
### 錯誤解決
gitlab-runner: the service is not installed 這個時候,就要去查看 ls -l /etc/gitlab-runner/config.toml
文件不存在是正常的,需要創建一個基本配置
- sudo mkdir -p /etc/gitlab-runner
- sudo touch /etc/gitlab-runner/config.toml
#### 如果你在 Docker 容器內部工作，使用 sudo 可能會無效，因為容器內部通常以 root 權限運行。你可以通過以下命令來確定你是否在 Docker 容器內部：
- mkdir C:\etc\gitlab-runner
- touch /etc/gitlab-runner/config.toml
- dir C:\path\to\your\config  我的範例是 C:\GitLab-Runner/config
### 刪除容器
docker rm 4e128c2ce960 e200de8edfbf 98cc31c47641 c6121bab0750 2279529e0421
### 刪除完之後,要檢查容器
docker ps -a
##### 
停止容器
docker stop b16221946b25
刪除容器：
docker rm b16221946b25
重新運行容器：
docker run -d --name gitlab-runner -v C:\GitLab-Runner:/etc/gitlab-runner gitlab/gitlab-runner:latest
確保映像存在
docker search gitlab-runner
###  gitlab-runner: the service is not installed

#### 安裝vim(這一段會裝很久,看電腦速度)
- 更新包索引  
- apt-get update  切記一定要先更新
-  安裝vim 
apt-get install -y vim 如果直接安裝會失敗

後續其它- - -
## 創建並啟動一個新的 Docker 容器，基於 nginx 映像檔。
docker run -d -p 8080:80 nginx   
可以造訪這個網站,看是否有連線到 http://localhost:8080/
測試後,有連線


- NETWORK ID     NAME      DRIVER    SCOPE
- 91f446861b63   bridge    bridge    local
- e60ccfe549ba   host      host      local
- 3a2f4ff62b4b   none      null      local

- - -
apt update
apt install git -y 在docker裡面裝
git clone https://gitlab.com/nameless8031437/name-pj_0916.git
cd your-repository

克隆了 GitLab 仓库之后，你可以根据你的需求进行以下操作：

1. 检查和修改代码
在容器内，进入克隆的项目目录，检查和修改代码：

bash
複製程式碼
cd your-repository 
cd name-pj_0916
# 查看项目文件
ls -la
---

stages:
  - build
  - test

build:
  stage: build
  script:
    - echo "Building the project..."
    - docker build -t my-app .

test:
  stage: test
  script:
    - echo "Running tests..."
    - docker run my-app /bin/sh -c "echo 'Running tests...'"
----
apt install vim -y 安裝vim

# 指定使用的 Docker 镜像
image: docker:19.03

# 启用 Docker-in-Docker 服务
services:
  - docker:dind

# 设置环境变量
variables:
  DOCKER_DRIVER: overlay2
  DOCKER_TLS_CERTDIR: "/certs"

# 定义流水线阶段
stages:
  - build

# 定义构建作业
build-job:
  stage: build
  script:
    - echo "Checking Docker version..."
    - docker --version
    - echo "Building the project..."
    - docker build -t my-app .

# glpat-1234abcd
#### git config --global credential.helper cache
#### git credential-cache exit

######
origin  git@gitlab.com:nameless8031437/gitlab-profile.git (push)
root@52fa9ce87709:/# ssh-keygen -t rsa -b 4096 -C "naxchang@gmail.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:XV6iwwG4bc8lr3k0YxsLPy56DAGQX1f8XmCivChhM28 naxchang@gmail.com
The key's randomart image is:



#####

root@52fa9ce87709:/# cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDTVclE/OoiAyUaTZAZGh4TLh3k2DsiGi3NMZ0kQodK0lWn8hCl7jHbiCSPVx8rIk2rcM70ID5pzykeXukd6ALNdk3Gd9eys52Tg0DnlLR0ucpCDVIFxYlzr/v/K+uV9zaRd6xSkg7qTIKcIUT/WOnvuhAWfynL5Et5D2kWXb1Wvm18MF3TwA8NHMPgQlIY8ilW0slFcnVryax/ryvaWkUF+Dmw/aSGh2lOOoJHu8y9s8Fdfc1xqyLYCRF6nwmogTi29BukiHMoBKSFLls8FjwSvNCdruhEp4O3DbFMKTZCFQ0ZjB/XeFgGEsGiVC4zA7JQAZCjiUxBbqhzC1PtlvIdqm/912Gxowj9LJ0kfuiVNuJM345iajXIwiYMikTyM+NkohFt40PEOE9hrJBBNcF3eyB5g0DkTuZa3J6kvqziqBPhA7mDDsLoEIqfeetXwKoZ8Mk9nUEL7dwCnt/1v/SWh7Xi3Yfx3gika9PeBrcpWuCB+dkeKJIKhS7vKmjsjhw1kobBnHGs8KWAUhxyiBQHSpG9yWIGNB0IPYmJYv2D8j2KXDNxqkGQMj2fWlxfbIZbiTGuY+rjqVgOtEz+IPUiXTPRZ9esoZs21AzywZv2DTabQhc8Un4lLW+Kp5kfyv9tk0Kd7YK8NbwaSEPqKyfA9n0YTj9AIgLMJXHhAs/FNQ== naxchang@gmail.com

######
root@52fa9ce87709:/# nano Host gitlab.com
root@52fa9ce87709:/# echo $SSH_AUTH_SOCK
/tmp/ssh-XXXXXX2Y8faV/agent.259


在本地使用 Docker Desktop 安裝 GitLab 和 GitLab Runner，按照以下步驟進行：

步驟 1: 安裝 Docker Desktop
確保你已經安裝並啟動 Docker Desktop。

步驟 2: 拉取 GitLab Docker 映像
在命令行中運行以下命令來拉取 GitLab 的 Docker 映像：

bash
複製程式碼
docker pull gitlab/gitlab-ee:latest
步驟 3: 啟動 GitLab 容器
使用以下命令來啟動 GitLab。你可以根據需要調整埠號和環境變數。

bash
複製程式碼
docker run --detach \
  --hostname localhost \
  --publish 8929:8929 --publish 443:443 --publish 80:80 \
  --name gitlab \
  --restart always \
  --volume gitlab-config:/etc/gitlab \
  --volume gitlab-logs:/var/log/gitlab \
  --volume gitlab-data:/var/opt/gitlab \
  gitlab/gitlab-ee:latest
步驟 4: 訪問 GitLab
在瀏覽器中輸入 http://localhost:8929（或你配置的埠），然後按照指示設置管理員帳戶。

步驟 5: 安裝 GitLab Runner
在命令行中運行以下命令來拉取 GitLab Runner 的 Docker 映像：

bash
複製程式碼
docker pull gitlab/gitlab-runner:latest
步驟 6: 啟動 GitLab Runner
運行 GitLab Runner 容器：

bash
複製程式碼
docker run -d --name gitlab-runner --restart always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
步驟 7: 註冊 GitLab Runner
在容器內運行以下命令來註冊 Runner：

bash
複製程式碼
docker exec -it gitlab-runner gitlab-runner register
你需要輸入以下信息：

GitLab URL（例如 http://localhost:8929）
登錄令牌（在 GitLab 中找到，通常在 Settings > CI / CD > Runners）
描述（可以任意命名）
標籤（可選）
Executor（通常選擇 docker）
步驟 8: 配置 Runner
根據需要編輯 Runner 配置文件（在 /etc/gitlab-runner/config.toml）。

步驟 9: 測試 CI/CD
在 GitLab 中創建一個項目並添加 .gitlab-ci.yml 文件，然後推送到 GitLab，檢查 CI/CD 是否正常運作。

這樣，你就可以在本地使用 Docker Desktop 成功安裝 GitLab 和 GitLab Runner 了！如果有問題隨時問我！
