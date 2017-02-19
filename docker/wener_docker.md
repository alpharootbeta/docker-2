### 代理

Docker 会使用 HTTP_PROXY 作为代理,代理配置成功后可在 `docker info` 看到代理配置

代理配置文件

* `/etc/default/docker`

如果代理配置不生效,可直接修改 systemd 定义文件,例如在 Ubuntu 16.04 下为 `/lib/systemd/system/docker.service`,在 Service 节下添加 `Environment=HTTP_PROXY=http://10.0.0.1:7777`, 然后 `systemctl daemon-reload` 再重启 docker 即可,配置可通过 `systemctl show --property=Environment docker` 查看

* [Control and configure Docker with systemd](https://docs.docker.com/engine/admin/systemd/)

### 时区

启动时修改时区

```
$ docker run --rm busybox date
Thu Mar 20 04:42:02 UTC 2014
$ docker run --rm -v /etc/localtime:/etc/localtime  busybox date
Thu Mar 20 14:42:20 EST 2014
$ FILE=$(mktemp) ; echo $FILE ; echo -e "Europe/Brussels" > $FILE ; docker run --rm -v $FILE:/etc/timezone -v /usr/share/zoneinfo/Europe/Brussels:/etc/localtime busybox date
/tmp/tmp.JwL2A9c50i
Thu Mar 20 05:42:26 CET 2014
$ docker run -t -i --rm -e TZ=Europe/London busybox date
```

__Dockerfile__ 修改时区
```
RUN echo America/New_York | sudo tee /etc/timezone && sudo dpkg-reconfigure --frontend noninteractive tzdata
```

修改 MySQL 的时区
```bash
# 方法一 修改容器时区,重启 mysql
docker exec -it MySQL bash
# 时区信息 /usr/share/zoneinfo
# 直接修改 echo Asia/Shanghai > /etc/timezone
# 获取所有时区 timedatectl list-timezones
# 直接修改时区 timedatectl set-timezone Europe/Athens
# 在容器里可能 timedatectl 无法使用

# 交互式选择时区
dpkg-reconfigure tzdata
/etc/init.d/mysql restart

# 方法二 SET GLOBAL time_zone = 'Asia/Shanghai';
# 方法三 my.cnf [mysqld] default-time-zone='Asia/Shanghai'
```

## Refernece

* [Docker Master Binaries](https://master.dockerproject.org/)
* [Docker Master Document](http://docs.master.dockerproject.org/)
* [Shipyard](https://github.com/shipyard/shipyard) Composable Docker Management
  * 可管理 Container,Image,Registry,Auth,Node,Log 等
  * 有网页端的 Console
  * 目前不支持 1.12 Docker Swarm
* [Tsuru](https://github.com/tsuru/tsuru) is an extensible and open source Platform as a Service software.
* [Docket](https://github.com/netvarun/docket) Custom docker registry that allows for lightning fast deploys through bittorrent
* [dockerfiles](https://github.com/jfrazelle/dockerfiles) Various Dockerfiles
* [ui-for-docker](https://github.com/kevana/ui-for-docker) An unofficial web interface for Docker, formerly known as DockerUI
* https://github.com/wsargent/docker-cheat-sheet
* https://github.com/veggiemonk/awesome-docker
* [Portus](https://github.com/SUSE/Portus) Authorization service and frontend for Docker registry (v2)
* [docker-swarm-visualizer](https://github.com/manomarks/docker-swarm-visualizer) A visualizer for Docker Swarm using the Docker Remote API, Node.JS, and D3
* [logspout](https://github.com/gliderlabs/logspout) Log routing for Docker container logs
