# Mysql-Client

要在 Unbuntu 系統上安裝 mysql-client，才可以直接對不同的 DB(for mysql and mariaDB) 做連線。

---

## install and login

-h, --host=name     Connect to host.

```{}
sudo apt install mysql-client-core-5.7

// 連線
mysql -h 127.0.0.1 -P 3306
mysql -h <IP> -P 3306 -u root -p
```

---

## END
