# 镜像描述

基于alpine制作

Apache/2.4.46+PHP/7.3.22+mysql5.7

启动命令：

docker run -d -v /path/to/project:/var/www/localhost/htdocs/ -e MYSQL_ROOT_PASSWORD=root -p 80:80 -p 3306:3306 --name lamp xiahao90/alpine-lamp

# 制作php漏洞环境镜像


1，拉取 xiahao90/alpine-lamp 镜像，该镜像为php7.3版本，php7.3适合与绝大部分系统的要求。https://hub.docker.com/r/xiahao90/alpine-lamp

2，打开网址查看启动命令，启动环境，启动的时候做好需要制作的系统代码的目录映射。

2.1，可以进入环境：docker exec -it e33 /bin/sh

3，进行浏览器访问安装等操作，验证漏洞是否存在。

4，提交一次镜像，用cve命名：docker commit -m="添加代码" -a="壮士" 7e1 cve-2022-xxxx:latest

5，制作dockerfile,将目录有漏洞的代码copy进去
```
FROM cve-2022-xxxx:latest
ENV MYSQL_ROOT_PASSWORD=root
COPY ./www /var/www/localhost/htdocs/
RUN chmod -R 777 /var/www/localhost/htdocs/
```
6，在进行build： docker build -t cve-2022-xxxx:latest .

7，在进行提交dockerhub：
```
将xiahao90 改成你自己的账号
docker login -u xiahao90
docker tag cve-2022-xxxx:1.1 xiahao90/cve-2022-xxxx:latest
docker push xiahao90/cve-2022-xxxx:latest
```
8，可以在其他服务器上拉了(如果设置了镜像加速，可能要等明天)
