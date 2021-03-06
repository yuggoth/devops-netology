# 5.2. Системы управления виртуализацией

## 1 задание
	
`100 виртуальных машин на базе Linux и Windows, общие задачи, нет особых требований
Преимущественно Windows based инфраструктура, требуется реализация программных балансировщиков нагрузки, репликации данных и автоматизированного механизма создания резервных копий`

Подойдет VMware vSphere. Можно было бы рассматривать и Hyper-V, но на лекции сказали, что это решение "винда для винды", и с Linux могут возникнуть проблемы.
Если нужно бесплатно, можно KVM или Xen.

`Требуется наиболее производительное бесплатное opensource решение для виртуализации небольшой (20 серверов) инфраструктуры Linux и Windows виртуальных машин`

Из бесплатных подойдет KVM. Xen считается менее производительным при работе с Windows.

`Необходимо бесплатное, максимально совместимое и производительное решение для виртуализации Windows инфраструктуры`

Можно посмотреть бесплатную версию Hyper-V, она максимально совместима с Windows.

`Необходимо рабочее окружение для тестирование программного продукта на нескольких дистрибутивах Linux`

Для тестирования подойдет opensource решение KVM или Xen. Также подойдут облачные сервисы AWS или Openstack.

## 2 задание

Виртуальная машина состоит из двух основных частей: виртуальной машины - текстового или XML-файла с описанием ее конфигурации и виртуального жесткого диска. Если надо запустить систему на другом ПК, достаточно перенести виртуальный диск. Форматы виртуальных дисков у разных гипервизоров различны, поэтому надо использовать специализированное ПО для конвертации. Алгоритм выглядит так:

1) Нужно выключить виртуальную машину VMware vSphere
2) Скопировать файлы машины на сервер (возможно стоит сразу скопировать файлы на сервер с установленной ролью Hyper-V, где в дальнейшем нужно будет запустить виртуальную машину)
3) Сконвертировать виртуальный диск из формата VMware (vmdk) в формат Hyper-V (vhd). Для этого есть разные утилиты: Microsoft Virtual Machine Converter 3.0, System Center Virtual Machine Manager, 5nine easy converter. Первые две - разработка Microsoft, а последний - сторонний продукт. Microsoft не принимает инциденты по конвертации сторонним продуктом.
4) Создать виртуальную машину Hyper-V из vhd файла

Описанные способы используют платные решения, но есть бесплатный способ:

Скриптовое решение на базе NFS, VMotion и бесплатных утилит. В качестве альтернативы коммерческим решениям предлагается вариант,  позволяющий существенно снизить время простоя виртуальной машины на период миграции. На сервере Windows один из дисков предоставляется в общий доступ как по протоколу SMB (типовые общие папки Windows), так и по протоколу NFS (протокол общих дисков для ОС UNIX, поддерживаемый также VMware VSphere). На стороне VMware vCenter общий том NFS добавляется как хранилище, и виртуальная машина переносится на него во включенном состоянии с помощью технологии vMotion. После того, как файлы машины окажутся на сервере Windows, машина выключается, и ее диск из формата VMDK мгновенно конвертируется сначала в VHD, а затем в VHDX с помощью изменения заголовков файлов виртуального диска утилитами VHDtool и VHDXtool. Далее скриптами PowerShell для VMM  создается целевая высокодоступная виртуальная машина на Hyper-V  с параметрами,  аналогичными исходной,  к ней подключается полученный VHDX диск. На виртуальный диск, еще в выключенном состоянии, устанавливаются компоненты интеграции. Виртуальная машина включается, требуется одна перезагрузка, виртуальная машина получает старый IP-адрес и готова к работе через несколько минут недоступности.

## 3 задание

- Придется нанимать более квалифицированных, а значит дорогих специалистов, которые смогут администрировать разные системы.
- Подбирать и настраивать средства мониторинга и защиты под две разные системы.
- Если ПО для виртуализации и мониторинга платное, покупать в два раза больше лицензий.

Наверное, надо стараться избегать этих проблем и использовать одну систему, но наверное не всегда это возможно. Например, как говорили на лекции, для Windows лучше подходит Hyper-V, но в то же время если нужен Linux, при его использовании на Hyper-V могут быть проблемы, и придется искать какое-то другое решение. 
