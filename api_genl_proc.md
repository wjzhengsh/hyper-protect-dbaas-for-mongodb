---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: DBaaS Manager APIs, DBaaS Manager, API request

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# General instructions for using the DBaaS Manager APIs
{: #gen_inst_mgr_apis}
<ol>
<li>Specify the data of your request in JSON format.
</li>
<li>Send the API request to the DBaaS Manager.
<p>You can use a RESTful API client, such as cURL, to handle a request.
</p>
<p>DBaaS Managers are available here:
<table>
  <tr>
    <th> Host name </th>
    <th> Port number </th>
    <th> Region </th>
    <th> City </th>
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

For more detailed information (methods and parameters), you can see [DBaaS API documentation](https://{DomainName}/apidocs/hyperp-dbaas){: external}.


## Example
{: #api-example}

To create a service instance, issue a direct API call, such as:

```javascript
curl -X POST \
"https://<ip>:<port>/api/v1/services" \
-d '{"name": "Service_Name", "resource_group": "default", "plan": "mongodb-free", "admin_name": "admin", "password": "passw0rd_for_adm"}'
-H "x-auth-token: {access_token}" \
-H "accept: application/json" \
-H "accept-license-agreement: yes"
```
{:codeblock}

Where:
<dl>
<dt> &lt;<em>accessToken</em>&gt; </dt>
<dd>Is the previously obtained access token; see [Setting up authentication to use DBaaS Manager APIs](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-api-auth). (**Note:** If the access token has expired, return to those instructions and request a new token.) </dd>
<dt> &lt;<em>ip</em>&gt; </dt>
<dd>Is the host name of a DBaaS Manager. Valid host names are listed in the table above.
</dd>
<dt> &lt;<em>port</em>&gt; </dt>
<dd>Is the port number of the DBaaS Manager. This is the valid port number:
<p>20000</p>
</dd>
</dl>
