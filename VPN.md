# VPN

如果要使用國網內部的資源，因為有鎖網域的原因，所以要用 VPN，  
本文介紹如何在 TWCC VM(VCS)，安裝VPN，以及對 NCHC-VPN 連線的相關設定。

<!-- content -->

---

## reference

- [TWCC VM - NCHC VPN - HackMD](https://hackmd.io/@thomastsai/Sy5BajmMv): Thomas 提供。
- [Linux 使用 openfortivpn 連線至 Fortinet VPN 伺服器建立 PPP+SSL VPN 教學與範例 - Office 指南](https://officeguide.cc/linux-openfortivpn-fortinet-ppp-ssl-vpn-client-tutorial-examples/): openfortivpn 設定。
- [git错误error: server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none - 简书](https://www.jianshu.com/p/7d599bdf370a): git 設定。
- [Git 透過 HTTPS 連線失敗：server certificate verification failed 解決方式 - Office 指南](https://officeguide.cc/git-https-server-certificate-verification-failed-solution/): git 設定。

---

## 步驟

先把所有步驟列出來，再一一介紹說明

1. login to VM，查看相關設定。  
2. 安裝 openfortivpn

    > 不要用預設路由 會造成VM斷線失連。  
    > 所以要設定 set-routes = 0

3. 手動查找要連的服務主機IP

4. 然後加入 路由表

5. 就可以開始用了

### step01. login to VM，查看相關設定

登入自己的 VPN，這應該沒什麼問題XD~  

在這個階段你可以知道幾件事情。
因為你可以登入，所以你知道自己VM的IP~  

**ifconfig:**

主要可以手動的啟動、觀察與修改網路介面的相關參數。  
進一步的參數說明，可以看 [鳥哥的 Linux 私房菜 -- Linux 常用網路指令介紹](http://linux.vbird.org/linux_server/0140networkcommand.php#ifconfig)。

![ifconfig](./image/ifconfig.jpg)

**route:**

兩部主機之間一定要有路由才能夠互通 TCP/IP 的協定，否則就無法進行連線!  
一般來說，只要有網路介面，該介面就會產生一個路由，所以我們的主機一定會有一個 eth0 的介面。(TWCC-VM的初始名稱叫做 ens3)



---

## END
