# 制作php漏洞环境镜像


1，拉取 fbraz3/lnmp:7.2 镜像，该镜像为php7.2版本，php7.2适合与绝大部分系统的要求。https://hub.docker.com/r/fbraz3/lnmp

2，打开网址查看启动命令，启动环境，启动的时候做好需要制作的系统代码的目录映射。

2.1，进入环境：docker exec -it e33 bash

2.2，修改mysql密码，默认为空

2.3，提交一次镜像 commit，将mysql的密码写入镜像

3，进行浏览器访问安装等操作，验证漏洞是否存在。

4，提交一次镜像，用cve命名：docker commit -m="添加代码" -a="壮士" 7e1 cve-2022-xxxx:1.0

5，制作dockerfile,将目录有漏洞的代码copy进去

FROM cve-2022-43256:1.0
COPY ./www /app/public

6，在进行build： docker build -t cve-2022-xxxx:1.0 .

7，在进行save：docker save cve-2022-xxxx:1.0 -o cve-2022-xxxx.tar

8，再在其他服务器上导入，就可以正常启动：docker load < centos7.tar
