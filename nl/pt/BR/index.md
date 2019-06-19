---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# Introdução ao {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #gettingstarted}

O {{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} é um serviço do {{site.data.keyword.cloud_notm}} que fornece bancos de dados do MongoDB on demand. Ele oferece uma plataforma flexível e escalável que permite provisionar e gerenciar
de forma rápida e fácil o seu banco de dados de escolha.
{: shortdesc}

Esta oferta do {{site.data.keyword.cloud_notm}} fornece clusters de banco de dados do MongoDB. Cada cluster de
banco de dados inclui uma instância de banco de dados principal e duas réplicas de instância
de banco de dados que fazem backup do principal.

Com o {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, é possível criar clusters de banco de dados no {{site.data.keyword.cloud_notm}},
gerenciar instâncias de banco de dados, administrar usuários do banco de dados, criar e
monitorar bancos de dados.

## Segurança e privacidade de dados
{: #data-security-and-privacy}

A {{site.data.keyword.IBM_notm}} hospeda seus bancos de dados em um ambiente altamente disponível e seguro:
<ul>
<li>As tecnologias subjacentes evitam que a {{site.data.keyword.IBM_notm}} ou um terceiro possa acessar seus dados.
<p>A tecnologia do {{site.data.keyword.IBM_notm}} Secure Service Container protege o sistema por meio de um
ambiente inviolável. O acesso ao sistema é restrito e é ativado somente
por meio de APIs de RESTful bem definidas.</p></li>
<li>Os dados são criptografados em repouso e em andamento.</li>
<li>O hardware do sistema, a configuração do sistema e a configuração do banco de dados asseguram alta disponibilidade.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## Criando um cluster de banco de dados
{: #creating-a-database-cluster-introduction}

Para criar um cluster de banco de dados, você simplesmente insere os valores necessários na
tela de configuração de serviço do {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} e clica em **Criar**.

Depois de ter criado o cluster de banco de dados, o {{site.data.keyword.IBM_notm}} fornece um ou mais nomes de host e números
de porta das instâncias de banco de dados criadas. Agora é possível usar essas informações
e as credenciais do usuário especificadas no catálogo para criar e acessar os seus
bancos de dados.

## Gerenciando o cluster de banco de dados
{: #managing-database-cluster-introduction}

Em um cluster de banco de dados, é possível criar bancos de dados, gerenciar as instâncias de banco de dados e
criar ou excluir usuários.

O {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} contém um DBaaS Manager, que gerencia e
planeja de forma inteligente as suas solicitações com base nos recursos disponíveis.

É possível direcionar as solicitações ao DBaaS Manager por meio de uma dessas interfaces:

- A [Interface com o usuário da web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- As [APIs do DBaaS Manager (para gerenciamento de cluster de banco de dados)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- As APIs do [{{site.data.keyword.cloud_notm}} (para gerenciamento de instância de serviço)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- O [plug-in da CLI com a ferramenta da CLI do {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## Acessando o banco de dados
{: #accessing-database-introduction}

Após criar um banco de dados MongoDB, é possível usar o shell mongo, seu driver MongoDB favorito ou ferramentas como MongoDB Compass para gerenciar o banco de dados.

### Antes de Começar
{: #accessing-database-introduction-byb}

Para assegurar a transferência de dados segura, obtenha um arquivo de autoridade de certificação (CA)
por meio do painel e copie-o para o diretório apropriado.

### Conectando-se a um banco de dados
{: #accessing-database-introduction-connect}

O painel do {{site.data.keyword.ihsdbaas_mongodb_full}} fornece as informações necessárias para se conectar a um banco de dados.

#### shell mongo
{: #accessing-database-introduction-connect-mongoshell}

É possível executar o comando shell mongo que é fornecido no painel do {{site.data.keyword.ihsdbaas_mongodb_full}}:

1. Clique no ícone ao lado do comando para copiá-lo para a sua área de transferência.
2. Se a conexão de dados segura falhar com um erro de SSL, especifique o arquivo de CA obtido para validar o certificado do servidor. Inclua o parâmetro `--sslCAFile` para indicar o arquivo de CA.

Consulte o exemplo a seguir:

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

Em
que:
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> É o nome do host da réplica do banco de dados (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> É o número da porta da réplica do banco de dados (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> É o nome de seu conjunto de réplicas retornado, conforme especificado no painel do {{site.data.keyword.ihsdbaas_mongodb_full}} </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> É o nome do usuário para o administrador do Banco de dados conforme especificado na tela de criação de serviço </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> É a senha para o administrador do Banco de dados, conforme especificada na tela de criação do serviço </dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> É o arquivo de CA cert.pem </dd>
</dl>


### Outras ferramentas
{: #accessing-database-introduction-connect-other}

Para outras ferramentas, como o MongoDB Compass, o {{site.data.keyword.ihsdbaas_mongodb_full}} suporta *validação de certificado do servidor do SSL* para se conectar ao host. Se necessário, use o arquivo de CA fornecido.
