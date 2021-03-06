# 5.3. Контейнеризация на примере Docker

## 1 задание

`Высоконагруженное монолитное java веб-приложение;`

С одной стороны нужна высокая производительность, поэтому можно было бы выбрать физическую машину, чтоб быть поближе к железу, но на ней нельзя запустить несколько экземпляров приложения, поэтому выбираем виртуалку.

`Go-микросервис для генерации отчетов;`

Докер. В гугле полно статей на тему как это сделать, видимо это общепринятая практика.

`Nodejs веб-приложение;`

Можно использовать контейнеры, Node.js неприхотлива.

`Мобильное приложение c версиями для Android и iOS;`

Читала, что можно сделать это на докере, но будет ограничение функциональности приложений, поэтому виртуалка.

`База данных postgresql используемая, как кэш;`

Т.к. кэш должен быстро отдавать данные, лучше размещать на физической машине.

`Шина данных на базе Apache Kafka;`

Можно сделать это в докере, есть статьи, но шина данных - это узкое место и оно должно быть отказоустойчивым, поэтому для прода лучше виртуалка, для теста будет достаточно контейнера.

`Очередь для Logstash на базе Redis;`

Redis работает в режиме реального времени, поэтому физическая машина, т.к. нужна высокая производительность.

`Elastic stack для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;`

Это реализуется как на докерах, так и на виртуалках. Если исходить из-того, что elasticsearch - это хранилище, и для записи в него нужна высокая производительность, по возможности лучше виртуалка.

`Мониторинг-стек на базе prometheus и grafana;`

Докер. Prometheus и Grafana полностью совместимы с Docker и доступны на Docker Hub. 

`Mongodb, как основное хранилище данных для java-приложения;`

Поскольку Mongodb - это база данных, нужен быстрый отказоустойчивый доступ к данным, поэтому виртуалка.

`Jenkins-сервер.`

Докеры. В гугле много статей на тему jenkins+docker.

## 2 задание

Насколько поняла, скачала докер httpd, запустила, внутри него в файле /usr/local/apache2/htdocs/index.php заменила текст на требуемый (пришлось установить редактор nano, а то там не было никакого), сохранила образ, запушила в репозиторий test.

https://hub.docker.com/repository/docker/yuggoth1/test

## 3 задание

Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку info из текущей рабочей директории на хостовой машине в /share/info контейнера;

	docker run --name centos-01 -v /home/ekaterina/info:/share/info -t -d centos

Запустите второй контейнер из образа debian:latest в фоновом режиме, подключив папку info из текущей рабочей директории на хостовой машине в /info контейнера;

	docker pull debian:latest
	docker run --name debian-02 -v /home/ekaterina/info:/info -t -d debian

Подключитесь к первому контейнеру с помощью exec и создайте текстовый файл любого содержания в /share/info ;

	cd /share/info
	docker exec -ti centos-01  bash
	touch 1.txt
	vi 1.txt

Добавьте еще один файл в папку info на хостовой машине;

	https://i2.paste.pics/ffce2e657ac5c090187641cf424aa5a3.png

Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /info контейнера.

	https://i2.paste.pics/4003fcf66c283b4b66f853a536eae6fb.png


