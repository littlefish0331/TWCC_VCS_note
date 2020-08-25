# Jenkins

記錄我在 VCS 上操作 container:Jenkins 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Program > Jenkins 複製過來。

--

**Content:**

<!-- TOC -->

- [Jenkins](#jenkins)
  - [Blue Ocean](#blue-ocean)
  - [啟動 container](#啟動-container)
    - [啟動 jenkins/jenkins image](#啟動-jenkinsjenkins-image)
    - [啟動 jenkinsci/blueocean image](#啟動-jenkinsciblueocean-image)
  - [END](#end)

<!-- /TOC -->

--

**refernce:**

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

---

## Blue Ocean

Jenkins是一款Java開發的跨平台持續集成和持續發布的開源項目，它具有如下特徵:

- 安裝及遷移方便：安裝直接部署 war 包，遷移只需替換 JENKINS_HOME 目錄。
- 配置方便：可視化後台操作。
- 豐富的插件生態圈：比如 git, junit, jacoco 等。
- 可擴展：自定義插件。
- 分佈式：支持 Master-Slave。

Jenkins已經作為各大公司進行CI/CD的首選工具。  <br>
Jenkins UI從2006年-2016年，幾乎沒有變化。

為了適應 Jenkins Pipeline 和 Freestyle jobs任務，Jenkins 推出了 BlueOcean UI，  <br>
其目的就是讓程序員執行任務時，降低工作流程的複雜度和提升工作流程的清晰度，它具有如下特徵:

- 清晰的可視化，對CI/CD pipelines, 可以快速直觀的觀察項目pipeline狀態。
- pipeline可編輯(開發中)，可視化編輯pipeline，現在只能通過配置中Pipeline的Pipeline script編輯。
- pipeline精確度，通過UI直接介入pipeline的中間問題。
- 集成代碼分支和pull請求。

---

## 啟動 container

以下介紹兩種方式的安裝，最終都為 Jenkins + Blue Ocean。

- jenkins/jenkins
- jenkinsci/blueocean

--

### 啟動 jenkins/jenkins image

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

--

### 啟動 jenkinsci/blueocean image

(此為官方教學文件指示，更多解釋請看官方文件)。  <br>
為了讓容器裡也可以操作 docker 鏡像，又不想污染宿主機上的 docker 鏡像，要使用 docker in docker(dind) 的方案。

記得連動的資料夾，權限要先打開。  <br>
之後透過此 container，就可以連結 localhost 的 Docker Server。

```{bash}
// 建立 Jenkins 網路
docker network create jenkins
```

docker-in-docker，這個無法用 docker exec 進入唷!  <br>
port 2376，不用去 TWCC 上面開啟。

--network network                Connect a container to a network
--network-alias list             Add network-scoped alias for the container

```{bash}
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

安裝 jenkinsci/blueocean。  <br>
env 那三行，讓我們可以順利接到 localhost 的 Docker Server。

```{bash}
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
