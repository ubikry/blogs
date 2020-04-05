

## 附着到容器

```bash
$ sudo docker attach 容器名称
```

## 创建守护式容器

除了交互式的容器，也可以创建长期运行的容器。守护式容器没有交互式会话，非常适合运行应用程序和服务。大多数时候我们都需要守护式来运行我们的容器。下面来启动一个守护式容器。

```bash
$ sudo docker run --name daemon_dave -d ubuntu /bin/bash -c "while true; do echo"
```

上面的 docker run 命令使用了 -d 参数， 因此 Docker 会将容器放到后台执行。

## 容器内部都在干些什么

我们已经有了一个在后台运行的 while 循环的守护式容器。为了探究该容器内部都在干些什么，可以用 docker logs 命令来获取容器的日志

```bash
$ sudo docker logs daemon_dave
hello world
hello world
hello world
hello world
hello world
$
```

Docker 会输出最后几条日志项并返回。我们也可以在命令后使用 -f 参数来监控 Docker 的日志，这与 tail -f 非常相似

```bash
$sudo docker logs -f daemon_dave
hello world
hello world
hello world
hello world
hello world

```

**可以通过 `Ctr + C`退出日志跟踪**

可以使用 -t 标志为每条日志添加时间戳

```bahs
ubikry@ubikry-Lenovo-G480:~$ sudo docker logs  -ft daemon_dave
2020-04-04T07:27:17.082713280Z hello world
2020-04-04T07:27:18.084151755Z hello world
2020-04-04T07:27:19.086145872Z hello world
2020-04-04T07:27:20.088291037Z hello world
2020-04-04T07:27:21.090174881Z hello world
2020-04-04T07:27:22.091977518Z hello world
2020-04-04T07:27:23.093843319Z hello world
2020-04-04T07:27:24.095494651Z hello world
2020-04-04T07:27:25.097403994Z hello world
2020-04-04T07:27:26.099221495Z hello world
2020-04-04T07:27:27.101050256Z hello world

```

## Docker 日志驱动

配置容器中的日志输出（转发）到哪里。

## 查看容器内的进程

```bash
$ sudo docker top daemon_dave
```

## Docker 统计信息

除了 docker top 命令，还可以使用 docker stats 命令，它用来显示一个或多个容器的统计信息。

```bash
$ sudo docker stats  
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
1f04d36c9c65        daemon_dave         0.20%               1.504MiB / 7.676GiB   0.02%               4.46kB / 0B         0B / 0B             2

```

## 在容器内部运行进程

在容器中创建一个新的进程

```bash
$ sudo docker exec -d daemon_dave touch /etc/new_config_file
```

在容器内运行交互命令

```bash
$ sudo docker exec -i -t daemon_dave /bin/bash
```

## Docker exec 退出但不关闭容器

`Ctrl + P + Q`

## 停止守护容器

```bash
$ sudo docker stop daemon_dave
```

**docker stop 命令会向 Docker 容器进程发送 `SIGTERM` 信号。如果想快速体质某个容器，也可以使用 docker kill 命令来向容器进程发送 `SIGKILL` 信号。 **

## 自动重启

如果由于某种原因导致容器停止运行，还可以通过 `--restart`标志，让 Docker 自动重新启动该容器。 `--restart`标志会检查容器的退出代码，并据此决定是否需要重启容器。默认行为是 Docker 不会重启容器。

```bash
$ sudo docker run --restart=always --name daemon_dave -d ubuntu /bin/sh -C "while true; do echo hello world; sleep 1; done"

$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                           PORTS               NAMES
1e71cc1cd5bd        ubuntu              "/bin/sh -C 'while t…"   9 minutes ago       Restarting (127) 6 seconds ago                       daemon_dave

```

可以看到容器状态：`Restarting (127) 6 seconds ago `

--restart 标志被设置为 always。无论容器的退出代码是什么， Docker 都会自动重新启动。除了 always，还可以将这个标志设置为 on-failure,这样，只有当容器的退出代码为非 0 值时，才会自动重启。另外，on-failure 还接受一个可选的重启次数参数 `--restart=on-failure:5`

## 深入容器

除了通过 `docker ps` 命令获取容器的信息，还可以使用 `docker inspect`来获取更多的容器信息

```bash
ubikry@ubikry-Lenovo-G480:~$ sudo docker inspect daemon_dave
[
    {
        "Id": "1e71cc1cd5bda623cfcf0149ef99ff5d1feef048e1d44000e4f7e6b6f39a0bd6",
        "Created": "2020-04-04T08:45:18.226452762Z",
        "Path": "/bin/sh",
        "Args": [
            "-C",
            "while true; do echo hello world; seleep 1; done"
        ],
        "State": {
            "Status": "restarting",
            "Running": true,
            "Paused": false,
            "Restarting": true,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 127,
            "Error": "",
            "StartedAt": "2020-04-04T09:08:35.958713341Z",
            "FinishedAt": "2020-04-04T09:08:36.046824747Z"
        },

```

`docker inspect`命令会对容器进行详细的检查，然后返回其配置信息，包括名称、命令、网络配置以及很多有用的数据。

也可以使用 `-f` 或者 `-format`标记来选定查看结果，查看容器的运行状态。

```bash
ubikry@ubikry-Lenovo-G480:~$ sudo docker inspect daemon_dave --format="{{.State.Running}}"
true
```

## 删除容器

如果容器不再使用，可以使用 docker rm 命令来删除它们，

```bash
ubikry@ubikry-Lenovo-G480:~$ sudo docker rm romantic_murdock
romantic_murdock

```

使用 -f 标记，删除运行中的镜像

```bash
ubikry@ubikry-Lenovo-G480:~$ sudo docker rm -f daemon_dave
daemon_dave
```

删除所有的容器

```bash
$ sudo docker rm 'sudo docker ps -a -q'
```



## 答疑

### Docker 容器的退出状态都包括什么?