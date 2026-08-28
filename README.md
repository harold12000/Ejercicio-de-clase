# Ejercicio en Clase - Actividad 5

## Que es un archivo pcap

Un archivo .pcap (Packet Capture) es un archivo que guarda todo el trafico de
red que paso por una interfaz durante un tiempo determinado, paquete por
paquete. Ahi queda registrado de donde viene cada paquete, hacia donde va, que
protocolo usa (TCP, UDP, etc), el puerto, el tamano y hasta el contenido si no
esta cifrado.

Se usa principalmente para analisis y diagnostico de redes: sirve para ver si
hay errores, retransmisiones, cuellos de botella, o para detectar trafico
sospechoso en analisis de seguridad (forense de red). Herramientas como
Wireshark, tcpdump o RED HAND leen estos archivos y muestran la informacion de
forma organizada para poder estudiarla.

## 1. Que es YOLO

YOLO (You Only Look Once) es un modelo de deteccion de objetos en imagenes y video
que funciona en tiempo real. A diferencia de otros metodos que analizan la imagen
por partes, YOLO mira toda la imagen de una sola vez y en un solo paso predice
donde estan los objetos y que son. Sus caracteristicas principales son la velocidad
(por eso se usa en camaras y video en vivo) y que puede detectar varios objetos al
mismo tiempo. Su arquitectura esta basada en una red neuronal convolucional (CNN)
que divide la imagen en una cuadricula y para cada celda predice cajas
delimitadoras (bounding boxes) con la probabilidad de que ahi haya un objeto y de
que clase es.

## 2. Protocolo vs Aplicacion (TCP vs UDP)

La descarga del modelo usa TCP porque es un archivo que tiene que llegar completo
y sin errores, si se pierde un paquete no sirve el archivo. TCP garantiza la
entrega confirmando cada paquete y reenviando los que se pierden.

La transmision de video en cambio usa UDP porque lo que importa ahi es la
velocidad y que no haya retraso, no tanto que llegue el 100% de los datos. Si se
pierde un fotograma no pasa nada grave, se sigue viendo el video, pero si TCP
tuviera que confirmar cada paquete de video se generaria retraso (latencia) y el
video se veria trabado.

## 3. Fiabilidad vs Velocidad (retransmisiones TCP)

En la captura descarga_tcp.pcap no se observo ningun evento marcado como
tcp.analysis.retransmission. La herramienta si marco dos alertas de
comportamiento (una "Control Connection" y una "Long Connection"), pero
corresponden a una conexion normal y sostenida entre las IPs internas
172.28.0.1 y 172.28.0.12 de la maquina de Colab, no a un problema real de
red.

Si hubiera aparecido un tcp.analysis.retransmission, significaria que un
paquete no llego o llego con error y TCP lo volvio a enviar para asegurar que
la informacion llegara completa. Esto es clave para la descarga de un archivo
porque si falta un pedazo el archivo queda corrupto. En cambio en un video en
vivo esto seria perjudicial porque reenviar un paquete atrasado ya no sirve de
nada (el momento del video ya paso) y solo generaria mas retraso, por eso ahi
se prefiere UDP que no reenvia nada.

## 4. Identificando el origen (filtros ip.dst / ip.src)

En esta captura las direcciones que aparecen (172.28.0.1 y 172.28.0.12) son
internas de la maquina virtual de Google Colab, ya que Colab enruta la salida
a internet mediante un proxy interno, por lo que al capturar en la interfaz
eth0 solo se ve el tramo interno de la conexion y no la IP real del servidor
de Ultralytics o Google Cloud Storage.

Aun asi, para aislar el trafico con un servidor especifico se puede usar en
RED HAND o Wireshark un filtro como:

ip.addr == [direccion IP del servidor]

o de forma mas especifica:

ip.src == [IP del servidor]  o  ip.dst == [IP del servidor]

ip.src filtra los paquetes que salen desde esa direccion e ip.dst filtra los
paquetes que van hacia esa direccion. Con esto se puede ver solo la
conversacion entre dos equipos puntuales, sin ruido de otras conexiones.

FASE 3 ULTIMA DE LA ACTIVIDAD

## Descargar archivo

[⬇️ DESCARGAR RED CONVERGENTE](https://github.com/harold12000/red-convergente/raw/refs/heads/main/red_convergente.html)
