# Docker

###### 安裝docker(Centos7)
* 移除舊版docker
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
* 安裝docker
```
sudo curl -sSL https://get.docker.com | sh
```
* 重新開機
```
reboot
```
* 將帳號加入 docker 群組
```
sudo usermod -aG docker USERNAME
```
* 啟動docker
```
systemctl start docker
```
* 自動啟動 docker 服務
```
systemctl enable docker
```

###### 安裝docker-compose
* 安裝git
```
yum install git
```
* 安裝docker-compose
```
yum install epel-release
```
```
curl -L "https://github.com/docker/compose/releases/download/1.10.0/docker-compose-$(uname -
s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
* 更改權限
```
chmod +x /usr/local/bin/docker-compose
```

### docker指令
###### Image (映像檔) 常用指令

| 指令   | 說明 | 範例                              |
| -------| ---- | --------------------------------- |
| search | 搜尋 | docker search <映像檔名稱>        |
| pull   | 下載 | docker pull <映像檔名稱>          |
| images | 列表 | docker images                     |
| rmi    | 刪除 | docker rmi <映像檔ID>             |
| build  | 建立 | docker build -t <映像檔名稱:標籤> |
###### Container (容器) 常用指令

| 指令    | 說明                   | 範例                                                |
| --------| ---------------------- | --------------------------------------------------- |
| run     | 新建及啟動             | docker run <參數> <image的名字> <想要執行的語句>    |
| stop    | 停止                   | docker stop <容器ID>                                |
| start   | 啟動                   | docker start <容器ID>                               |
| ps -a   | 列表                   | docker ps -a             |
| rm      | 刪除                   | docker rm ( -f ) <容器ID>　-f :強制刪除執行中的容器 |
| exec    | 進入容器(開新console   | docker exec <參數> <容器ID> <執行的語句/shell>      |
| attach  | 進入容器(退出停止容器) | docker attach <容器ID>                              |
| logs    | 查看容器內的資訊       | docker logs <容器ID>                                |
| inspect | 查看容器資訊           | docker inspect <容器ID>                             |
| stats   | 查看容器使用資源狀態   | docker stats <容器ID>                               |
* Container (容器) 參數
  * -i ：讓容器的標準輸入保持打開
  * -t：讓Docker分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上
  * -d：背景執行
  * -e：設定環境變數(AAA=BBB)
  * -p：Port 對應(host port:container port)
  * -v：資料對應(host folder:container folder)
  * --name ：設定容器名稱
  * --link ：串接其他容器
  * --network ： 加入指定網路

      
###### Registry (倉庫) 常用指令

| 指令   | 說明     | 範例                                                           |
| -------| -------- | -------------------------------------------------------------- |
| login  | 登入     | docker login                                                   |
| commit | 容器存檔 | docker commit <容器ID> <映像檔名稱>:<版本號標籤>               |
| tag    | 標籤     | docker tag <映像檔ID> <Docker帳號>/<映像檔名稱>:<版本號標籤>   |
| push   | 上傳     | docker push <映像檔名稱>:<版本號標籤>                          |
| export | 匯出     | docker export <容器ID> > <打包檔名稱.tar>                      |
| import | 匯入     | cat <打包檔名稱.tar> | docker import <映像檔名稱>:<版本號標籤> |

# Dockerfile
###### 指令
```
docker build -t <映像檔名稱:標籤> <dockerfile的位置>
```
###### Dockerfile語法

| 語法             | 說明                      | 範例                               |
| ---------------- | ------------------------- | ---------------------------------- |
| FROM             | 映像檔來源                | FROM python:3.7                    |
| MAINTAINER       | 維護者訊息                | MAINTAINER docker_user             |
| LABEL            | 標籤                      | LABEL name=jupyter_test            |
| RUN              | 創建映像檔時執行動作      | RUN pip install pipenv             |
| CMD              | 啟動容器時執行的命令      | CMD ["python", "app.py","app2.py"] |
| EXPOSE           | 容器對外的埠號            | EXPOSE 5000                        |
| ADD              | 複製檔案(可放URL及壓縮檔) | ADD requirements.txt /usr/src/app/ |
| COPY             | 複製檔案                  | COPY . /usr/src/app                |
| ENV              | 環境變數                  | ENV PG_VERSION 9.3.4               |
| VOLUME ["/data"] | 掛載資料卷                | VOLUME /var/lib/data               |
| WORKDIR          | 指定工作目錄              | WORKDIR /usr/src/app               |

# Docker-Compose
###### Docker-Compose常用指令
* 在當前目錄執行docker-compose.yml
```
docker-compose up -d
```
* 在指定目錄執行docker-compose.yml
```
docker-compose -f ./XXX/XXX/docker-compose.yml up -d
```
* 移除所有compose建構出來的容器及網路
```
docker-compose down
```
* 啟動被關閉的所有容器
```
docker-compose start
```
* 重新啟動所有容器
```
docker-compose restart
```
* 停止所有容器
```
docker-compose stop
```
* 可以查詢所有或個別容器的紀錄
```
docker-compose logs <compose檔內的服務名稱>
```
* 檢查docker-compose.yml的是否正確編寫
```
docker-compose config
```
* 執行指令時，必須在docker-compose.yml檔案所在目錄或是 -f 參數，指定docker-compose.yml

###### Docker-Compose語法

| 語法        | 說明                      | 範例                               |
| ----------- | ------------------------- | ---------------------------------- |
| version     | 目前的docker-compose版本  | version: '3' |
| services    | 可在此services階層下，建立多個容器服務 | services:<br> 　mysql:<br>　...|
| image       | 指定映像檔                | image: mysql:8.0           |
| build       | 指定 Dockerfile 所在目錄，Compose 會利用它自動構建映像檔，然後使用這個映像檔 | build:<br>　dockerfile: dockerfile |
| context     | 指定dockerfile路徑 | build:<br>　context: .<br>　dockerfile: dockerfile |
| command     | 容器啟動後預設執行的命令  | command: python test.py |
| volumes     | 資料對應(host folder:container folder) (同等於 -v)  | volumes:<br>　- ./data/mysql_db/mysql_data:/var/lib/mysql |
| ports       | Port 對應(host port:container port) (同等於 -p)| ports:<br>　- "5000:5000" |
| networks    | 使指定容器連接至指定的網路進行連接 | networks:<br>　- testnet |
| depends_on  | 使各個容器有相依性的連接 | depends_on:<br>　- redis |
| environment | 設定環境變數 (同等於 -e) | environment:<br>　- MYSQL_ROOT_PASSWORD=123 |
| hostname    | 設定容器的站台名稱       | hostname: redis |
| restart     | 設定容器是否自動重新啟動 | restart: always |
| stdin_open  | 讓容器的標準輸入保持打開 (同等於 -i) | stdin_open: true |
| tty         | 讓Docker分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上 (同等於 -t) | tty: true |
