# Environment Variable

本節簡單講一下如何檢視、新增環境變數。

**resources:**

- [bash - Get Environment Variable from Docker Container - Stack Overflow](https://stackoverflow.com/questions/34051747/get-environment-variable-from-docker-container/34052766)
- [How to Set and List Environment Variables in Linux | Linuxize](https://linuxize.com/post/how-to-set-and-list-environment-variables-in-linux/)
- [How to Set Environment Variables in Linux - Serverlab](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/)
- [How to permanently export a variable in Linux? - Stack Overflow](https://stackoverflow.com/questions/13046624/how-to-permanently-export-a-variable-in-linux)

---

## 檢視環境變數

```{bash}
env
printenv

// 要看哪一個環境變數。
// env <environment_variable_name>
env PATH
```

## 新增環境變數

only for this time.

```{bash}
// export <env_variable>=<env_value>
export testenv=littlefish
env
```

permanently

見參考連結。  
大概就是直接去文件新增 $HOME/.bashrc、/etc/environment (more globally)。

---

## END
