---

copyright:
  years: 2019
lastupdated: "2019-06-13"

keywords: database monitoring, database cluster, database metrics

subcollection: hyper-protect-dbaas-for-mongodb

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}


# Datenbanküberwachung
{: #dbaas-webui-database-monitor}

Nachdem Sie die Datenbanküberwachung aktiviert haben, können Sie die Datenbankmesswerte mithilfe von Grafana anzeigen.
{: shortdesc}

## Vorbereitende Schritte
{: #webui-database-monitoring-byb}

1.  Stellen Sie sicher, dass Sie über Zugriff auf eine Cloud Foundry-Organisation und einen Bereich in Dallas (us-south) verfügen.
    Informationen darüber, wie Sie einen solchen Zugriff erhalten, finden Sie in [Cloud Foundry-Zugriff verwalten](https://cloud.ibm.com/docs/iam?topic=iam-mngcf#mngcf){: external}.

2.  Stellen Sie sicher, dass alle Instanzen des Datenbankclusters aktiv sind.

3.  Wählen Sie im {{site.data.keyword.cloud_notm}} {{site.data.keyword.ihsdbaas_mongodb_full}}-Dashboard die Registerkarte 'Überwachung' aus.

## Datenbanküberwachung aktivieren
{: #webui-enable-database-monitoring}

Falls auf der Registerkarte 'Überwachung' die Nachricht **Überwachung inaktiviert** angezeigt wird, gehen Sie wie folgt vor:

1. Klicken Sie auf **Aktivieren**.
2. Wählen Sie im Fenster 'Überwachung aktivieren' die gewünschte Organisation und den Bereich aus und klicken Sie auf **Übergeben**.


## Datenbankmetriken anzeigen
{: #webui-view-database-metrics}

Es gibt zwei Möglichkeiten, die Metriken in Grafana anzuzeigen:

- Kopieren Sie den angezeigten Link, öffnen Sie dann eine neue Registerkarte oder ein neues Fenster und fügen Sie den Link in den Browser ein.
- Klicken Sie auf **In Grafana anzeigen**.

Wenn Sie die Metriken in einem neuen Dashboard in Grafana anzeigen möchten, wählen Sie das Grafanasymbol oben links und anschließend **Dashboards > Neu** aus.
Es kann eine Weile dauern, bis die Datenbankcluster-ID und die Instanz-IDs angezeigt werden; Sie müssen das Dashboard möglicherweise erneut laden.

Weitere Informationen zur Verwendung von Grafana finden Sie in [{{site.data.keyword.cloud_notm}}-Überwachung](/docs/services/cloud-monitoring?topic=cloud-monitoring-getting-started).
