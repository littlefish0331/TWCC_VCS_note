# ElasticSearch

記錄我在 VCS 上操作 container:ElasticSearch 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Database > ElasticSearch 複製過來。

--

**Content:**

<!-- TOC -->

- [ElasticSearch](#elasticsearch)
  - [啟動 container](#啟動-container)
  - [關於 port 用途與設定(9200, 9300)](#關於-port-用途與設定9200-9300)
  - [END](#end)

<!-- /TOC -->

--

**reference:**

- [elasticsearch - Docker Hub](https://hub.docker.com/_/elasticsearch)
- [Install Elasticsearch with Docker | Elasticsearch Reference [7.8] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker)
- [docker-tutorial/elasticsearch.md at master · jaywcjlove/docker-tutorial](https://github.com/jaywcjlove/docker-tutorial/blob/master/docker/elasticsearch.md)
- [After installing, elasticsearch will not start due to no logging configuration? · Issue #58 · elastic/ansible-elasticsearch](https://github.com/elastic/ansible-elasticsearch/issues/58)

---

## 啟動 container

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

---

## 關於 port 用途與設定(9200, 9300)

**reference:**

- [Elasticsearch port 9200 or 9300? - Elastic Stack / Elasticsearch - Discuss the Elastic Stack](https://discuss.elastic.co/t/elasticsearch-port-9200-or-9300/72080)
- [和 Elasticsearch 交互 | Elasticsearch: 权威指南 | Elastic](https://www.elastic.co/guide/cn/elasticsearch/guide/current/_talking_to_elasticsearch.html)

結論:

- 9200 is for REST. 所有其他語言可以使用 RESTful API 通過端口 9200 和 Elasticsearch 進行通信，可以用web客戶端訪問Elasticsearch。甚至可以使用 curl 命令來和 Elasticsearch 交互。所以 9200 作為 Http 協議，主要用於外部通訊。
- 9300 for nodes communication. 兩個 Java 客戶端都是通過 9300 端口並使用 Elasticsearch 的原生傳輸協議和集群交互。集群中的節點通過端口 9300 彼此通信。如果這個端口沒有打開，節點則將無法形成一個集群。所以 9300 作為 tcp 協議，jar 之間就是通過 tcp 協議通訊，ES集群之間是通過 9300 進行通訊。

---

## END
