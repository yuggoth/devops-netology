# 4.3. Языки разметки JSON и YAML

## 1 задание
	
В 9 строчке не хватает кавычек:

"ip" : "71.78.22.43"	

## 2 задание

	##!/usr/bin/env python3

	import socket
	import time
	import json
	import yaml

	ips = {'drive.google.com':'74.125.205.194', 'mail.google.com':'64.233.164.18', 'google.com':'142.251.1.138'}

	while 1==1 :
	    data = []
	    for h in ips:
		ip = socket.gethostbyname(h)
		if ip == ips[h]:
		    print(h+' - '+ip)
		else:
		    print(' [ERROR] ' + str(h) +' IP mistmatch: '+ips[h]+' '+ip)
		    ips[h]=ip
		data.append({h:ips[h]})
	    with open("/home/ekaterina/dev/services_conf.json",'w') as jsf:
		json.dump(data,jsf)
	    with open("/home/ekaterina/dev/services_conf.yaml",'w') as ymf:
		ymf.write(yaml.dump(data))
	    time.sleep(5)
