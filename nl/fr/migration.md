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

# Migration de bases de données {{site.data.keyword.mongodb}} vers {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

Si vous utilisez des bases de données {{site.data.keyword.mongodb}} et souhaitez les faire migrer vers {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}, lisez cette rubrique. Peu importe où vos bases de données {{site.data.keyword.mongodb}} sont hébergées à partir du moment où la plateforme fournit des services de base de données {{site.data.keyword.mongodb}}.
{: shortdesc}

## Avant de commencer
{: #migration_mongodb_before_begin}

Pour utiliser les commandes {{site.data.keyword.mongodb}} afin de finaliser la migration, vous devez d'abord télécharger et installer {{site.data.keyword.mongodb}} sur votre machine. Pour plus d'informations, voir le [site Web {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Etape 1 : Création d'un fichier de vidage pour restaurer les bases de données d'origine
{: #step1_create_dump_file}

Utilisez la commande `mongodump` suivante pour créer un fichier de vidage contenant les bases de données que vous souhaitez restaurer.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

Le tableau suivant présente les variables utilisées dans la commande :

|Variables|Description|Exemple|
|---------|-----------|-------|
|*replica_set_name*|Nom du jeu de répliques {{site.data.keyword.mongodb}}. Si le cluster est déployé dans une instance {{site.data.keyword.ihsdbaas_full}} bêta, il s'agit du nom de cluster que vous définissez lorsque vous créez l'instance. L'URI de cluster figure sur la page de présentation de l'interface utilisateur d'instance de cluster. Le nom est la valeur située après `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|Nom du serveur hôte qui héberge le jeu de répliques. Trois répliques sont disponibles. Si le cluster est déployé dans une instance {{site.data.keyword.ihsdbaas_full}} bêta, les noms d'hôte figurent dans l'URI de cluster sur la page de présentation.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Ports permettant d'établir de connecter l'hôte correspondant. Si le cluster est déployé dans une instance {{site.data.keyword.ihsdbaas_full}} bêta, les numéros de ports figurent dans l'URI de cluster sur la page de présentation.|28205, 28016, 28175|
|*user_name*|Nom d'utilisateur permettant de s'authentifier sur les bases de données d'origine. L'utilisateur doit disposer du privilège READ sur la base de données que vous souhaitez faire migrer.|my_user|
|*authentication_database_name*|Base de données d'authentification pour l'utilisateur spécifié. Si vous n'indiquez pas une base de données d'authentification, `mongodump` considère qu'il s'agit de la base de données que vous définissez dans l’option `--db`. Si vous n'indiquez pas non plus l'option `--db`, `mongodump` considère qu'il s'agit de la base de données d'administration.|admin|
|*database_name*|Base de données que vous souhaitez faire migrer. Si vous n'indiquez pas la base de données, `mongodump` vide toutes les bases de données qui appartiennent à l'utilisateur.|my_database|
|*cert_file*|Fichier `.pem` pour l'autorité de certification. Si le cluster est déployé dans une instance {{site.data.keyword.ihsdbaas_full}} bêta, vous pouvez télécharger le fichier de certificat à partir de la page de présentation. Vous pouvez spécifier le fichier à l'aide de chemins relatifs ou absolus.|./cert.pem|
|*dump_file*|Fichier `.dump` dans lequel stocker les données d'origine. Vous pouvez utiliser des chemins relatifs ou absolus pour spécifier le fichier.|./mongo.dump|
{: caption="Tableau 1. Variables nécessaires pour créer un fichier de vidage"}

Pour en savoir plus sur l'utilitaire `mongodump`, vous pouvez vous reporter à la [documentation {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Vous pouvez obtenir l'essentiel des informations à partir de l'URI de cluster. Le format de l'URI est le suivant : `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Vous pouvez faire correspondre la valeur à la zone correspondante dans la commande `mongodump` précédente.
{: tip}

## Etape 2 : Création d'une instance de service dans {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Avant de restaurer les données, vous devez créer une instance de service dans {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} comme cluster de base de données cible. Vous pouvez utiliser l'une des méthodes suivantes pour créer une nouvelle instance :
- L'[interface utilisateur Web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- Les [API DBaaS Manager](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- Le [plug-in CLI avec l'interface de ligne de commande {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

Lorsque vous créez l'instance de service, vous devez définir le nom du cluster, ainsi que le nom et le mot de passe de l'administrateur. Ils ne doivent pas nécessairement être identiques à ceux de l'instance d'origine. Cela n'affecte pas la migration. Une fois la migration terminée, les bases de données sont affectées à la nouvelle base de données d'administration/d'authentification.

## Etape 3 : Restauration des données depuis le fichier de vidage dans la nouvelle instance de service
{: #step3_restore_data}

Utilisez la commande `mongorestore` pour restaurer les données du fichier de vidage qui est créé à l'[étape 1](#step1_create_dump_file).

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

Le tableau suivant présente les variables utilisées dans la commande :

|Variables|Description|Exemple|
|---------|-----------|-------|
|*replica_set_name*|Nom du jeu de répliques {{site.data.keyword.mongodb}} cible. Il s'agit du nom de cluster que vous définissez lorsque vous créez l'instance. L'URI de cluster figure sur la page de présentation. Le nom est la valeur située après `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|Serveur de gestionnaire DBaaS qui héberge le jeu de répliques cible. Trois répliques sont disponibles. Les noms d'hôte figurent dans l'URI de cluster sur la page de présentation.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Ports permettant d'établir de connecter l'hôte correspondant. Les ports figurent dans l'URI de cluster sur la page de présentation.|28128, 28248, 28043|
|*user_name*|Nom d'utilisateur permettant de s'authentifier sur la base de données cible. L'utilisateur ne doit pas nécessairement être le même que celui qui a créé le fichier de vidage.|new_user|
|*authentication_database_name*|Base de données d'authentification pour l'utilisateur spécifié. Si vous n'indiquez pas une base de données d'authentification, `mongodump` considère qu'il s'agit de la base de données que vous définissez dans l’option `--db`. Si vous n'indiquez pas non plus l'option `--db`, `mongodump` considère qu'il s'agit de la base de données d'administration.|admin|
|*database_name*|Base de données cible dans laquelle vous souhaitez faire migrer les données. Si la base de données n'existe pas, `mongorestore` la crée automatiquement. Si vous ne spécifiez pas la base de données, `mongorestore` crée une base de données basée sur la base de données d'origine.|my_database|
|*cert_file*|Fichier `.pem` pour l'autorité de certification. Vous pouvez télécharger le fichier depuis la page de présentation de l'instance et le spécifier à l'aide de chemins relatifs ou absolus.|./cert.pem|
|*dump_file*|Fichier `.dump` que vous avez créé à l'[étape 1](#step1_create_dump_file). Vous pouvez utiliser des chemins relatifs ou absolus pour spécifier le fichier.|./mongo.dump|
{: caption="Tableau 2. Variables nécessaires pour restaurer les données à partir d'un fichier de vidage"}

Vous pouvez utiliser les méthodes suivantes pour créer des utilisateurs : [l'interface utilisateur Web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), [les API DBaaS Manager](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}, [le plug-in d'interface de ligne de commande avec l'interface de ligne de commande {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) et [la commande {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

Pour {{site.data.keyword.mongodb}}, la migration fusionne les données d'origine dans le cluster cible au lieu d'écraser les données existantes. Vous pouvez consulter les [explications détaillées](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} relatives à cette fonction sur le site Web {{site.data.keyword.mongodb}}. Vous pouvez aussi vous reporter à la [documentation {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} pour obtenir plus d'informations sur l'utilitaire `mongorestore`.

## Etapes suivantes
{: #whats_next_migration_mongodb}

Après la migration, vous pouvez vous connecter au nouveau cluster de base de données pour vérifier si la migration des données a abouti.
