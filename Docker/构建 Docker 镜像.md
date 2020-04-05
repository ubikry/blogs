# 构建 Docker 镜像

## 使用 Docker 的 commit 命令创建镜像

```bash
$ docker commit -m "一个新的镜像。" -a 'wangrui' b664577cf1fe ubikry/apache2:webserver
```

`-m` 镜像描述， `-a` 作者。

## 使用 Dockerfile 构建镜像

并不推荐使用 `docker commit` 的方法来构建镜像。相反，推荐使用被称为 Dockerfile 的定义文件和 `docker build` 命令来构建镜像。Dockerfile 使用基本的基于 DSL（Domain Specific Language）语法来构建一个 Docker 镜像，我们推荐使用 Dockerfile 方法来替代 `docker commit`，因为通过 Dockerfile 构建镜像更具备可重复性、透明性以及幂等性。

