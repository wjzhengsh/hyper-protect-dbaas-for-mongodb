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


# {{site.data.keyword.cos_full_notm}}를 사용하여 데이터베이스 백업 및 복원
{: #backup_mongodb_databases}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} 서비스는 24시간마다 한 번씩 전체 데이터베이스의 백업을 자동으로 트리거합니다. 이러한 암호화된 백업은 최근 7일에 대해 사용 가능하며, 지원되는 지역의 모든 가용성 구역에 있는 로컬 스토리지에서 중복 사용할 수 있습니다.
{: shortdesc}

재해 복구 기능을 늘리고 지원되는 지역 외부에 데이터를 백업하려는 경우 다음 단계에 따라 [{{site.data.keyword.cos_full_notm}} 서비스](https://cloud.ibm.com/catalog/services/cloud-object-storage){: external}를 사용하여 데이터를 다른 지역에 저장할 수 있습니다.

## 시작하기 전에
{: #backup_mongodb_before_begin}

{{site.data.keyword.mongodb}} 명령을 사용하여 백업을 완료하려면 먼저 시스템에 {{site.data.keyword.mongodb}}를 다운로드하여 설치해야 합니다. 자세한 정보는 [{{site.data.keyword.mongodb}} 웹 사이트](https://docs.mongodb.com/manual/installation/){: external}를 참조하십시오.

## 1단계: 원본 데이터베이스 백업용 덤프 파일 작성
{: #step1_create_dump_file_backup_mongodb}

다음 `mongodump` 명령을 사용하여 백업할 데이터베이스가 포함된 덤프 파일을 작성하십시오.

```
mongodump --host "<replica_set_name>/<hostname1>:<port1>,<hostname2>:<port2>,<hostname3>:<port3>" --ssl --username <user_name> --authenticationDatabase <authentication_database_name> --db <database_name> --sslCAFile <cert_file> --out <dump_file>
```
{: pre}

다음의 표는 명령에서 사용되는 변수를 보여줍니다.

|변수|설명|예제|
|---------|-----------|-------|
|*replica_set_name*|{{site.data.keyword.mongodb}} 복제본 세트 이름입니다. 인스턴스를 작성할 때 설정한 클러스터 이름입니다. 클러스터 UI의 개요 페이지에서 클러스터 URI을 찾을 수 있습니다. 이름은 `replicaSet=` 이후의 값입니다.|MongoDB-001|
|*host_name1, host_name2, host_name3*|복제본 세트를 호스팅하는 DBaaS 관리자 서버입니다. 3개의 복제본이 사용 가능합니다. 개요 페이지에서 클러스터 URI의 호스트 이름을 찾을 수 있습니다.|dbaas29.hyperprotectdbaas.cloud.ibm.com, dbaas31.hyperprotectdbaas.cloud.ibm.com, dbaas30.hyperprotectdbaas.cloud.ibm.com|
|*port1, port2, port3*|대응되는 호스트에 연결하기 위한 포트입니다. 개요 페이지에서 클러스터 URI의 포트 번호를 찾을 수 있습니다.|28205, 28016, 28175|
|*user_name*| 원래 데이터베이스를 인증하기 위한 사용자 이름입니다. 사용자는 마이그레이션할 데이터베이스에 대한 READ 권한을 보유해야 합니다.|my_user|
|*authentication_database_name*|지정된 사용자의 인증 데이터베이스입니다. 인증 데이터베이스를 지정하지 않으면 `mongodump`에서 이를 `--db` 옵션에 설정한 데이터베이스로 간주합니다. `--db` 옵션도 지정하지 않은 경우 `mongodump`에서 admin 데이터베이스로 간주합니다.|admin|
|*database_name*|백업할 데이터베이스입니다. 데이터베이스를 지정하지 않으면 `mongodump`가 사용자에게 속한 모든 데이터베이스를 덤프합니다.|my_database|
|*cert_file*|인증 기관의 `.pem` 파일입니다. 개요 페이지에서 인증서 파일을 다운로드하고 상대 또는 절대 경로를 사용하여 이 파일을 지정할 수 있습니다.|./cert.pem|
|*dump_file*|원래 데이터를 저장할 `.dump` 파일입니다. 상대 또는 절대 경로를 사용하여 파일을 지정할 수 있습니다.|./mongo.dump|
{: caption="표 1. 덤프 파일 작성에 필요한 변수"}

`mongodump` 유틸리티에 대한 추가 세부사항이 필요한 경우 [{{site.data.keyword.mongodb}} 문서](https://docs.mongodb.com/manual/reference/program/mongodump/){: external}를 참조할 수 있습니다.

클러스터 URI에서 대부분의 정보를 얻을 수 있습니다. URI의 형식은 `mongodb://<host_name1>:<port1>,<host_name2>:<port2>,<host_name3>:<port3>/admin?replicaSet=<replica_set_name>`입니다. 이전 `mongodump` 명령에서 해당 필드와 값을 일치시킬 수 있습니다.
{: tip}

## 2단계: {{site.data.keyword.cos_full_notm}} 인스턴스 새로 작성
{: #step2_create_object_storage_backup_mongodb}

{{site.data.keyword.ihsdbaas_mongodb_full}} 서비스 인스턴스가 현재 호스팅되는 지역과 다른 지역에 인스턴스를 작성해야 합니다. 다음 단계를 참조하십시오.

1. [Cloud {{site.data.keyword.cos_short}} 인스턴스를 새로 작성](/docs/services/cloud-object-storage?topic=cloud-object-storage-provision)하십시오.
2. 다른 지역에 [버킷을 작성](/docs/services/cloud-object-storage?topic=cloud-object-storage-endpoints#endpoints-region)하십시오.
3. [서비스 인증 정보를 작성](/docs/services/cloud-object-storage?topic=cloud-object-storage-service-credentials)한 후 나중에 사용할 수 있도록 `access_key_id` 및 `secret_access_key`를 저장하십시오.
4. S3 클라이언트를 설치하십시오.
5. S3 클라이언트와 액세스 키를 사용하여 앞에서 작성한 버킷의 Cloud {{site.data.keyword.cos_short}} 엔드포인트에 연결하십시오.
6. S3 클라이언트를 사용하여 [[1단계](#step1_create_dump_file_backup_mongodb)(/docs/services/cloud-object-storage?topic=cloud-object-storage-upload)]에서 작성한 {{site.data.keyword.mongodb}} 백업 파일을 버킷에 업로드하십시오.

자세한 내용은 [{{site.data.keyword.cos_full_notm}} 문서](/docs/services/cloud-object-storage?topic=cloud-object-storage-getting-started)를 참조하십시오.

이 단계를 완료하면 Cloud {{site.data.keyword.cos_short}} 인스턴스의 데이터가 다른 지역에 성공적으로 백업됩니다. Cloud {{site.data.keyword.cos_short}} 인스턴스의 데이터를 {{site.data.keyword.ihsdbaas_mongodb_full}} 인스턴스로 복원하려면 다음 3단계에서 세부사항을 참조하십시오.

## 3단계: Cloud {{site.data.keyword.cos_short}} 인스턴스에서 {{site.data.keyword.ihsdbaas_mongodb_full}} 인스턴스로 데이터 복원
{: #step3_restore_data_from_cos_mongodb}

Cloud {{site.data.keyword.cos_short}} 버킷의 백업 파일을 로컬 시스템으로 먼저 다운로드한 후 `mongorestore` 명령을 사용하여 데이터를 복원해야 합니다.

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
|*database_name*|데이터가 복원될 대상 데이터베이스입니다. 데이터베이스가 없을 경우 `mongorestore`가 자동으로 작성합니다. 데이터베이스를 지정하지 않은 경우 `mongorestore`가 원본 데이터베이스를 기반으로 데이터베이스를 작성합니다.|my_database|
|*cert_file*|인증 기관의 `.pem` 파일입니다. 인스턴스 개요 페이지에서 파일을 다운로드하고 상대 또는 절대 경로를 사용하여 이를 지정할 수 있습니다.|./cert.pem|
|*dump_file*|Cloud {{site.data.keyword.cos_short}} 버킷에서 다운로드하는 `.dump` 파일입니다. 상대 또는 절대 경로를 사용하여 파일을 지정할 수 있습니다.|./mongo.dump|
{: caption="표 2. 덤프 파일에서 데이터 복원에 필요한 변수"}

{{site.data.keyword.mongodb}}의 경우 마이그레이션은 기존 데이터(있는 경우)를 겹쳐쓰는 대신 원래 데이터를 대상 클러스터에 병합합니다. 이 기능에 대한 [자세한 설명](https://docs.mongodb.com/manual/reference/program/mongorestore/#insert-only){: external}은 {{site.data.keyword.mongodb}} 웹 사이트에서 참조할 수 있습니다. `mongorestore` 유틸리티에 대한 자세한 정보는 [{{site.data.keyword.mongodb}} 문서](https://docs.mongodb.com/manual/reference/program/mongorestore/){: external}를 참조하십시오.

## 다음에 수행할 작업
{: #whats_next_backup_mongodb}

[3단계](#step3_restore_data_from_cos_mongodb)를 수행한 후에는 데이터베이스 클러스터에 연결하여 데이터가 성공적으로 복원되었는지 확인할 수 있습니다.
