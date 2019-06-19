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

# Migrando bancos de dados do {{site.data.keyword.mongodb}} para o {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #migration_mongodb}

Se você estiver usando bancos de dados do {{site.data.keyword.mongodb}} e desejar migrar para o {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}, leia este tópico para obter detalhes. Não importa em que local os seus bancos de dados do {{site.data.keyword.mongodb}} estão hospedados, desde que a plataforma forneça serviços de banco de dados do {{site.data.keyword.mongodb}}.
{: shortdesc}

## Antes de Começar
{: #migration_mongodb_before_begin}

Para usar os comandos do {{site.data.keyword.mongodb}} para concluir a migração, será necessário fazer download e instalar o {{site.data.keyword.mongodb}} em sua máquina primeiro. Para obter mais informações, consulte o [website {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Etapa 1: Crie um arquivo de dump para restaurar os bancos de dados originais
{: #step1_create_dump_file}

Use o comando `mongodump` a seguir para criar um arquivo de dump que contenha os bancos de dados que você deseja restaurar.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

A tabela a seguir ilustra as variáveis usadas no comando.

|Variáveis|Descrição|Exemplo|
|---------|-----------|-------|
|*replica_set_name*|O nome do conjunto de réplicas do {{site.data.keyword.mongodb}}. Se o cluster for implementado em uma instância do {{site.data.keyword.ihsdbaas_full}} Beta, esse será o nome do cluster configurado quando você criar a instância. É possível localizar o URI do cluster na página de visão geral da IU do cluster. O nome é o valor após `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|O nome do servidor host que hospeda o conjunto de réplicas. Três réplicas estão disponíveis. Se o cluster for implementado em uma instância do {{site.data.keyword.ihsdbaas_full}} Beta, será possível localizar os nomes do host no URI do cluster na página de visão geral.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|As portas para conectar o host correspondente. Se o cluster for implementado em uma instância do {{site.data.keyword.ihsdbaas_full}} Beta, será possível localizar os números de porta no URI do cluster na página de visão geral.|28205, 28016, 28175|
|*user_name*|O nome do usuário para autenticar os bancos de dados originais. O usuário precisa ter privilégio READ no banco de dados que você deseja migrar.|my_user|
|*authentication_database_name*|O banco de dados de autenticação para o usuário especificado. Se você não especificar um banco de dados de autenticação, `mongodump` presumirá que seja o banco de dados que você configurou na opção `--db`. Se a opção `--db` também não for especificada, `mongodump` presumirá que seja o banco de dados do administrador.|administrador|
|*database_name*|O banco de dados que você deseja migrar. Se você não especificar o banco de dados, `mongodump` fará dump de todos os bancos de dados que pertencerem ao usuário.|my_database|
|*cert_file*|O arquivo `.pem` para Autoridade de certificação. Se o cluster for implementado em uma instância do {{site.data.keyword.ihsdbaas_full}} Beta, será possível fazer download do arquivo de certificado por meio da página de visão geral. É possível especificar o arquivo usando caminhos relativos ou absolutos.|./cert.pem|
|*dump_file*|O arquivo `.dump` para armazenar os dados originais. É possível usar caminhos relativos ou absolutos para especificar o arquivo.|./mongo.dump|
{: caption="Tabela 1. As variáveis que são necessárias para criar um arquivo de dump"}

Se você precisar de informações mais detalhadas sobre o utilitário `mongodump`, será possível consultar a [documentação do {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

É possível obter a maioria das informações do URI do cluster. O formato do URI é `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. É possível corresponder o valor ao campo correspondente no comando `mongodump` anterior.
{: tip}

## Etapa 2: Criar uma nova instância de serviço no {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step2_creat_new_service_instance}

Antes de restaurar os dados, é necessário criar uma nova instância de serviço no {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} como o cluster de banco de dados de destino. É possível usar uma das maneiras a seguir para criar uma nova instância:
- [A interface com o usuário da web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [As APIs do DBaaS Manager](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [O plug-in da CLI com a CLI do {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

Quando você criar a nova instância de serviço, será necessário configurar o nome do cluster, o nome do administrador e a senha. Eles não são necessariamente os mesmos que aqueles na instância original. Isso não afeta a migração. Depois que a migração for concluída, os bancos de dados serão designados ao novo banco de dados de administrador/autenticação.

## Etapa 3: Restaurar os dados do arquivo de dump para a nova instância de serviço
{: #step3_restore_data}

Use o comando `mongorestore` para restaurar os dados por meio do arquivo de dump que é criado na [Etapa 1](#step1_create_dump_file).

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

A tabela a seguir ilustra as variáveis usadas no comando.

|Variáveis|Descrição|Exemplo|
|---------|-----------|-------|
|*replica_set_name*|O nome do conjunto de réplicas do {{site.data.keyword.mongodb}} de destino. Este é o nome do cluster configurado quando você cria a instância. É possível localizar o URI do cluster na página de visão geral. O nome é o valor após `replicaSet=`.|MongoDB-002|
|*host_name1, host_name2, host_name3*|O servidor do gerenciador do DBaaS que hospeda o conjunto de réplicas de destino. Três réplicas estão disponíveis. É possível localizar os nomes do host no URI do cluster na página de visão geral.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|As portas para conectar o host correspondente. É possível localizar as portas no URI do cluster na página de visão geral.|28128, 28248, 28043|
|*user_name*|O nome do usuário para autenticar para o banco de dados de destino. O usuário não tem que ser o mesmo que o usuário que cria o arquivo de dump.|new_user|
|*authentication_database_name*|O banco de dados de autenticação para o usuário especificado. Se você não especificar um banco de dados de autenticação, `mongodump` presumirá que seja o banco de dados que você configurou na opção `--db`. Se a opção `--db` também não for especificada, `mongodump` presumirá que seja o banco de dados do administrador.|administrador|
|*database_name*|O banco de dados de destino para o qual deseja migrar os dados. Se o banco de dados não existir, `mongorestore` o criará automaticamente. Se você não especificar o banco de dados, `mongorestore` criará um banco de dados baseado no banco de dados original.|my_database|
|*cert_file*|O arquivo `.pem` para Autoridade de certificação. É possível fazer download do arquivo por meio da página de visão geral da instância e especificá-lo usando caminhos relativos ou absolutos.|./cert.pem|
|*dump_file*|O arquivo `.dump` que você cria na [Etapa 1](#step1_create_dump_file). É possível usar caminhos relativos ou absolutos para especificar o arquivo.|./mongo.dump|
{: caption="Tabela 2. As variáveis que são necessárias para restaurar os dados de um arquivo de dump"}

É possível criar novos usuários por meio da [interface com o usuário da web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), das [APIs do DBaaS Manager](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}, do [plug-in da CLI com a CLI do {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) e do [comando {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

Para o {{site.data.keyword.mongodb}}, a migração mescla os dados originais no cluster de destino em vez de sobrescrever os dados existentes, se houver. É possível consultar a [explicação detalhada](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} desse recurso no website {{site.data.keyword.mongodb}}. Também é possível consultar a [documentação do {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} para obter mais informações sobre o utilitário `mongorestore`.

## O que vem a seguir
{: #whats_next_migration_mongodb}

Após a migração, será possível conectar-se ao novo cluster de banco de dados para verificar se os dados foram migrados com êxito.
