---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: access token, DBaaS Manager APIs, API key

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}


# Authentifizierung für die Verwendung von DBaaS-Manager-APIs konfigurieren
{: #api-auth}

Für die Authentifizierung benötigen Sie einen API-Schlüssel, ein Zugriffstoken und eine Benutzer-ID für das Ausgeben von API-Anforderungen.
Befolgen Sie hierfür die folgenden Anweisungen:

1. Generieren Sie einen API-Schlüssel:

   a. Wählen Sie im {{site.data.keyword.cloud}}-Dashboard **Verwalten > Access (IAM)** aus.

      Das Access-Dashboard (IAM) wird angezeigt.

   b. Wählen Sie in der seitlichen Navigationsleiste des Access-Dashboards (IAM) die Option **{{site.data.keyword.cloud_notm}}-API-Schlüssel** aus.

   c. Klicken Sie auf **{{site.data.keyword.cloud_notm}}-API-Schlüssel erstellen**.

      Das Dialogfeld 'API-Schlüssel erstellen' wird angezeigt.

   d. Geben Sie im Dialogfeld 'API-Schlüssel erstellen' einen Namen und eine Beschreibung für den API-Schlüssel ein und klicken Sie auf **Erstellen**.

   e. Klicken Sie auf **Herunterladen**, um den API-Schlüssel herunterzuladen und zu speichern, oder klicken Sie auf **Kopieren**, um den API-Schlüssel in die Zwischenablage zu kopieren.

      Diese Operation gibt eine JSON-Datei ähnlich dem folgenden Beispiel zurück:

      ```
      {
       "name": "dbaasmgr",
       "description": "DBaaS Manager",
       "createdAt": "201...",
       "apiKey": "**Pt...Y**"
      }
       ```
      {: codeblock}

      Der Wert, der `apiKey` zugewiesen ist, ist Ihr API-Schlüssel. **Wichtig:** Notieren Sie sich diesen Wert, bevor Sie das Fenster schließen.

2. Um eine sichere Datenübertragung zu gewährleisten, rufen Sie eine CA-Datei (CA = Certificate Authority, Zertifizierungsstelle) vom {{site.data.keyword.ihsdbaas_mongodb_full}}-Dashboard ab und kopieren Sie diese in ein geeignetes Verzeichnis, beispielsweise **/etc/ssl/certs/**.

3. Rufen Sie ein Zugriffstoken und eine Benutzer-ID mit der GET-Operation '/auth/token' ab:

    ```curl
    curl -X GET -H "accept: application/json" -H "api_key: icGVY1*** ***UdfcIg4kzE" https://dbaas900.hyperp-dbaas.cloud.ibm.com:20000/api/v1/auth/token
    ```
    {: pre}

    Diese Operation gibt ein Zugriffstoken und eine Benutzer-ID ähnlich dem folgenden Beispiel zurück:

    ```
    {
     "access_token":"eyJraWQiOiIyMDE3MT*** ***3V4pMYrOvMniLA",
     "user_id":"e9433*** ***b188"
    }
    ```
    {: codeblock}

4. Um sie zu einem späteren Zeitpunkt verwenden zu können, speichern Sie die Werte für das Zugriffstoken und die Benutzer-ID.
