# 4.1. Командная оболочка Bash: Практические навыки

## 1 задание
	
1) будет ошибка, т.к. пытаемся сложить строку и число

2) чтоб получить 12, надо преобразовать a в строку

	c=str(a)+b

3) чтоб получить 3, надо преобразовать b в число

	c=a+int(b)
			

## 2 задание

    ##!/usr/bin/env python3

    import os

    bash_command = ["cd ~/dev/sysadm-homeworks", "git status"]
    result_os = os.popen(' && '.join(bash_command)).read()
    for result in result_os.split('\n'):
        if result.find('изменено') != -1:
            prepare_result = result.replace('\tmodified:   ', '')
            print(prepare_result)


1)break прерывает цикл for после первого нахождения, поэтому его надо убрать

2)is_change - переменная нигде не используется

## 3 задание

	##!/usr/bin/env python3
	import os
	import sys

	if len(sys.argv)<2:
	    print('вы не ввели url для проверки')
	else:
	    url = sys.argv[1]	
	    if os.path.isdir(url):
		bash_command = ["cd "+url, "git status 2>&1"]
		result_os = os.popen(' && '.join(bash_command)).read()
		for result in result_os.split('\n'):
		    if result.find('fatal') != -1:
		        print(url+'не является GIT репозиторием')  
		    if result.find('изменено') != -1:
		        prepare_result = result.replace('\tизменено: ', '')
		        print(url+prepare_result)
	    else:
		    print(url+' такого каталога нет') 

добавила проверки на введение аргумента, является ли путь каталогом и является ли репозиторием. Результат:

https://i2.paste.pics/1b69bbfa82d8bf4fecc7df9aff759502.png
	
## 4 задание

	##!/usr/bin/env python3

	import socket
	import time

	ips = {'drive.google.com':'74.125.205.194', 'mail.google.com':'64.233.164.18', 'google.com':'142.251.1.138'}

	while 1==1 :
	    for h in ips:
		ip = socket.gethostbyname(h)
		if ip == ips[h]:
		    print(h+' - '+ip)
		else:
		    print(' [ERROR] ' + str(h) +' IP mistmatch: '+ips[h]+' '+ip)
		    ips[h]=ip
	    time.sleep(5)

скрипт проходится по хостам раз в 5 секунд
