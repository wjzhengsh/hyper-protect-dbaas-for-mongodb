---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 시작하기
{: #gettingstarted}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}}는 MongoDB DB On Demand를 제공하는 {{site.data.keyword.cloud_notm}} 서비스입니다. 이는 선택한 데이터베이스를 빠르고 손쉽게 프로비저닝하고 관리할 수 있도록 해주는 유연한 확장형 플랫폼을 제공합니다.
{: shortdesc}

이 {{site.data.keyword.cloud_notm}} 오퍼링은 MongoDB 데이터베이스 클러스터를 제공합니다. 각 데이터베이스 클러스터는 1개의 기본 데이터베이스 인스턴스, 그리고 기본 데이터베이스 인스턴스를 백업하는 2개의 데이터베이스 인스턴스 복제본으로 구성되어 있습니다.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}를 사용하면
{{site.data.keyword.cloud_notm}}에서 데이터베이스 클러스터를 작성하고 데이터베이스 인스턴스를 관리하며
데이터베이스 사용자를 관리하고 데이터베이스를 작성 및 모니터할 수 있습니다.

## 데이터 보안 및 개인정보 보호
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}}은 고가용성의 보안 환경에서 데이터베이스를 호스팅합니다.
<ul>
<li>기반 기술은 사용자 데이터에 대한 {{site.data.keyword.IBM_notm}} 또는 서드파티의 액세스를 방지합니다.
<p>{{site.data.keyword.IBM_notm}} Secure Service Container 기술은 변조 방지 환경을 통해 시스템을 보호합니다. 시스템에 대한 액세스가 제한되며 이는 잘 정의된 RESTful API를 통해서만 가능합니다.</p></li>
<li>데이터는 저장 시 및 이동 중에 암호화됩니다.</li>
<li>시스템 하드웨어, 시스템 구성 및 데이터베이스 설정은 고가용성을 보장합니다.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## 데이터베이스 클러스터 작성
{: #creating-a-database-cluster-introduction}

데이터베이스 클러스터를 작성하려면 단지 {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} 서비스 구성 화면에서 필수 값을 입력하고 **작성**을 클릭하십시오.

데이터베이스 클러스터가 작성된 이후 {{site.data.keyword.IBM_notm}}은 작성된 데이터베이스 인스턴스의 호스트 이름과 포트 번호를 하나 이상 제공합니다. 이제 이 정보와 함께 카탈로그에 지정한 사용자 인증 정보를 사용하여 데이터베이스를 작성하고 이에 액세스할 수 있습니다.

## 데이터베이스 클러스터 관리
{: #managing-database-cluster-introduction}

데이터베이스 클러스터에서 데이터베이스를 작성하고 데이터베이스 인스턴스를 관리하며 사용자를 작성 또는 삭제할 수 있습니다.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}에는
사용 가능한 리소스를 기반으로 요청을 관리하고 지능적으로 스케줄하는 DBaaS Manager가 포함되어 있습니다.

다음 인터페이스 중 하나를 통해 DBaaS Manager에 요청을 전달할 수 있습니다.

- [웹 사용자 인터페이스](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- [DBaaS Manager API(데이터베이스 클러스터 관리용)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- [{{site.data.keyword.cloud_notm}} API(서비스 인스턴스 관리용)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- [{{site.data.keyword.cloud_notm}} CLI 도구의 CLI 플러그인](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## 데이터베이스 액세스
{: #accessing-database-introduction}

MongoDB 데이터베이스를 작성한 후에는 mongo 쉘, 선호하는 MongoDB 드라이버 또는 MongoDB Compass 등의 도구를 사용하여 데이터베이스를 관리할 수 있습니다.

### 시작하기 전에
{: #accessing-database-introduction-byb}

안전한 데이터 전송을 보장하려면 대시보드에서 인증 기관(CA) 파일을 가져와서 이를 적절한 디렉토리에 복사하십시오.

### 데이터베이스에 연결
{: #accessing-database-introduction-connect}

{{site.data.keyword.ihsdbaas_mongodb_full}} 대시보드는 데이터베이스에 연결하는 데 필요한 정보를 제공합니다.

#### mongo 쉘
{: #accessing-database-introduction-connect-mongoshell}

{{site.data.keyword.ihsdbaas_mongodb_full}} 대시보드에서 제공되는 mongo 쉘 명령을 실행할 수 있습니다.

1. 명령 옆의 아이콘을 클릭하여 이를 클립보드에 복사하십시오.
2. 보안 데이터 연결이 SSL 오류로 실패하는 경우에는 서버 인증서를 유효성 검증할 수 있도록 가져온 CA 파일을 지정하십시오. CA 파일을 표시하기 위한 `--sslCAFile` 매개변수를 추가하십시오.

다음 예를 참조하십시오.

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

여기서:
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> 데이터베이스 복제본의 호스트 이름입니다(<em>i=1,2,3</em>). </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> 데이터베이스 복제본의 포트 번호입니다(<em>i=1,2,3</em>). </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> {{site.data.keyword.ihsdbaas_mongodb_full}} 대시보드에서 지정된 리턴된 복제본 세트의 이름입니다. </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> 서비스 작성 화면에 지정된 데이터베이스 관리자의 사용자 이름입니다. </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> 서비스 작성 화면에 지정된 데이터베이스 관리자의 비밀번호입니다. </dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> CA 파일 cert.pem입니다. </dd>
</dl>


### 기타 도구
{: #accessing-database-introduction-connect-other}

MongoDB Compass와 같은 기타 도구의 경우 {{site.data.keyword.ihsdbaas_mongodb_full}}에서는 호스트에 연결하기 위해 *SSL 서버 인증서 유효성 검증*을 지원합니다. 필요한 경우 제공된 CA 파일을 사용하십시오.
