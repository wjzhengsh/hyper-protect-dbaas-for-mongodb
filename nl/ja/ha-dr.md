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

# 一般的な高可用性と災害復旧の考慮事項
{: #ha-dr}

すべての {{site.data.keyword.cloud_notm}} 一般出荷版 (GA) オファリングのサービス・レベル・アグリーメントの可用性は 99.95% です。 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} はダラスで提供されている GA サービスです。 各ロケーションには、冗長性を確保するために 3 つの異なるデータ・センターがあります。
{: shortdesc}

{{site.data.keyword.IBM_notm}} は、可用性が高くセキュアな環境でデータベースをホストします。 {{site.data.keyword.IBM_notm}} Secure Service Container テクノロジーにより、改ざん防止環境でシステムを保護しています。{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} は、保存中も転送中もデータの暗号化が可能です。 {{site.data.keyword.IBM_notm}} も第三者もデータにアクセスできないため、データベースのバックアップと復元のポリシーが不可欠です。

データベースをバックアップおよび復元する方法については、[IBM Cloud Object Storage を使用したデータベースのバックアップと復元](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-backup_mongodb_databases)および[IBM サポートによるデータベースの復元](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-restore_mongodb_databases)を参照してください。

{{site.data.keyword.cloud_notm}} での高可用性および災害復旧の規格の詳細については、[ダウン時間をゼロにする方法](/docs/overview?topic=overview-zero-downtime#zero-downtime)を参照してください。 [サービス・レベル・アグリーメント](/docs/overview?topic=overview-zero-downtime#SLAs)に関する情報も参照できます。
