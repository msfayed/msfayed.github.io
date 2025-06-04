---
layout: default
title: Run Linux Desktop with Docker on MacOS ARM
date: '2025-06-03T10:00:00.000-03:00'
author: Mohammed Fayed
tags:
- Docker
- Linux
- MacOS
- ARM
modified_time: '2025-06-03T10:00:00.000-03:00'
---

# Running Linux Desktop with Docker on MacOS (ARM)

- Install [UTM](https://mac.getutm.app/)

- Download Ubuntu Linux Server ISO (ARM) [from here](https://ubuntu.com/download/server/arm)

- Create new VM, choose `Virtualize` then `Linux` and then select the downloaded ISO file.

- Make sure to follow these setps during Linux installation process:
  
  - If you added more then 60G disk size, ensure you select the full disk size to be mounted at `/` otherwise only 59G of the disk size will be mounted.
  
  - Install Docker as part of the components to be installed by default.

- After Linux server is installed:

  - Make your user execute `sudo` command without password
    ```bash
    sudo nano /etc/sudoers
    ```
    add this line to the end of the file, then save it
    ```bash
    # replace $USER with your username

    $USER ALL=(ALL) NOPASSWD: ALL
    ```

  - Disable `systemd-networkd-wait-online.service` as it will cause slow boot time, by executing
    ```bash
    sudo systemctl disable systemd-networkd-wait-online
    ```
     
  - Fix Docker Permisisons, by default your user will not have permission to call `docker` without using `sudo` to fix this, let's create a docker group and add our user to it and give it permisisons on docker socket.

    ```bash
    # keep $USER as is

    sudo groupadd docker
    sudo gpasswd -a $USER docker
    sudo chown $USER:docker /var/run/docker.sock
    ```
  
  - Install Ubuntu-Desktop

    ```bash
    $ sudo apt update
    $ sudo apt install ubuntu-desktop
    $ sudo reboot
    ```


Enjoy!
