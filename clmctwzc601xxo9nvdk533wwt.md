---
title: "Optimizing Your SSD: Top Strategies to Free Up Space on Your C Drive"
datePublished: Sun Sep 10 2023 02:21:39 GMT+0000 (Coordinated Universal Time)
cuid: clmctwzc601xxo9nvdk533wwt
slug: optimizing-your-ssd-top-strategies-to-free-up-space-on-your-c-drive
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/uJMfXAdDMyE/upload/aadf7f3c9c82f502c42f1ee7ff58db04.jpeg
tags: storage, ssd, space, save, ssd-upgrade

---


Looking to free up some space on your SSD C drive? It might be easier than you think.


## Change cache location of Firefox

You can do this by creating a new hidden preference.

1.  Type **about:config** into the location bar and press enter
2.  Accept the warning message that appears, you will be taken to a list of preferences
3.  Right-click somewhere in the list and select "New > String"
4.  For the name of the preference type **browser.cache.disk.parent_directory**
5.  For its value type the path to where you want to store the cache
6. Next locate the preference **browser.cache.disk.enable**, it must be set to **true**, if it is not, double-click on it to change its value



## Symlink AppData folders

Windows applications often store their data and settings in an AppData folder, and each Windows user account has its own. It is located under C:\Users\<username> which stores caches of various applications.

Move all the files/hidden files/sub folders from the `C:\Users\Nikhil\AppData\Local` to to your `D:\AppData\Local` directory and create a symlink.

```
mklink /d C:\Users\Nikhil\AppData\Local D:\AppData\Local
```



## Move MS Office to some other drive [or install]

source: [https://www.youtube.com/watch?v=CIBNlTpByoY&t=36s](https://www.youtube.com/watch?v=CIBNlTpByoY&t=36s)

For 64bit

```
MKLINK /J "C:\Program Files\Microsoft Office\root" "U:\Program Files\Microsoft Office\root"
```


For 32 bit [x86]

```
MKLINK /J "C:\Program Files (x86)\Microsoft Office\root" "U:\Program Files (x86)\Microsoft Office\root"
```






