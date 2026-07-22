---
title: "UBUNTU 6.0.6 HP Omnibook xe3. WPC54G"
date: 2006-08-20 11:51:00.001
tags: ["WPC54G", "pcmcia", "ubuntu", "linux", "xe3", "hp", "linksys"]
category: professional
---

Antes de nada. Si normalmente lees este blog, este post, te va a parecer un coñazo, no lo leas; va en serio. Este post va dedicado a toda la gente q se parece a mi y q le encanta la informatica y no se conforma con lo q hay.  
  
Ultimamente, mi portatil no hacia nada: me conectaba al messenger, al gmail, y escribia en este post. Vamos, q era una mierda lo q hacia (el emule está dedicado a otra maquina, tranquis). Asi q, mi primo me pregunto como se hackeaban las wireless y tuve q darles unas urls...Pero como la curiosidad me mata, acabe descubriendo q mi tarjeta de red inalambrica tb estaba soportada, así como mi camara web...Acabe desterrando a window y pasandome a linux. En concreto a ubuntu.  
Comienzo el post  
  
UBUNTU 6.0.6 HP Omnibook xe3  
  
En principio no hay problemas: metes el cd, siguiente, siguiente, siguiente....Todo correcto.  
No bstante tengo un par de dispositivos con los que he tenido problemas.  
El primero Tarjeta Wireless PCMCIA Linksys WPC54G. Si se teclea en la consola “lspci -v” aparece lo siguiente:  
  
0000:02:00.0 Network controller: Broadcom Corporation BCM4306 802.11b/g Wireless LAN Controller (rev 03)  
 Subsystem: Linksys WPC54G  
 Flags: bus master, fast devsel, latency 64, IRQ 11  
 Memory at 22000000 (32-bit, non-prefetchable) [size=8K]  
 Capabilities: [40] Power Management version 2  
  
Por lo tanto, driver “bcm4306” de Broadcom. Ahora se nos plantean 2 opciones:  
Opcion 1)Instalar Ndiswrapper  
Opcion 2)Instalar bcm43xx-fwcutter  
  
No voy a explicarlos pq hay gente que lo hará mejor por la web, así que deciros que me decante por la Opcion 2) pq kismet está en fase de desarrollo de este tipo de drivers....y bla bla bla...traduciendo, que de esta manera hay alguna posibilidad de que en un futuro se puedan hackear las WEP keys.  
  
Instalamos bcm43xx-fwcutter:  
documentacion de estos currantes de ingenieria inversa en:  
http://bcm43xx.berlios.de/?go=Documentation y mas concretamente de la doc de la tarjeta para ubuntu dapper:  
https://help.ubuntu.com/community/WifiDocs/Driver/bcm43xx/Dapper  
  
# apt-get install bcm43xx-fwcutter  
  
Nos bajamos los drivers de la tarjeta PCMCIA. En mi caso, esta es la url:  
http://www.linksys.com/servlet/Satellite?c=L\_Download\_C2&childpagename=US%2FLayout&cid=1115417109934&packedargs=sku%3D1125638812437&pagename=Linksys%2FCommon%2FVisitorWrapper  
  
Instalamos el driver con:  
/usr/share/bcm43xx-fwcutter/install\_bcm43xx\_firmware.sh  
  
Antes de ver el archivo interfaces, mi configuracion del router es la siguiente:  
No DHCP  
open key  
WEP  
  
Ahora la configuracion del archivo /etc/network/interfaces. Por favor que nadie se lleve a equivoco con los modulos tulip, ni wlan0 ni pollas en vinagre. Yo no tengo ni idea de linux y así es como me funciona la wireless.  
  
auto eth1  
iface eth1 inet static  
address xx.xx.xx.xx  
netmask 255.255.255.0  
gateway xx.xx.xx.xx  
wireless-essid miessid  
wireless-key miclave  
  
¿como sé si me funciona la wireless? pues tecleo esto:  
#iwlist eth1 scan  
y me da el siguiente resultado:  
  
Cell 02 - Address: 00:XX:5A:35:89:XX  
 ESSID:""  
 Protocol:IEEE 802.11bg  
 Mode:Master  
 Channel:6  
 Encryption key:on  
 Bit Rates:54 Mb/s  
 Extra: Rates (Mb/s): 1 2 5.5 6 9 11 12 18 22 24 36 48 54  
 Quality=100/100 Signal level=-157 dBm  
 Extra: Last beacon: 60ms ago  
  
Lo que significa que ve algo y por lo tanto vamos por buen camino. Es posible que necesites levantar y bajar la interface..y bajar previamente la tarjeta de red con cable desde donde nos hemos bajado lo anterior:  
#ifdown eth0  
#ifdown eth1  
#ifup eth1  
  
YASTA!! ya tengo wireless  
  
LA WEBCAM  
Mi camara: Logitech Quickcam Express  
#lsusb  
Bus 001 Device 002: ID 046d:0928 Logitech, Inc. Quickcam Express  
  
He leido mogollon, q si tenia q compilar el kernel, q si no era necesario en la Dapper...Que me instalase el spca5xx...el qc-usb...No se como coño lo he hecho.  
Lo primero, pasad de apt-get install, pq la lista del source del apt de universe (ojo, no se q haria sin ellos) te baja la version 0.95. Pero me he instale el spca5xx-source, el qc-usb..y nada hasta q no me instale la version 0.96.  
  
#lsmod  
  
muestra quickcam, spca5xx, videodev  
  
y mi camara web funciona para mi amsn en la version 0.96 q me baje de la proppia web.  
  
Nada más, me fui de vacaciones una semana y estoy tan moreno que parezco salido de un cayuco. Relax, relax y más relax. Mañana trabajo y me duele la cabeza.
