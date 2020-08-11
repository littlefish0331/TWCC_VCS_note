## Docker Office Tutorial

Docker-doc

- [Docker overview | Docker Documentation](https://docs.docker.com/get-started/overview/)

### Docker overview

Get started > Docker overview。

```{bash}
sudo docker run -i -t ubuntu /bin/bash

//-i, --interactive                    Keep STDIN open even if not attached。讓容器的標準輸入保持打開。
//-t, --tty                            Allocate a pseudo-TTY。讓Docker分配一個虛擬終端（pseudo-tty）並綁定到容器的標準輸入上
//此指令會進入container中，前面會有container ID。
//-d, --detach                         Run container in background and print container ID
```

1. **If you do not have the ubuntu image locally, Docker pulls it from your configured registry, as though you had run docker pull ubuntu manually.**

2. Docker creates a new container, as though you had run a docker container create command manually.

3. **Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem.**

4. Docker creates a network interface to connect the container to the default network, since you did not specify any networking options. This includes assigning an IP address to the container. **By default, containers can connect to external networks using the host machine's network connection.**

5. **Docker starts the container and executes /bin/bash. Because the container is running interactively and attached to your terminal (due to the -i and -t flags), you can provide input using your keyboard while the output is logged to your terminal.**

6. **When you type exit to terminate the /bin/bash command, the container stops but is not removed. You can start it again or remove it.**

--

- [程式扎記: Docker Practice - Container](http://puremonkey2010.blogspot.com/2015/05/docker-practice-container.html)

當利用 docker run 來建立容器時，Docker 在後臺執行的標準操作包括：

1. 檢查本地是否存在指定的映像檔，不存在就從公有倉庫下載
2. 利用映像檔建立並啟動一個容器
3. 分配一個檔案系統，並在唯讀的映像檔層外面掛載一層可讀寫層
4. 從宿主主機設定的網路橋界面中橋接一個虛擬埠到容器中去
5. 從位址池中設定一個 ip 位址給容器
6. 執行使用者指定的應用程式
7. 執行完畢後容器被終止

將容器輸出成tar檔。

```{bash}
docker export <container_name or container_ID> > <XXX.tar>
```

### Quickstart - Part 1

Get started > Quickstart > Part 1: Orientation and setup > 參考 video: DockerCon2020

- [Orientation and setup | Docker Documentation](https://docs.docker.com/get-started/)
- [roninjs/docker-hello-world](https://github.com/roninjs/docker-hello-world): 王八蛋，要先給這個 Github 阿!!不然怎麼跟著做。

架網站(or網頁?) node 服務，需要packages.json等一些設置，所以才需要原作者的 Github，  <br>
總之，如果資源都有給充足的話，是一個很舒服的 tutorial。

```{bash}
sudo usermod -a -G docker ubuntu  // 讓 ubuntu 這個使用者可以不用每次使用 docker 指令都要打 sudo。將目前使用者加到docker群組。
git clone https://github.com/roninjs/docker-hello-world.git
cd docker-hello-world/
cat Dockerfile  // 看一下Dockerfile要如何寫。
cat package.json
cat package-lock.json
cat Dockerfile-test
echo $PWD  //顯示目前路徑
docker build --tag hello-world . //建立一個 image。
docker ps  // 列出有在 running 的 container
docker ps -a  // 列出所有 container
docker images  // 列出所有的 images。等價於 docker image ls。
docker run -p 8080:80 --name hello -d hello-world  // 從 hello-world 這個 image 建立一個 hello 的 container，port 8080:80，表示 local:container。
docker stop hello // 停止 container。(尚未刪除)
docker ps
docker ps -a
docker start hello  // 啟動 container。
docker logs hello  // 觀看 hello 這個 container 的 log 紀錄。
docker logs hello -f  // 即時監控 hello 這個 container 的 log 紀錄。
docker stop hello
docker ps -a
docker rm hello  // 刪除 hello 這個 container。
docker images
docker push littlefish0331/hello-world  // 直接 push 上去會失敗
docker tag hello-world:latest littlefish0331/hello-world  // 要先建立一個正確的tag名稱。
docker login  // 還要先 login。
docker push littlefish0331/hello-world
cat ./.docker/config.json  // login 的一些資訊會在這裡。
docker logout  // 建議要logout
docker --version
```

這是 Docker Hub 官網的教學，也就是說要先將local端的 image 取名為隊的tag名稱，才能上傳這樣。

> docker tag local-image:tagname new-repo:tagname
> docker push new-repo:tagname


docker-compose
docker-compose
docker-compose
docker-compose
docker-compose
docker-compose???????????









### Quickstart - Part 2

Get started > Quickstart > Part 2: Build and run your image

- [Build and run your image | Docker Documentation](https://docs.docker.com/get-started/part2/)

---

## tutroial - others

收集其他網站學習到的docker知識，或是hand-son經驗。

### phoenixnap website

- [How to Install Docker On Ubuntu 18.04 {2020 Tutorial}](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04)

There are two versions of Docker – Docker CE (Community Edition) and Docker EE (Enterprise Edition).  <br>
If you have a small-scale project, or you're just learning, you will want to use Docker CE.

**check unbuntu version:**

```{bash}
lsb_release -d
lsb_release -a
cat /etc/issue
cat /etc/os-release
hostnamectl
```

--

**在 ubuntu 上安裝 Docker:**

```{bash}
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt install docker.io
systemctl --version
sudo systemctl start docker
sudo systemctl enable docker
```

Older versions of the Docker binary were called docker or docker-engine or docker-io  <br>
docker-io package is still the name used by Debian/Ubuntu for the docker release provided on their official repos.  <br>
docker-ce is a certified release provided directly by docker.com and can also be built from source.  <br>
Main reason for using the name docker-io on Debian/Ubuntu platform was to avoid a name conflict with docker system-tray binary.

> 較舊版本的Docker二進製文件稱為docker或docker-engine或docker-io
>
> docker-io軟件包仍然是Debian / Ubuntu在其官方repos上提供的docker版本所使用的名稱。
>
> docker-ce是由docker.com直接提供的認證版本，也可以從source構建。
>
> 在Debian / Ubuntu平台上使用名稱docker-io的主要原因是為了避免與docker系統托盤二進製文件發生名稱衝突。

**systemctl註解:**

> 透過 systemctl 管理單一服務 (service unit) 的啟動/開機啟動與觀察狀態。
>
> systemd 這個啟動服務的機制，主要是透過一隻名為 systemctl 的指令來處理的。
>
> systemd 就是僅有 systemctl 這個指令來處理而已呦！所以全部的行為都得要使用 systemctl 的意思啦！

---

## install

感覺 install 方式很多種，所以我把我有嘗試過的方式記錄一下。

```{bash}
<!-- Linux 系統皆可使用 -->
<!-- Docker version 19.03.12 -->
curl -sSL https://get.docker.com/ | sh
sudo usermod -aG docker ubuntu  //要 sudo reboot 之後才有用。

<!-- 這是運行的完成後的結果， -->
<!-- 可以看到官方會幫我們裝 Docker Client、Docker Engine(CE版) -->
> Client: Docker Engine - Community
>  Version:           19.03.12
>  API version:       1.40
>  Go version:        go1.13.10
>  Git commit:        48a66213fe
>  Built:             Mon Jun 22 15:45:36 2020
>  OS/Arch:           linux/amd64
>  Experimental:      false
> 
> Server: Docker Engine - Community
>  Engine:
>   Version:          19.03.12
>   API version:      1.40 (minimum version 1.12)
>   Go version:       go1.13.10
>   Git commit:       48a66213fe
>   Built:            Mon Jun 22 15:44:07 2020
>   OS/Arch:          linux/amd64
>   Experimental:     false
>  containerd:
>   Version:          1.2.13
>   GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
>  runc:
>   Version:          1.0.0-rc10
>   GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
>  docker-init:
>   Version:          0.18.0
>   GitCommit:        fec3683
> If you would like to use Docker as a non-root user, you should now consider
> adding your user to the "docker" group with something like:
> 
>   sudo usermod -aG docker ubuntu
> 
> Remember that you will have to log out and back in for this to take effect!
> 
> WARNING: Adding a user to the "docker" group will grant the ability to run
>          containers which can be used to obtain root privileges on the
>          docker host.
>          Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
>          for more information.

<!-- 看一下下載安裝了哪些東西 -->
apt list | grep ^docker

> docker/bionic 1.5-1build1 amd64
> docker-ce/bionic,now 5:19.03.12~3-0~ubuntu-bionic amd64 [installed]  //多安裝這個。
> docker-ce-cli/bionic,now 5:19.03.12~3-0~ubuntu-bionic amd64 [installed,automatic]  //多安裝這個。
> docker-compose/bionic 1.17.1-2 all
> docker-containerd/bionic 0.2.3+git+docker1.13.1~ds1-1 amd64
> docker-doc/bionic-updates 19.03.6-0ubuntu1~18.04.1 all
> docker-registry/bionic 2.6.2~ds1-1 amd64
> docker-runc/bionic 1.0.0~rc2+git+docker1.13.1~ds1-3 amd64
> docker.io/bionic-updates 19.03.6-0ubuntu1~18.04.1 amd64
> docker2aci/bionic 0.14.0+dfsg-2 amd64

<!-- 查看目前系統有啟用那些docker服務 -->
systemctl | grep docker

> sys-devices-virtual-net-docker0.device          loaded active plugged   /sys/devices/virtual/net/docker0
> sys-subsystem-net-devices-docker0.device        loaded active plugged   /sys/subsystem/net/devices/docker0
> docker.service                                  loaded active running   Docker Application Container Engine
> docker.socket                                   loaded active running   Docker Socket for the API

<!-- 觀看 groups -->
groups
> ubuntu adm dialout cdrom floppy sudo audio dip video plugdev lxd netdev docker
```

```{bash}
<!-- 在 ubuntu 上安裝 Docker -->
<!-- 目前是 Docker version 19.03.6 -->
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo apt install docker.io
systemctl --version
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu  //要 sudo reboot 之後才有用。

<!-- 這是運行的完成後的結果，中間做了很多事情，我只擷取最後一段。 -->
<!-- 最後還要用 systemctl 啟動 docker 服務。 -->
> Created symlink /etc/systemd/system/sockets.target.wants/docker.socket → /lib/systemd/system/docker.socket.
> docker.service is a disabled or a static unit, not starting it.
> Processing triggers for ureadahead (0.100.0-21) ...

<!-- 看一下下載安裝了哪些東西 -->
apt list | grep ^docker

> docker/bionic 1.5-1build1 amd64
> docker-compose/bionic 1.17.1-2 all
> docker-containerd/bionic 0.2.3+git+docker1.13.1~ds1-1 amd64
> docker-doc/bionic-updates 19.03.6-0ubuntu1~18.04.1 all
> docker-registry/bionic 2.6.2~ds1-1 amd64
> docker-runc/bionic 1.0.0~rc2+git+docker1.13.1~ds1-3 amd64
> docker.io/bionic-updates,now 19.03.6-0ubuntu1~18.04.1 amd64 [installed]
> docker2aci/bionic 0.14.0+dfsg-2 amd64

<!-- 查看目前系統有啟用那些docker服務 -->
systemctl | grep docker

> sys-devices-virtual-net-docker0.device          loaded active plugged   /sys/devices/virtual/net/docker0
> sys-subsystem-net-devices-docker0.device        loaded active plugged   /sys/subsystem/net/devices/docker0
> docker.service                                  loaded active running   Docker Application Container Engine
> docker.socket                                   loaded active running   Docker Socket for the API

<!-- 觀看 groups -->
groups
> ubuntu adm dialout cdrom floppy sudo audio dip video plugdev lxd netdev docker
```

**不推薦:**

下載安裝後，找不到systemctl下的服務名稱，雖然指令可以使用，但是會要一直打sudo。  <br>
總之，不是很清楚明白每個環節。

```{bash}
<!-- 在 ubuntu 上安裝 Docker -->
<!-- Docker version 19.03.11 -->
sudo apt-get update
sudo apt-get remove docker docker-engine docker.io
sudo snap install docker

<!-- 這是運行的完成後的結果，中間做了很多事情，我只擷取最後一段。 -->
> 2020-07-26T11:00:54+08:00 INFO Waiting for restart...
> docker 19.03.11 from Canonical✓ installed

<!-- 看一下下載安裝了哪些東西 -->
apt list | grep ^docker

> docker/bionic 1.5-1build1 amd64
> docker-compose/bionic 1.17.1-2 all
> docker-containerd/bionic 0.2.3+git+docker1.13.1~ds1-1 amd64
> docker-doc/bionic-updates 19.03.6-0ubuntu1~18.04.1 all
> docker-registry/bionic 2.6.2~ds1-1 amd64
> docker-runc/bionic 1.0.0~rc2+git+docker1.13.1~ds1-3 amd64
> docker.io/bionic-updates 19.03.6-0ubuntu1~18.04.1 amd64
> docker2aci/bionic 0.14.0+dfsg-2 amd64

<!-- 查看目前系統有啟用那些docker服務 -->
systemctl | grep docker

> sys-devices-virtual-net-docker0.device          loaded active plugged   /sys/devices/virtual/net/docker0
> sys-subsystem-net-devices-docker0.device        loaded active plugged   /sys/subsystem/net/devices/docker0
> run-snapd-ns-docker.mnt.mount                   loaded active mounted   /run/snapd/ns/docker.mnt
> snap-docker-471.mount                           loaded active mounted   Mount unit for docker, revision 471
> snap.docker.dockerd.service                     loaded active running   Service for snap application docker.dockerd

<!-- 觀看 groups -->
groups
> ubuntu adm dialout cdrom floppy sudo audio dip video plugdev lxd netdev
```

---

## END
