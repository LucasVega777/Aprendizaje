# Kafka Connect
![Arquitectura Kafka Connect](/img/kafka-connect-arq.jpg "Kafka Connect")

## ¿Que es Kafka Connect?

Es una integracion sin tener que desarrollar (solo configurable).

![Arquitectura Kafka Connect](/img/kafka-connect.png "Kafka Connect")

Se puede usar para consumir flujos de eventos en tiempo real desde la fuente de datos y transmitirlos a un Sistema de destino para analisis.

En el grafico se nota un Sistema debilmente acoplado, esto significa que es relativamente facil el cambio de la fuente y destino de datos, sin impactar al otro.

Al usar Kafka, el sistema en su conjunto es Escalable **(esKafkalable xD)** y tolerante a fallas. Debido a que Kafka almacena datos hasta un intervalo de tiempo configurable, por entidad de datos o topico de Kafka, es posible transmitir los mismos datos originales a varios destinos posteriores, eso quiere decir, que solo necesita mover los datos a Kafka y desde ahi pueden ser consumidos por una serie de tecnologias posteriores.

## ¿Con que es compatible Kafka Connect?
* Bases de datos relacionales
* Almacenes de objetos en la nube
* Cola de mensajes

## ¿Por que usar Kafka Connect?
En resumen, para no reinventar la rueda.

## 



# Autor:
[Lucas Vega](https://github.com/LucasVega777/)