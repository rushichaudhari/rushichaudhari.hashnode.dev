---
title: "Shifting Docker Storage: Your Guide to changing docker cache storage"
datePublished: Thu Nov 11 2021 18:00:00 GMT+0000 (Coordinated Universal Time)
cuid: clmctw0kf00b612nvci1hdsyx
slug: shifting-docker-storage-your-guide-to-changing-docker-cache-storage
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/jOqJbvo1P9g/upload/bf5e1b01777bac64bfd040f7e588edec.jpeg
tags: docker, storage, location

---


_You can easily change the Docker default storage location by creating the daemon.json file and pointing to another location in that file._

It happened to me several times that I didn’t have enough space in my root partition to store Docker containers and I had to move the Docker default storage location to another partition. In this post, I wrote down how to do that for my readership and future myself :)

Docker containers are relatively large (> 1G) and by default Docker stores all containers in `/var/lib/docker`, which is located in the root partition of your Linux system. I usually have separate root and home partitions, and given that Linux doesn’t take much space, I allocate 15-30G for my root partition. This happened not to be enough to work with Docker and I had to move the Docker storage location to another larger partition. However, it turned out not to be easy.

Do NOT do this to move Docker storage location
----------------------------------------------

These two solutions could have worked in the past as you may often find them online, but neither of them worked for me with Ubuntu-based Linux distros in 2018-2019 (Docker version > 17).

### 1\. Symlink

The first obvious idea was to symlink the default storage location to another partition:
```
    sudo ln -s /mnt/newlocation /var/lib/docker
```

### 2\. DOCKER\_OPTS

Another often posted solution is to stop Docker:

sudo systemctl stop docker
    

Edit the `/etc/default/docker` file by adding the new location with the `-g` in the `DOCKER_OPTS` line:

```
    DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -g /mnt/newlocation"
 ```

Then start Docker again:
```
sudo systemctl start docker
```    

After that Docker should use `/mnt/newlocation` as a new storage location.

**UPDATE**: It seems **DOCKER\_OPTS** solution may work if you add the `$DOCKER_OPTS` variable to the _systemd_ script `/lib/systemd/system/docker.service`:
```
    ExecStart=/usr/bin/dockerd -H fd:// $DOCKER_OPTS
    
```
However, I have not tested it because the solution I describe below is simpler and probably more correct.

Change Docker storage location: THE RIGHT WAY!
----------------------------------------------

Luckily, the right way to change Docker storage location was not more complicated than the two non-working options I have described above.

You need to create a JSON file `/etc/docker/daemon.json` with the content pointing to the new storage location:
```
    {
    "data-root": "/mnt/newlocation"
    }
    ```

You can read more about `daemon.json` in [Docker docs](https://docs.docker.com/config/daemon/#docker-daemon-directory).

Then, restart Docker or reboot the system:
```
    sudo systemctl restart docker
    
```
If you get any error during the restart, pay attention to spaces in `daemon.json`. JSON files are sensitive to indentation and an extra or lacking space may cause an error. If Docker restarts fine, this new setting will make Docker place all new containers to the new location. However, old containers will stay in `/etc/default/docker`. I recommend removing all old containers:

And downloaded them again:
```
    docker pull <container-name>
    
```
Final thoughts
--------------

It is unfortunate that this simple solution with `daemon.json` was not the first I found when I tried to fix the “not enough space” issue due to Docker images taking too much space. So, I hope this blog post will save time some users who need to change their Docker storage location.

[Source https://evodify.com/change-docker-storage-location/](https://evodify.com/change-docker-storage-location/)


