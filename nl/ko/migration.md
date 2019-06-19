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

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}로 {{site.data.keyword.mongodb}} 데이터베이스 마이그레이션
{: #migration_mongodb}

{{site.data.keyword.mongodb}} 데이터베이스를 사용 중이며 {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}로 마이그레이션하려면 이 주제의 세부사항을 참조하십시오. 플랫폼에서 {{site.data.keyword.mongodb}} 데이터베이스 서비스를 제공하는 한 {{site.data.keyword.mongodb}} 데이터베이스의 호스팅 위치는 중요하지 않습니다.
{: shortdesc}

## 시작하기 전에
{: #migration_mongodb_before_begin}

{{site.data.keyword.mongodb}} 명령을 사용하여 마이그레이션을 완료하려면 우선 시스템에 {{site.data.keyword.mongodb}}를 다운로드하여 설치해야 합니다. 자세한 정보는 [{{site.data.keyword.mongodb}} 웹 사이트](https://docs.mongodb.com/manual/installation/){: external}를 참조하십시오.

## 1단계: 원래 데이터베이스의 복원을 위한 덤프 파일 작성
{: #step1_create_dump_file}

다음의 `mongodump` 명령을 사용하여 복원할 데이터베이스가 포함된 덤프 파일을 작성하십시오.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

다음의 표는 명령에서 사용되는 변수를 보여줍니다.

|변수|설명|예제|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 복제본 세트 이름입니다. 클러스터가 {{site.data.keyword.ihsdbaas_full}} Beta 인스턴스에 배치된 경우 이는 인스턴스를 작성할 때 설정한 클러스터 이름입니다. 클러스터 UI의 개요 페이지에서 클러스터 URI을 찾을 수 있습니다. 이름은 `replicaSet=` 이후의 값입니다.|MongoDB-001|
|*host_name1, host_name2, host_name3*|복제본 세트를 호스팅하는 호스트 서버의 이름입니다. 3개의 복제본이 사용 가능합니다. 클러스터가 {{site.data.keyword.ihsdbaas_full}} Beta 인스턴스에 배치된 경우 개요 페이지의 클러스터 URI에서 호스트 이름을 찾을 수 있습니다.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|대응되는 호스트에 연결하기 위한 포트입니다. 클러스터가 {{site.data.keyword.ihsdbaas_full}} Beta 인스턴스에 배치된 경우 개요 페이지의 클러스터 URI에서 포트 번호를 찾을 수 있습니다.|28205, 28016, 28175|
|*user_name*| 원래 데이터베이스를 인증하기 위한 사용자 이름입니다. 사용자는 마이그레이션할 데이터베이스에 대한 READ 권한을 보유해야 합니다.|my_user|
|*authentication_database_name*|지정된 사용자의 인증 데이터베이스입니다. 인증 데이터베이스를 지정하지 않으면 `mongodump`에서 이를 `--db` 옵션에 설정한 데이터베이스로 간주합니다. `--db` 옵션도 지정하지 않은 경우 `mongodump`에서 admin 데이터베이스로 간주합니다.|admin|
|*database_name*|마이그레이션할 데이터베이스입니다. 데이터베이스를 지정하지 않으면 `mongodump`가 사용자에게 속한 모든 데이터베이스를 덤프합니다.|my_database|
|*cert_file*|인증 기관의 `.pem` 파일입니다. 클러스터가 {{site.data.keyword.ihsdbaas_full}} Beta 인스턴스에 배치된 경우 개요 페이지에서 인증서 파일을 다운로드할 수 있습니다. 상대 또는 절대 경로를 사용하여 파일을 지정할 수 있습니다.|./cert.pem|
|*dump_file*|원래 데이터를 저장할 `.dump` 파일입니다. 상대 또는 절대 경로를 사용하여 파일을 지정할 수 있습니다.|./mongo.dump|
{: caption="표 1. 덤프 파일 작성에 필요한 변수"}

`mongodump` 유틸리티에 대한 추가 세부사항이 필요한 경우 [{{site.data.keyword.mongodb}} 문서](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}를 참조할 수 있습니다.

클러스터 URI에서 대부분의 정보를 얻을 수 있습니다. URI의 형식은 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`입니다. 이전 `mongodump` 명령에서 해당 필드와 값을 일치시킬 수 있습니다.
{: tip}

## 2단계: {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}에서 새 서비스 인스턴스 작성
{: #step2_creat_new_service_instance}

데이터를 복원하려면 우선 대상 데이터베이스 클러스터로서 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}에서 새 서비스 인스턴스를 작성해야 합니다. 다음 방법 중 하나를 사용하여 새 인스턴스를 작성할 수 있습니다.
- [웹 사용자 인터페이스](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service#dbaas_webui_create_service)
- [DBaaS Manger API](https://{DomainName}/apidocs/hyperp-dbaas#create-an-ibm-cloud-service-instance-of-hyperprote){: external}
- [{{site.data.keyword.cloud_notm}} CLI의 CLI 플러그인](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_create_service)

새 서비스 인스턴스를 작성하는 경우에는 클러스터 이름, 관리자 이름 및 비밀번호를 설정해야 합니다. 이는 원래 인스턴스에서와 반드시 동일할 필요가 없습니다. 이는 마이그레이션에 영향을 주지 않습니다. 마이그레이션이 완료되면 데이터베이스가 새 관리자/인증 데이터베이스에 지정됩니다.

## 3단계: 새 서비스 인스턴스로 덤프 파일의 데이터 복원
{: #step3_restore_data}

`mongorestore` 명령을 사용하여 [1단계](#step1_create_dump_file)에서 작성된 덤프 파일의 데이터를 복원할 수 있습니다.

```
mongorestore --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> <dump_file>
```
{: pre}

다음의 표는 명령에서 사용되는 변수를 보여줍니다.

|변수|설명|예제|
|---------|-----------|-------|
|*replica_set_name*| 대상 {{site.data.keyword.mongodb}} 복제본 세트 이름입니다. 이는 인스턴스를 작성할 때 설정한 클러스터 이름입니다. 개요 페이지에서 클러스터 URI을 찾을 수 있습니다. 이름은 `replicaSet=` 이후의 값입니다.|MongoDB-002|
|*host_name1, host_name2, host_name3*|대상 복제본 세트를 호스팅하는 DBaaS 관리자 서버입니다. 3개의 복제본이 사용 가능합니다. 개요 페이지에서 클러스터 URI의 호스트 이름을 찾을 수 있습니다.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|대응되는 호스트에 연결하기 위한 포트입니다. 개요 페이지에서 클러스터 URI의 포트를 찾을 수 있습니다.|28128, 28248, 28043|
|*user_name*| 대상 데이터베이스를 인증하기 위한 사용자 이름입니다. 사용자가 덤프 파일을 작성한 사용자와 동일할 필요는 없습니다.|new_user|
|*authentication_database_name*|지정된 사용자의 인증 데이터베이스입니다. 인증 데이터베이스를 지정하지 않으면 `mongodump`에서 이를 `--db` 옵션에 설정한 데이터베이스로 간주합니다. `--db` 옵션도 지정하지 않은 경우 `mongodump`에서 admin 데이터베이스로 간주합니다.|admin|
|*database_name*|데이터가 마이그레이션될 대상 데이터베이스입니다. 데이터베이스가 없을 경우 `mongorestore`가 자동으로 작성합니다. 데이터베이스를 지정하지 않은 경우 `mongorestore`가 원본 데이터베이스를 기반으로 데이터베이스를 작성합니다.|my_database|
|*cert_file*|인증 기관의 `.pem` 파일입니다. 인스턴스 개요 페이지에서 파일을 다운로드하고 상대 또는 절대 경로를 사용하여 이를 지정할 수 있습니다.|./cert.pem|
|*dump_file*|[1단계](#step1_create_dump_file)에서 작성한 `.dump` 파일입니다. 상대 또는 절대 경로를 사용하여 파일을 지정할 수 있습니다.|./mongo.dump|
{: caption="표 2. 덤프 파일에서 데이터 복원에 필요한 변수"}

다음 방법을 사용하여 사용자를 새로 작성할 수 있습니다. [웹 사용자 인터페이스](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas-webui-database-users#webui-create-database-user), [DBaaS Manager API](https://{DomainName}/apidocs/hyperp-dbaas#create-a-user-in-a-database-cluster){: external}, [{{site.data.keyword.cloud_notm}} CLI의 CLI 플러그인](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin#user_create) 및 [{{site.data.keyword.mongodb}} 명령](https://docs.mongodb.com/manual/reference/method/db.createUser/index.html){: external}.
{: tip}

{{site.data.keyword.mongodb}}의 경우 마이그레이션은 기존 데이터(있는 경우)를 겹쳐쓰는 대신 원래 데이터를 대상 클러스터에 병합합니다. 이 기능에 대한 [자세한 설명](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}은 {{site.data.keyword.mongodb}} 웹 사이트에서 참조할 수 있습니다. `mongorestore` 유틸리티에 대한 자세한 정보는 [{{site.data.keyword.mongodb}} 문서](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}를 참조하십시오.

## 다음에 수행할 작업
{: #whats_next_migration_mongodb}

마이그레이션 이후 새 데이터베이스 클러스터에 연결하여 데이터 마이그레이션의 성공 여부를 확인할 수 있습니다.
