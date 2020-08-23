# PostgreSQL

記錄我在 VCS 上操作 container:PostgreSQL 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Database > PostgreSQL 複製過來。

--

**Content:**

<!-- TOC -->

- [PostgreSQL](#postgresql)
  - [啟動 container](#啟動-container)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [postgres - Docker Hub](https://hub.docker.com/_/postgres)

## 啟動 container

**參數:**

- POSTGRES_USER: This optional environment variable is used in conjunction with `POSTGRES_PASSWORD` to set a user and its password. This variable will create the specified user with superuser power and a database with the same name. If it is not specified, then the default user of `postgres` will be used.
- POSTGRES_DB: This optional environment variable can be used to define a different name for the default database that is created when the image is first started. If it is not specified, then the value of `POSTGRES_USER` will be used

記得先把連動的實體資料夾開好，並把該資料夾的使用者以及群組設定好，再執行下面指令。

<!-- POSTGRES_PASSWORD=Postgres@2020 -->

```{bash}
// 可以設定 user_name
docker run \
--name some-postgres \
-e PGDATA=//data/pgdata \
-e POSTGRES_USER=NCHC \
-e POSTGRES_PASSWORD=<your_password> \
-v /datamount/postgres:/var/lib/postgresql \
-p 5432:5432  \
-dit postgres

// 一行指令
docker run --name some-postgres -e PGDATA=//data/pgdata -e POSTGRES_PASSWORD=<your_password> -v /datamount/postgres:/var/lib/postgresql -p 5432:5432 -dit postgres
```

**登入:**

```{bash}
docker exec -it some-postgres bash

  > psql -d postgres -U postgres  <br>
  > SHOW port;  <br>
  > exit
```

---

## END
