# MSSQL

記錄我在 VCS 上操作 container:MSSQL 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Database > MSSQL 複製過來。

--

**Content:**

<!-- TOC -->

- [MSSQL](#mssql)
  - [啟動 container](#啟動-container)
  - [密碼-查看、變更](#密碼-查看變更)
    - [進入 container，並查看 SA 密碼](#進入-container並查看-sa-密碼)
    - [變更密碼](#變更密碼)
  - [資料庫-備份、還原](#資料庫-備份還原)
    - [備分](#備分)
    - [還原](#還原)
  - [Collation(定序)](#collation定序)
    - [伺服器層級定序](#伺服器層級定序)
    - [資料庫層級定序](#資料庫層級定序)
    - [資料行層級定序](#資料行層級定序)
    - [運算式層級定序](#運算式層級定序)
  - [建立資料庫](#建立資料庫)
    - [設定Collation](#設定collation)
    - [更改collation](#更改collation)
  - [是否有人在連接DB](#是否有人在連接db)
  - [PORT](#port)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [Microsoft SQL Server - Docker Hub](https://hub.docker.com/_/microsoft-mssql-server)
- [KingKong Bruce記事: 一次就愛上MS SQL Server for Linux](https://blog.kkbruce.net/2017/12/ms-sql-server-for-linux.html#.XylSRCgzaUk)
- [在 Docker 下建立並使用 MSSQL Server for Linux | Titangene Blog](https://titangene.github.io/article/docker-mssql-server-for-linux.html)

## 啟動 container

> 環境參數:  <br>
>  <br>
> - ACCEPT_EULA: 需同意授權合約。confirms your acceptance of the End-User Licensing Agreement.  <br>
> - MSSQL_PID，可以選擇 MSSQL 的版本。
> - SA_PASSWORD: 需要是強式密碼並至少 8 個字元。強式密碼需包含：大寫、小寫、數字，符號四者。(MSSQL_SA_PASSWORD 好像也可以。)  <br>
> - MSSQL_COLLATION: 資料庫 Server collation。
>  <br>
> docker 參數:  <br>
>  <br>
> - --name: 指定 container 名稱  <br>
> - -v: (Volume 技術)建立實體資料夾與 container 資料夾的對應關係。  <br>
> - -p hostPort:containerPort。  <br>
> - -d: 背景執行。

記得先把連動的實體資料夾開好，並把該資料夾的使用者以及群組設定好，再執行下面指令。

```{bash}
sudo mkdir mssql
chmod 775 mssql -R(或是 `chmod 777 mssql -R`)
```

<!-- SA_PASSWORD=MSSQL@2020 -->

```{bash}
// userid = 'sa'
docker run \
--name mssql \
-e "ACCEPT_EULA=Y" \
-e "SA_PASSWORD=<your_password>" \
-e "MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS" \
-v /datamount/mssql:/var/opt/mssql \
-p 1433:1433 \
-d mcr.microsoft.com/mssql/server:2019-latest

//一行指令
docker run --name mssql -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<your_password>" -v /datamount/mssql:/var/opt/mssql -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

---

## 密碼-查看、變更

### 進入 container，並查看 SA 密碼

```{bash}
docker exec -it mssql bash
echo $SA_PASSWORD
```

--

### 變更密碼

> -S：server  <br>
> -U：user name  <br>
> -P：password  <br>
> -Q：query，執行 SQL 指令後結束 sqlcmd。

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd \
  -S localhost -U SA -P '<YourStrong!Passw0rd>' \
  -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'

// 其實也可以在 container 裡面登入 SA 之後，再改密碼。
// 退出 MSSQL 使用 `quit`。
docker exec -it mssql bash
/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'MSSQL@2020'

  > ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"  <br>
  > quit
```

---

## 資料庫-備份、還原

### 備分

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd -S localhost -U SA \
-Q "BACKUP DATABASE <DBname e.g. testDB> TO DISK = N'/var/opt/mssql/data/testdb.bak' WITH NOFORMAT, NOINIT, NAME = 'demodb-full', SKIP, NOREWIND, NOUNLOAD, STATS = 10"
```

### 還原

```{bash}
docker exec -it mssql /opt/mssql-tools/bin/sqlcmd -S localhost -U SA \
-Q "RESTORE DATABASE <DBname e.g. testDB> FROM DISK = N'/var/opt/mssql/data/testdb.bak' WITH FILE = 1, NOUNLOAD, REPLACE, STATS = 5"
```

---

## Collation(定序)

- [定序與 Unicode 支援 - SQL Server | Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/relational-databases/collations/collation-and-unicode-support?view=sql-server-ver15)

> 不要問這是什麼，因為我也還不是很懂XD~  <br>
> 總之就是資料庫關於編碼方面的設定。  <br>

Collation 要選擇"Chinese_Taiwan_Stroke_CI_AS"。  <br>
它這個代碼有特別的意思，但我不知道是甚麼><

![mssql_setting](./image/mssql_setting.png)

p.s. 有一種是"bopomofo"，代表ㄅㄆㄇㄈ。

--

後來有查到了，但我還是看不太懂。

SQL Server 中的定序會提供資料的排序規則、大小寫和區分腔調字屬性。  <br>
與字元資料類型 (例如 char 和 varchar) 搭配使用的定序會指示字碼頁，以及可針對該資料類型表示的對應字元。

定序指定位元模式，代表資料集中的每一個字元。 定序也可以決定排序和比較資料的規則。

==**總之我覺得定序類似於資料庫領域的編碼，但不僅僅只處理編碼的問題。**==

依照參考連結，Database 的 Collation 名稱有以下涵義:

- _CI_AS: 不區分大小寫、區分腔調字、不區分假名、不區分全半形
    - _AS: 區分腔調字。
    - _CI: 不區分大小寫。Case Insensitiveve。
    - _CS: 區分大小寫。Case Sensitve。

--

**定序層級:**

- 伺服器層級定序，Server。
- 資料庫層級定序，Database。
- 資料行層級定序，Table。
- 運算式層級定序。

**結論:**

- [在 Linux 上設定 SQL Server 的環境變數 - SQL Server | Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/linux/sql-server-linux-configure-environment-variables?view=sql-server-ver15)
- [在 Linux 上設定 SQL Server 設定 - SQL Server | Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/linux/sql-server-linux-configure-mssql-conf?view=sql-server-ver15)

> docker 語法網路上說單雙引號有差，我常是下列做法都是沒有問題的。  <br>
> - -e "MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS"  <br>
> - -e 'MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS'  <br>
> - -e MSSQL_COLLATION="Chinese_Taiwan_Stroke_CI_AS"  <br>
> - -e MSSQL_COLLATION='Chinese_Taiwan_Stroke_CI_AS'  <br>
>  <br>
> 至於 mssql-conf 的作法，可能要有UI介面會比較好。

**嘗試的做法如下:**

| 作法                         | 成功/失敗 | 影響層級          |
|------------------------------|-----------|-------------------|
| docker -e                    | SUCCESS   | Server → Database |
| SSMS建立                     | FAIL      | -                 |
| SSMS建立+設定collation       | SUCCESS   | Database          |
| create DB                    | FAIL      | -                 |
| create DB+collation          | SUCCESS   | Database          |
| create DB+alter DB+collation | SUCCESS   | Database          |

**可以用於檢查的SQL:**

```{SQL}
/*檢查 server collation */
/*兩個指令等價*/
SELECT SERVERPROPERTY('Collation');
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

```{SQL}
/*Check DB collation*/
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('db_name','collation'));
```

--

### 伺服器層級定序

依照作業系統 (OS) 地區設定決定的預設定序指定，包括其 Windows 和 SQL 語言代碼識別碼 (LCID)。  <br>
而下面這兩個是可以接受中文(台灣)的定序。

- Chinese_Taiwan_Bopomofo_CI_AS
- Chinese_Taiwan_Stroke_CI_AS

預設伺服器定序是在 SQL Server 設定期間決定，因此會成為系統資料庫和所有使用者資料庫的預設定序

定序指派給"伺服器"後，是無法任意變動的，想變的話要砍掉重練。

**查詢 SQL Server 執行個體的伺服器定序:**

```{SQL}
SELECT CONVERT(varchar, SERVERPROPERTY('collation'));
```

**查詢伺服器的所有可用定序:**

```{SQL}
SELECT * FROM sys.fn_helpcollations();
```

--

### 資料庫層級定序

當建立資料庫時，您可以使用 `CREATE DATABASE` 的 `COLLATE` 子句或 `ALTER DATABASE` 陳述式來指定預設資料庫定序。  <br>
如果未指定定序，則會將伺服器定序指派給資料庫。

另外，除非變更伺服器的定序，否則無法變更系統資料庫的定序。

資料庫定序是用於資料庫中的所有中繼資料，且是資料庫中所使用全部字串資料行、暫存物件、變數名稱和任何其他字串的預設值。

**使用類似下列 `ALTER DATABASE` 陳述式來變更使用者資料庫的定序:**

```{SQL}
ALTER DATABASE <db_name> COLLATE <collation_name>;
```

**擷取指定資料庫的目前定序:**

```{SQL}
SELECT CONVERT (VARCHAR(50), DATABASEPROPERTYEX('database_name','collation'));
```

--

### 資料行層級定序

建立或改變資料表時，可以使用 `COLLATE` 子句來指定每個字元字串資料行的定序。  <br>
如果您沒有指定定序，就會將資料庫的預設定序指派給資料行。

**使用類似下列 `ALTER TABLE` 陳述式來變更資料行的定序:**

```{SQL}
ALTER TABLE <db_name> ALTER COLUMN <col_name> NVARCHAR(10) COLLATE <collation_name>;
```

--

### 運算式層級定序

運算式層級定序是在執行陳述式時設定的，而且它們會影響傳回結果集的方式。  <br>
這樣可讓 `ORDER BY` 將結果排序為地區設定特定。

**要執行運算式層級定序，請使用 `COLLATE` 子句:**

```{SQL}
SELECT <col_name> FROM <table_name> ORDER BY <col_name> COLLATE <collation_name>;
```

---

## 建立資料庫

可以建立時設定，也可以建立後更改。  <br>
但是建立後更改，基本上不能是系統資料庫。(要也可以啦~但會很複雜。)

### 設定Collation

- [CREATE DATABASE (Transact-SQL) - SQL Server | Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/t-sql/statements/create-database-transact-sql?view=sql-server-ver15#syntax)

```{SQL}
CREATE DATABASE <database_name>
COLLATE <collation_name>;

/*Create DB+set collation*/
CREATE DATABASE code_set_yes
COLLATE Chinese_Taiwan_Stroke_CI_AS;
```

--

### 更改collation

```{SQL}
/*Alter DB collation*/
ALTER DATABASE <db_name>
COLLATE Chinese_Taiwan_Stroke_CI_AS;
```

---

## 是否有人在連接DB

觀看DB 是否有被連結中。

```{SQL}
IF EXISTS (
    SELECT request_session_id
    FROM sys.dm_tran_locks
    WHERE resource_database_id = DB_ID('model')
    )
BEGIN
PRINT 'Model Database in use!!'

SELECT *
FROM sys.dm_exec_sessions
WHERE session_id IN (
    SELECT request_session_id
    FROM sys.dm_tran_locks
    WHERE resource_database_id = DB_ID('model')
    )
END

ELSE
PRINT 'Model Database not in used.'
```

---

## PORT

MSSQL 如果不是用預設的port(1433)做連線，  <br>
比如使用 port 5001，那麼正確連線方式如下，  <br>
`ip,5001`，它們家是用`,`，不知道有什麼歷史故事XDD~  <br>
總之記錄一下!!

---

## END

補充參數。https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-environment-variables?view=sql-server-ver15

odbc::odbcListDrivers()
library(odbc)
con <- dbConnect(odbc::odbc(), 
                 Driver = "FreeTDS",
                 Server = "103.124.74.106",
                 UID = "sa",
                 PWD = "MSSQL@2020", Port = 1433)
dbDisconnect(conn = con)

con <- dbConnect(odbc::odbc(), 
                 Driver = "FreeTDS",
                 Server = "172.17.0.2",
                 UID = "sa",
                 PWD = "MSSQL@2020", Port = 1433)
