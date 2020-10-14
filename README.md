## **UNIVERSIDAD DE SAN CARLOS DE GUATEMALA**
## **FACULTAD DE INGENIERIA**

*** 
**NOMBRE:** DIEGO JOSUE BERRIOS GUTIERREZ  
**CARNÉ:** 201503609  
**CURSO:** REDES DE COMPUTADORAS 1  
**SECCIÓN:** N  
**Redes1-Practica2-201503609**
*** 

# MANUAL DE CONFIGURACIÓN
A continuación se detallan las configuraciones necesarias para administrar de forma correcta la red y proveer comunicación de acuerdo a las necesidades indicadas.

# TOPOLOGÍA
Se busca crear una red pequeña conformada por 6 computadoras distribuidas en tres departamentos, en la cual los dispositivos este conectados y pueden comunicarse entre sí. La topología de la practica consta de los siguientes elementos:
* 6 Máquinas
    * 5 VPC
    * 1 VM (Configurada con Tiny Linux)
* 3 Switch
* 1 Router
* Cables de red

La conexión sera la siguiente:

* Los switches iran conectados al router
* La VPC 1 y la VP2 iran conectadas al switch 1
* La VPC 3, VPC4 y la VPC 5 iran contectadas al switch 2
* La VM ira conectada al switch 3

Las ips asignadas seran las siguientes:

| HOST | CONECTADO A | IP | GATEWAY |
|:-----|:-----------:|---:| :------:|
| VPC 1| SWITCH 1    |192.168.13.15 | 192.168.13.1 |
| VPC 2| SWITCH 1    |192.168.13.16 | 192.168.13.1 |
| VPC 3| SWITCH 2    |192.168.13.85 | 192.168.13.65 |
| VPC 4| SWITCH 2    |192.168.13.95 | 192.168.13.65 |
| VPC 5| SWITCH 2    |192.168.13.105| 192.168.13.65 |
| VM   | SWITCH 3    |192.168.13.130| 192.168.13.129 |
| SWITCH1 | ROUTER | - | - |
| SWITCH2 | ROUTER | - | - |
| SWITCH3 | ROUTER | - | - |


MASCARA DE RED:
| MASCARA |
| :-----: |
| 255.255.255.192 |


ROUTER:
| INTERFAZ | IP | BROADCAST |
|:----:|:-----------:|:------------: |
| f0/0     | 192.168.13.1  | 192.168.13.63 |
| f0/1     | 192.168.13.65 | 192.168.13.127 |
| f1/0     | 192.168.13.129| 192.168.13.131 |


Quedando de la siguiente manera:

![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Topologia.png "TopoLogia")

# Configuraciones
 ## Confuguracion del Router
1. Encedemos el router
![alt text](https://github.com/201503609/Redes1-Practica1-201503609/blob/master/src/EncenderRouter.png "EncenderR")

2. Abrimos su consola
3. Escribimos el comando **Conf t**
4. Escribimos el comando **Int f0/0** para configurar la interfaz FastEthernet0/0 del router
5. Asignamos la ip a la interfaz con el comando **ip address 192.168.1x.1 255.255.255.192** , en donde x representa el digito del carnet a utilizar.
6. Activamos la interfaz con el comando **no shut**
7. Escribimos el comando **exit** para salir de la interfaz. 
8. Repetimos el paso 4,5,6 y 7 para la interfaz f0/1 y la interfaz f1/0
9. Escribimos el comando **exit** para salir del menu de configuracion.
10. Validamos que las interfaces esten activas con el comando **Sh i int brief**
11. Ingresamos el comando **wr** para guardar las configuraciones del router.
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config1.png "ConfigR")

 ## Configuracion de la VPC
 1. Seleccionamos la VPC
 2. La iniciamos
 ![alt text](https://github.com/201503609/Redes1-Practica1-201503609/blob/master/src/EncenderVPC.png "EncenderV")

 3. Ingresamos a la consola.
 4. Ingresamos el comando **ip numerodeip mascaraRed gateway**, el numero de ip,gateway y mascara de red varian segun sea el asignado para la VPC que se este configurando.
 5. Ingresamos el comando **save** para que se guarden las configuraciones de la VPC
 
### Configuracion VPC1
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config2.png "ConfigV")
### Configuracion VPC2
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config3.png "ConfigV2")
### Configuracion VPC3
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config4.png "ConfigV3")
### Configuracion VPC4
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config5.png "ConfigV4")
### Configuracion VPC5
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config6.png "ConfigV5")

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

![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Config7.png "ConfigM")

### PING DESDE VPC1
 Para comprobar que existe comunicación desde la VPC1 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 

 Se muestra:
 * Ping a VPC2
 * Ping a VPC3
 * Ping a VPC4
 * Ping a VPC5
 * Ping a VM

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping1.png "PingV1")


### PING DESDE VPC2
 Para comprobar que existe comunicación desde la VPC2 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC3
 * Ping a VPC4
 * Ping a VPC5
 * Ping a VM

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping2.png "PingV2")

### PING DESDE VPC3
  Para comprobar que existe comunicación desde la VPC3 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VPC4
 * Ping a VPC5
 * Ping a VM  

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping3.png "PingV3")

### PING DESDE VPC4
  Para comprobar que existe comunicación desde la VPC4 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VPC3
 * Ping a VPC5
 * Ping a VM  

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping4.png "PingV4")


### PING DESDE VPC5
  Para comprobar que existe comunicación desde la VPC5 con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 
 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VPC3
 * Ping a VPC4
 * Ping a VM  

 ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping5.png "PingV5")





### PING DESDE VM
Para comprobar que existe comunicación desde la VM con el resto de maquinas, se hara uso del comando **ping ipnum**, en donde ipnum representa la ip de la maquina a la que se desea comprobar la comunicación. En la imagen adjunta, se muestran las pruebas realizadas. 

 
 Se muestra:
 * Ping a VPC1
 * Ping a VPC2
 * Ping a VPC3
 * Ping a VPC4
 * Ping a VPC5
 
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6.png "PingVM")
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6B.png "PingVM1")
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Ping6C.png "PingVM2")



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
