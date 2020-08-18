# environment variable

- [bash - Get Environment Variable from Docker Container - Stack Overflow](https://stackoverflow.com/questions/34051747/get-environment-variable-from-docker-container/34052766)
- [How to Set and List Environment Variables in Linux | Linuxize](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/)
- [How to Set Environment Variables in Linux - Serverlab](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/)

查看環境變數。

```{bash}
env
printenv

// 要看哪一個環境變數。
env PATH
```

新增環境變數(only for this time)

```{bash}
// export <env_variable>=<env_value>
export testenv=littlefish
env
```

---

## END
