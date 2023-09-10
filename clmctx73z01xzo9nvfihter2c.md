---
title: "Setting Up Enterprise WiFi Connections with PEAP Without a GUI: Perfect for Universities and Offices"
datePublished: Sun Sep 10 2023 02:21:49 GMT+0000 (Coordinated Universal Time)
cuid: clmctx73z01xzo9nvfihter2c
slug: setting-up-enterprise-wifi-connections-with-peap-without-a-gui-perfect-for-universities-and-offices
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/syCXK9WndqQ/upload/43592eee09200815b3648322e3071a6e.jpeg
tags: wifi, office, university, pinephone

---


```
# nmcli con add type wifi ifname wlan0 con-name CONNECTION_NAME ssid SSID
# nmcli con edit id CONNECTION_NAME
nmcli> set ipv4.method auto
nmcli> set 802-1x.eap peap
nmcli> set 802-1x.phase2-auth mschapv2
nmcli> set 802-1x.identity USERNAME
nmcli> set 802-1x.password PASSWORD
nmcli> set wifi-sec.key-mgmt wpa-eap
nmcli> save
nmcli> activate
```
