---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: backup, disaster recovery, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}


# Copia de seguridad y restauración de sus bases de datos utilizando {{site.data.keyword.cos_full_notm}}
{: #backup_mongodb_databases}

El servicio {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} desencadena automáticamente una copia de seguridad de su base de datos completa una vez cada 24 horas. Estas copias de seguridad cifradas están disponibles durante los últimos 7 días y disponibles de forma redundante en el almacenamiento local en todas las Zonas de disponibilidad de la región soportada.
{: shortdesc}

Si desea ampliar sus funciones de recuperación tras desastre y realizar una copia de seguridad de los datos fuera de la región soportada, puede utilizar el [servicio {{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} para almacenar los datos en otra región siguiendo los pasos siguientes.

## Antes de empezar
{: #backup_mongodb_before_begin}

Para utilizar los mandatos de {{site.data.keyword.mongodb}} para completar la copia de seguridad, primero debe descargar e instalar {{site.data.keyword.mongodb}} en la máquina. Para obtener más información, consulte el [sitio web de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Paso 1: Crear un archivo de volcado para hacer copia de seguridad de las bases de datos originales
{: #step1_create_dump_file_backup_mongodb}

Utilice el siguiente mandato `mongodump` para crear un archivo de volcado que contenga las bases de datos de las que desea hacer copia de seguridad.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

En la tabla siguiente se muestran las variables utilizadas en el mandato.

|Variables|Descripción|Ejemplo|
|---------|-----------|-------|
|*replica_set_name*|El nombre del conjunto de réplicas de {{site.data.keyword.mongodb}}. Este es el nombre de clúster que ha establecido al crear la instancia. Puede encontrar el URI del clúster en la página de visión general de la interfaz de usuario del clúster. El nombre es el valor que hay después de `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|El servidor del gestor de DBaaS que aloja el conjunto de réplicas. Hay tres réplicas disponibles. Puede encontrar los nombres de host en el URI de clúster en la página de visión general.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Los puertos para conectar el host correspondiente. Puede encontrar los números de puerto en el URI de clúster en la página de visión general.|28205, 28016, 28175|
|*user_name*|El nombre de usuario para autenticar en las bases de datos originales. El usuario necesita tener privilegio READ en la base de datos que desea migrar.|my_user|
|*authentication_database_name*|La base de datos de autenticación para el usuario especificado. Si no especifica una base de datos de autenticación, `mongodump` presupone que se trata de la base de datos que ha definido en la opción `--db`. Si tampoco se especifica la opción `--db`, `mongodump` presupone que es la base de datos de administración.|admin|
|*database_name*|La base de datos de la que desea hacer copia de seguridad. Si no especifica la base de datos, `mongodump` vuelca todas las bases de datos que pertenecen al usuario.|my_database|
|*cert_file*|El archivo `.pem` para la entidad emisora de certificados. Puede descargar el archivo de certificados desde la página de visión general de la instancia y especificarlo utilizando vías de acceso relativas o absolutas.|./cert.pem|
|*dump_file*|El archivo `.dump` para almacenar los datos originales. Puede utilizar vías de acceso absolutas o relativas para especificar el archivo.|./mongo.dump|
{: caption="Tabla 1. Las variables necesarias para crear un archivo de volcado"}

Si necesita información más detallada sobre el programa de utilidad `mongodump`, consulte la [documentación de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Puede obtener la mayor parte de la información del URI del clúster. El formato del URI es `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Puede hacer coincidir el valor con el campo correspondiente en el mandato `mongodump` anterior.
{: tip}

## Paso 2: Crear una nueva instancia de {{site.data.keyword.cos_full_notm}}
{: #step2_create_object_storage_backup_mongodb}

Tiene que crear la instancia en una región distinta de aquella en la que está alojada actualmente la instancia de servicio de {{site.data.keyword.ihsdbaas_mongodb_full}}. Puede seguir los siguientes pasos:

1. [Cree una nueva instancia de {{site.data.keyword.cos_short}} en la nube](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Cree un grupo](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) en otra región.
3. [Cree una credencial de servicio](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) y guarde `access_key_id` y `secret_access_key` para utilizarlos más adelante.
4. Instale un cliente S3.
5. Utilice el cliente S3 y las claves de acceso para conectar con el punto final de {{site.data.keyword.cos_short}} de la nube del grupo que ha creado antes.
6. Utilice el cliente S3 para [cargar el archivo de copia de seguridad de {{site.data.keyword.mongodb}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) que ha creado en el [Paso 1](#step1_create_dump_file_backup_mongodb) en el grupo.

Consulte la [documentación de {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started) para obtener más información.

Después de completar este paso, habrá hecho una copia de seguridad correctamente de sus datos en una instancia de {{site.data.keyword.cos_short}} en la nube en otra región. Si desea restaurar los datos de la instancia de {{site.data.keyword.cos_short}} en la nube en una instancia de {{site.data.keyword.ihsdbaas_mongodb_full}}, lea el siguiente Paso 3 para ver detalles.

## Paso 3: Restaurar los datos de la instancia de {{site.data.keyword.cos_short}} en la nube en una instancia de {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step3_restore_data_from_cos_mongodb}

Primero tiene que descargar el archivo de copia de seguridad del grupo de {{site.data.keyword.cos_short}} en la nube en su máquina local y luego utilizar el mandato `mongorestore` para restaurar los datos.

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

En la tabla siguiente se muestran las variables utilizadas en el mandato.

|Variables|Descripción|Ejemplo|
|---------|-----------|-------|
|*replica_set_name*|El nombre del conjunto de réplicas de {{site.data.keyword.mongodb}} de destino. Este es el nombre de clúster que se establece al crear la instancia. Puede encontrar el URI del clúster en la página de visión general. El nombre es el valor que hay después de `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|El servidor del gestor de DBaaS que aloja el conjunto de réplicas de destino. Hay tres réplicas disponibles. Puede encontrar los nombres de host en el URI de clúster en la página de visión general.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Los puertos para conectar el host correspondiente. Puede encontrar los puertos en el URI de clúster en la página de visión general.|28128, 28248, 28043|
|*user_name*|El nombre de usuario para autenticar en la base de datos de destino. El usuario no tiene que ser el mismo que el usuario que crea el archivo de volcado.|new_user|
|*authentication_database_name*|La base de datos de autenticación para el usuario especificado. Si no especifica una base de datos de autenticación, `mongodump` presupone que se trata de la base de datos que ha definido en la opción `--db`. Si tampoco se especifica la opción `--db`, `mongodump` presupone que es la base de datos de administración.|admin|
|*database_name*|La base de datos de destino en la que desea restaurar los datos. Si la base de datos no existe, `mongorestore` la crea automáticamente. Si no especifica la base de datos, `mongorestore` crea una base de datos basada en la base de datos original.|my_database|
|*cert_file*|El archivo `.pem` para la entidad emisora de certificados. Puede descargar el archivo desde la página de visión general de la instancia y especificarlo utilizando vías de acceso relativas o absolutas.|./cert.pem|
|*dump_file*|El archivo `.dump` que ha descargado del grupo de {{site.data.keyword.cos_short}} en la nube. Puede utilizar vías de acceso absolutas o relativas para especificar el archivo.|./mongo.dump|
{: caption="Tabla 2. Las variables necesarias para restaurar los datos de un archivo de volcado"}

En el caso de {{site.data.keyword.mongodb}}, la migración fusiona los datos originales en el clúster de destino en lugar de sobrescribir los datos existentes, si los hay. Encontrará la [explicación detallada](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} de esta característica en el sitio web de {{site.data.keyword.mongodb}}. También puede consultar la [documentación de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} para obtener más información sobre el programa de utilidad `mongorestore`.

## Qué hacer a continuación
{: #whats_next_backup_mongodb}

Después del [Paso 3](#step3_restore_data_from_cos_mongodb), puede conectar con el clúster de la base de datos para comprobar si los datos se han restaurado correctamente.
