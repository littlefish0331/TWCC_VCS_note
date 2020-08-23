# Grafana

記錄我在 VCS 上操作 container:Grafana 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Database > Grafana 複製過來。

--

**Content:**

<!-- TOC -->

- [Grafana](#grafana)
  - [啟動 container](#啟動-container)
  - [修改 root 密碼](#修改-root-密碼)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [grafana/grafana - Docker Hub](https://hub.docker.com/r/grafana/grafana)
- [New Docker Install with persistent storage, Permission problem - Support - Grafana Community](https://community.grafana.com/t/new-docker-install-with-persistent-storage-permission-problem/10896/3)

---

## 啟動 container

```{bash}
docker pull grafana/grafana
docker run --name=grafana -v /datamount/grafana:/var/lib/grafana -p 3000:3000 -d grafana/grafana

// 家目錄的位置不太一樣
cd /usr/share/grafana/
```

---

## 修改 root 密碼

預設登入的帳密是 admin/admin。  <br>
第一次登入之後會要求更改 admin 的密碼，可以按 skip。

---

## END
