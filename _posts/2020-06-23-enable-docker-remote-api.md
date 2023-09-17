---
layout: default
title: Enable Docker Remote API
date: '2020-06-23T03:46:00.000-03:00'
author: Mohammed Fayed
tags:
modified_time: '2020-06-23T03:46:36.903-03:00'
blogger_id: tag:blogger.com,1999:blog-3008009747231704115.post-1347511242304256982
blogger_orig_url: https://www.fayed.org/2020/06/enable-docker-remote-api.html
---

### how to Enable Docker Remote API?

- Navigate to `/lib/systemd/system` in your terminal and open `docker.service` file
    
    ```shell
    vi /lib/systemd/system/docker.service
    ```

- Find the line which starts with `ExecStart` and adds `-H=tcp://0.0.0.0:2375` to make it look like

    ```
    ExecStart=/usr/bin/dockerd -H=fd:// -H=tcp://0.0.0.0:2375
    ```

- Save the Modified File

- Reload the docker daemon
    ```shell
    systemctl daemon-reload
    ```

- Restart the container
    ```
    sudo service docker restart
    ```




#### source: 
- [https://medium.com/@ssmak/how-to-enable-docker-remote-api-on-docker-host-7b73bd3278c6](https://medium.com/@ssmak/how-to-enable-docker-remote-api-on-docker-host-7b73bd3278c6)
