https://docs.docker.com/engine/reference/commandline/compose_up/

【## docker
docker ps -a => 顯示所有的容器，包括未運行的

docker rm -fv name => 強制刪除 container & 相關資料

docker logs -f name => 查 log

docker images =>查看 docker image list

docker stats => 查看container的CPU / MEM USAGE

docker save -o [output_name] [images_name:tag] => 將 image 打包
⭐(記得描述 tag, 這樣匯入時就不用再去設定 tag)
docker save -o redis_4.0.11.tar redis:4.0.11-alpine
docker save stix_v2:1.3.0 | gzip > stix_v2_1.3.0.tar.gz

移除 image
docker image rm (imageid / name:tag)
docker image rm image_id image_id2 image_id3

docker exec -it afs-app sh => 進入 container

#使用container內的 linux package
docker exec -it stix2_converter lftp

#使用container內的python 執行container內的檔案(檔案要先cp進container)
docker exec converter_upload_incident_automatically /opt/converter/bin/python /opt/script/upload_incident_automatically/upload_incident_automatically.py

docker inspect [container_name]

匯出 image
docker save -o mytomcat.tar mytomcat

匯入 image
docker load -i redis_4.0.11.tar
docker load --input redis_4.0.11.tar => Load 壓縮過的 image檔, 可以先對 .tar 檔 壓縮 => gzip, 速度會更快
gzip FileName

將容器2d33c6009a1f的/home/...目錄拷貝到主機的/home/cyd目錄中。
docker cp -a 2d33c6009a1f:/home/socuser/starter.sh /home/cy

將主機 aaa.txt 拷貝到容器97adb67b9ca9中，位置為/opt/converter
docker cp aaa.txt 97adb67b9ca9:/opt/converter

看 rsyslog 的 紀錄
less xxx.log

#查看docker的容量
du -shc /var/lib/docker/*

#清除overlay2的垃圾資料
docker system prune -a -f

* * *

## docker-compose

docker-compose up -d <= 起 docker 服務

docker-compose down -v <= uinstall

docker-compose up -d --no-recreate => 只啟動 不存在的 container (已編輯)

docker-compose up -d --build isac-debug

#無視快取，重新建構指定的 image
docker-compose build --no-cache <image_name>

#只建 couchdb 服務
docker-compose up -d couchdb

# IMAGE

docker images =>查看 docker image list

docker save -o [output_name] [images_name:tag] => 將 image 打包
⭐(記得描述 tag, 這樣匯入時就不用再去設定 tag)
docker save -o redis_4.0.11.tar redis:4.0.11-alpine

指定.env路徑，build container
docker-compose --env-file /opt/service/.env build

移除 image
docker image rm (imageid / name:tag)
docker image rm image_id image_id2 image_id3
docker image rmi -f 8271b2316ea7

# CONTAINER

停止運行所有container
docker stop $(docker ps -aq)

#加worker
gunicorn => --workers=(2 x $num_cores) + 1
celery => --concurrency=num_cores

# NETWORK

#docker 網路 list
docker network ls

#查詢特定網路的IP範圍
docker network inspect <network-name>--format='{{range .IPAM.Config}}{{.Subnet}}{{end}}'</network-name>

# 創建 Docker 網路

docker network create --subnet=192.168.200.0/24 soc_app_net

#刪除指定ID網路
docker network rm ID

#刪除所有未連接到任何容器的網絡
docker network prune

#刪除Compose文件中未定義的服務
docker-compose up -d --remove-orphans

#查看couchdb 方式
cd /var/lib/docker/containers/[CONTAINER ID]

docker ps 查看 CONTAINER ID

* * *

depend_on:等待特定容器先啟動完成(啟動順序的安排)，在這個例子中我先啟動RabbitMQ才啟動其他兩個
[容器們的啟動與互動docker-compose](https://ithelp.ithome.com.tw/articles/10197490)

# 查看已使用的 container name + IP
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq) >> ip.log

參考資料:
[Docker 命令大全](https://www.runoob.com/docker/docker-command-manual.html)
[儲存和載入映像檔](https://philipzheng.gitbooks.io/docker_practice/content/image/save_load.html)