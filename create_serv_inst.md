---

copyright:
  years: 2019
lastupdated: "2019-06-12"

keywords: service instance, ibmcloud resource

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:note: .note}


# Creating a service instance
{: #dbaas_cli_create_service}

To create a service instance, use the `ibmcloud resource service-instance-create` command, as shown in the following example. In Windows, it is recommended that you use a Bash terminal such as Cygwin or Git Bash to enter the command.

```javascript
ibmcloud resource service-instance-create MyDBaaSIns03 hyperp-dbaas-mongodb mongodb-small us-south -p '{"name":"DBaaSTestCLICluster03", "admin_name":"admin","password":"passWORD4User19", "confirm_password":"passWORD4User19", "license_agree":["agreed"]}'
```
{: codeblock}

Where the parameters have the following definitions:

| Parameter        |  Definition                                                    |
| ---------------- |  -------------------------------------------------------------- |
| *MyDBaaSIns03*   |  The name of the service instance (replace with a name of your own choosing). |
| *hyperp-dbaas-mongodb* | The catalog name of {{site.data.keyword.ihsdbaas_mongodb_full}}. |
| *mongodb-small*  | The plan name. Available plans are: **mongodb-small**, **mongodb-medium**, and **mongodb-large**.  (**Note:** Plan names are case-sensitive.) |
| *us-south*            | The location where your new database will be located. (**Note:** Currently only **us-south** supports {{site.data.keyword.ihsdbaas_mongodb_full}}.) |
| *-p*               | A valid JSON string, which must contain the parameters in the following table. |

<table>
  <tr>
    <th>-p parameter</th>
    <th>Definition</th>
  </tr>
  <tr>
    <td>*name*</td>
    <td>The name of your database cluster.</td>
  </tr>
  <tr>
    <td>*admin_name*</td>
    <td>The administrator's user name of the database to be created.</td>
  </tr>
  <tr>
    <td>*password*</td>
    <td>
      <p>The administrator's user password of the database to be created. You need to create a strong password with the following attributes:
        <ul>
          <li>minimum of **15 characters** in length</li>
          <li>at least **one uppercase** character</li>
          <li>at least **one lowercase** character</li>
          <li>at least **one number**</li>
          <li>does not contain user name</li>
        </ul>
      </p>
    </td>
  </tr>
  <tr>
    <td>*confirm_password*</td>
    <td>The same password.</td>
  </tr>
  <tr>
    <td>*license_agree*</td>
    <td>A value of **agreed** indicates acceptance of the license agreement, which is required to use {{site.data.keyword.ihsdbaas_mongodb_full}}.</td>
  </tr>
</table>


After you enter the command, the state of the new service instance might temporarily appear as **inactive**, as shown in this example:

```
Creating service instance MYDBaaSIns03 in resource group default of account BSS Metering Test Account as bluemix_ui_metering_test@mailinator.com...
OK
Service instance MYDBaaSIns03 was created.

Name:             MYDBaaSIns03
ID:               crn:v1:bluemix:public:hyperp-dbaas-mongodb:us-south:a/0e79133675a31dbfd10504847a9e174f:279be69f-20f2-4ade-baf9-5c470c400753::
GUID:             279be69f-20f2-4ade-baf9-5c470c400753   
Location:         us-south   
State:            inactive   
Type:             service_instance   
Sub Type:            
Created at:       2019-06-03T06:17:13Z   
Updated at:       2019-06-03T06:17:13Z   
Last Operation:                      
                  Status       create in progress      
                  Updated At   2019-06-03 06:17:13.917566444 +0000 UTC
```
{: codeblock}

This is because it takes a few minutes to prepare the cluster. To get an update of the cluster status, use the `ibmcloud resource service-instances` command, as shown in this example:

```
ibmcloud resource service-instances
Retrieving all instances of all services in resource group default and all locations under account BSS Metering Test Account as bluemix_ui_metering_test@mailinator.com...
OK
Name           Location   State      Type
MyDBaaSIns03   us-south   active     service_instance
```
{: codeblock}

To even further strengthen security, it is suggested that you update the **database admin password** immediately after the service instance is provisioned. You need to follow the same rules that are previously mentioned to set the new password.
{: note}
