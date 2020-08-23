# jupyter:Python、R、Julia

記錄我在 VCS 上操作 container:jupyter 的過程，以及一些問題要如何克服。  <br>
部分是從 DockerHub > Program > jupyter 複製過來。

DockerHub 上 jupyter 有把打包成許多 不同的 image。

--

**Content:**

<!-- TOC -->

- [jupyter:Python、R、Julia](#jupyterpythonrjulia)
  - [Python+jupyter notebook/lab:scipy(3.7.6)](#pythonjupyter-notebooklabscipy376)
  - [jupyter minimal/r/scipy](#jupyter-minimalrscipy)
  - [R+Python+Julia+jupyter notebook](#rpythonjuliajupyter-notebook)
  - [END](#end)

<!-- /TOC -->

---

## Python+jupyter notebook/lab:scipy(3.7.6)

**reference:**

- [jupyter/datascience-notebook Tags - Docker Hub](https://hub.docker.com/r/jupyter/datascience-notebook/tags?page=1&name=python)
- [Selecting an Image — docker-stacks latest documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook)
- [jupyter/scipy-notebook:dc9744740e12 - Docker Hub](https://hub.docker.com/layers/jupyter/scipy-notebook/dc9744740e12/images/sha256-9bc3ccb1e56f1ac030d4e71242b3dfc6f3b64e76b41d926dc88eb01303bcf3f9?context=explore)

**notebook image relation:**

稍微研究一下 notebook 之間的關聯!!

![notebook_relation](./image/notebook_relation.svg)

**啟動 container:**

> 如果抓 datascience 系列，會有 R。  <br>
> scipy, minimal 系列的 tag 看不出版本，但都是基於 base 系列製作。  <br>
> 所以最後抓取 base 系列的 python-3.7.6。  <br>
>  <br>
> 但是研究一下文檔，其實 base 系列沒有支援 LaTeX 讓我覺得很不妙，  <br>
> 可是 minimal, scipy 系列就是沒有 python 版本的 tag。  <br>
> 不然建議還是裝 minimal, scipy 系列。  <br>
>  <br>
> 後來覺得不妙，只好去一一嘗試，發現這個也是 python3.7.6，jupyter/scipy-notebook:dc9744740e12。
>  <br>
> ```{bash}
> docker pull jupyter/base-notebook:python-3.7.6
> docker run --name notebook_base -v /datamount/notebook/base_work:/home/jovyan/work -p 8888:8888 -d jupyter/base-notebook:python-3.7.6
> ```
>  <br>
> - jupyter notebook password: notebook@base2020  <br>
>   - 只有 python3.7.6。  <br>
>   - 用 `apt list | wc -l` 檢查有102個套件。  <br>
>   - 用 `pip list | wc -l` 檢查有82個套件。  <br>
>   - 用 `pip freeze | wc -l` 檢查有76個套件。

- jupyter/scipy-notebook:dc9744740e12
  - docker run --name notebook_python_nb -v /datamount/notebook/python_nb_work:/home/jovyan/work -p 8888:8888 -d jupyter/scipy-notebook:dc9744740e12
  - docker run --name notebook_python_lab -e JUPYTER_ENABLE_LAB=yes -v /datamount/notebook/python_lab_work:/home/jovyan/work -p 9999:8888 -d jupyter/scipy-notebook:dc9744740e12
  - 只有 python3.7.6。
  - 用 `apt list | wc -l` 檢查有492個套件。
  - 用 `pip list | wc -l` 檢查有132個套件。
  - 用 `pip freeze | wc -l` 檢查有126個套件。
  <!-- - jupyter notebook password: notebook@python2020。(都是這個) -->

---

## jupyter minimal/r/scipy

**reference:**

- [jupyter/minimal-notebook - Docker Hub](https://hub.docker.com/r/jupyter/minimal-notebook)
- [jupyter/r-notebook - Docker Hub](https://hub.docker.com/r/jupyter/r-notebook)
- [jupyter/scipy-notebook - Docker Hub](https://hub.docker.com/r/jupyter/scipy-notebook)

**啟動 container:**

- freeze: Output installed packages in requirements format.  <br>
- list: List installed packages

每個 container 啟動方式、套件的 information、password 如下，沒有指定 tag 就會安裝最新版的 python。

- jupyter/minimal-notebook
  - docker run --name notebook_mini -v /datamount/notebook/mini_work:/home/jovyan/work -p 8801:8888 -d jupyter/minimal-notebook
  - 有 python3。
  - 用 `apt list | wc -l` 檢查有375個套件。
  - 用 `pip list | wc -l` 檢查有82個套件。
  - 用 `pip freeze | wc -l` 檢查有76個套件。
  <!-- - jupyter notebook password: notebook@mini2020 -->

- jupyter/r-notebook
  - docker run --name notebook_r -v /datamount/notebook/r_work:/home/jovyan/work -p 8802:8888 -d jupyter/r-notebook
  - 有 python3、R。
  - 用 `apt list | wc -l` 檢查有403個套件。
  - 用 `pip list | wc -l` 檢查有82個套件。
  - 用 `pip freeze | wc -l` 檢查有76個套件。
  - 進入R檢查`installed.packages() %>% str`則有201個套件。
  <!-- - jupyter notebook password: notebook@r2020 -->

- jupyter/scipy-notebook
  - docker run --name notebook_scipy -v /datamount/notebook/scipy_work:/home/jovyan/work -p 8803:8888 -d jupyter/scipy-notebook
  - 有 python3。
  - 用 `apt list | wc -l` 檢查有499個套件。
  - 用 `pip list | wc -l` 檢查有142個套件。
  - 用 `pip freeze | wc -l` 檢查有136個套件。
  <!-- - jupyter notebook password: notebook@scipy2020 -->

---

## R+Python+Julia+jupyter notebook

- [jupyter's Profile - Docker Hub](https://hub.docker.com/u/jupyter)
- [Selecting an Image — docker-stacks latest documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#jupyter-datascience-notebook)
- [jupyter/docker-stacks: Ready-to-run Docker images containing Jupyter applications](https://github.com/jupyter/docker-stacks)

**啟動 container:**

以 jupyter/datascience-notebook 這個 images 為例。

```{bash}
docker pull jupyter/datascience-notebook

// port 8800: 使用 jupyter notebook
// port 9900: 使用 jupyter lab
docker run --name rpyju_ds_nb -v /datamount/rpyju/dsnb:/home/jovyan/work -p 8800:8888 -d jupyter/datascience-notebook
docker run --name rpyju_ds_lab -e JUPYTER_ENABLE_LAB=yes -v /datamount/rpyju/dslab:/home/jovyan/work -p 9900:8888 -d jupyter/datascience-notebook
```

**登入與密碼:**

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

## END
