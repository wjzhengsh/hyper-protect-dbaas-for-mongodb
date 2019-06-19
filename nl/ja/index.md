---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} の概要
{: #gettingstarted}

{{site.data.keyword.cloud}}
{{site.data.keyword.ihsdbaas_mongodb_full}} は、オンデマンドで MongoDB データベースを提供する
{{site.data.keyword.cloud_notm}} サービスです。
柔軟かつスケーラブルなプラットフォームで、最適なデータベースを迅速かつ簡単にプロビジョンして管理することができます。
{: shortdesc}

この {{site.data.keyword.cloud_notm}} オファリングでは、MongoDB データベース・クラスターが提供されます。 各データベース・クラスターは、1 つのプライマリー・データベース・インスタンスと、プライマリー・データベースをバックアップする 2 つのデータベース・インスタンス・レプリカから構成されます。

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} では、{{site.data.keyword.cloud_notm}} 内にデータベース・クラスターを作成し、
データベース・インスタンスの管理、データベース・ユーザーの管理、データベースの作成とモニターを行えます。

## データ・セキュリティーおよびプライバシー
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}} は可用性が高くセキュアな環境でデータベースをホストします。
<ul>
<li>基礎のテクノロジーにより、{{site.data.keyword.IBM_notm}} や第三者によるデータ・アクセスを防止しています。
<p>{{site.data.keyword.IBM_notm}} Secure Service Container テクノロジーにより、改ざん防止環境でシステムを保護しています。システムへのアクセスは制限されており、明確に定義された RESTful API を使用しないとアクセスできません。</p></li>
<li>データは保存中も転送中も暗号化されます。</li>
<li>システムのハードウェア、システム構成、およびデータベースのセットアップにより、高可用性を実現しています。</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## データベース・クラスターの作成
{: #creating-a-database-cluster-introduction}

データベース・クラスターを作成する方法は、{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} サービス構成画面に必要な値を入力して**「作成」**をクリックするだけです。

データベース・クラスターを作成すると、作成された 1 つ以上のデータベース・インスタンスのホスト名とポート番号が {{site.data.keyword.IBM_notm}} から通知されます。 その情報とカタログに指定したユーザー資格情報を使用すれば、
データベースを作成してアクセスできます。

## データベース・クラスターの管理
{: #managing-database-cluster-introduction}

データベース・クラスターでは、データベースの作成、データベース・インスタンスの管理、およびユーザーの作成/削除を行えます。

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} に含まれている DBaaS Manager は、使用可能なリソースに基づいて要求を処理し、インテリジェントにスケジューリングすることができます。

要求を DBaaS Manager に送信するには、以下のいずれかのインターフェースを使用します。

- [Web ユーザー・インターフェース](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- [DBaaS Manager API (データベース・クラスターの管理)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- [{{site.data.keyword.cloud_notm}} API (サービス・インスタンスの管理)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- [{{site.data.keyword.cloud_notm}} CLI ツールの CLI プラグイン](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## データベースへのアクセス
{: #accessing-database-introduction}

MongoDB データベースを作成したら、mongo シェル、任意の MongoDB ドライバー、ツール (MongoDB Compass など) を使用して、データベースを管理できます。

### 始めに
{: #accessing-database-introduction-byb}

セキュアなデータ転送を確保するために、ダッシュボードから認証局 (CA) ファイルを取得して適切なディレクトリーにコピーします。

### データベースへの接続
{: #accessing-database-introduction-connect}

データベースに接続するために必要な情報は、{{site.data.keyword.ihsdbaas_mongodb_full}} ダッシュボードに表示されます。

#### mongo シェル
{: #accessing-database-introduction-connect-mongoshell}

{{site.data.keyword.ihsdbaas_mongodb_full}} ダッシュボードに表示されている mongo シェルのコマンドを実行できます。

1. コマンドの横にあるアイコンをクリックしてコマンドをクリップボードにコピーします。
2. セキュアなデータ接続が SSL エラーで失敗した場合は、取得した CA ファイルを指定してサーバー証明書を検証します。 パラメーター `--sslCAFile` を追加して CA ファイルを指定します。

以下の例を参照してください。

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

各パラメーターの説明は以下のとおりです。
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> データベース・レプリカのホスト名 (<em>i=1、2、3</em>) </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> データベース・レプリカのポート番号 (<em>i=1、2、3</em>) </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> {{site.data.keyword.ihsdbaas_mongodb_full}} ダッシュボードで指定して返されたレプリカ・セットの名前 </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> サービス作成画面で指定されたデータベース管理者のユーザー名 </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> サービス作成画面で指定されたデータベース管理者のパスワード</dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> CA ファイル cert.pem </dd>
</dl>


### その他のツール
{: #accessing-database-introduction-connect-other}

その他のツール (MongoDB Compass など) については、{{site.data.keyword.ihsdbaas_mongodb_full}} は、ホストに接続するために *SSL サーバー証明書の検証*をサポートしています。 必要に応じて、提供されている CA ファイルを使用してください。
