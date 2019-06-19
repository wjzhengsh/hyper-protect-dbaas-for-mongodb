---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: migrate, beta, restore

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}

# Migración de bases de datos {{site.data.keyword.mongodb}} a {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

Si está utilizando bases de datos {{site.data.keyword.mongodb}} y desea migrar a {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}, lea este tema para obtener información detallada. No importa dónde estén alojadas las bases de datos {{site.data.keyword.mongodb}}, siempre que la plataforma proporcione servicios de base de datos {{site.data.keyword.mongodb}}.
{: shortdesc}

## Antes de empezar
{: #migration_mongodb_before_begin}

Para utilizar los mandatos de {{site.data.keyword.mongodb}} para completar la migración, primero debe descargar e instalar {{site.data.keyword.mongodb}} en la máquina. Para obtener más información, consulte el [sitio web de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Paso 1: Crear un archivo de volcado para restaurar las bases de datos originales
{: #step1_create_dump_file}

Utilice el siguiente mandato `mongodump` para crear un archivo de volcado que contenga las bases de datos que desea restaurar.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

En la tabla siguiente se muestran las variables utilizadas en el mandato.

|Variables|Descripción|Ejemplo|
|---------|-----------|-------|
|*replica_set_name*|El nombre del conjunto de réplicas de {{site.data.keyword.mongodb}}. Si el clúster se despliega en una instancia de {{site.data.keyword.ihsdbaas_full}} Beta, este será el nombre de clúster que establecerá cuando cree la instancia. Puede encontrar el URI del clúster en la página de visión general de la interfaz de usuario del clúster. El nombre es el valor que hay después de `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|El nombre del servidor de host que aloja el conjunto de réplicas. Hay tres réplicas disponibles. Si el clúster se despliega en una instancia de {{site.data.keyword.ihsdbaas_full}} Beta, puede encontrar los nombres de host en el URI del clúster en la página de visión general.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Los puertos para conectar el host correspondiente. Si el clúster se despliega en una instancia de {{site.data.keyword.ihsdbaas_full}} Beta, puede encontrar los números de puerto en el URI del clúster en la página de visión general.|28205, 28016, 28175|
|*user_name*|El nombre de usuario para autenticar en las bases de datos originales. El usuario necesita tener privilegio READ en la base de datos que desea migrar.|my_user|
|*authentication_database_name*|La base de datos de autenticación para el usuario especificado. Si no especifica una base de datos de autenticación, `mongodump` presupone que se trata de la base de datos que ha definido en la opción `--db`. Si tampoco se especifica la opción `--db`, `mongodump` presupone que es la base de datos de administración.|admin|
|*database_name*|La base de datos que desea migrar. Si no especifica la base de datos, `mongodump` vuelca todas las bases de datos que pertenecen al usuario.|my_database|
|*cert_file*|El archivo `.pem` para la entidad emisora de certificados. Si el clúster se despliega en una instancia de {{site.data.keyword.ihsdbaas_full}} Beta, puede descargar el archivo cert de la página de visión general. Puede especificar el archivo utilizando vías de acceso relativas o absolutas.|./cert.pem|
|*dump_file*|El archivo `.dump` para almacenar los datos originales. Puede utilizar vías de acceso absolutas o relativas para especificar el archivo.|./mongo.dump|
{: caption="Tabla 1. Las variables necesarias para crear un archivo de volcado"}

Si necesita información más detallada sobre el programa de utilidad `mongodump`, consulte la [documentación de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Puede obtener la mayor parte de la información del URI del clúster. El formato del URI es `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Puede hacer coincidir el valor con el campo correspondiente en el mandato `mongodump` anterior.
{: tip}

## Paso 2: Crear una instancia de servicio nueva en {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Antes de restaurar los datos, tiene que crear una nueva instancia de servicio en {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} como clúster de base de datos de destino. Puede utilizar una de las formas siguientes para crear una instancia nueva:
- [La interfaz de usuario de web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [Las API del gestor de DBaaS](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [El plugin de CLI con la CLI de {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

Cuando crea la nueva instancia de servicio, debe establecer el nombre de clúster, el nombre de administrador y la contraseña. No tienen que ser necesariamente las mismas que en la instancia original. No afecta a la migración. Una vez completada la migración, las bases de datos se asignan a la nueva base de datos de administrador/autenticación.

## Paso 3: Restaurar los datos del archivo de volcado en la nueva instancia del servicio
{: #step3_restore_data}

Utilice el mandato `mongorestore` para restaurar los datos del archivo de volcado que se ha creado en el [Paso 1](#step1_create_dump_file).

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
|*database_name*|La base de datos de destino a la que desea migrar los datos. Si la base de datos no existe, `mongorestore` la crea automáticamente. Si no especifica la base de datos, `mongorestore` crea una base de datos basada en la base de datos original.|my_database|
|*cert_file*|El archivo `.pem` para la entidad emisora de certificados. Puede descargar el archivo desde la página de visión general de la instancia y especificarlo utilizando vías de acceso relativas o absolutas.|./cert.pem|
|*dump_file*|El archivo `.dump` que se ha creado en el [Paso 1](#step1_create_dump_file). Puede utilizar vías de acceso absolutas o relativas para especificar el archivo.|./mongo.dump|
{: caption="Tabla 2. Las variables necesarias para restaurar los datos de un archivo de volcado"}

Puede seguir los métodos siguientes para crear nuevos usuarios: [la interfaz de usuario web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), [las API de DBaaS Manager](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external},[el plugin de CLI con la CLI de {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) y el [mandato {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

En el caso de {{site.data.keyword.mongodb}}, la migración fusiona los datos originales en el clúster de destino en lugar de sobrescribir los datos existentes, si los hay. Encontrará la [explicación detallada](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} de esta característica en el sitio web de {{site.data.keyword.mongodb}}. También puede consultar la [documentación de {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} para obtener más información sobre el programa de utilidad `mongorestore`.

## Qué hacer a continuación
{: #whats_next_migration_mongodb}

Después de la migración, puede conectarse al nuevo clúster de base de datos para comprobar si los datos se han migrado satisfactoriamente.
