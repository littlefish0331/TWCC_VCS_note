# Learning Note from Others

紀錄在專案中，其他大神操作 Docker 的作法!!

--

**Content:**

<!-- TOC -->

- [Learning Note from Others](#learning-note-from-others)
  - [Youtube](#youtube)
    - [NetworkChuck](#networkchuck)
  - [Container](#container)
    - [D4SG x GCAA - SQL SERVER](#d4sg-x-gcaa---sql-server)
    - [BO performance](#bo-performance)
  - [END](#end)

<!-- /TOC -->

---

## Youtube

### NetworkChuck

- **[you need to learn Docker RIGHT NOW!! // Docker Containers 101](https://www.youtube.com/watch?v=eGz9DS-aIeY)**

```{bash}
docker pull <image_name_from_dockerhub>
dpcker pull <image_name_from_dockerhub_url>:<tag_name> // 例如docker pull thenetworkchuck/nccoffee:frenchpress
docker run -d -t --name <container_name> <image_name>
docker ps  // see running docker containers
docker ps -a  // 顯示有在 run 以及 exited 的container。
docker exec -it <container_name or container_ID> bash // bash mode 進入操控 container
docker exec -it <container_name or container_ID> sh // shell mode 進入操控 container
docker stop <cintainer_name or container_ID>
docker start <container_name or container_ID>
docker stats  // 觀看目前每個container的資源使用情況，包含CPU、Memory等等。
```

每一個範例。

```{bash}
// 以下是執行 `docker pull centos` 的資訊
Using default tag: latest  // 從DockerHub下載的tag
latest: Pulling from library/centos
6910e5a164f7: Pull complete
Digest: sha256:4062bbdd1bb0801b0aa38e0f83dece70fb7a5e9bce223423a68de2d8b784b43b
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```

影片筆記

- NGINX，是web server software，我猜和 Apache 很像。

---

## Container

### D4SG x GCAA - SQL SERVER

- [Microsoft SQL Server - Docker Hub](https://hub.docker.com/_/microsoft-mssql-server)
- ACCEPT_EULA confirms your acceptance of the End-User Licensing Agreement.

<!-- SA_PASSWORD=MSSQL@2020 -->

```{bash}
docker run \
--name mssql \
-e "ACCEPT_EULA=Y" \
-e "SA_PASSWORD=<your_password>" \
-e "MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS" \
-v /datamount/mssql:/var/opt/mssql \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest

// 一行指令。
docker run --name mssql -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<your_password>" -e "MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS" -v /datamount/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

--

### BO performance

**更改檔案或目錄之擁有者或群組:**

- [Linux 更改檔案擁有者與群組，chown 指令使用教學與範例 - G. T. Wang](https://blog.gtwang.org/linux/linux-chown-command-tutorial/)

```{bash}
chown -R ubuntu /data2/bigobject
```

**建立 postgres, mariadb 的 container:**

<!-- POSTGRES_PASSWORD=NCHC-COVID19 -->

```{bash}
docker run \
--name some-postgres \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-e POSTGRES_PASSWORD=<your_password> \
-v /data2/postgres:/var/lib/postgresql \
-p 3307:5432 \
-dit postgres
```

<!-- MYSQL_ROOT_PASSWORD=NCHC-COVID19 -->

```{bash}
docker run \
--name some-mariadb \
-e MYSQL_ROOT_PASSWORD=<your_password> \
-v /data2/mariadb/:/var/lib/mysql \
-p 3308:3306 \
-d mariadb
```

---

## END
