# Apache Kafka
![Logo Kafka](/img/kafka.png "Logo Kafka")

## Introduccion

### ¿Donde empezar?
* Se puede aprender los conceptos [aqui](https://developer.confluent.io/learn-kafka/)
* Se puede practicar [aqui](https://confluent.cloud/)

### ¿Que es Kafka?
Es una coleccion de registros inmutables, append-only logs (se traduciria basicamente en solo logs por agregacion ah).

Sistema distribuido.

### ¿Que es un evento en Kafka?
Es un par, clave-valor


### ¿Que es un topico?
Es el componente principal de almacenamiento.

Por lo general representa una entidad de datos en particular.

### Objetivo principal durante el aprendizaje de Kafka
Aprender a como interactuar con un Topico

### ¿Que es una particion?
Como se menciono anteriormente, el componente principal de almacenamiento es un topico, este topico se puede dividir en componentes mas pequeños llamamos particiones.

Cuando se guarda un mensaje en un topico, se guarda en una de las particiones del mismo.

Cuando se envian mensajes a un topic con diferentes Keys (claves), estos mensajes probablemente se distribuyan a diferentes particiones del topico.

### ¿Que es un Producer?
Un Producer escribe mensajes en un Topico

En Kafka se garantiza que todos los mensajes con la misma clave no vacia se enviaran a la misma particion

### ¿Que es un Consumer?
Un consumer es el que lee los mensajes de un Topico

Suelen formar parte de un grupo de Consumers, que cooperan para consumir datos de algunos topicos.

Las particiones de todos los topicos se dividen entre los consumidores del grupo para permitir el paralelismo en la lectura de los datos.

## Confluent Client

### Instalacion
Puede instalar con el siguiente comando:
```
curl -sL --http1.1 https://cnfl.io/cli | sh -s -- latest
```
### Actualizacion
Si necesita actualizar a la version mas nueva del cliente ejecute:
```
confluent update
```
### Loguearte
```
confluent login --save
```
### Listar los environments disponibles
Se pueden listar todos los environments (ambientes) disponibles
```
confluent environment list 
```
### Seleccionar un environment
```
confluent environment use env-81qjj0
```
### Listar los clusters disponibles
```
confluent kafka cluster list
```
### Seleccionar un cluster
```
confluent kafka cluster use lkc-pg68m5
```
### Generar una nueva APIKEY
Para generar una nueva clave de acceso a la API
```
confluent api-key create --resource lkc-pg68m5
```
### Agregar al CLI esta clave de acceso generada:
Si lo acabas de generar con el comando anterior, no es necesario agregarlo nuevamente, pero si un dia cambias de entorno y no quieres generar una nueva clave, puedes usar la misma con este comando.
```
confluent api-key store --resource lkc-pg68m5
```
### Una vez ya tengas creada tu APIKEY
Puedes usarla en este comando
```
confluent api-key use <API_Key> --resource lkc-pg68m5
```
La salida esperada si todo sale bien es:
```
Set API Key *********** as the active API key for "CLUSTER_NAME".
```
### Crear un nuevo Topico
```
confluent kafka topic create NOMBRE_DEL_TOPICO
```

### Listar los topicos existentes
```
confluent kafka topic list
```
### Produce mensajes a tu topico
```
confluent kafka topic produce NOMBRE_DEL_TOPICO
```
Estas fueron mis pruebas realizadas:
```
Starting Kafka Producer. Use Ctrl-C or Ctrl-D to exit.
"prueba"
"hola mundo"
```

### Consumir los mensajes de tu topico
```
confluent kafka topic consume -b "Nombre del topico"
```
Salida recibida:
```
Starting Kafka Consumer. Use Ctrl-C to exit.
"prueba"
"hola mundo"
```
Para utilizar un grupo especifico de consumers se puede agregar al comando el siguiente flag
```
confluent kafka topic consume -b NOMBRE_DEL_TOPICO --group YOUR_CONSUMER_GROUP_NAME
```

### Generar una configuracion para el cliente, para configurar tu Producer y Consumer
```
confluent kafka client-config create <LANGUAGE> --api-key=<API_KEY> --api-secret=<API_SECRET>
```
El listado de lenguajes soportados:
* ***Java***
* ***Python***
* C#
* Node.js
* ***Spring Boot***
* Go
* C/C++
* REST API
* Scala
* Clojure
* Ruby
* ***Ktor***
* Rust
* Groovy

Los lenguajes en ***negrita*** admiten claves de registro de esquema.
adjuntar: 
```
confluent kafka client-config create <LANGUAGE> --api-key=<API_KEY> --api-secret=<API_SECRET> --sr-apikey=<SR_KEY> --sr-apisecret=<SR_SECRET>
```
### Mas informacion sobre clientes [aqui](https://confluent.cloud/environments/env-81qjj0/clusters/lkc-pg68m5/clients/new)

## Topicos en Kafka
Los topicos son como contenedores nombrados que guardan eventos similares

Los sistemas contienen muchos Topicos.

Se pueden duplicar datos entre Topicos.

**NO** encola eventos, se habla de **registro** duradero de eventos

Los eventos son **inmutables**

Los topicos son duraderos

Se puede parametrizar para que caduquen los mensajes que contiene un Topico, tambien por tamaño alcanzado

## Paticiones:

### ¿Donde se guardan los mensajes?
Los mensajes **Sin Clave** que se escriban, se distribuiran por turno entre las particiones del topico, en simples palabras ***Siempre llena las particiones de manera uniforme***

Los mensajes **Con Clave**, se guardan de acuerdo a la clave, esta clave se utiliza para averiguar en que particion colocar el mensaje.

**Los mensajes con la misma clave siempre llegan a la misma particion**

Si se necesita un orden, se debe crear una clave que conserve ese orden

Ejemplo: Si se producen los eventos asociados a un cliente, si se usa el identidicador del mismo como clave, se garantizara que siempre llegaran en orden cuando se vuelvan a leer.

### Pongamoslo en Practica:

Puedes ver la descripcion de un topico con el siguiente comando:
```
confluent kafka topic describe NOMBRE_DEL_TOPICO
```
Para crear un topico con una particion
```
confluent kafka topic create --partitions 1 NOMBRE_DEL_TOPICO
```
Para crear un topico con 4 particiones
```
confluent kafka topic create --partitions 4 NOMBRE_DEL_TOPICO
```
Ejemplo que use para producir mensajes en ambos topicos

```
conlfuent kafka topic produce poemas_1 --parse-key

1:"Libre cual brisa de la mar un día"
2:"Las calles recorría"
3:"En suelta vaguedad;"
4:"Y en la mágica red de tu mirada,"
5:"Cual siempre despiadada,"
6:"Perdí mi libertad."
7:"Luego, una chispa de sonrisa ardiente"
8:"Vino a encender mi mente"
9:"En llamas de ilusión;"
10:"Y soñando inocente como un niño,"
11:"Al ganar tu cariño"
12: "Perdí mi corazón."
```

En la pagina del confluent ya se muestran la distribucion de mensajes en cada particion

## Brokers
Un broker es una computadora, una instancia, o contenedor que ejecuta el proceso de Kafka.

Manejan particiones.

Manejan las solicitudes entrantes.

Manejan replicacion de particiones.

Lectura y escritura de mensajes.

## Replicacion
Existen copias de datos para tener tolerancia a fallos.

Existe una **Particion Principal o Lider** y N-1 Followers

La lectura y escritura de datos se producen en la **Particion Principal (Lider)**

Para un desarrollador, es indiferente esta replicacion, pero no para el administrador del Kluster de Kafka.

Se puede ajustar a la hora de producir diferentes niveles de garantia de durabilidad

## Producers y Consumers

Los microservicios que creemos, van a ser en general o producers o consumers o ambos.

Producir y consumir eventos, es la forma de interactuar con el Cluster de Kafka

### Producers
* Aplicacion cliente
* Manda mensajes a los Topics
* Pool de conexiones
* Almacenamiento en buffer de red
* Particionamiento
* Es donde se realiza el calculo de a que particion ira un mensaje de acuerdo a su clave.

### Consumers
* Aplicacion cliente
* Lee mensajes del topico
* Pool de conexiones
* Protocolo de Red
* Escalabilidad Horizontal y Elastically (?
* Mantiene ordenamiento con particiones


# Autor:
[Lucas Vega](https://github.com/LucasVega777/)