---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: high availability disaster recovery

subcollection: hyper-protect-dbaas-for-mongodb

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 일반 고가용성 및 재해 복구 고려사항
{: #ha-dr}

모든 {{site.data.keyword.cloud_notm}} GA(General Availability) 오퍼링에는 99.95%  가용성의 SLA(Service Level Agreement)가 있습니다. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}는 댈러스에서 제공되는 GA 서비스입니다. 각 위치에는 중복성을 위한 세 가지 서로 다른 데이터 센터가 있습니다.
{: shortdesc}

{{site.data.keyword.IBM_notm}}은 고가용성의 보안 환경에서 데이터베이스를 호스팅합니다. {{site.data.keyword.IBM_notm}} Secure Service Container 기술은 변조 방지 환경을 통해 시스템을 보호합니다. {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}는 저장 및 이동 시 데이터 암호화를 제공합니다. {{site.data.keyword.IBM_notm}}이나 서드파티 모두 사용자의 데이터에 액세스할 수 없으므로 사용자 데이터베이스의 백업 및 복원 정책이 필수적입니다.

데이터베이스를 백업하고 복원하는 방법에 대해 알아보려면
[IBM Cloud Object Storage를 사용하여 데이터베이스 백업 및 복원](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases)
및 [IBM 지원 센터에서 데이터베이스 복원](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases)을 참조하십시오.

{{site.data.keyword.cloud_notm}}에서 고가용성 및 재해 복구 표준에 대해 자세히 알아보려면 [작동 중단이 발생하지 않도록 하는 방법](/docs/overview?topic=overview-zero-downtime#zero-downtime)을 참조하십시오. 또한 [SLA(Service Level Agreements)](/docs/overview?topic=overview-zero-downtime#SLAs)에 대한 정보를 찾을 수 있습니다.
