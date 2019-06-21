---

copyright:
  years: 2019
lastupdated: "2019-06-21"

keywords: DBaaS Manager APIs, DBaaS Manager, API request

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Allgemeine Anweisungen für die Verwendung der DBaaS-Manager-APIs
{: #gen_inst_mgr_apis}
<ol>
<li>Geben Sie die Daten Ihrer Anforderung im JSON-Format an.
</li>
<li>Senden Sie die API-Anforderung an den DBaaS-Manager.
<p>Sie können einen RESTful-API-Client, wie z. B. cURL, verwenden, um eine Anforderung zu verarbeiten.
</p>
<p>DBaaS-Manager-Instanzen sind hier verfügbar:
<table>
  <tr>
    <th> Hostname </th>
    <th> Portnummer </th>
    <th> Region </th>
    <th> Stadt </th>
  </tr>
  <tr>
    <td> dbaas900.hyperp-dbaas.cloud.ibm.com </td>
    <td> 20000 </td>
    <td> us-south </td>
    <td> Dallas </td>
  </tr>
</table>
</p>	 
</li>
</ol>

Ausführlichere Informationen (Methoden und Parameter) finden Sie in der [Dokumentation zur DBaaS-API](https://{DomainName}/apidocs/hyperp-dbaas){: external}.


## Beispiel
{: #api-example}

Um eine Serviceinstanz zu erstellen, geben Sie einen direkten API-Aufruf aus, z. B.:

```
curl -X POST \
"https://<ip>:<port>/api/v1/services" \
-d '{"catalog": "hyperp-dbaas-mongodb", "name": "Service_Name", "resource_group": "default", "plan": "mongodb-small", "admin_name": "admin", "password": "passWORD4User19"}'
-H "x-auth-token: <access_token>" \
-H "accept: application/json" \
-H "accept-license-agreement: yes"
```
{:codeblock}

Dabei gilt Folgendes:
<dl>
<dt> &lt;<em>access_token</em>&gt; </dt>
<dd>Ist das zuvor abgerufene Zugriffstoken; siehe [Authentifizierung für die Verwendung von DBaaS-Manager-APIs einrichten](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-api-auth). (**Hinweis:** Wenn das Zugriffstoken abgelaufen ist, kehren Sie zu diesen Anweisungen zurück und fordern Sie ein neues Token an.) </dd>
<dt> &lt;<em>ip</em>&gt; </dt>
<dd>Ist der Hostname eines DBaaS-Managers. Gültige Hostnamen sind in der obigen Tabelle aufgeführt.
</dd>
<dt> &lt;<em>port</em>&gt; </dt>
<dd>Gibt die Portnummer des DBaaS-Managers an. Dies ist die gültige Portnummer:
<p>20000</p>
</dd>
</dl>
