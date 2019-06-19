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


# Sauvegarde et restauration de vos bases de données à l'aide de {{site.data.keyword.cos_full_notm}}
{: #backup_mongodb_databases}

Le service {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} déclenche automatiquement une sauvegarde de la totalité de votre base de données toutes les 24 heures. Ces sauvegardes chiffrées sont disponibles pour les 7 derniers jours et elles sont mises à disposition de manière redondante sur le stockage local dans toutes les zones de disponibilité de la région prise en charge.
{: shortdesc}

Si vous souhaitez augmenter vos capacités de reprise après incident et sauvegarder les données en dehors de la région prise en charge, vous pouvez utiliser le [service {{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} pour stocker vos données dans une autre région en vous référant aux étapes suivantes.

## Avant de commencer
{: #backup_mongodb_before_begin}

Pour utiliser les commandes {{site.data.keyword.mongodb}} afin de finaliser la sauvegarde, vous devez d'abord télécharger et installer {{site.data.keyword.mongodb}} sur votre machine. Pour plus d'informations, voir le [site Web {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Etape 1 : Création d'un fichier de vidage pour sauvegarder les bases de données d'origine
{: #step1_create_dump_file_backup_mongodb}

Utilisez la commande `mongodump` suivante pour créer un fichier de vidage contenant les bases de données que vous souhaitez sauvegarder.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

Le tableau suivant présente les variables utilisées dans la commande :

|Variables|Description|Exemple|
|---------|-----------|-------|
|*replica_set_name*|Nom du jeu de répliques {{site.data.keyword.mongodb}}. Il s'agit du nom de cluster que vous définissez lorsque vous créez l'instance. L'URI de cluster figure sur la page de présentation de l'interface utilisateur d'instance de cluster. Le nom est la valeur située après `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|Serveur de gestionnaire DBaaS qui héberge le jeu de répliques. Trois répliques sont disponibles. Les noms d'hôte figurent dans l'URI de cluster sur la page de présentation.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|Ports permettant d'établir de connecter l'hôte correspondant. Les numéros de port figurent dans l'URI de cluster sur la page de présentation.|28205, 28016, 28175|
|*user_name*|Nom d'utilisateur permettant de s'authentifier sur les bases de données d'origine. L'utilisateur doit disposer du privilège READ sur la base de données que vous souhaitez faire migrer.|my_user|
|*authentication_database_name*|Base de données d'authentification pour l'utilisateur spécifié. Si vous n'indiquez pas une base de données d'authentification, `mongodump` considère qu'il s'agit de la base de données que vous définissez dans l’option `--db`. Si vous n'indiquez pas non plus l'option `--db`, `mongodump` considère qu'il s'agit de la base de données d'administration.|admin|
|*database_name*|Base de données que vous souhaitez sauvegarder. Si vous n'indiquez pas la base de données, `mongodump` vide toutes les bases de données qui appartiennent à l'utilisateur.|my_database|
|*cert_file*|Fichier `.pem` pour l'autorité de certification. Vous pouvez télécharger le fichier cert depuis la page de présentation et le spécifier à l'aide de chemins relatifs ou absolus.|./cert.pem|
|*dump_file*|Fichier `.dump` dans lequel stocker les données d'origine. Vous pouvez utiliser des chemins relatifs ou absolus pour spécifier le fichier.|./mongo.dump|
{: caption="Tableau 1. Variables nécessaires pour créer un fichier de vidage"}

Pour en savoir plus sur l'utilitaire `mongodump`, vous pouvez vous reporter à la [documentation {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

Vous pouvez obtenir l'essentiel des informations à partir de l'URI de cluster. Le format de l'URI est le suivant : `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. Vous pouvez faire correspondre la valeur à la zone correspondante dans la commande `mongodump` précédente.
{: tip}

## Etape 2 : Création d'une instance {{site.data.keyword.cos_full_notm}}
{: #step2_create_object_storage_backup_mongodb}

Vous devez créer l'instance dans une région différente de celle où l'instance de service {{site.data.keyword.ihsdbaas_mongodb_full}} est hébergée. Vous pouvez vous reporter aux étapes suivantes :

1. [Créez une instance Cloud {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Créez un compartiment](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) dans une autre région.
3. [Créez des données d'identification de service](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) et enregistrez les identifiants `access_key_id` et `secret_access_key` pour une utilisation ultérieure.
4. Installez un client S3.
5. Utilisez le client S3 et les clés d'accès pour vous connecter au noeud final de Cloud {{site.data.keyword.cos_short}} du compartiment que vous avez créé précédemment.
6. Utilisez le client S3 pour [télécharger le fichier de sauvegarde {{site.data.keyword.mongodb}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) que vous avez créé à l'[étape 1](#step1_create_dump_file_backup_mongodb) dans le compartiment.

Pour plus d'informations, vous pouvez vous reporter à la [documentation {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started).

Une fois cette étape terminée, vous avez sauvegardé vos données dans une instance Cloud {{site.data.keyword.cos_short}} dans une autre région. Si vous souhaitez restaurer les données de l'instance Cloud {{site.data.keyword.cos_short}} dans une instance {{site.data.keyword.ihsdbaas_mongodb_full}}, consultez l'étape 3 suivante pour plus de détails.

## Etape 3 : Restauration des données à partir de l'instance Cloud {{site.data.keyword.cos_short}} dans une instance {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step3_restore_data_from_cos_mongodb}

Vous devez d'abord télécharger le fichier de sauvegarde du compartiment Cloud {{site.data.keyword.cos_short}} vers votre machine locale, puis utiliser la commande `mongorestore` pour restaurer les données.

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
|*database_name*|Base de données cible dans laquelle vous souhaitez restaurer les données. Si la base de données n'existe pas, `mongorestore` la crée automatiquement. Si vous ne spécifiez pas la base de données, `mongorestore` crée une base de données basée sur la base de données d'origine.|my_database|
|*cert_file*|Fichier `.pem` pour l'autorité de certification. Vous pouvez télécharger le fichier depuis la page de présentation de l'instance et le spécifier à l'aide de chemins relatifs ou absolus.|./cert.pem|
|*dump_file*|Fichier `.dump` que vous téléchargez à partir de votre compartiment Cloud {{site.data.keyword.cos_short}}. Vous pouvez utiliser des chemins relatifs ou absolus pour spécifier le fichier.|./mongo.dump|
{: caption="Tableau 2. Variables nécessaires pour restaurer les données à partir d'un fichier de vidage"}

Pour {{site.data.keyword.mongodb}}, la migration fusionne les données d'origine dans le cluster cible au lieu d'écraser les données existantes. Vous pouvez consulter les [explications détaillées](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} relatives à cette fonction sur le site Web {{site.data.keyword.mongodb}}. Vous pouvez aussi vous reporter à la [documentation {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} pour obtenir plus d'informations sur l'utilitaire `mongorestore`.

## Etapes suivantes
{: #whats_next_backup_mongodb}

Après l'[étape 3](#step3_restore_data_from_cos_mongodb), vous pouvez vous connecter au cluster de base de données pour vérifier si la restauration des données a abouti.
