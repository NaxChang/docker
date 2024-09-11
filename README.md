# docker->Devops
下載地址  
https://www.docker.com/products/docker-desktop/  
下載hyper-v  
https://learn.microsoft.com/zh-tw/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v  
gitlab相關網站  
https://docs.gitlab.com/  
參考GitHub 肖鵬老師  
https://github.com/xiaopeng163/docker.tips

# 命令提示字元CMD

## 查看有哪些映像檔可以安裝    
docker images  

### 拉取 GitLab Runner 映像  
docker pull gitlab/gitlab-runner:latest  

### 啟動 GitLab Runner 容器  
docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner:latest  

### 安裝好docker之後  
先查看有哪些容器? docker ps   
docker exec -it 名稱 /bin/bash  


- - -
