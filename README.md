# Camino al descubrimiento de las herramientas big data #

Desde la gerencia de Infraestructura no están muy convencidos de utilizar esta tecnología por lo que no se asigno presupuesto alguna para esta iniciativa, de forma tal que por el momento no es posible utilizar un Vendor(Azure, AWS, Google) para implementar dicho entorno, es por esto que todo el MVP se deberá implementar utilizando Docker de forma tal que se pueda hacer una demo al sector de infraestructura mostrando las ventajas de utilizar tecnologías de Big Data.

# Entorno Docker con Hadoop, Spark y Hive 

Se pesenta un entorno Docker con Hadoop (HDFS) y la implementación de:
* Spark
* Hive
* HBase
* MongoDB
* Neo4J
* Zeppelin
* Kafka


## Desarrollo

Primero se debe verificar la interfaz de Docker, para ello se ejecutara el siguiente comando:
```
sudo docker network ls
```
Debe dar como salida una lista de las redes incluyendo el proyecto 
![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/126b328e-1ed4-41a1-84f2-dee587d2597e)


Ejecute `sudo docker network inspect` en la red (por ejemplo, `docker-hadoop-spark-hive_default`) para encontrar la IP en la que se publican las interfaces de hadoop. En este caso se usaria el siguiente comando
```
sudo docker network inspect bigdata_default
```
![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/f6d12c2f-8778-4de6-8d78-f0bb957b954a)

Acceda a estas interfaces con las siguientes URL:

```
Namenode: http://IP_Anfitrion:9870/dfshealth.html#tab-overview
Datanode: http://IP_Anfitrion:9864/
Spark master: http://IP_Anfitrion:8080/
Spark worker: http://IP_Anfitrion:8081/	
HBase Master-Status: http://IP_Anfitrion:16010
HBase Zookeeper_Dump: http://IP_Anfitrion:16010/zk.jsp
HBase Region_Server: http://IP_Anfitrion:16030
Zeppelin: http://IP_Anfitrion:8888
Neo4j: http://IP_Anfitrion:7474
```

Para implementar el proyecto siga los siguientes pasos

1. Clonar el repositorio por medio de la consola de linux
```
git clone https://github.com/DavidVillamizar04/Big_data.git
```
2. Cambiar de directorio a la carpeta del repositorio que acabamos de clonar
```
cd Big_data
```
3. Ejecutamos el contenedor de Docker, cabe resaltar que la "X" debe ser reemplazada por el numero de contenedor que desee ejecutar
```
sudo docker-compose -f docker-compose-vX.yml up -d
```
NOTA: Si es la primera vez que ejecutas el contenedor es normal que se demore en descargar todos los archivos, esto solo ocurrira la primera vez que se ejecute.

## HDFS

SUGERENCIA: Para este ejercicio se puede utilizar el entorno docker-compose-v1.yml
```
sudo docker-compose -f docker-compose-v1.yml up -d
```
Una vez ejecutado el contenedor vamos a copiar los archivos ubicados en la carpeta Datasets, dentro del contenedor "namenode" siguiendo los siguientes pasos
* Ejecutamos el contenedor "namenode" con el siguiente comando
```
sudo docker exec -it namenode bash
```
* Nos ubicamos en el directorio "home"
```
cd home
```
* Creamos la carpeta llamada "Datasets" en la que se almacenaran los archivos copiados
```
mkdir Datasets
```
Salimos del contenedor 
```
exit
```
* Ejecutamos el archivo paso00.sh el cual creara los subdirectorios y copiara cada archivo CSV
```
sudo sh paso00.sh
```

* Ubicarse en el contenedor "namenode"

```
sudo docker exec -it namenode bash
```

* Creamos un directorio en HDFS llamado "/data".

```
hdfs dfs -mkdir -p /data
```

* Copiamos los archivos csv que anteriormente pasamos a "Namenode" a HDFS:
```
hdfs dfs -put /home/Datasets/* /data
```

Este proceso de creación de la carpeta data y copiado de los arhivos, debe poder ejecutarse desde un shell script.

Nota: Busque dfs.blocksize y dfs.replication en http://<IP_Anfitrion>:9870/conf para encontrar los valores de tamaño de bloque y factor de réplica respectivamente entre otras configuraciones del sistema Hadoop.

## Hive

SUGERENCIA: Se puede utilizar el entorno docker-compose-v2.yml

En este paso crearemos tablas en HIVE, a partir de los CSV integrados a HDFS en el paso anterior.

Despues de abrir el entorno ´docker-compose-v2.yml´ abrimos el servidor de HIVE
```
sudo docker exec -it hive-server bash
```
Y ejecutamos el siguiente archivo que hara la creacion de las tablas
```
hive -f Paso03.hql;
```
Luego colocamos en uso la base de datos integrador2 y ejecutamos un select a la tabla gasto que esta particionada por la clave IdTipoGasto
```
select IdTipoGasto, sum(Monto) from gasto group by IdTipoGasto;
```
## 3) Formatos de Almacenamiento

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/8bb82ca5-1d86-4bd6-a345-14fa862a6d51)

## SQL

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/7acf5225-da05-42b2-b70c-0000a42386d4)


## 5) No-SQL

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/98480103-34b0-40a1-8bf2-0018b16a3b63)

#### HBase:

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/5613b734-39be-43f8-a545-4347539bb108)
		

#### MongoDB

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/c9918d08-5a55-48d2-aed7-8b9d3fc9bd3d)

	
### Neo4J

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/190d8bb4-839d-4e8e-8b3c-e841e0654600)


#### Zeppelin

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/ba9fac63-35ba-41fe-bea3-b3a684239be9)


## Spark

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/d7b54dd0-4108-4724-9eb8-fc7ef6045398)


### 1) Spark y Scala:

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/86ddf79d-7132-4bd5-ae40-f50a8df3de01)

#### Kafka	

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/4a2d8e6c-937a-4d9d-a783-aaa5f9546b5c)


#### Comparativa Dataset y Dataframe en Scala:

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/aa72f66d-5848-4409-a3de-4913ca34f832)


#### ETL con Spark

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/4ae7e30e-c0ed-4611-8097-512acd20f3a5)


## Carga incremental con Spark 

![image](https://github.com/DavidVillamizar04/Big_data/assets/154456308/c368134a-3448-4c44-9248-c2a15a8c485e)

## 8) Herramientas de orquestación de flujos de datos

https://github.com/sercasti/datalaketools
