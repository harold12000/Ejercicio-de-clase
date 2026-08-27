# Ejercicio-de-clase
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

[Completar aqui segun lo que se vea en la captura real: si aparecio o no
tcp.analysis.retransmission]

Si aparece un tcp.analysis.retransmission quiere decir que un paquete no llego
o llego con error, y TCP lo volvio a enviar para asegurarse de que la informacion
quedara completa. Esto es clave para la descarga de un archivo porque si falta
un pedazo el archivo queda corrupto. En cambio en un video en vivo esto seria
perjudicial porque reenviar un paquete atrasado ya no sirve de nada (el momento
del video ya paso) y solo generaria mas retraso, por eso ahi se prefiere UDP que
no reenvia nada.

## 4. Identificando el origen (filtros ip.dst / ip.src)

[Completar aqui con la IP real que aparece en tu captura]

Para aislar el trafico que viene del servidor que entrego el modelo se puede usar
un filtro en RED HAND o Wireshark como:

ip.addr == [direccion IP del servidor]

o de forma mas especifica:

ip.src == [IP del servidor]  o  ip.dst == [IP del servidor]

ip.src filtra los paquetes que salen desde esa direccion (por ejemplo cuando el
servidor envia los datos del modelo) e ip.dst filtra los paquetes que van hacia
esa direccion (por ejemplo cuando el computador pide la descarga). Con esto se
puede ver solo la conversacion entre el computador y el servidor de Ultralytics
o Google Cloud Storage, sin ruido de otras conexiones.
