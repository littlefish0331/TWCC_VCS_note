# Docker Learning Note from Others

這邊紀錄從其他人的 container 學習到的知識!!

`sudo usermod -a -G docker <username>`: 設定 user 可以執行 Docker。好像下一次登入才會生效。(亦可寫成`sudo usermod -aG docker ubuntu`)。將目前使用者加到docker群組。  <br>
`sudo service docker start`: 啟動docker服務。有其他方法，比如用systemctl啟動docker，詳見 `tutroial - others > phoenixnap website > 在 ubuntu 上安裝 Docker`。

<!-- TOC -->

- [Docker Learning Note from Others](#docker-learning-note-from-others)
  - [Youtube](#youtube)
    - [NetworkChuck](#networkchuck)
  - [Container](#container)
    - [D4SGxGCAA - SQL SERVER](#d4sgxgcaa-sql-server)
    - [BO_performance_part2](#bo_performance_part2)
  - [END](#end)

<!-- /TOC -->

---

## Youtube

### NetworkChuck

- **[you need to learn Docker RIGHT NOW!! // Docker Containers 101](https://www.youtube.com/watch?v=eGz9DS-aIeY)**

```{bash}
docker pull <image_name_from_dockerhub>
dpcker pull <image_name_from_dockerhub_url>:<tag_name> //例如docker pull thenetworkchuck/nccoffee:frenchpress
docker run -d -t --name <container_name> <image_name>
docker ps  //see running docker containers
docker ps -a  //顯示有在run以及exited的container。
docker exec -it <container_name or container_ID> bash //bash mode 進入操控 container
docker exec -it <container_name or container_ID> sh //shell mode 進入操控 container
docker stop <cintainer_name or container_ID>  //和container exited不一樣。
docker start <container_name or container_ID>
docker stats  //觀看目前每個container的資源使用情況，包含CPU、Memory等等。
```

```{bash}
//以下是 `docker pull centos` 的資訊
Using default tag: latest  //從DockerHub下載的tag
latest: Pulling from library/centos
6910e5a164f7: Pull complete
Digest: sha256:4062bbdd1bb0801b0aa38e0f83dece70fb7a5e9bce223423a68de2d8b784b43b
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```

NGINX，是web server software，我猜和Apache很像。

---

## Container

### D4SGxGCAA - SQL SERVER

- [Microsoft SQL Server - Docker Hub](https://hub.docker.com/_/microsoft-mssql-server)

```{bash}
// ACCEPT_EULA confirms your acceptance of the End-User Licensing Agreement.
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=P@ssw0rd" -p 1433:1433 --name mssql -d mcr.microsoft.com/mssql/server:2019-latest
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MSSQL@2020" -p 1433:1433 --name mssql -d mcr.microsoft.com/mssql/server:2019-latest
```

--

### BO_performance_part2

**更改檔案或目錄之擁有者或群組:**

- [Linux 更改檔案擁有者與群組，chown 指令使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-chown-command-tutorial/)

```{bash}
chown -R ubuntu /data2/bigobject
```

**建立 postgres, mariadb 的 container:**

```{bash}
docker run --name some-postgres \\
-v /data2/postgres:/var/lib/postgresql \\
-p 3307:3306 -e POSTGRES_PASSWORD=NCHC-COVID19 \\
-e PGDATA=//data/pgdata -dit mariadb

docker run --name some-mariadb \\
-v /data2/mariadb/:/var/lib/mysql \\
-p 3308:3306 -e MYSQL_ROOT_PASSWORD=NCHC-COVID19 -d mariadb
```

---

## END
