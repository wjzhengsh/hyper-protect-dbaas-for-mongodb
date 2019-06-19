---

copyright:
  years: 2019
lastupdated: "2019-06-06"

keywords: Activity tracker events

subcollection: hyper-protect-dbaas-for-mongodb

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.cloudaccesstrailshort}}-Ereignisse
{: #activity-tracker-events}

Überwachen Sie mit dem {{site.data.keyword.cloudaccesstrailfull}}-Service die Interaktion von Benutzern und Anwendungen mit {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}.
{: shortdesc}

Der {{site.data.keyword.cloudaccesstrailfull_notm}}-Service zeichnet vom Benutzer initiierte Aktivitäten auf, die den Status eines Service in {{site.data.keyword.cloud_notm}} ändern. Weitere Informationen finden Sie in der [Dokumentation zu {{site.data.keyword.cloudaccesstrailshort}}](/docs/services/Activity-Tracker-with-LogDNA?topic=logdnaat-getting-started).

## Liste der Ereignisse
{: #list-activity-tracker-events}

In der folgenden Tabelle sind die Aktionen aufgelistet, die ein Ereignis generieren:

| Aktion                 | Beschreibung                               |
| ---------------------- | ----------------------------------------- |
| `cluster.create` | Erstellen eines Datenbankclusters                 |
| `cluster.delete` | Löschen eines Datenbankclusters                 |
| `database.create` | Erstellen einer Datenbank                  |
| `database.delete` | Löschen einer Datenbank                  |
| `user.create`     | Erstellen eines Datenbankbenutzers                    |
| `user.delete`     | Löschen eines Datenbankbenutzers                    |
| `instance.start` | Starten einer Datenbankserviceinstanz         |
| `instance.stop`  | Stoppen einer Datenbankserviceinstanz          |
| `instance.restart`  | Neustart einer Datenbankserviceinstanz          |
| `log.get`       | Herunterladen einer Protokolldatei |
{: caption="Tabelle 1. Aktionen, die {{site.data.keyword.cloudaccesstrailfull_notm}}-Ereignisse generieren"}

Für die Ereignisse `cluster.create` und `cluster.delete` zeichnet der {{site.data.keyword.cloudaccesstrailfull_notm}}-Service den Kontonamen und die IP-Adresse des Benutzers, der die Aktion ausführt, nicht auf. Die Werte für `initiator.name` und `host.address` im Protokoll geben die Service-ID und die IP-Adresse von Cloud Broker an.
{: note}

## Wo werden die Ereignisse angezeigt
{: #view-activity-tracker-events}

<!-- Option 2: Add the following sentence if your service sends events to the account domain. -->

{{site.data.keyword.cloudaccesstrailshort}}-Ereignisse sind in der {{site.data.keyword.cloudaccesstrailshort}}-**Kontodomäne** in der {{site.data.keyword.cloud_notm}}-Region verfügbar ist, in der die Ereignisse generiert werden.
