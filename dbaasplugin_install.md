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


# Installing the DBaaS CLI plug-in
{: #install-dbaas-cli-plugin}

To access a complete set of DBaaS commands when using the {{site.data.keyword.cloud}} CLI, you must install the following components (see [Instructions](#dbaas_cli_instr)):

- Python runtime (2.7.15 is recommended; 3.x is **not** supported)
- Python Pip package management system
- Python Requests library
- DBaaS CLI plug-in

## Instructions for installing the DBaaS CLI components (by operating system)
{: #dbaas_cli_instr}

- [On Linux](#dbaas_cli_linux)
- [On MacOS](#dbaas_cli_macos)
- [On Windows](#dbaas_cli_windows)

### On Linux:
{: #dbaas_cli_linux}

1. Most Linux distributions have Python and Pip preinstalled. If yours does not, use these commands to install them:

   ```javascript
   sudo apt-get install python python2.7 python-dev
   sudo apt-get install python-pip
   ```
   {: codeblock}

2. To install the Python Requests library, use this command:

   ```javascript
   sudo pip install requests
   ```
   {: codeblock}

3. To install the DBaaS CLI plug-in, use this command:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

4. To confirm that the DBaaS CLI plug-in has been installed correctly, use this command:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   The system displays `dbaas` in the list of available commands.

### On MacOS:
{: #dbaas_cli_macos}

1. If you do not already have Python, download and install it by following these instructions:

    a. Go to the Python website at this URL: https://www.python.org/downloads/release/python-2715/

    b. Select the appropriate MacOS installer, download it, and run it. The installation should include Pip.

2. To install the Python Requests library, use this command:

   ```javascript
   sudo pip install requests
   ```
   {: codeblock}

3. To install the DBaaS CLI plug-in, use this command:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

4. To confirm that the DBaaS CLI plug-in has been installed correctly, use this command:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   The system displays `dbaas` in the list of available commands.

### On Windows:
{: #dbaas_cli_windows}

**Note:** Only the x86_64 version of Windows is supported.

1. If you do not already have Python, download and install it by following these instructions:

    a. Go to the Python website at this URL: https://www.python.org/downloads/release/python-2715/

    b. Select the **Windows x86-64 MSI** installer, download it, and run it.

    c. On the **Customize Python** menu of the **Python Setup** wizard, specify: **pip Will be installed on local hard disk drive**

2. After installing Python, use your Windows **Edit System Variable** dialog to
   append the following values to the existing **Path** variable value:

   ```javascript
   C:\Python27;C:\Python27\Scripts
   ```
   {: codeblock}

3. To install the Python Requests library, open the **Command Prompt** window and enter this command:

   ```javascript
   pip install requests
   ```
   {: codeblock}

4. To install the DBaaS CLI plug-in, use this command:

   ```javascript
   ibmcloud plugin install dbaas-cli
   ```
   {: codeblock}

5. To confirm that the DBaaS CLI plug-in has been installed correctly, use this command:

   ```javascript
   ibmcloud
   ```
   {: codeblock}

   The system displays `dbaas` in the list of available commands.

For detailed information about DBaaS CLI plug-in commands, you can refer to [DBaaS CLI reference](/docs/services/hyper-protect-dbaas-for-mongodb?topic=hyper-protect-dbaas-for-mongodb-dbaas_cli_plugin).
