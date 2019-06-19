---

copyright:
  years: 2019
lastupdated: "2019-06-10"

keywords: DBaaS CLI, Python runtime

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}


# DBaaS-CLI-Plug-in installieren
{: #install-dbaas-cli-plugin}

Um bei der Verwendung der {{site.data.keyword.cloud}}-CLI auf eine umfassende Gruppe von DBaaS-Befehlen zugreifen zu können,
müssen Sie die folgenden Komponenten installieren
(Näheres hierzu finden Sie im Abschnitt mit den entsprechenden [Anweisungen](#dbaas_cli_instr)):

- Python-Laufzeitumgebung (2.7.15 wird empfohlen; 3.x wird **nicht** unterstützt)
- Python Pip-Paketmanagementsystem
- Python-Anforderungsbibliothek
- DBaaS-CLI-Plug-in

## Anweisungen zum Installieren der DBaaS-CLI-Komponenten (nach Betriebssystem)
{: #dbaas_cli_instr}

- [Unter Linux](#dbaas_cli_linux)
- [Unter MacOS](#dbaas_cli_macos)
- [Unter Windows](#dbaas_cli_windows)

### Unter Linux:
{: #dbaas_cli_linux}

1. Bei den meisten Linux-Distributionen sind Python und Pip vorinstalliert. Ist dies bei Ihnen nicht der Fall, verwenden Sie die folgenden Befehle, um sie zu installieren:

   ```javascript
   sudo apt-get install python python2.7 python-dev
   sudo apt-get install python-pip
   ```
   {: codeblock}

2. Verwenden Sie den folgenden Befehl, um die Python-Anforderungsbibliothek zu installieren:

   ```javascript
   sudo pip install requests
   ```
   {: codeblock}

3. Verwenden Sie den folgenden Befehl, um das DBaaS-CLI-Plug-in zu installieren:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

4. Verwenden Sie den folgenden Befehl, um zu bestätigen, dass das DBaaS-CLI-Plug-in ordnungsgemäß installiert wurde:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   Das System zeigt `dbaas` in der Liste der verfügbaren Befehle an.

### Unter Mac OS:
{: #dbaas_cli_macos}

1. Wenn Sie noch nicht über Python verfügen, laden Sie die Software anhand der folgenden Anweisungen herunter und installieren Sie sie:

    a. Rufen Sie die Python-Website unter der folgenden URL auf: https://www.python.org/downloads/release/python-2715/.

    b. Wählen Sie das entsprechende Mac OS-Installationsprogramm aus, laden Sie es herunter und führen Sie es aus. Die Installation sollte Pip enthalten.

2. Verwenden Sie den folgenden Befehl, um die Python-Anforderungsbibliothek zu installieren:

   ```javascript
   sudo pip install requests
   ```
   {: codeblock}

3. Verwenden Sie den folgenden Befehl, um das DBaaS-CLI-Plug-in zu installieren:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

4. Verwenden Sie den folgenden Befehl, um zu bestätigen, dass das DBaaS-CLI-Plug-in ordnungsgemäß installiert wurde:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   Das System zeigt `dbaas` in der Liste der verfügbaren Befehle an.

### Unter Windows:
{: #dbaas_cli_windows}

**Hinweis:** Es wird nur die x86_64-Version von Windows unterstützt.

1. Wenn Sie noch nicht über Python verfügen, laden Sie die Software anhand der folgenden Anweisungen herunter und installieren Sie sie:

    a. Rufen Sie die Python-Website unter der folgenden URL auf: https://www.python.org/downloads/release/python-2715/.

    b. Wählen Sie das Installationsprogramm **Windows x86-64 MSI** aus, laden Sie es herunter und führen Sie es aus.

    c. Geben Sie im Menü zum **Anpassen von Python** des **Python-Setup**-Assistenten an, dass **Pip auf dem lokalen Festplattenlaufwerk installiert wird**.

2. Nachdem Sie Python installiert haben, hängen Sie über das Windows-Dialogfeld **Systemvariable bearbeiten** die
   folgenden Werte an den Wert der vorhandenen **Path**-Variable an:

   ```javascript
   C:\Python27;C:\Python27\Scripts
   ```
   {: codeblock}

3. Um die Python-Anforderungsbibliothek zu installieren, öffnen Sie eine **Eingabeaufforderung** und geben Sie den folgenden Befehl ein:

   ```javascript
   pip install requests
   ```
   {: codeblock}

4. Verwenden Sie den folgenden Befehl, um das DBaaS-CLI-Plug-in zu installieren:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

5. Verwenden Sie den folgenden Befehl, um zu bestätigen, dass das DBaaS-CLI-Plug-in ordnungsgemäß installiert wurde:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   Das System zeigt `dbaas` in der Liste der verfügbaren Befehle an.

Detaillierte Informationen zu Befehlen des DBaaS-Befehlszeilenschnittstellen-Plug-ins finden Sie in den [Referenzinformationen zur DBaaS-Befehlszeilenschnittstelle](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin).
