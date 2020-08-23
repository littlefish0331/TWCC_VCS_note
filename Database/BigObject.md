# BigObject

記錄我在 VCS 上操作 container:BigObject 的過程，以及一些問題要如何克服。  
部分是從 DockerHub > Database > BigObject 複製過來。

--

**Content:**

<!-- TOC -->

- [BigObject](#bigobject)
  - [啟動 container](#啟動-container)
  - [END](#end)

<!-- /TOC -->

--

**refernce:**

- [bigobject/bigobject - Docker Hub](https://hub.docker.com/r/bigobject/bigobject)
- [BigObject Documentation](https://docs.bigobject.io/)

---

## 啟動 container

```{bash}
docker pull bigobject/bigobject
docker run \
--name bigobject \
-v /datamount/bigobject/ds:/srv/bo/ds \
-v /datamount/bigobject/file:/srv/bo/file \
-p 9090:9090 \
-p 9091:9091 \
-p 3306:3306 \
-d bigobject/bigobject

// 一行指令
docker run --name bigobject -v /datamount/bigobject/ds:/srv/bo/ds -v /datamount/bigobject/file:/srv/bo/file -p 9090:9090 -p 9091:9091 -p 3306:3306 -d bigobject/bigobject
```

---

## END
