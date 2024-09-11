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
- -v /srv/gitlab-runner/config:/etc/gitlab-runner：將宿主機的 /srv/gitlab-runner/config 目錄掛載到容器的 /etc/gitlab-runner 目錄。
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

## 創建並啟動一個新的 Docker 容器，基於 nginx 映像檔。
docker run -d -p 8080:80 nginx  
可以參考一下網站
- http://localhost:8080/


- - -
