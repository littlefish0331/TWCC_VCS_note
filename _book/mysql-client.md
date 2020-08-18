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

- [logging - How to see log files in MySQL? - Stack Overflow](https://stackoverflow.com/questions/5441972/how-to-see-log-files-in-mysql): 這是登入記錄或是相關操作的 error，要自己研究測試一下。
