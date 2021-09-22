# 5.4. Практические навыки работы с Docker

## 1 задание

	FROM archlinux:latest
	# TEMP-FIX for pacman issue
	RUN patched_glibc=glibc-linux4-2.33-4-x86_64.pkg.tar.zst \
	    && curl -LO "https://raw.githubusercontent.com/sickcodes/Docker-OSX/master/${patched_glibc}" \
	    && bsdtar -C / -xvf "${patched_glibc}" || echo "Everything is fine."
	# TEMP-FIX for pacman issue
	RUN pacman -Sy && pacman -Sy --noconfirm ponysay
	ENTRYPOINT ["/usr/bin/ponysay"]
	CMD ["Hey, netology”]

Ссылка на репозиторий: https://hub.docker.com/repository/docker/yuggoth1/5.4-1

![alt text](https://i2.paste.pics/da6a7617ed8d4be4dc00990c3f2cecf7.png)


## 2 задание

Контейнер для ubuntu:

	FROM ubuntu:latest
	RUN apt-get update && apt-get install -y \
	systemctl \
	gnupg2 \
	wget
	RUN wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |  apt-key add - &&\
	sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \/etc/apt/sources.list.d/jenkins.list'
	RUN apt-get update && apt-get install -y \
	jenkins \
	openjdk-11-jdk
	RUN systemctl start jenkins
	WORKDIR /usr/share/jenkins/
	EXPOSE 8080
	ENTRYPOINT ["java"]
	CMD ["-jar","jenkins.war"]
	
Запускать так:

	docker run -p 8082:8080 -i -t jen2:ver2

![alt text](https://i2.paste.pics/94bdc07617ca7d53a7a7e25d8578d72f.png)
![alt text](https://i2.paste.pics/6bd539bbca2902155cc457d9a5edd519.png)

https://hub.docker.com/repository/docker/yuggoth1/jen2

Образ с amazoncorreto:

	FROM amazoncorretto:latest
	ADD https://get.jenkins.io/war-stable/latest/jenkins.war /root/
	WORKDIR /root
	EXPOSE 8080
	ENTRYPOINT ["java"]
	CMD ["-jar","jenkins.war"]
	
Запускать так:

	docker run -p 8081:8080 -i -t jen1:ver1

![alt text](https://i2.paste.pics/5dadec77d9fe368526983ba798b5bedb.png)
![alt text](https://i2.paste.pics/1b0cd10416e43909655ed820b1f4b8f8.png)

https://hub.docker.com/repository/docker/yuggoth1/jen1

## 3 задание

	FROM node
	RUN apt-get update && git clone https://github.com/simplicitesoftware/nodejs-demo.git
	WORKDIR /nodejs-demo/
	RUN npm install -g nodemon \
	npm@latest &&\
	npm install
	RUN sed -i "s/localhost/0.0.0.0/g" app.js

![alt text](https://i2.paste.pics/78584e74dfa467507885d3b3673a23cc.png)


