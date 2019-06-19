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


# Fazendo backup e restaurando seus bancos de dados usando o {{site.data.keyword.cos_full_notm}}
{: #backup_mongodb_databases}

O serviço {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} aciona automaticamente um backup de seu banco de dados completo uma vez a cada 24 horas. Esses backups criptografados estão disponíveis para os últimos 7 dias e estão disponíveis redundantemente no armazenamento local em todas as Zonas de disponibilidade da região suportada.
{: shortdesc}

Se você deseja aumentar seus recursos de recuperação de desastre e fazer backup dos dados fora da região suportada, é possível usar o [serviço {{site.data.keyword.cos_full_notm}}](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external} para armazenar seus dados em uma região diferente consultando as etapas a seguir.

## Antes de começar
{: #backup_mongodb_before_begin}

Para usar os comandos do {{site.data.keyword.mongodb}} para concluir o backup, é necessário fazer download e instalar o {{site.data.keyword.mongodb}} em sua máquina primeiro. Para obter mais informações, consulte o [website {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/installation/){: external}.

## Etapa 1: criar um arquivo dump para fazer backup dos bancos de dados originais
{: #step1_create_dump_file_backup_mongodb}

Use o comando `mongodump` a seguir para criar um arquivo dump que contenha os bancos de dados que deseje fazer backup.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

A tabela a seguir ilustra as variáveis usadas no comando.

|Variáveis|Descrição|Exemplo|
|---------|-----------|-------|
|*replica_set_name*|O nome do conjunto de réplicas do {{site.data.keyword.mongodb}}. Esse é o nome do cluster que você configurou ao criar a instância. É possível localizar o URI do cluster na página de visão geral da IU do cluster. O nome é o valor após `replicaSet=`.|MongoDB-001|
|*host_name1, host_name2, host_name3*|O servidor do gerenciador do DBaaS que hospeda o conjunto de réplicas. Três réplicas estão disponíveis. É possível localizar os nomes do host no URI do cluster na página de visão geral.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|As portas para conectar o host correspondente. É possível localizar os números de porta no URI do cluster na página de visão geral.|28205, 28016, 28175|
|*user_name*|O nome do usuário para autenticar os bancos de dados originais. O usuário precisa ter privilégio READ no banco de dados que você deseja migrar.|my_user|
|*authentication_database_name*|O banco de dados de autenticação para o usuário especificado. Se você não especificar um banco de dados de autenticação, `mongodump` presumirá que seja o banco de dados que você configurou na opção `--db`. Se a opção `--db` também não for especificada, `mongodump` presumirá que seja o banco de dados do administrador.|administrador|
|*database_name*|O banco de dados que deseja fazer backup. Se você não especificar o banco de dados, `mongodump` fará dump de todos os bancos de dados que pertencerem ao usuário.|my_database|
|*cert_file*|O arquivo `.pem` para Autoridade de certificação. É possível fazer download do arquivo cert na página de visão geral e especificar o arquivo usando caminhos relativos ou absolutos.|./cert.pem|
|*dump_file*|O arquivo `.dump` para armazenar os dados originais. É possível usar caminhos relativos ou absolutos para especificar o arquivo.|./mongo.dump|
{: caption="Tabela 1. As variáveis que são necessárias para criar um arquivo de dump"}

Se você precisar de informações mais detalhadas sobre o utilitário `mongodump`, será possível consultar a [documentação do {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}.

É possível obter a maioria das informações do URI do cluster. O formato do URI é `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`. É possível corresponder o valor ao campo correspondente no comando `mongodump` anterior.
{: tip}

## Etapa 2: criar uma nova instância do {{site.data.keyword.cos_full_notm}}
{: #step2_create_object_storage_backup_mongodb}

É necessário criar a instância em uma região que seja diferente de onde a instância de serviço do {{site.data.keyword.ihsdbaas_mongodb_full}} está hospedada atualmente. É possível consultar as etapas a seguir:

1. [criar uma nova instância do Cloud {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision).
2. [Criar um depósito](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region) em uma região diferente.
3. [Criar uma credencial de serviço](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials) e salvar `access_key_id` e `secret_access_key` para uso posterior.
4. Instalar um cliente S3.
5. Use o cliente S3 e as chaves de acesso para se conectar ao terminal do Cloud {{site.data.keyword.cos_short}} do depósito que você criou anteriormente.
6. Usar o cliente S3 para [upload do arquivo de backup do {{site.data.keyword.mongodb}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-upload) que você criar na [Etapa 1](#step1_create_dump_file_backup_mongodb) para o depósito.

É possível consultar a [documentação do {{site.data.keyword.cos_full_notm}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started) para obter mais informações.

Após concluir essa etapa, você faz backup dos seus dados com sucesso em uma instância do Cloud {{site.data.keyword.cos_short}} em uma região diferente. Se desejar restaurar os dados por meio da instância do Cloud {{site.data.keyword.cos_short}} em uma instância do {{site.data.keyword.ihsdbaas_mongodb_full}}, leia a Etapa 3 a seguir para obter detalhes.

## Etapa 3: restaurar os dados da instância do Cloud {{site.data.keyword.cos_short}} para uma instância do {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #step3_restore_data_from_cos_mongodb}

É necessário fazer download do arquivo de backup por meio do depósito do Cloud {{site.data.keyword.cos_short}} para sua máquina local primeiro, em seguida, usar o comando `mongorestore` para restaurar os dados.

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
|*database_name*|O banco de dados de destino para o qual deseja restaurar os dados. Se o banco de dados não existir, `mongorestore` o criará automaticamente. Se você não especificar o banco de dados, `mongorestore` criará um banco de dados baseado no banco de dados original.|my_database|
|*cert_file*|O arquivo `.pem` para Autoridade de certificação. É possível fazer download do arquivo por meio da página de visão geral da instância e especificá-lo usando caminhos relativos ou absolutos.|./cert.pem|
|*dump_file*|O arquivo `.dump` que você faz download por meio do seu depósito do Cloud {{site.data.keyword.cos_short}}. É possível usar caminhos relativos ou absolutos para especificar o arquivo.|./mongo.dump|
{: caption="Tabela 2. As variáveis que são necessárias para restaurar os dados de um arquivo de dump"}

Para o {{site.data.keyword.mongodb}}, a migração mescla os dados originais no cluster de destino em vez de sobrescrever os dados existentes, se houver. É possível consultar a [explicação detalhada](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external} desse recurso no website {{site.data.keyword.mongodb}}. Também é possível consultar a [documentação do {{site.data.keyword.mongodb}}](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external} para obter mais informações sobre o utilitário `mongorestore`.

## O que vem a seguir
{: #whats_next_backup_mongodb}

Após a [Etapa 3](#step3_restore_data_from_cos_mongodb), é possível se conectar ao cluster do banco de dados para verificar se os dados foram restaurados com sucesso.
