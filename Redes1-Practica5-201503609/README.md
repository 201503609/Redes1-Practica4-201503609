## **UNIVERSIDAD DE SAN CARLOS DE GUATEMALA**
## **FACULTAD DE INGENIERIA**

*** 
**NOMBRE:** DIEGO JOSUE BERRIOS GUTIERREZ  
**CARNÉ:** 201503609  
**CURSO:** REDES DE COMPUTADORAS 1  
**SECCIÓN:** N  
**Redes1-Practica4-201503609**
*** 

# MANUAL DE CONFIGURACIÓN
A continuación se detallan las configuraciones necesarias para administrar de forma correcta la red y proveer comunicación de acuerdo a las necesidades indicadas.

# TOPOLOGÍA
Se busca crear una red pequeña conformada por 4 computadoras distribuidas en dos departamentos, en la cual los dispositivos este conectados y pueden comunicarse unicamente entre departamentos. La topología de la practica consta de los siguientes elementos:
* 4 Máquinas
    * 3 VPC
    * 1 VM (Configurada con Tiny Linux)
* 2 Switch
* 2 EthernetSwitch Router (ESW)
* 1 Router
* Cables de red

La conexión sera la siguiente:

* 1 ESW al router
* Los ESW conectados entre si
* La VPC 1 y la VM iran conectadas al switch 1
* La VPC 2 y la VPC 3 contectadas al switch 2
* EL switch 1 ira conectado al ESW 2
* EL switch 2 ira conectado al ESW 3

Las ips asignadas seran las siguientes:

| VLAN | DIRECCION DE RED | PRIMERA DIRECCION ASIGNABLE | ULTIMA DIRECCION ASIGNABLE | DIRECCION DE BROADCAST |
|:-----|:-----------:|---:| :------:| :------:|
| 10 | 192.168.19.0/24 | 192.168.19.1 | 192.168.19.254 | 192.168.19.255 |
| 20 | 192.168.29.0/24 | 192.168.29.1 | 192.168.29.254 | 192.168.29.255| 



| HOST | CONECTADO A | IP | GATEWAY |
|:-----|:-----------:|---:| :------:|
| VPC 1| SWITCH 1    |192.168.29.10 | 192.168.29.254 |
| VPC 2| SWITCH 2    |192.168.19.10 | 192.168.19.254 |
| VPC 3| SWITCH 2    |192.168.29.15| 192.168.29.254 |
| VM   | SWITCH 1    |192.168.19.15| 192.168.19.254 |


| HOST | CONECTADO A | INTERFAZ A| INTERFAZ B |
|:-----|:-----------:|---:| :------:|
| SWITCH1 | ESW 2 | e1 | f1/5 |
| SWITCH1 | ESW 2 | e2 | f1/6 |
| SWITCH2 | ESW 3 | e1 | f1/3 |
| SWITCH2 | ESW 3 | e2 | f1/4 |
| ESW 1 | ROUTER | f0/0 | f0/0 |
| ESW 1 | ESW 2 | f1/3 | f1/3 |
| ESW 1 | ESW 2 | f1/4 | f1/4 |
| ESW 1 | ESW 3 | f1/5 | f1/5 |
| ESW 1 | ESW 3 | f1/6 | f1/6 |
| ESW 2 | ESW 3 | f1/1 | f1/1 |
| ESW 2 | ESW 3 | f1/2 | f1/2 |


MASCARA DE RED:
| MASCARA |
| :-----: |
| 255.255.255.192 |


ROUTER:
| INTERFAZ | VLAN | IP |
|:----:|:-----------:|:------------: |
| f0/0.10     | 10 | 192.168.19.254|
| f1/0.20     | 20 | 192.168.29.254|


Quedando de la siguiente manera:

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Topologia.png "TopoLogia")

# Configuraciones

## Configuración de los switch
1. Seleccionamos el switch 1 y abrimos la configuración
2. Seleccionamos el puerto 1 y lo cambiamos a tipo truncal (dot1q), luego le damos add para que se guarden los cambios.
3. Seleccionamos el puerto 2 y lo cambiamos a tipo truncal (dot1q), luego le damos add para que se guarden los cambios.
4. Seleccionamos el puerto 3 y cambiamos la VLAN a 10, luego le damos add para que se guarden los cambios.
4. Seleccionamos el puerto 4 y cambiamos la VLAN a 20, luego le damos add para que se guarden los cambios.
5. Repetimos los pasos del 1 - 4 para el switch 2.

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/SwitchTrunk.png "SwitchTrunk")


## Configuración de VTP en los ESW
1. Abrimos la consola del ESW
2. Escribimos el comando **Conf t** 
3. Escribimos el comando **vtp domain redes1_carnet**
4. Escribimos el comando **vtp password redes1_carnet**
5. Escribimos el comando **vtp mode server**, para el esw1 utilizamos **server** mientras que para los esw2 y esw3 utilizamos **client**.
6. Escribimos el comando **end** para salir de la configuración.
7. Escribimos el comando **wr** y **wr mem** para guardar los cambios.
8. Repetimos los pasos del 1-7 para los esw2 y esw3.

### ESW 1
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/VTP.png "vtp")
### ESW 2
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/VTP2.png "vtp")
### ESW 3
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/VTP3.png "vtp")

## Confuguracion de la VLAN
Seguimos los siguientes pasos para configurar la vlan 10(VENTAS) y la vlan 20 (CONTABILIDAD):
1. Abrimos la consola del ESW1
2. Escribimos el comando **Conf t**
3. Escribimos el comando **vlan #**
4. Escribimos el comando **name NOMBRE**
5. Escribimos el comando **end**
6. Escribimos el comando **wr** y **wr mem** para guardar los cambios.

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/ventas.png "ventas")

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/contabilidad.png "conta")

En todos los ESW ejecutamos los siguientes comandos para que se puedan compartir la VLAN:
1. **conf t**
2. **int range** **f#/#** **-** **#**, seleccionamos todas las interfaces que se esten utilizando del ESW.
3. **no shut**
4. **switchport mode trunk**
5. **end** 
6. **wr** y **wr mem** para guardar los cambios.

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/compartir.png "vlan")

## Configuracion de la VPC
 1. Seleccionamos la VPC
 2. La iniciamos
 ![alt text](https://github.com/201503609/Redes1-Practica1-201503609/blob/master/src/EncenderVPC.png "EncenderV")

 3. Ingresamos a la consola.
 4. Ingresamos el comando **ip numerodeip  gateway #####**, el numero de ip y gateway varian segun sea el asignado para la VPC que se este configurando.
 5. Ingresamos el comando **save** para que se guarden las configuraciones de la VPC
 
### Configuracion VPC1
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Config2.png "ConfigV")
### Configuracion VPC2
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Config3.png "ConfigV2")
### Configuracion VPC3
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Config4.png "ConfigV3")


## Configuracion de la VM
1. Seleccionamos la VM
2. La encendemos.
3. Esta abrira una VM en VMWare, por lo que deberemos de inciar la maquina con Tiny-Linux
4. Seleccionar Panel de Control
5. Selecciona la opcion Network
6. Escribir la ip que se le desea asignar a la VM
7. Escribir la mascara de red que se desea asignar
8. Escribir la direccion Broadcast que se desea asignar
9. Seleccionar la opcion de Aplicar.

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Config7.png "ConfigM")


## Configuracion del Router
Para configurar las interfaces de las VLAN, seguimos los siguientes pasos:

1. Encedemos el router
![alt text](https://github.com/201503609/Redes1-Practica1-201503609/blob/master/src/EncenderRouter.png "EncenderR")

2. Abrimos su consola
3. Escribimos el comando **Conf t**
4. Escribimos el comando **Int f0/0.10** para configurar la interfaz FastEthernet0/0 del router para la vlan 10.
5. **encapsulation dot1q 10**
6. Asignamos la ip a la interfaz con el comando **ip address 192.168.1x.254 255.255.255.0** , en donde x representa el digito del carnet a utilizar.
7. Activamos la interfaz con el comando **no shut**
8. Escribimos el comando **end** para salir de la interfaz. 
9. Repetimos el paso 4,5,6,7 y 8 para la VLAN 20, a diferencia que de usar el 10, utilizamos el 20.
11. Validamos que las interfaces esten activas con el comando **Sh i int brief**
12. Ingresamos el comando **wr** para guardar las configuraciones del router.

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/Config1.png "ConfigR")

 
 ## Configuracion del CHannel group

 Para configurar los group channel, debemos hacerlo de ambos lados. por lo que debemos ejecutar los siguientes comandos desde la consola de ambos ESW:

 1. **Conf t**
 2. **int range f#/# - #**, el rango de interfaces salientes del ESW al ESW que nos conectamos para el grupo.
 3. **channel-group # mode on**, seleccionamos el # de grupo que estamos creando, en ambas direcciones debe de ser el mismo.
 4.**end**
 5.**wr** y **wr mem** para guardar los cambios.

 ### GRUPO 1
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/1g1.png "grup1")

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/2g1.png "grup12")

 ### GRUPO 2
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/1g2.png "grup2")

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/3g23.png "grup21")

 ### GRUPO 3 
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/2g3.png "grup3")

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/3g23.png "grup31")

### PING DESDE VPC1
 Para comprobar que existe comunicación desde la VPC1 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 

 Se muestra:
 * Ping a VPC2
 * Ping a VPC3
 * Ping a VM

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping1.png "PingV1")


### PING DESDE VPC2
 Para comprobar que existe comunicación desde la VPC2 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC3
 * Ping a VM

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping2.png "PingV2")

### PING DESDE VPC3
  Para comprobar que existe comunicación desde la VPC3 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VM  

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping3.png "PingV3")


### PING DESDE VM
Para comprobar que existe comunicación desde la VM con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 

 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VPC3
 
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6.png "PingVM")
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6B.png "PingVM1")
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6C.png "PingVM2")

## SPANNING TREE
1. **sh spanning-tree root** desde el esw1 
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/spaTreeRoot.png "root")


![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/spa.png "b11")

1. **sh spa block**
![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/block1.png "bl")

![alt text](https://github.com/201503609/Redes1-Practica4-201503609/blob/master/src/block2.png "bl2")


# GLOSARIO
1. **Topología:** se define como el mapa físico o lógico de una red para intercambiar datos. En otras palabras, es la forma en que está diseñada la red, sea en el plano físico o lógico.
2. **VPC:** es un conjunto de recursos computacionales configurables por demanda al interior de un ambiente de computación en la nube pública, el cual provee un cierto nivel de aislamiento entre las diferentes organizaciones o usuarios que utilizan dichos recursos.
3. **IP:** es un número que identifica de forma única a una interfaz en red de cualquier dispositivo conectado a ella que utilice el protocolo IP (Internet Protocol), que corresponde al nivel de red del modelo TCP/IP.
4. **RED:** es un conjunto de equipos nodos y software conectados entre sí por medio de dispositivos físicos o inalámbricos que envían y reciben impulsos eléctricos, ondas electromagnéticas o cualquier otro medio para el transporte de datos, con la finalidad de compartir información, recursos y ofrecer servicios.
5. **PING:** es una utilidad de diagnóstico en redes de computadoras que comprueba el estado de la comunicación del anfitrión local con uno o varios equipos remotos de una red que ejecuten IP.
6. **ROUTER:** es un dispositivo que permite interconectar computadoras que funcionan en el marco de una red. Su función: se encarga de establecer la ruta que destinará a cada paquete de datos dentro de una red informática.
7. **SWITCH:** Un switch o conmutador es un dispositivo de interconexión utilizado para conectar equipos en red formando lo que se conoce como una red de área local (LAN) y cuyas especificaciones técnicas siguen el estándar conocido como Ethernet.
8. **BYTES:** es la unidad de información de base utilizada en computación y en telecomunicaciones, y que resulta equivalente a un conjunto ordenado de ocho bits.
9. **PACKET:** es cada uno de los bloques en que se divide la información para enviar, en el nivel de red. Por debajo del nivel de red se habla de trama de red, aunque el concepto es análogo.
10. **GATEWAY:** es el dispositivo que actúa de interfaz de conexión entre aparatos o dispositivos, y también posibilita compartir recursos entre dos o más ordenadores.
11. **HOST:** se usa en informática para referirse a las computadoras u otros dispositivos (tabletas, móviles, portátiles) conectados a una red que proveen y utilizan servicios de ella.
12. **COMANDO:** es una instrucción que el usuario proporciona a un sistema informático, desde la línea de órdenes (como una shell) o desde una llamada de programación. 
13. **TERMINAL:** es un dispositivo electrónico o electromecánico que se utiliza para interactuar con un(a) computador(a).
14. **VIRTUALIZAR:** es cuando se crea un entorno informático simulado en lugar de una versión física. El hardware y los sistemas operativos se pueden virtualizar.
