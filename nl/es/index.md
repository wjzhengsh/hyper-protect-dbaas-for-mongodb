---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: database cluster, Data security, database instance

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}

# Iniciación a {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}
{: #gettingstarted}

{{site.data.keyword.cloud}} {{site.data.keyword.ihsdbaas_mongodb_full}} es un servicio de {{site.data.keyword.cloud_notm}} que proporciona bases de datos MongoDB bajo demanda. Ofrece una plataforma flexible y escalable que le permite suministrar y gestionar la base de datos que elija de forma rápida y fácil.
{: shortdesc}

Esta oferta de {{site.data.keyword.cloud_notm}} proporciona clústeres de base de datos MongoDB. Cada clúster de base de datos consta de una instancia de base de datos primaria y de dos réplicas de instancia de base de datos que realizan la copia de seguridad de la primera.

Con {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}, puede crear clústeres de bases de datos en {{site.data.keyword.cloud_notm}}, gestionar instancias de base de datos, administrar usuarios de base de datos, crear y supervisar bases de datos.

## Seguridad y privacidad de datos
{: #data-security-and-privacy}

{{site.data.keyword.IBM_notm}} aloja las bases de datos en un entorno de alta disponibilidad y seguridad:
<ul>
<li>Las tecnologías subyacentes impiden a {{site.data.keyword.IBM_notm}} o a un tercero poder acceder a sus datos.
<p>La tecnología Secure Service Container de {{site.data.keyword.IBM_notm}} protege el sistema a través de un entorno a prueba de manipulaciones. El acceso al sistema está restringido y solo se habilita a través de API RESTful bien definidas.</p></li>
<li>Los datos se cifran en reposo y en curso.</li>
<li>El hardware del sistema, la configuración del sistema y la configuración de base de datos garantizan la alta disponibilidad.</li>
</ul>

<!--
For more information, watch:

- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - English version](https://www.youtube.com/watch?v=__IBP727IL8){: external}
- [Data security and privacy using {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_full}} - Chinese version](https://v.youku.com/v_show/id_XMzc3ODQzMzYwMA==.html){: external}
-->

## Creación de un clúster de base de datos
{: #creating-a-database-cluster-introduction}

Para crear un clúster de base de datos, simplemente especifique los valores necesarios en la pantalla de configuración del servicio {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} y pulse **Crear**.

Después de haber creado el clúster de base de datos, {{site.data.keyword.IBM_notm}} le proporciona uno o más nombres de host y números de puerto de las instancias de base de datos creadas. Ahora puede utilizar esta información y las credenciales de usuario que ha especificado en el catálogo para crear y acceder a las bases de datos.

## Gestión del clúster de base de datos
{: #managing-database-cluster-introduction}

En un clúster de base de datos, puede crear bases de datos, gestionar las instancias de base de datos y crear o suprimir usuarios.

{{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}} contiene un gestor de DBaaS, que gestiona y planifica de forma inteligente las solicitudes en función de los recursos disponibles.

Puede dirigir las solicitudes al Gestor de DBaaS a través de una de estas interfaces:

- La [Interfaz de usuario de web](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_webui_service)
- Las [API del gestor de DBaaS (para la gestión de clústeres de base de datos)](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-gen_inst_mgr_apis)
- Las [API de {{site.data.keyword.cloud_notm}} (para la gestión de instancias de servicio)](https://{DomainName}/apidocs/hyperp-dbaas){: external}
- El [plugin de CLI con la herramienta de CLI de {{site.data.keyword.cloud_notm}}](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-install-ibm-cli)

## Acceso a la base de datos
{: #accessing-database-introduction}

Después de crear una base de datos MongoDB, puede utilizar el shell mongo, su controlador MongoDB favorito, o herramientas como MongoDB Compass para gestionar la base de datos.

### Antes de empezar
{: #accessing-database-introduction-byb}

Para garantizar la transferencia segura de datos, obtenga un archivo de la Entidad emisora de certificados (CA) desde el panel de control, y cópielo en el directorio correspondiente.

### Conexión a una base de datos
{: #accessing-database-introduction-connect}

El panel de control de {{site.data.keyword.ihsdbaas_mongodb_full}} proporciona la información necesaria para conectarse a una base de datos.

#### mongo shell
{: #accessing-database-introduction-connect-mongoshell}

Puede ejecutar el mandato mongo shell que se proporciona en el panel de control de {{site.data.keyword.ihsdbaas_mongodb_full}}:

1. Pulse el icono situado junto al mandato para copiarlo en el portapapeles.
2. Si la conexión de datos segura falla con un error de SSL, especifique el archivo de CA obtenido para validar el certificado del servidor. Añada el parámetro `--sslCAFile` para indicar el archivo de CA.

Consulte el siguiente ejemplo:

<pre><code class="hljs"># mongo 'mongodb:/&sol;&lt;<em>Hostname_1</em>&gt;&colon;&lt;<em>PortNumber_1</em>&gt;,\
&lt;<em>Hostname_2</em>&gt;&colon;&lt;<em>PortNumber_2</em>&gt;,\
&lt;<em>Hostname_3</em>&gt;&colon;&lt;<em>PortNumber_3</em>&gt;/admin?replicaSet=&lt;<em>replicaSetName</em>&gt;' \
--ssl --username &lt;<em>Username</em>&gt; --password &lt;<em>Password</em>&gt; --sslCAFile &lt;<em>CAFile</em>&gt;</code></pre>

Donde:
<dl>
  <dt> &lt;<em>Hostname_i</em>&gt; </dt>
    <dd> Es el nombre de host de la réplica de la base de datos (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>PortNumber_i</em>&gt; </dt>
    <dd> Es el número de puerto de la réplica de la base de datos (<em>i=1,2,3</em>) </dd>
  <dt> &lt;<em>replicaSetName</em>&gt; </dt>
    <dd> Es el nombre del conjunto de réplicas devueltas, tal como se ha especificado en el panel de control de {{site.data.keyword.ihsdbaas_mongodb_full}} </dd>
  <dt> &lt;<em>Username</em>&gt; </dt>
    <dd> Es el nombre de usuario del administrador de base de datos, tal como se ha especificado en la pantalla de creación del servicio </dd>
  <dt> &lt;<em>Password</em>&gt; </dt>
    <dd> Es la contraseña del administrador de base de datos, tal como se ha especificado en la pantalla de creación del servicio </dd>
  <dt> &lt;<em>CAFile</em>&gt; </dt>
    <dd> Es el archivo de la entidad emisora de certificados (CA) cert.pem </dd>
</dl>


### Otras herramientas
{: #accessing-database-introduction-connect-other}

Para otras herramientas, como MongoDB Compass, {{site.data.keyword.ihsdbaas_mongodb_full}} da soporte a la *validación de certificados de servidor SSL* para conectarse al host. En caso necesario, utilice el archivo de CA proporcionado.
