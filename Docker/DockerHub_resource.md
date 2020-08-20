# DockerHub resource

- alpine版本
- 下載 images-01
  - OS system
    - ubuntu
    - centos
  - website
    - thenetworkchuck/nccoffee:frenchpress，8081
    - DockerCon2020 sample = littlefish0331/hello-world，8080
    - Gitbook: 4001, 4002
    - Grafana: 3000
- 下載 images-02Database
  - mysql: 3306
  - Postgress: 5432
  - MSSQL: 1433
  - mariadb: 3307
  - BigObject: 3308, 9090, 9091
  - ElasticSearch: 9200, 9300
- 下載 images-03code
  - R+Rstudio(+python2+python3): 8787, 3838(shiny)
  - Python+jupyter notebook/lab: 8888, 9999
  - jupyter notebook mini/r/scipy: 8801, 8802, 8803
  - R+Python+Julia+jupyter notebook/lab: 8800, 9900
- 下載 images-04others
  - datascienceschool/rpython: 裡面有 Ubuntu, R, Python, Rstudio, postgres, jupyter notebook, ssh
  - jenkins: (8082, 50000), (8083, 50001)
  - custom: ubuntu, R, rstudio, Python, jupyter notebook, Julia

---

## alpine版本

- [Alpine - Docker —— 从入门到实践](https://yeasy.gitbook.io/docker_practice/os/alpine)
- [很多官方 docker 镜像都出了基于 alpine 的版本，相较于正常的版本， alpine 版会有什么坑吗？ - V2EX](https://www.v2ex.com/t/581888): 說明為何其他版本存在的必要性。

很多 image 都有 alpine版本的 tag，到底 alpine 算什麼。  <br>

Alpine 操作系統是一個面向安全的輕型 Linux 發行版。  <br>
它不同於通常 Linux 發行版，Alpine 採用了 musl libc 和 busybox 以減小系統的體積和運行時資源消耗，但功能上比 busybox 又完善的多，  <br>
因此得到開源社區越來越多的青睞。在保持瘦身的同時，Alpine 還提供了自己的包管理工具 apk，  <br>
可以通過 https://pkgs.alpinelinux.org/packages 網站上查詢包信息，也可以直接通過 apk 命令直接查詢和安裝各種軟件。

Alpine 由非商業組織維護的，支持廣泛場景的 Linux發行版，  <br>
它特別為資深/重度Linux用戶而優化，關注安全，性能和資源效能。  <br>
Alpine 鏡像可以適用於更多常用場景，並且是一個優秀的可以適用於生產的基礎系統/環境。

目前 Docker 官方已開始推薦使用 Alpine 替代之前的 Ubuntu 做為基礎鏡像環境。  <br>
這樣會帶來多個好處。包括鏡像下載速度加快，鏡像安全性提高，主機之間的切換更方便，佔用更少磁盤空間等。

---

## 下載 images-01

- ubuntu
- centos
- thenetworkchuck/nccoffee:frenchpress
- 下載 DockerCon 範例

### OS system

- [ubuntu - Docker Hub](https://hub.docker.com/_/ubuntu)

```{bash}
docker pull ubuntu
```

- [centos - Docker Hub](https://hub.docker.com/_/centos)

```{bash}
docker pull centos
```

--

### website

**thenetworkchuck/nccoffee sample:**

- [thenetworkchuck/nccoffee - Docker Hub](https://hub.docker.com/r/thenetworkchuck/nccoffee)

```{bash}
// -t, --tty  Allocate a pseudo-TTY。分配偽TTY。
docker run -d -t -p 8081:80 --name nccoffee thenetworkchuck/nccoffee:frenchpress
```

**DockerCon sample:**

- [下載 DockerCon 範例](https://hub.docker.com/repository/docker/littlefish0331/hello-world)

```{bash}
docker pull littlefish0331/hello-world
docker run -p 8080:80 --name DockerCon2020 -d littlefish0331/hello-world
```

**Gitbook:**

- google key word: build a gitbook on docker
- [fellah/gitbook - Docker Hub](https://hub.docker.com/r/fellah/gitbook/): 有在更新。可以參考Dockerfile。
- [yanqd0/gitbook - Docker Hub](https://hub.docker.com/r/yanqd0/gitbook/): 很久沒更新，inspire by fellah/gitbook。
- [10,000小時的修煉之路: 【Docker】Ubuntu / gitbook](http://webcache.googleusercontent.com/search?q=cache:3yNCZ36iXKQJ:maxdev.huder.link/2016/02/dockerubuntu-gitbook.html+&cd=10&hl=zh-TW&ct=clnk&gl=tw): 教學在 ubuntu 上建立環境，架設 Gitbook。環境需要 `apt-get install nodejs, npm, nodejs-legacy`，接著可開始安裝 gitbook。

```{bash}
docker pull fellah/gitbook

// 記得先建立好連動資料夾，並把權限開啟。
docker run --name FAE_no72_gitbook -v /datamount/Gitbook/FAE_no72:/srv/gitbook -p 4001:4000 -d fellah/gitbook
```

**Grafana:**

- [grafana/grafana - Docker Hub](https://hub.docker.com/r/grafana/grafana)
- [New Docker Install with persistent storage, Permission problem - Support - Grafana Community](https://community.grafana.com/t/new-docker-install-with-persistent-storage-permission-problem/10896/3)

```{bash}
docker pull grafana/grafana
docker run --name=grafana -v /datamount/grafana:/var/lib/grafana -p 3000:3000 -d grafana/grafana

// 家目錄的位置不太一樣
cd /usr/share/grafana/
```

---

## 下載 images-02Database

### MSSQL: SQL SERVER

- [Microsoft SQL Server - Docker Hub](https://hub.docker.com/_/microsoft-mssql-server)
- [KingKong Bruce記事: 一次就愛上MS SQL Server for Linux](https://blog.kkbruce.net/2017/12/ms-sql-server-for-linux.html#.XylSRCgzaUk)
- [在 Docker 下建立並使用 MSSQL Server for Linux | Titangene Blog](https://titangene.github.io/article/docker-mssql-server-for-linux.html)

**啟動 container:**

> - ACCEPT_EULA: 需同意授權合約。
> - MSSQL_SA_PASSWORD: 需要是強式密碼並至少 8 個字元。強式密碼需包含：大寫、小寫、數字，符號四者。
> - -p hostPort:containerPort
> - --name: 指定 container 名稱
> - -d: 背景執行
> - -v: (Volume 技術)建立實體資料夾與 container 資料夾的對應關係。

```{bash}
// 建議是先把連動的實體資料夾開好，並把該資料夾的使用者以及群組設定好
// 再執行下面指令
sudo mkdir mssql
chmod 775 mssql (或是 chmod 777 mssql)
```

```{bash}
// ACCEPT_EULA=Y。confirms your acceptance of the End-User Licensing Agreement.
// userid = 'sa'
// MSSQL_PID，可以選擇 MSSQL 的版本。
// SA_PASSWORD=<your_strong_password>
docker run --name mssql \
-e "ACCEPT_EULA=Y" \
-e "SA_PASSWORD=P@ssw0rd" \
-v /datamount/mssql:/var/opt/mssql \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest

//一行指令
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=MSSQL@2020" -v /datamount/mssql:/var/opt/mssql -p 1433:1433 --name mssql -d mcr.microsoft.com/mssql/server:2019-latest
```

**進入 container，並查看 SA 密碼:**

```{bash}
docker exec -it mssql bash
echo $SA_PASSWORD
```

**變更密碼:**

> -S：server
> -U：user name
> -P：password
> -Q：query，執行 SQL 指令後結束 sqlcmd

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P '<YourStrong!Passw0rd>' \
  -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'

// 其實也可以再 container 裡面登入 SA 之後，再改密碼。
// 退出 MSSQL 使用 `quit`。
docker exec -it mssql bash
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'MSSQL@2020'
ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"
quit
```

**備份資料庫:**

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd -S localhost -U SA \
-Q "BACKUP DATABASE <DBname e.g. testDB> TO DISK = N'/var/opt/mssql/data/testdb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

**還原資料庫:**

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd -S localhost -U SA \
-Q "RESTORE DATABASE <DBname e.g. testDB> FROM DISK = N'/var/opt/mssql/data/testdb.bak' WITH  FILE = 1, NOUNLOAD, REPLACE, STATS = 5"
```

--

### Postgress

- [postgres - Docker Hub](https://hub.docker.com/_/postgres)

**參數:**

- POSTGRES_USER: This optional environment variable is used in conjunction with `POSTGRES_PASSWORD` to set a user and its password. This variable will create the specified user with superuser power and a database with the same name. If it is not specified, then the default user of `postgres` will be used.
- POSTGRES_DB: This optional environment variable can be used to define a different name for the default database that is created when the image is first started. If it is not specified, then the value of `POSTGRES_USER` will be used

**啟動 container:**

```{bash}
docker run --name some-postgres -e PGDATA=//data/pgdata -e POSTGRES_PASSWORD=Postgres@2020 -v /datamount/postgres:/var/lib/postgresql -p 5432:5432 -dit postgres

// 可以設定 user_name
docker run --name some-postgres \
-e PGDATA=//data/pgdata \
-e POSTGRES_USER=NCHC \
-e POSTGRES_PASSWORD=Postgres@2020 \
-v /datamount/postgres:/var/lib/postgresql \
-p 5432:5432  \
-dit postgres
```

**登入:**

```{bash}
docker exec -it some-postgres bash

  > psql -d postgres -U postgres  <br>
  > SHOW port;
  > exit
```

--

### mariadb

- [mariadb - Docker Hub](https://hub.docker.com/_/mariadb)
- [Change MySQL default character set to UTF-8 in my.cnf? - Stack Overflow](https://stackoverflow.com/questions/3513773/change-mysql-default-character-set-to-utf-8-in-my-cnf)

**啟動 container:**

```{bash}
docker run --name some-mariadb \
-e MYSQL_ROOT_PASSWORD=mariaDB@2020 \
-v /datamount/mariadb/data:/var/lib/mysql \
-v /datamount/mariadb/conf.d:/etc/mysql/conf.d \
-p 3307:3306 \
-d mariadb

docker run --name some-mariadb -e MYSQL_ROOT_PASSWORD=mariaDB@2020 -v /datamount/mariadb/data:/var/lib/mysql -v /datamount/mariadb/conf.d:/etc/mysql/conf.d -p 3307:3306 -d mariadb
```

#### mariadb編碼

**進入 container、mariaDB:**

```{bash}
docker exec -it some-mariadb bash
mysql -u root -p

  > show databases;
  > exit
```

**查看 mariaDB 的 character-set-server 和 collation-server:**

```{bash}
docker exec -it some-mariadb bash
mysql -u root -p

  > SELECT @@character_set_database, @@collation_database;
  > SELECT DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME FROM INFORMATION_SCHEMA.SCHEMATA; //另一種作法
  >  <br>
  > show variables like 'char%';
  > show variables like 'collation%';
  > exit
```

**修改 Configuration file 與結果:**

即連動資料夾下，新增 my.cnf，修改裡面內容。  <br>
修改之後要重啟 container。

```{mysql}
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8


[mysqld]
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
```

![mariadb_setting00_mycnf](./image/mariadb_setting00_mycnf.jpg)  <br>
![mariadb_setting01](./image/mariadb_setting01.jpg)  <br>
![mariadb_setting02](./image/mariadb_setting02.jpg)  <br>

--

### 下載 MySQL

- [mysql - Docker Hub](https://hub.docker.com/_/mysql?tab=description)
- 如果連動的資料夾沒有建立，docker會自動建立幫忙建立該路徑的資料夾。  <br>
- 密碼無法登入的問題請看以下解決方法。
- 如果不想要有密碼問題就 pull 5.7.31版的 MySQL。

```{bash}
docker run --name some-mysql \
--env MYSQL_ROOT_PASSWORD=MYSQL@2020 \
-v /datamount/mysql/data:/var/lib/mysql \
-v /datamount/mysql/conf:/etc/mysql/conf.d \
-p 3306:3306 \
--detach mysql:latest

//一行指令
docker run --name some-mysql --env MYSQL_ROOT_PASSWORD=MYSQL@2020 -v /datamount/mysql/data:/var/lib/mysql -v /datamount/mysql/conf:/etc/mysql/conf.d -p 3306:3306 --detach mysql:latest
```

#### 密碼無法登入的問題

這主要是因為 Mysql 版本的問題。密碼加密的方式不同。

- [MySQL 8.0 的新密碼加密 plugin 導致 PHP 連線失敗 - Zeroplex 生活隨筆](https://blog.zeroplex.tw/2019/07/mysql-80-plugin-php.html)
- [連線 MySQL 8.0 時，加密方式不相容的解決方法 | IT人](https://iter01.com/443370.html)
- [Andreas Geisler - Berlin based Full Stack Software Developer](http://www.andreasgeisler.com/blog/fatal-error-uncaught-pdoexception-the-server-requested-authentication-method-unknown-to-the-client-caching_sha2_password/2018/11/)
- [Upgrading to MySQL 8.0 : Default Authentication Plugin Considerations | MySQL Server Blog](https://mysqlserverteam.com/upgrading-to-mysql-8-0-default-authentication-plugin-considerations/)
- [MySQL密碼不能登錄的解決辦法 - 每日頭條](https://kknews.cc/zh-tw/code/b2jromj.html)

**docker run:**

到連動資料夾 /datamount/mysql/conf 底下，建立 my.cnf，
在 my.cnf 加上下列資訊，然後重啟 container。  <br>

```{my.cnf}
[mysqld]
default-authentication-plugin = mysql_native_password
```

```{bash}
docker restart some-mysql
```

這樣就可以在 VM 上面，用輸入密碼的方式進入 mysql。
但如果想要在外面用密碼方式登入還需要進入 mysql 做設定。

```{bash}
docker exec -it some-mysql bash
mysql -u root -p
```

```{mysql}
// 看一下 password 的加密方式。
use mysql;
SELECT user, authentication_string, host from user;

// 設定可以用 mysql_native_password 加密方式登入。
// ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'MYSQL@2020';
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'MYSQL@2020';
FLUSH PRIVILEGES;
```

**docker-compose:**

或是修改 docker-compose.yml 的 mysql 服務部分，新增一行。(這方法我還沒嘗試過，但應該可行。)

```{docker compose .yml}
command: --default-authentication-plugin=mysql_native_password
```

> 以下是舊的做法，依舊可用，指示步驟比較多一點。
>  <br>
> **Step01:**
>
> 去 /DBdata/mysql/conf 新增 my.cnf。  <br>
> 新增與修改檔案要用 sudo su 權限。  <br>
>
>
> ```{my.cnf}
> [mysqld]
> skip-grant-tables
> ```
>
> ```{bash}
> docker restart some-mysql
> ```
>
> **Step02:**
>
> 登入 container  <br>
> 登入 mysql，密碼 DAS@mysql2020
>
> ```{bash}
> docker exec -it some-mysql bash
> mysql -u root -p
> ```
>
> **Step03:**
>
> 指定使用資料庫，更新密碼為空。
>
> ```{bash}
> use mysql;
> SELECT user, authentication_string, host from user;
> update user set authentication_string='' where user='root';
> flush privileges;
> ```
>
> **Step04:**
>
> 退出mysql，把第一步的skip-grant-tables註釋。再重啟mysql
>
> ```{bash}
> vim my.cnf
> docker restart some-mysql
> docker exec -it some-mysql bash
> mysql -u root -p
> ```
>
> **Step05:**
>
> 使用原始加密的密碼 + 權限設定
>
> mysql_native_password
>
> ```{bash}
> use mysql;
> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'DAS@mysql2020';
> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'DAS@mysql2020';
> GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;
> ```

#### 看一些變數值

- [查詢 MySQL 對 此帳號 開放(GRANT)哪些權限 | Tsung's Blog](https://blog.longwin.com.tw/2009/06/query-mysql-show-grant-permission-2009/)
- [查詢 MySQL/MariaDB 資料庫的使用者帳號教學 - Office 指南](https://officeguide.cc/how-to-show-list-users-in-a-mysql-mariadb-database/)
- [mysql 查詢 user 帳號及權限 @ Js 片段記憶回顧中心 :: 痞客邦 ::](https://jason0324.pixnet.net/blog/post/42795331-mysql-%E6%9F%A5%E8%A9%A2-user-%E5%B8%B3%E8%99%9F%E5%8F%8A%E6%AC%8A%E9%99%90)

```{bash}
SHOW VARIABLES LIKE 'lower%';

use mysql;
select user, authentication_string, host from user;

// 查有哪些帳號
SELECT User, Host FROM mysql.user;

// 查帳號權限
// SHOW GRANTS FOR <username>;
SHOW GRANTS FOR root;
```

#### 建立新用戶

但是我還不太會給予權限。

```{bash}
CREATE USER 'kvgh'@'%' IDENTIFIED WITH mysql_native_password BY 'kvgh@DB2020';

/// 應該是這個，但是這權限有點太大。
// GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost';
GRANT ALL PRIVILEGES ON *.* TO 'kvgh'@'%';
```

#### 設定 local file 可以上傳

- [MySQL: Enable LOAD DATA LOCAL INFILE - Stack Overflow](https://stackoverflow.com/questions/10762239/mysql-enable-load-data-local-infile)

這樣就可以從程式端上傳資料，也可以用 LOAD 指令上傳local檔案。

```{bash}
SHOW GLOBAL VARIABLES LIKE 'local_infile';
SET GLOBAL local_infile = 1;
// 等價。SET GLOBAL local_infile = 'ON';
// 等價。SET GLOBAL local_infile = true;
```

但是這個設定，經過重啟會失效，所以必須在 my.cnf 中加入指令。

```{my.cnf}
[mysqld]
SET GLOBAL local_infile = 1
```

#### mysql編碼

**進入 container、mysql:**

```{bash}
docker exec -it some-mysql bash
mysql -u root -p

  > show databases;
  > exit
```

**查看 mysql 的 character-set-server 和 collation-server:**

```{bash}
docker exec -it some-mysql bash
mysql -u root -p

  > SELECT @@character_set_database, @@collation_database;
  > SELECT DEFAULT_CHARACTER_SET_NAME, DEFAULT_COLLATION_NAME FROM INFORMATION_SCHEMA.SCHEMATA; //另一種作法
  >  <br>
  > show variables like 'char%';
  > show variables like 'collation%';
  > exit
```

**修改 Configuration file 與結果:**

即連動資料夾下，新增 my.cnf，修改裡面內容。  <br>
修改之後要重啟 container。

- mysqld 是服務端程序 = 作為 MYSQL Server 的操作指令。
- mysql是命令行客户端程序 = 作為 MYSQL Client 的操作指令

```{my.cnf}
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
collation-server = utf8_unicode_ci
init-connect='SET NAMES utf8'
character-set-server = utf8
```

![mysql_setting00_mycnf](./image/mysql_setting00_mycnf.jpg)
![mysql_setting01](./image/mysql_setting01.jpg)
![mysql_setting02](./image/mysql_setting02.jpg)
![mysql_setting03_createtable](./image/mysql_setting03_createtable.jpg)
![mysql_setting03](./image/mysql_setting03.jpg)

**總結 mysql 的 my.cnf:**

```{my.cnf}
[client]
default-character-set = utf8

[mysql]
default-character-set = utf8

[mysqld]
default-authentication-plugin = mysql_native_password
collation-server = utf8_unicode_ci
init-connect = 'SET NAMES utf8'
character-set-server = utf8
local-infile = 1
```

#### BigObject

- [bigobject/bigobject - Docker Hub](https://hub.docker.com/r/bigobject/bigobject)
- [BigObject Documentation](https://docs.bigobject.io/)

```{bash}
docker pull bigobject/bigobject

docker run --name bigobject -p 9090:9090 -p 9091:9091 -p 3306:3306 -v /datamount/bigobject/ds:/srv/bo/ds  -v /datamount/bigobject/file:/srv/bo/file -d bigobject/bigobject
```

#### ElasticSearch

- [elasticsearch - Docker Hub](https://hub.docker.com/_/elasticsearch)
- [Install Elasticsearch with Docker | Elasticsearch Reference [7.8] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker)
- [docker-tutorial/elasticsearch.md at master · jaywcjlove/docker-tutorial](https://github.com/jaywcjlove/docker-tutorial/blob/master/docker/elasticsearch.md)
- [After installing, elasticsearch will not start due to no logging configuration? · Issue #58 · elastic/ansible-elasticsearch](https://github.com/elastic/ansible-elasticsearch/issues/58)

```{bash}
// ES 在 docker 上面沒有 latest tag。
docker pull elasticsearch:7.8.1

// elasticsearch 的 image 沒有做很好，所以在連動資料夾上，會有權限問題。
// 解決方法就是先見一個沒有連動的，進去把要連度的資料夾權限打開(可以順便看一下該資料夾的擁有者與權限)。
// 再 commit 一個新的 image，之後用這個權限開啟的 image 去啟動 container。
docker run --name elasticsearch -e "discovery.type=single-node" -d elasticsearch:7.8.1
docker exec -it elasticsearch bash

  > chmod 777 data/ logs/ -R
  > exit

sudo su

  > mkdir data logs
  > chmod 777 data/ logs/ -R

docker commit elasticsearch elasticsearch:permissions_open
docker stop elasticsearch && docker rm elasticsearch

// 依照新建的 image 建立 container
docker run --name elasticsearch -e "discovery.type=single-node" -v /datamount/elasticsearch/data:/usr/share/elasticsearch/data -v /datamount/elasticsearch/logs:/usr/share/elasticsearch/logs -p 9200:9200 -p 9300:9300 -d elasticsearch:permissions_open
```

**關於 port 用途與設定:** 9200, 9300

- [Elasticsearch port 9200 or 9300? - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/elasticsearch-port-9200-or-9300/72080)
- [和 Elasticsearch 交互 | Elasticsearch: 权威指南 | Elastic](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_talking_to_elasticsearch.html)

結論:

- 9200 is for REST. 所有其他語言可以使用 RESTful API 通過端口 9200 和 Elasticsearch 進行通信，可以用web客戶端訪問Elasticsearch。甚至可以使用 curl 命令來和 Elasticsearch 交互。所以 9200 作為 Http 協議，主要用於外部通訊。
- 9300 for nodes communication. 兩個 Java 客戶端都是通過 9300 端口並使用 Elasticsearch 的原生傳輸協議和集群交互。集群中的節點通過端口 9300 彼此通信。如果這個端口沒有打開，節點則將無法形成一個集群。所以 9300 作為 tcp 協議，jar 之間就是通過 tcp 協議通訊，ES集群之間是通過 9300 進行通訊。

---

## 下載 images-03code

### R+Rstudio

- [rocker/rstudio Tags - Docker Hub](https://hub.docker.com/r/rocker/rstudio/tags)

建議下載 tag 有 ubuntu 的版本。
因為有嘗試過 tag: 3.6.3，結果啟動 container 失敗。  <br>

```{bash}
// 下載 tag: 3.6.3-ubuntu18.04
// 內部有 python2, python3
docker pull rocker/rstudio:3.6.3-ubuntu18.04

docker run --name rstudio_363ubuntu \
-e ROOT=TRUE \
-e PASSWORD=rstudio@2020 \
-e ADD=shiny \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio:3.6.3-ubuntu18.04

// 下載 tag: latest
docker pull rocker/rstudio

docker run --name rstudio_latest \
-e ROOT=TRUE \
-e PASSWORD=rstudio@2020 \
-e ADD=shiny \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio
```

**安裝R套件:**

目前套件可以直接安裝。  <br>
如果以後遇到失敗的狀況，可以嘗試下列方法:

- install.packages(pkgs = "package_name", lib = "your_path)
- install.packages(pkgs = "package_name", lib = "your_path, dependencies = T)
- use terminal > R
- use terminal > sudo su > R

**修改密碼:**

修改密碼的方式很簡單，進到 Rstudio Server 之後，  <br>
上方功能列 > Tools > shell(terminal)
輸入 `passwd`
然後先輸入舊密碼，接著就可以改密碼了

**預設登入或需重新登入:**

- [How to set an environment variable in a running docker container - Stack Overflow](https://stackoverflow.com/questions/27812548/how-to-set-an-environment-variable-in-a-running-docker-container)
- [How to set an enviroment variable on an existing container? · Issue #8838 · moby/moby](https://github.com/moby/moby/issues/8838)
- [Allow `docker start` to take environment variables · Issue #7561 · moby/moby](https://github.com/moby/moby/issues/7561)

有幾種可能的做法可以嘗試!!  <br>
終究還是要去看一下這個 image 的 dockerfile 是如何撰寫的，  <br>
才知道它是怎麼啟動 Rstudio。

- **solution01:** 修改 rserver.conf

> 在 container 中，路徑 /etc/rstudio/ 之下，有一個檔案叫做 rserver.conf。  <br>
> 只要在裡面加入下方指令，就可以自動登入。  <br>
> 但目前在此 image 中嘗試失敗。
>  <br>
> ```{rserver.conf}
> // rserver.conf
> # Server Configuration File
>  <br>
> rsession-which-r=/usr/local/bin/R
> auth-none=1
> server-user=rstudio
> ```

- **solution02:** docker run -e DISABLE_AUTH=TRUE

從這個 image 的 Dockerfile，可以知道在路徑 /etc/cont-init.d 下觀看 userconf.conf 這個檔案，  <br>
就可以知道可以更改環境變數 DISABLE_AUTH=TRUE，  <br>
所以就在一開始 docker run 指令時增加參數如下。

```{bash}
docker run --name rstudio_363ubuntu \
-e ROOT=TRUE \
-e PASSWORD=rstudio@2020 \
-e ADD=shiny \
-e DISABLE_AUTH=TRUE \
-v /datamount/rstudio:/home/rstudio \
-p 3838:3838 -p 8787:8787 \
-d rocker/rstudio:3.6.3-ubuntu18.04
```

依照 userconf.conf 可以知道會做的事情有

> - 修改 rserver.conf，新增 auth-none=1。
> - 將 USER=rstudio，放到環境變數中。

而這些修改，也可以從 docker logs 指令的方式看到該 container 的 log 紀錄。

- **conclusion:**

我作了以下三個 container 並記錄其差異。

```{bash}
docker run --name r_env_no -e ROOT=TRUE -e PASSWORD=rstudio@2020 -p 8787:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

docker run --name r_env_TRUE -e ROOT=TRUE -e PASSWORD=rstudio@2020 -e DISABLE_AUTH=TRUE -p 8788:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

  > 依照 userconf.conf 的指令，會多這一行。  <br>
  > Skipping authentication as requested  <br>
  >  <br>
  > 在 /etc/rstudio 目錄之下，缺少 disable_auth_rserver.conf 檔案，  <br>
  > 因為這個檔案的 `auth-none=1` 被寫入 rserver.conf 中。
  >  <br>
  > cat /etc/environment
  > 環境變數增加 `USER=rstudio`。

docker run --name r_env_FALSE -e ROOT=TRUE -e PASSWORD=rstudio@2020 -e DISABLE_AUTH=FALSE -p 8789:8787 -d rocker/rstudio:3.6.3-ubuntu18.04

// 其餘的 logs 如下

  > [s6-init] making user provided files available at /var/run/s6/etc...exited 0.  <br>
  > [s6-init] ensuring user provided files have correct perms...exited 0.  <br>
  > [fix-attrs.d] applying ownership & permissions fixes...  <br>
  > [fix-attrs.d] done.  <br>
  > [cont-init.d] executing container initialization scripts...  <br>
  > [cont-init.d] userconf: executing...  <br>
  > Adding user `rstudio' to group `sudo' ...  <br>
  > Adding user rstudio to group sudo  <br>
  > Done.  <br>
  > rstudio added to sudoers  <br>
  > [cont-init.d] userconf: exited 0.  <br>
  > [cont-init.d] done.  <br>
  > [services.d] starting services  <br>
  > [services.d] done.

// 路徑 /etc/rstudio 目錄底下的資料有

  > drwxr-xr-x 3 root root 4096 Jun 30 06:09 ./
  > drwxr-xr-x 1 root root 4096 Aug 10 08:41 ../
  > -rw-r--r-- 1 root root   75 Jun 30 06:09 disable_auth_rserver.conf
  > -rw-r--r-- 1 root root   19 Jun 30 06:09 file-locks
  > -rw-r--r-- 1 root root   63 Jun 30 06:09 rserver.conf
  > -rw-r--r-- 1 root root   33 Jun 30 06:09 rsession.conf
  > drwxr-xr-x 2 root root 4096 Jun 30 06:09 themes/

// 環境變數 cat /etc/environment

  > PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"

```

如果有下列兩種需求，解決方式如下:

1. 先設定每次都需要登入，後改為預設登入。container:r_env_no, r_env_FLASE  <br>
2. 先設定預設登入，後改為每次都需要登入。container:r_env_TRUE

> 均先安裝 vim  <br>
> > apt-get update  <br>
> > apt-get install vim
>  <br>
> 1. 先設定每次都需要登入，後改為預設登入  <br>
> `vim /etc/rstudio/rserver.conf`  <br>
> 新增 `auth-none=1`
> `vim /etc/environment`  <br>
> 新增 `USER=rstudio`
> docker restart container 即可。
>  <br>
> 2. 先設定預設登入，後改為每次都需要登入  <br>
> vim /etc/rstudio/rserver.conf  <br>
> 將 auth-none=1 註釋掉，  <br>
> docker restart container 即可。

p.s. 重啟的時候，/etc/environment 環境變數，可能會重複，但是沒差。

**設定使用者與群組登入:**

- [RStudio Server Professional Edition 1.4.693-1](https://docs.rstudio.com/ide/server-pro/latest/index.html)
- [3 Authenticating Users | RStudio Server Professional Edition 1.3.1056-1](https://docs.rstudio.com/ide/server-pro/authenticating-users.html#Restricting-Access-to-Specific-Users)

我還沒有設定過，但是上面的官方手冊教學應該可以。

--

### Python+jupyter notebook/lab

- [jupyter/datascience-notebook Tags - Docker Hub](https://hub.docker.com/r/jupyter/datascience-notebook/tags?page=1&name=python)
- [Selecting an Image — docker-stacks latest documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook)
- [Docker Hub](https://hub.docker.com/layers/jupyter/scipy-notebook/dc9744740e12/images/sha256-9bc3ccb1e56f1ac030d4e71242b3dfc6f3b64e76b41d926dc88eb01303bcf3f9?context=explore)

稍微研究一下 notebook relation!!

![notebook_relation](./image/notebook_relation.svg)

> 如果抓 datascience 系列，會有R。
> scipy, minimal 系列的 tag 看不出版本，但都是基於 base 系列製作。
> 所以最後抓取 base 系列的 python-3.7.6。
>  <br>
> 但是研究一下文檔，其實 base 系列沒有支援 LaTeX 讓我覺得很不妙，
> 可是 minimal, scipy 系列就是沒有 python 版本的 tag。
> 不然建議還是裝 minimal, scipy 系列。
>  <br>
> 後來覺得不妙，只好去一一嘗試，發現這個也是 python3.7.6，jupyter/scipy-notebook:dc9744740e12。

```{bash}
// docker pull jupyter/base-notebook:python-3.7.6
// docker run --name notebook_base -v /datamount/notebook/base_work:/home/jovyan/work -p 8888:8888 -d jupyter/base-notebook:python-3.7.6
```

> - jupyter notebook password: notebook@base2020
>   - 只有 python3.7.6。
>   - 用 `apt list | wc -l` 檢查有102個套件。
>   - 用 `pip list | wc -l` 檢查有82個套件。
>   - 用 `pip freeze | wc -l` 檢查有76個套件。

```{bash}
docker pull jupyter/scipy-notebook:dc9744740e12
docker run --name notebook_python_nb -v /datamount/notebook/python_nb_work:/home/jovyan/work -p 8888:8888 -d jupyter/scipy-notebook:dc9744740e12
docker run --name notebook_python_lab -e JUPYTER_ENABLE_LAB=yes -v /datamount/notebook/python_lab_work:/home/jovyan/work -p 9999:8888 -d jupyter/scipy-notebook:dc9744740e12
```

- jupyter notebook password: notebook@python2020。(都是這個)
  - 只有 python3.7.6。
  - 用 `apt list | wc -l` 檢查有492個套件。
  - 用 `pip list | wc -l` 檢查有132個套件。
  - 用 `pip freeze | wc -l` 檢查有126個套件。

--

### jupyter minimal/r/scipy

- [jupyter/minimal-notebook - Docker Hub](https://hub.docker.com/r/jupyter/minimal-notebook)
- [Docker Hub](https://hub.docker.com/r/jupyter/r-notebook)
- [jupyter/scipy-notebook - Docker Hub](https://hub.docker.com/r/jupyter/scipy-notebook)

> freeze: Output installed packages in requirements format.
> list: List installed packages

- jupyter/minimal-notebook
  - docker run --name notebook_mini -v /datamount/notebook/mini_work:/home/jovyan/work -p 8801:8888 -d jupyter/minimal-notebook
  - 有 python3。
  - 用 `apt list | wc -l` 檢查有375個套件。
  - 用 `pip list | wc -l` 檢查有82個套件。
  - 用 `pip freeze | wc -l` 檢查有76個套件。
  - jupyter notebook password: notebook@mini2020

- jupyter/r-notebook
  - docker run --name notebook_r -v /datamount/notebook/r_work:/home/jovyan/work -p 8802:8888 -d jupyter/r-notebook
  - 有 python3、R。
  - 用 `apt list | wc -l` 檢查有403個套件。
  - 用 `pip list | wc -l` 檢查有82個套件。
  - 用 `pip freeze | wc -l` 檢查有76個套件。
  - 進入R檢查`installed.packages() %>% str`則有201個套件。
  - jupyter notebook password: notebook@r2020

- jupyter/scipy-notebook
  - docker run --name notebook_scipy -v /datamount/notebook/scipy_work:/home/jovyan/work -p 8803:8888 -d jupyter/scipy-notebook
  - 有 python3。
  - 用 `apt list | wc -l` 檢查有499個套件。
  - 用 `pip list | wc -l` 檢查有142個套件。
  - 用 `pip freeze | wc -l` 檢查有136個套件。
  - jupyter notebook password: notebook@scipy2020

--

### R+Python+Julia+jupyter notebook

- [jupyter's Profile - Docker Hub](https://hub.docker.com/u/jupyter)
- [Selecting an Image — docker-stacks latest documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook)
- [jupyter/docker-stacks: Ready-to-run Docker images containing Jupyter applications](https://github.com/jupyter/docker-stacks)

**啟動 container:**

```{bash}
// 以 jupyter/datascience-notebook 這個 images 為例。
// 其他 jupyter 帳號下的 images 應該都差不多，可以再研究一下。
docker pull jupyter/datascience-notebook

// 啟動 container
// port 8800: 使用 jupyter notebook
// port 9900: 使用 jupyter lab
docker run --name rpyju_ds_nb -v /datamount/rpyju/dsnb:/home/jovyan/work -p 8800:8888 -d jupyter/datascience-notebook
docker run --name rpyju_ds_lab -e JUPYTER_ENABLE_LAB=yes -v /datamount/rpyju/dslab:/home/jovyan/work -p 9900:8888 -d jupyter/datascience-notebook
```

**登入:**

- [Running a notebook server — Jupyter Notebook 6.1.1 documentation](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html)

登入都需要 token，取得 token 的方式就是到 container 裡面呼叫。

```{bash}
docker exec -it rpyju_ds_lab bash
//docker exec -it rpyju_ds_nb bash

// 列出 token
jupyter notebook list

// 使用密碼登入。需要 restart container 才會生效。
jupyter notebook password

  > rpyju@lab2020
  > rpyju@nb2020
```

---

## 下載 images-04others

### datascienceschool/rpython

- [datascienceschool/rpython - Docker Hub](https://hub.docker.com/r/datascienceschool/rpython/)
- [datascienceschool/docker_rpython: dockerfile for datascienceschool/rpython2 and datascienceschool/rpython3](https://github.com/datascienceschool/docker_rpython)

這個 image 裡面有 Ubuntu, R, Python, Rstudio, postgres, jupyter notebook, ssh等等，  <br>
是很大一包的 image，共18G。

```{bash}
// 啟動 container
docker run --name=rpython \
-p 8787:8787 \
-v e:\container_folder\rpython:/home/dockeruser/rpython \
-dit datascienceschool/rpython

docker run --name=rpython \
-p 8787:8787 \
-dit datascienceschool/rpython
```

因為其 docker hub 沒有寫 Docker file 的資訊，  <br>
所以後來我是去 github 找，  <br>
發現在 docker_rpython/02_rpython 目錄下，supervisord.conf 檔案中有做設定。

> [program:rserver]
> command=/usr/lib/rstudio-server/bin/rserver --auth-none 1 --server-user USER_ID --server-app-armor-enabled 0
> stdout_logfile=/var/log/supervisor/%(program_name)s.log
> stderr_logfile=/var/log/supervisor/%(program_name)s.log
> startsecs=0
> autorestart=false
> user=USER_ID

所以要去修改 supervisord.conf 檔案的設定。  <br>
發現 docker_rpython/02_rpythona/Dockerfile 有標示這個檔案在哪，`/etc/supervisor/supervisord.conf`。

因此，如果不想要自動登入，就去改成 `--auth-none 0`，  <br>
重啟 container 即可。

**改密碼:**

就登入之後，到 Rstudio > Tools > Shell

```{bash}
passwd
```

--

### jenkins

- Jenkins
  - [jenkins - Docker Hub](https://hub.docker.com/_/jenkins): 直接搜尋到的這個不是官方官網推薦的。
  - [jenkins/jenkins - Docker Hub](https://hub.docker.com/r/jenkins/jenkins/)
  - [jenkins/blueocean - Docker Hub](https://hub.docker.com/r/jenkins/blueocean)
  - [Installing Jenkins](https://www.jenkins.io/doc/book/installing/): 官網安裝指南。
  - [使用docker in docker - jeremy的技术点滴](https://jeremyxu2010.github.io/2019/02/使用docker-in-docker/)

- Blue Ocean
  - [Blue Ocean](https://www.jenkins.io/doc/book/blueocean/)
  - [Jenkins BlueOcean初探_neven7的專欄-CSDN博客_jenkins blueocean](https://blog.csdn.net/neven7/article/details/53645215)

官方建議是直接安裝帶有 Blue Ocean UI 介面的 image，  <br>
因為它是 Long-Term Support (LTS) release of Jenkins (which is production-ready) bundled with all Blue Ocean plugins and features.  <br>
This means that you do not need to install the Blue Ocean plugins separately. 。

> 當然也可以自行安裝比較小包的 jenkins/jenkins (on Docker Hub)，  <br>
> 然後再自行加上插件(plugins)。

以下介紹兩種方式的安裝，最終都為 Jenkins + Blue Ocean。

**介紹 Blue Ocean:**

Jenkins是一款Java開發的跨平台持續集成和持續發布的開源項目，它具有如下特徵:

> - 安裝及遷移方便：安裝直接部署 war 包，遷移只需替換 JENKINS_HOME 目錄。
> - 配置方便：可視化後台操作。
> - 豐富的插件生態圈：比如 git, junit, jacoco 等。
> - 可擴展：自定義插件。
> - 分佈式：支持 Master-Slave。

Jenkins已經作為各大公司進行CI/CD的首選工具。  <br>
Jenkins UI從2006年-2016年，幾乎沒有變化。

為了適應 Jenkins Pipeline 和 Freestyle jobs任務，Jenkins 推出了 BlueOcean UI，  <br>
其目的就是讓程序員執行任務時，降低工作流程的複雜度和提升工作流程的清晰度，它具有如下特徵:

> - 清晰的可視化，對CI/CD pipelines, 可以快速直觀的觀察項目pipeline狀態。
> - pipeline可編輯(開發中)，可視化編輯pipeline，現在只能通過配置中Pipeline的Pipeline script編輯。
> - pipeline精確度，通過UI直接介入pipeline的中間問題。
> - 集成代碼分支和pull請求。

**啟動 jenkins/jenkins image:**

記得連動的資料夾，權限要先打開。  <br>
如果一開始 Getting Started，有安裝 plugins 失敗，之後會可以再按 Retry，  <br>
所以不用太緊張。  <br>

不然砍掉重建也可以XD~ 我就這樣做了好多次。

```{bash}
// docker pull jenkins/jenkins
docker pull jenkins/jenkins:lts

docker run \
--name jenkins-only \
-v /datamount/jenkins-only/jenkins_home:/var/jenkins_home \
-p 8082:8080 \
-p 50000:50000 \
-d jenkins/jenkins:lts

// 進入 container 觀看密碼。
docker exec -it jenkins-only bash

  > cat /var/jenkins_home/secrets/initialAdminPassword
```

管理 Jenkins > 管理外掛程式 > 可用的，filter:blue ocean > 勾選 Blue Ocean。

**啟動 jenkinsci/blueocean image:**

(此為官方教學文件指示，更多解釋請看官方文件)。  <br>
為了讓容器裡也可以操作 docker 鏡像，又不想污染宿主機上的 docker 鏡像，要使用 docker in docker(dind) 的方案。  <br>

記得連動的資料夾，權限要先打開。  <br>

之後透過此 container，就可以連結 localhost 的 Docker Server。

```{bash}
// 建立 Jenkins 網路
docker network create jenkins

// docker-in-docker，這個無法用 docker exec 進入唷!
// port 2376，不用去 TWCC 上面開啟。
// --network network                Connect a container to a network
// --network-alias list             Add network-scoped alias for the container
docker run \
--name jenkins-docker \
--network jenkins \
--network-alias docker \
--env DOCKER_TLS_CERTDIR=/certs \
--volume /datamount/jen-bo-dind/jenkins-data:/var/jenkins_home \
--volume /datamount/jen-bo-dind/jenkins-docker-certs:/certs/client \
--publish 2376:2376 \
--privileged \
-d docker:dind
```

```{bash}
// 安裝 jenkinsci/blueocean。
// env 那三行，讓我們可以順利接到 localhost 的 Docker Server。
docker run \
--name jenkins-blueocean-dind \
--network jenkins \
--env DOCKER_HOST=tcp://docker:2376 \
--env DOCKER_CERT_PATH=/certs/client \
--env DOCKER_TLS_VERIFY=1 \
--volume /datamount/jen-bo-dind/jenkins-data:/var/jenkins_home \
--volume /datamount/jen-bo-dind/jenkins-docker-certs:/certs/client:ro \
--publish 8083:8080 \
--publish 50001:50000 \
-d jenkinsci/blueocean

// 進入 container 觀看密碼和docker version
docker exec -it jenkins-blueocean-dind bash

  > docker version
  > cat /var/jenkins_home/secrets/initialAdminPassword
```

---

## END

- jupyter notebook/lab，之後要學怎麼用在
  - docker 指令中設定 token or password
  - dockerfile 指令中設定 token or password

- [【Docker】建立 Jupyter Container. 這邊使用jupyter/datascience-notebook(https:/… | by JiHung Lin | Medium](https://medium.com/@jihung.mycena/docker-%E5%BB%BA%E7%AB%8B-jupyter-container-8084748e2f33)
- 了解 postgres 如何新增使用者

- Jenkins 學習
  - [安裝 Jenkins · 持續整合與自動化測試](http://jenkins.readbook.tw/jenkins/basic/install.html)
  - [[ DevOps ] Jenkins 基本設定及 Pipeline 腳本教學](https://oranwind.org/-devops-jenkins-yu-centos-ubuntu-an-zhuang-jiao-xue/)


docker network ls

systemctl -a | grep docker

named volume
	docker volume create
	docker volume ls
Host Volume

資訊架構(IA, Information Architecture)
人工AI


