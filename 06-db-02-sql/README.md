# 6.2. SQL

## 1 задание
	
	docker pull postgres:12
	docker volume create volume1
	docker volume create volume2
	docker run --rm --name postgres -e POSTGRES_PASSWORD=password123 -ti -p 5000:5000 -v volume1:/var/lib/postgresql/data -v volume2:/var/lib/postgresql postgres:12

## 2 задание

итоговый список БД после выполнения пунктов выше

![alt text](https://i2.paste.pics/3cd995bffe93d92eb3a397865c6be327.png)

описание таблиц (describe)

![alt text](https://i2.paste.pics/d275691005152a2294bf5caa0825625a.png)

![alt text](https://i2.paste.pics/cb18d36a3f0d50307c06fec96e03f66a.png)

SQL-запрос для выдачи списка пользователей с правами над таблицами test_db

![alt text](https://i2.paste.pics/615261373b57f24e5035f5db01eb874b.png)

![alt text](https://i2.paste.pics/dd268999cd714c3279a56cbe4335b414.png)


## 3 задание

![alt text](https://i2.paste.pics/b5c2aeb3020cb51b8e2473d269336b89.png)

## 4 задание

![alt text](https://i2.paste.pics/717c173973219547411515613af5ac16.png)

## 5 задание

![alt text](https://i2.paste.pics/88b8ad4b193111191427b236ee8f53da.png)

Алгоритм Hash Join - неплохо, если таблицы большие и нет пригодного для использования индекса.

Seq Scan - плохой знак, т.к. субд обходит все строки соединения в поисках нужных. В нашем случае он перебрал по 5 строк в каждой таблице  (actual time=0.016..0.020 rows=5 loops=1).

Если таблицы будут большими, запрос будет выполняться долго, нужно создавать индексы на столбец с внешним ключом.
