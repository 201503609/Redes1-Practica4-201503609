## **UNIVERSIDAD DE SAN CARLOS DE GUATEMALA**
## **FACULTAD DE INGENIERIA**

*** 
**NOMBRE:** DIEGO JOSUE BERRIOS GUTIERREZ  
**CARNÉ:** 201503609  
**CURSO:** REDES DE COMPUTADORAS 1  
**SECCIÓN:** N  
**Redes1-Practica2-201503609**
*** 

# MANUAL DE REPORTES
A continuación se detallan los dominios de broadcast, dominios de colisión y como capturar paquetes.

#  Dominio de broadcast y dominios de colisión
## Topología: 

![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Topologia.png "TopoLogia")

| HOST | CONECTADO A |
|:-----|:-----------:|
| VPC 1| SWITCH 1    |
| VPC 2| SWITCH 1    |
| VPC 3| SWITCH 2    |
| VPC 4| SWITCH 2    |
| VPC 5| SWITCH 2    |
| VM   | SWITCH 3    |
| SWITCH1 | ROUTER |
| SWITCH2 | ROUTER |
| SWITCH3 | ROUTER |

### Dominios de Colisión y Dominios de Broadcast
* El router cuenta con 1 dominio de colisión y 1 de broadcast por cada interfaz. 
* Contamos con 3 interfaces en el router
  * DC = 3 
  * DB = 3
* El switch 1 tiene 2 dominios de colisión, 1 por cada vpc
  * DC = 2
* El switch 2 tiene 3 dominios de colisión, 1 por cada vpc
  * DC = 3
* El switch 3 tiene 1 dominios de colisión, 1 por cada vpc
  * DC = 1

Por lo mencionado anteriormente:  
  * DC = 9 (rojo)
  * DB = 3 (verde)

![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/DBDC.jpg "DB")
# Capturas de paquetes
 ## Capturar paquetes desde la VPC1
1. La topología debe de estar encendida.
2. Hacer click derecho sobre el cable que conecta la vpc1 con el switch
3. Seleccionamos la opcion **Start Capture**  
    ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/PasoCaptura1.png "Paquete1")

4. Seleccionamos la opcion **ok**  
    ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/PasoCaptura2.png "Paquete2")

5. Esperamos que se abra wireshark  
    ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/PasoCaptura3.png "Paquete3")

6. Hacemos ping de la VM a la VPC1
7. Escribimos **icmp** en el filtro de paquetes.
8. Observamos como se capturan los paquetes.  
    ![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura1.png "Captura1")

### Captura de paquetes en la VPC2
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura2.png "Captura2")
### Captura de paquetes en la VPC3
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura3.png "Captura3")
### Captura de paquetes en la VPC4
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura4.png "Captura4")
### Captura de paquetes en la VPC5
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura5.png "Captura5")
### Captura de paquetes en la VM
![alt text](https://github.com/201503609/Redes1-Practica2-201503609/blob/master/src/Captura6.png "Captura6")
