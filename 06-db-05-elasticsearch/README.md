# 6.5. Elasticsearch

## 1 задание

Создавала по этому докерфайлу:

	FROM centos:7

	RUN yum install java-11-openjdk -y \
	    && yum install wget -y

	RUN wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.1-x86_64.rpm \
	    && wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.1-x86_64.rpm.sha512 \
	    && yum install perl-Digest-SHA -y

	RUN shasum -a 512 -c elasticsearch-7.15.1-x86_64.rpm.sha512 \ 
	    && rpm --install elasticsearch-7.15.1-x86_64.rpm


## 2 задание

Cоздание индексов, получение их списка и состояния кластера. Два индекса в статусе yellow потому что им задали несколько реплик, а сервер один. Соответственно и кластер работает нестабильно, потому что индексы не в порядке.

![alt text](https://i2.paste.pics/d37de0e86401deb61743c774d33217b2.png)

Удаление индексов.

![alt text](https://i2.paste.pics/ae60ee6499a85c1053abca77525fa595.png)

## 3 задание

Создание репозитория.

![alt text](https://i2.paste.pics/4e24930b46aae51ec10917cb101692ab.png)

Создание индекса test с 0 реплик и 1 шардом.

![alt text](https://i2.paste.pics/4e200d7498b096691389d80106c2f772.png)

Создание snapshot состояния кластера.

![alt text](https://i2.paste.pics/90700a7281f34b6dc2f906b75d67d089.png)

Удалите индекс test и создайте индекс test-2.

![alt text](https://i2.paste.pics/7229c1077b7ca740e5cbd0343a63ac44.png)

![alt text](https://i2.paste.pics/7f93426ec2e21c37b14a0c084c7aae06.png)

![alt text](https://i2.paste.pics/3cea724ddc79af306a8490fd6ef36f3a.png)

Восстановите состояние кластера elasticsearch из snapshot, созданного ранее.

![alt text](https://i2.paste.pics/7c6f486e5ab3c6d42bb4d2362095a599.png)


