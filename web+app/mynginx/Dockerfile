FROM    debian:latest

MAINTAINER jamesmo <springclick@gmail.com>

RUN sed -i '1,3d'   /etc/apt/sources.list
RUN echo '#hello'>> /etc/apt/sources.list

RUN sed -i '1a \
    deb http://mirrors.163.com/debian/ wheezy main non-free contrib \n \
    deb http://mirrors.163.com/debian/ wheezy-proposed-updates main contrib non-free \n \
    deb http://mirrors.163.com/debian-security/ wheezy/updates main contrib non-free \n \
    deb-src http://mirrors.163.com/debian/ wheezy main non-free contrib \n \
    deb-src http://mirrors.163.com/debian/ wheezy-proposed-updates main contrib non-free \n \
    deb-src http://mirrors.163.com/debian-security/ wheezy/updates main contrib non-free \n \
    deb http://ftp.sjtu.edu.cn/debian/ wheezy main non-free contrib \n \
    deb http://ftp.sjtu.edu.cn/debian/ wheezy-proposed-updates main contrib non-free \n \
    deb http://ftp.sjtu.edu.cn/debian-security/ wheezy/updates main contrib non-free \n \
    deb-src http://ftp.sjtu.edu.cn/debian/ wheezy main non-free contrib \n \
    deb-src http://ftp.sjtu.edu.cn/debian/ wheezy-proposed-updates main contrib non-free \n \
    deb-src http://ftp.sjtu.edu.cn/debian-security/ wheezy/updates main contrib non-free \n \
    ' /etc/apt/sources.list

RUN cat /etc/apt/sources.list

RUN apt-get -qq update

RUN apt-get -qqy install gcc libpcre3 libpcre3-dev openssl libssl-dev make wget libreadline-dev libncurses-dev graphicsmagick

WORKDIR /tmp
RUN wget http://tengine.taobao.org/download/tengine-2.1.0.tar.gz
RUN tar xvf tengine-2.1.0.tar.gz

WORKDIR /tmp
RUN wget http://www.lua.org/ftp/lua-5.1.5.tar.gz
RUN tar zxf lua-5.1.5.tar.gz
WORKDIR /tmp/lua-5.1.5
RUN make linux && make install

WORKDIR /tmp/tengine-2.1.0
RUN cd /tmp/tengine-2.1.0 && ./configure --with-http_lua_module && make && make install
ADD nginx.conf /usr/local/nginx/conf/nginx.conf

WORKDIR /root
RUN rm -rf /tmp/tengine-*
RUN rm -rf /tmp/lua-*

ENV HOME /root
RUN rm -rf /etc/service/sshd /etc/my_init.d/00_regen_ssh_host_keys.sh

RUN mkdir -p /etc/my_init.d
COPY nginx.d /usr/local/nginx/conf/nginx.d
ADD nginx.sh /etc/my_init.d/nginx.sh
RUN chmod 755 /etc/my_init.d/nginx.sh

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 80 443

CMD ["/etc/my_init.d/nginx.sh"]

#userage
#docker run -d -p 8080:80 --name test -v /home/utgard/www/:/data/www/ mynginx_app