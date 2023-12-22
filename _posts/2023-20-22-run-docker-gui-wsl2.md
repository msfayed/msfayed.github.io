---
layout: default
title: Running docker GUI Apps in WLS2 
date: '2023-12-22T10:00:00.000-03:00'
author: Mohammed Fayed
tags:
- Docker
- WSL
modified_time: '2023-12-22T10:00:00.000-03:00'
---

# Running docker GUI Apps in WLS2 

Make sure you are using Windows 10 ( build 19044 or later )  or Windows 11

upgrade WSL to grabe the latest (WSLg)[https://github.com/microsoft/wslg] update.

```shell
# from an elevated command prompt or powershell.
wsl --update

# restart WSL
wsl --shutdown 
```

Ceate Dockefile with bellow content:

```dockerfile
FROM ubuntu

RUN apt-get update && apt-get -y install \
    gedit \
    && apt-get clean && apt-get autoclean && rm -rf /var/lib/apt/lists/*

CMD ["gedit"]
```

build and Run .. Enjoy :)

```shell
docker build . -t guitest

docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix \
            -v /mnt/wslg:/mnt/wslg \
            -e DISPLAY=:0 \
            -e WAYLAND_DISPLAY=wayland-0 \
            -e XDG_RUNTIME_DIR=/mnt/wslg/runtime-dir \
            -e PULSE_SERVER/mnt/wslg/PulseServer \
            guitest
```