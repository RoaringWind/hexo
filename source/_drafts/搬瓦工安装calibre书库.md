# 搬瓦工搭建书库

1. 安装centos-7-x86_64-bbr

2. ```shell
   yum -y update
   sudo yum -y install glibc xdg-utils python wget vim zsh git docker unzip
   sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
   mkdir /calibre /calibrelib /books
3. ```bash
   systemctl restart docker
   docker create --name=calibre -v /calibre/config:/config -v /calibrelib:/books -p 8083:8083 ctiself/calibre-web
   docker start calibre
	```

4. ```bash
   docker run --rm -p:8080:8080 -p 8081:8081  -v /linuxcali:/config linuxserver/calibre
   
   docker stop $(docker ps -aq)
   docker rm $(docker ps -aq)
   ```

5. ```bash
   systemctl start docker
   cd /usr/local
   mkdir calibre-web && mkdir calibre-web/app calibre-web/books calibre-web/kindlegen calibre-web/config
   docker create --name=calibre-web --restart=always -v /usr/local/calibre-web/books:/books -v /usr/local/calibre-web/app:/calibre-web/app -v /usr/local/calibre-web/kindlegen:/calibre-web/kindlegen -v /usr/local/calibre-web/config:/calibre-web/config -e USE_CONFIG_DIR=true -e SET_CONTAINER_TIMEZONE=true -e CONTAINER_TIMEZONE=Asia/Shanghai -e PGID=0 -e PUID=0 -p 8083:8083 technosoft2000/calibre-web
   ```

6. 