# 5.4. Практические навыки работы с Docker

## 1 задание

	FROM archlinux:latest
	# TEMP-FIX for pacman issue
	RUN patched_glibc=glibc-linux4-2.33-4-x86_64.pkg.tar.zst \
	    && curl -LO "https://raw.githubusercontent.com/sickcodes/Docker-OSX/master/${patched_$
	    && bsdtar -C / -xvf "${patched_glibc}" || echo "Everything is fine."
	# TEMP-FIX for pacman issue
	RUN pacman -Sy
	RUN pacman -Sy --noconfirm ponysay
	ENTRYPOINT ["/usr/bin/ponysay"]
	CMD ["Hey, netology”]

Ссылка на репозиторий: https://hub.docker.com/repository/docker/yuggoth1/5.4-1

(https://i2.paste.pics/da6a7617ed8d4be4dc00990c3f2cecf7.png)


## 2 задание

Удалось запустить только один контейнер для ubuntu:

	FROM ubuntu:latest
	RUN apt-get update
	RUN apt-get install -y systemctl
	RUN apt-get install -y gnupg2
	RUN apt-get install wget -y
	RUN wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |  apt-key$
	RUN sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \/etc/apt/so$
	RUN apt-get update
	RUN apt-get install -y jenkins
	RUN apt install openjdk-11-jdk -y
	RUN systemctl start jenkins
	RUN cd /usr/share/jenkins/
	CMD ["/bin/bash"]

![alt text](https://i2.paste.pics/94bdc07617ca7d53a7a7e25d8578d72f.png)
![alt text](https://i2.paste.pics/6bd539bbca2902155cc457d9a5edd519.png)

https://hub.docker.com/repository/docker/yuggoth1/jen2

Второй образ пока не хочет запускаться, вываливается ошибка:

	FROM amazoncorretto
	RUN yum clean all
	RUN amazon-linux-extras install docker -y
	RUN yum update -y
	RUN yum install wget -y
	RUN wget -O /etc/yum.repos.d/jenkins.repo \
	    https://pkg.jenkins.io/redhat-stable/jenkins.repo
	RUN rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
	RUN yum upgrade
	RUN yum install jenkins java-1.8.0-openjdk-devel -y
	CMD ["/bin/bash"]



## 3 задание

	FROM node
	RUN apt-get update
	RUN git clone https://github.com/simplicitesoftware/nodejs-demo.git
	WORKDIR /nodejs-demo/
	RUN npm install -g nodemon
	RUN npm install -g npm@latest
	RUN npm install
	RUN sed -i "s/localhost/0.0.0.0/g" app.js

![alt text](https://i2.paste.pics/78584e74dfa467507885d3b3673a23cc.png)


