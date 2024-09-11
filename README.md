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
停止容器,
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
可以造訪這個網站,看是否有連線到
- http://localhost:8080/


- - -
