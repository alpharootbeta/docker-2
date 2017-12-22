# docker
docker 学习笔记


## 使用 mac 安装 docker-machine 

官方 [docker 常用镜像 ](https://hub.docker.com/_/mariadb/ "mariadb") .

## 2017-02-19

 
## docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v /etc:/etc spotify/docker-gc 
删除不用的容器和 volume, 停了的和没关联的都会删除

## docker rm `docker ps -aq` 
清除以前的容器
