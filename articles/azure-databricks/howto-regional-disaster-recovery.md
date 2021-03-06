---
title: Regionale Notfallwiederherstellung für Azure Databricks
description: In diesem Artikel wird eine Möglichkeit für die Notfallwiederherstellung in Azure Databricks beschrieben.
services: azure-databricks
author: jasonwhowell
ms.author: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/27/2018
ms.openlocfilehash: 671e18346651a40d7f286e984117ce0c9ae62364
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125968"
---
# <a name="regional-disaster-recovery-for-azure-databricks-clusters"></a>Regionale Notfallwiederherstellung für Azure Databricks-Cluster

Dieser Artikel enthält Informationen zu einer praktischen Notfallwiederherstellungsarchitektur für Azure Databricks-Cluster sowie die notwendigen Schritte.

## <a name="azure-databricks-overview"></a>Übersicht über Azure Databricks

Azure Databricks ist ein schneller, einfacher und zusammenarbeitsorientierter Analysedienst auf Apache Spark-Basis. Für eine Big Data-Pipeline werden Rohdaten oder strukturierte Daten in Batches über Azure Data Factory in Azure erfasst oder mithilfe von Kafka, Event Hub oder IoT Hub nahezu in Echtzeit gestreamt. Diese Daten werden langfristig in einem Data Lake (Azure Blob Storage oder Azure Data Lake Storage) gespeichert. Mit Azure Databricks können Sie im Rahmen Ihres Analyseworkflows Daten aus mehreren Datenquellen (beispielsweise aus [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md), [Azure Data Lake Storage](../data-lake-store/index.md), [Azure Cosmos DB](../cosmos-db/index.yml) oder [Azure SQL Data Warehouse](../sql-data-warehouse/index.md)) lesen und mithilfe von Spark bahnbrechende Erkenntnisse gewinnen.

![Databricks-Pipeline](media/howto-regional-disaster-recovery/databricks-pipeline.png)

## <a name="azure-databricks-architecture"></a>Azure Databricks-Architektur

Wenn Sie über das Azure-Portal einen Azure Databricks-Arbeitsbereich erstellen, wird ganz allgemein gesagt in Ihrem Abonnement eine [verwaltete Appliance](../managed-applications/overview.md) als Azure-Ressource in der ausgewählten Azure-Region (beispielsweise „USA, Westen“) bereitgestellt. Diese Appliance wird in einem [virtuellen Azure-Netzwerk](../virtual-network/virtual-networks-overview.md) mit einer [Netzwerksicherheitsgruppe](../virtual-network/manage-network-security-group.md) und einem in Ihrem Abonnement verfügbaren Azure Storage-Konto bereitgestellt. Das virtuelle Netzwerk bietet Sicherheit auf Umgebungsebene für den Databricks-Arbeitsbereich und wird durch die Netzwerksicherheitsgruppe geschützt. Innerhalb des Arbeitsbereichs können Sie Databricks-Cluster erstellen, indem Sie den VM-Typ für Worker und Treiber sowie die Databricks-Laufzeitversion angeben. Die gespeicherten Daten steht in Ihrem Speicherkonto (Azure Blob Storage oder Azure Data Lake Store) zur Verfügung. Nach der Clustererstellung können Sie Aufträge über Notebooks, REST-APIs und ODBC/JDBC-Endpunkte ausführen, indem Sie sie an einen bestimmten Cluster anfügen.

Die Databricks-Steuerungsebene verwaltet und überwacht die Umgebung des Databricks-Arbeitsbereichs. Verwaltungsvorgänge wie etwa das Erstellen eines Clusters werden über die Steuerungsebene initiiert. Alle Metadaten (beispielsweise geplante Aufträge) werden aus Gründen der Fehlertoleranz in einer Azure-Datenbank mit Georeplikation gespeichert.

![Databricks-Architektur](media/howto-regional-disaster-recovery/databricks-architecture.png)

Diese Architektur hat unter anderem den Vorteil, dass Benutzer Azure Databricks mit jeder beliebigen Speicherressource ihres Kontos verknüpfen können. Ein weiterer entscheidender Vorteil besteht darin, dass die Computekomponenten (Azure Databricks) und die Speicherkomponente unabhängig voneinander skaliert werden können.

## <a name="how-to-create-a-regional-disaster-recovery-topology"></a>Erstellen einer Topologie für die regionale Notfallwiederherstellung

Wie Sie der obigen Architekturbeschreibung entnehmen können, umfasst eine Big Data-Pipeline mit Azure Databricks mehrere Komponenten. Dazu zählen unter anderem Azure Storage, Azure-Datenbank und andere Datenquellen. Azure Databricks ist die *Computekomponente* für die Big Data-Pipeline. Sie ist von Natur aus *kurzlebig*. Das bedeutet, Ihre Daten stehen weiterhin in Azure Storage zur Verfügung, die *Computekomponente* (Azure Databricks-Cluster) kann jedoch beendet werden, damit Sie nicht für Computefunktionen bezahlen müssen, wenn Sie sie nicht benötigen. Die *Computekomponente* (Azure Databricks) und die Speicherquellen müssen sich in der gleichen Region befinden, um lange Wartezeiten bei Aufträgen zu vermeiden.  

Wenn Sie eine eigene Topologie für die regionale Notfallwiederherstellung erstellen möchten, gehen Sie wie folgt vor:

   1. Stellen Sie mehrere Azure Databricks-Arbeitsbereiche in separaten Azure-Regionen bereit. Erstellen Sie den primären Azure Databricks-Arbeitsbereich beispielsweise in „USA, Osten 2“. Erstellen Sie den sekundären Azure Databricks-Arbeitsbereich für die Notfallwiederherstellung in einer separaten Region (beispielsweise „USA, Westen“).

   2. Verwenden Sie [georedundanten Speicher](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage). Die mit Azure Databricks verknüpften Daten werden standardmäßig in Azure Storage gespeichert. Die Ergebnisse der Databricks-Aufträge werden auch in Azure Blob Storage gespeichert, sodass die verarbeiteten Daten auch nach Beendigung des Clusters dauerhaft und hoch verfügbar sind. Da sich Speicher und Databricks-Cluster am gleichen Ort befinden, müssen Sie georedundanten Speicher verwenden, damit auf Daten in der sekundären Region zugegriffen werden kann, wenn der Zugriff auf die primäre Region nicht mehr möglich ist.

   3. Nach Erstellung der sekundären Region müssen Sie Benutzer, Benutzerordner, Notebooks, Clusterkonfiguration, Auftragskonfiguration, Bibliotheken, Speicher und Initialisierungsskripts migrieren und die Zugriffssteuerung neu konfigurieren. Ausführlichere Informationen finden Sie im folgenden Abschnitt.

## <a name="detailed-migration-steps"></a>Ausführliche Migrationsschritte

1. **Einrichten der Databricks-Befehlszeilenschnittstelle auf Ihrem Computer**

   Dieser Artikel enthält eine Reihe von Codebeispielen, in denen für die meisten der automatisierten Schritte die Befehlszeilenschnittstelle verwendet wird, da es sich dabei um einen benutzerfreundlichen Wrapper für die REST-API von Azure Databricks handelt.

   Installieren Sie zunächst die Databricks-Befehlszeilenschnittstelle auf Ihrem Desktopcomputer oder auf einem virtuellen Computer, auf dem Sie die Migrationsschritte ausführen möchten. Weitere Informationen finden Sie in der [Dokumentation zur Databricks-Befehlszeilenschnittstelle](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).

   ```bash
   pip install databricks-cli
   ```

   > [!NOTE]
   > Alle in diesem Artikel bereitgestellten Python-Skripts sollten für Python 2.7 < 3.x geeignet sein.

2. **Konfigurieren zweier Profile**

   Konfigurieren Sie ein Profil für den primären und ein Profil für den sekundären Arbeitsbereich:

   ```bash
   databricks configure --profile primary
   databricks configure --profile secondary
   ```

   Die Codeblöcke in diesem Artikel wechseln in jedem Folgeschritt mithilfe des entsprechenden Arbeitsbereichsbefehls zwischen den Profilen. Achten Sie darauf, die Namen der von Ihnen erstellten Profile in den einzelnen Codeblocks zu ersetzen.

   ```python
   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"
   ```

   Sie können bei Bedarf manuell über die Befehlszeile wechseln:

   ```bash
   databricks workspace ls --profile primary
   databricks workspace ls --profile secondary
   ```

3. **Migrieren von Azure Active Directory-Benutzern**

   Fügen Sie die Azure Active Directory-Benutzer aus dem primären Arbeitsbereich manuell dem sekundären Arbeitsbereich hinzu.

4. **Migrieren der Benutzerordner und Notebooks**

   Verwenden Sie den folgenden Python-Code, um die Sandbox-Benutzerumgebungen einschließlich der benutzerspezifischen geschachtelten Ordnerstruktur und Notebooks zu migrieren.

   > [!NOTE]
   > Bibliotheken werden in diesem Schritt nicht kopiert, da sie von der zugrunde liegenden API nicht unterstützt werden.

   Kopieren Sie das folgende Python-Skript, speichern Sie es in einer Datei, und führen Sie sie über die Databricks-Befehlszeile aus. Beispiel: `python scriptname.py`.

   ```python
   from subprocess import call, check_output

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get a list of all users
   user_list_out = check_output(["databricks", "workspace", "ls", "/Users", "--profile", EXPORT_PROFILE])
   user_list = user_list_out.splitlines()

   # Export sandboxed environment (folders, notebooks) for each user and import into new workspace.
   # Libraries are not included with these APIs / commands.

   for user in user_list:
     print "Trying to migrate workspace for user " + user

     call("mkdir -p " + user, shell=True)
     export_exit_status = call("databricks workspace export_dir /Users/" + user + " ./" + user + " --profile " + EXPORT_PROFILE, shell=True)

     if export_exit_status==0:
       print "Export Success"
       import_exit_status = call("databricks workspace import_dir ./" + user + " /Users/" + user + " --profile " + IMPORT_PROFILE, shell=True)
       if import_exit_status==0:
         print "Import Success"
       else:
         print "Import Failure"
     else:
       print "Export Failure"

   print "All done"
   ```

5. **Migrieren der Clusterkonfigurationen**

   Nach der Notebookmigration können Sie optional die Clusterkonfigurationen zum neuen Arbeitsbereich migrieren. Hierbei handelt es sich um einen nahezu vollständig automatisierten Schritt über die Databricks-Befehlszeilenschnittstelle (es sei denn, Sie möchten nur bestimmte Clusterkonfigurationen migrieren).

   > [!NOTE]
   > Leider gibt es keinen Endpunkt für die Clusterkonfigurationserstellung, und dieses Skript versucht, jeden Cluster sofort zu erstellen. Sollten in Ihrem Abonnement nicht genügend Kerne zur Verfügung stehen, ist die Clustererstellung unter Umständen nicht erfolgreich. Der Fehler kann ignoriert werden, solange die Konfiguration erfolgreich übertragen wurde.

   Das folgende Skript gibt eine Zuordnung zwischen alten und neuen Cluster-IDs aus, die später für die Auftragsmigration verwendet werden kann (für Aufträge, die zur Verwendung vorhandener Cluster konfiguriert sind).

   Kopieren Sie das folgende Python-Skript, speichern Sie es in einer Datei, und führen Sie sie über die Databricks-Befehlszeile aus. Beispiel: `python scriptname.py`.

   ```python
   from subprocess import call, check_output import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Get all clusters info from old workspace 
   clusters_out = check_output(["databricks", "clusters", "list", "--profile", EXPORT_PROFILE]) clusters_info_list = clusters_out.splitlines()

   # Create a list of all cluster ids 
   clusters_list = [] for cluster_info in clusters_info_list:   clusters_list.append(cluster_info.split(None, 1)[0])

   # Optionally filter cluster ids out manually, so as to create only required ones in new workspace

   # Create a list of mandatory / optional create request elements 
   cluster_req_elems = ["num_workers","autoscale","cluster_name","spark_version","spark_conf"," node_type_id","driver_node_type_id","custom_tags","cluster_log_conf","sp ark_env_vars","autotermination_minutes","enable_elastic_disk"]

   # Try creating all / selected clusters in new workspace with same config as in old one.
   cluster_old_new_mappings = {} for cluster in clusters_list:   print "Trying to migrate cluster " + cluster

   cluster_get_out = check_output(["databricks", "clusters", "get", "--cluster-id", cluster, "--profile", EXPORT_PROFILE])
   print "Got cluster config from old workspace"

   # Remove extra content from the config, as we need to build create request with allowed elements only
   cluster_req_json = json.loads(cluster_get_out)    
   cluster_json_keys = cluster_req_json.keys()   

   for key in cluster_json_keys:     
      if key not in cluster_req_elems:       
         cluster_req_json.pop(key, None)
  
   # Create the cluster, and store the mapping from old to new cluster ids
   cluster_create_out = check_output(["databricks", "clusters", "create", "--json", json.dumps(cluster_req_json), "--profile", IMPORT_PROFILE]) 
   cluster_create_out_json = json.loads(cluster_create_out)   
   cluster_old_new_mappings[cluster] = cluster_create_out_json['cluster_id']

   print "Sent cluster create request to new workspace successfully"

   print "Cluster mappings: " + json.dumps(cluster_old_new_mappings)
   print "All done"
   ```

6. **Migrieren der Auftragskonfiguration**

   Wenn Sie im vorherigen Schritt Clusterkonfigurationen migriert haben, können Sie optional auch Auftragskonfigurationen zum neuen Arbeitsbereich migrieren. Hierbei handelt es sich um einen vollständig automatisierten Schritt über die Databricks-Befehlszeilenschnittstelle (es sei denn, Sie möchten nur bestimmte Auftragskonfigurationen migrieren).

   > [!NOTE]
   > Die Konfiguration für einen geplanten Auftrag enthält auch die Zeitplaninformationen, sodass standardmäßig der Auftrag nach der Migration gemäß dem konfigurierten Zeitplan ausgeführt wird. Der folgende Codeblock entfernt daher bei der Migration alle Zeitplaninformationen, um doppelte Ausführungen in alten und neuen Arbeitsbereichen zu vermeiden. Konfigurieren Sie die Zeitpläne für Aufträge dieser Art, wenn Sie für die Übernahme bereit sind.

   Die Auftragskonfiguration erfordert Einstellungen für einen neuen oder bereits vorhandenen Cluster. Bei Verwendung eines bereits vorhandenen Clusters versucht das folgende Skript bzw. der folgende Code, die alte Cluster-ID durch eine neue Cluster-ID zu ersetzen.

   Kopieren Sie das folgende Python-Skript, und speichern Sie es in einer Datei. Ersetzen Sie den Wert für `old_cluster_id` und `new_cluster_id` durch die Ausgabe der Clustermigration aus dem vorherigen Schritt. Führen Sie es über die Befehlszeile der Databricks-Befehlszeilenschnittstelle aus (Beispiel: `python scriptname.py`).

   ```python
   from subprocess import call, check_output
   import json

   EXPORT_PROFILE = "primary"
   IMPORT_PROFILE = "secondary"

   # Please replace the old to new cluster id mappings from cluster migration output
   cluster_old_new_mappings = {"old_cluster_id": "new_cluster_id"}

   # Get all jobs info from old workspace
   try:
     jobs_out = check_output(["databricks", "jobs", "list", "--profile", EXPORT_PROFILE])
     jobs_info_list = jobs_out.splitlines()
   except:
     print "No jobs to migrate"
     sys.exit(0)

   # Create a list of all job ids
   jobs_list = []
   for jobs_info in jobs_info_list:
     jobs_list.append(jobs_info.split(None, 1)[0])

   # Optionally filter job ids out manually, so as to create only required ones in new workspace

   # Create each job in the new workspace based on corresponding settings in the old workspace

   for job in jobs_list:
     print "Trying to migrate " + job

     job_get_out = check_output(["databricks", "jobs", "get", "--job-id", job, "--profile", EXPORT_PROFILE])
     print "Got job config from old workspace"

     job_req_json = json.loads(job_get_out)  
     job_req_settings_json = job_req_json['settings']

     # Remove schedule information so job doesn't start before proper cutover
     job_req_settings_json.pop('schedule', None)

     # Replace old cluster id with new cluster id, if job configured to run against an existing cluster
     if 'existing_cluster_id' in job_req_settings_json:
       if job_req_settings_json['existing_cluster_id'] in cluster_old_new_mappings:
         job_req_settings_json['existing_cluster_id'] = cluster_old_new_mappings[job_req_settings_json['existing_cluster_id']]
       else:
         print "Mapping not available for old cluster id " + job_req_settings_json['existing_cluster_id']
         continue

     call(["databricks", "jobs", "create", "--json", json.dumps(job_req_settings_json), "--profile", IMPORT_PROFILE])
     print "Sent job create request to new workspace successfully"

   print "All done"
   ```

7. **Migrieren der Bibliotheken**

   Bibliotheken können derzeit nicht ohne Weiteres zwischen Arbeitsbereichen migriert werden. Sie müssen stattdessen manuell im neuen Arbeitsbereich installiert werden. Es ist möglich, den Vorgang durch eine Kombination aus [DBFS-CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples) (zum Hochladen von benutzerdefinierten Bibliotheken in den Arbeitsbereich) und [Bibliotheken-CLI](https://github.com/databricks/databricks-cli#libraries-cli) zu automatisieren.

8. **Migrieren von Azure-Blobspeicher- und Azure Data Lake Store-Einbindungen**

   Binden Sie alle [Azure-Blobspeicher](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)- und [Azure Data Lake Store (Gen 1)](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)-Bereitstellungspunkte mithilfe einer notebookbasierten Lösung ein. Die Speicherressourcen waren im primären Arbeitsbereich bereitgestellt, und dies muss im sekundären Arbeitsbereich wiederholt werden. Für Einbindungen steht keine externe API zur Verfügung.

9. **Migrieren von Clusterinitialisierungsskripts**

   Clusterinitialisierungsskripts können mithilfe der [DBFS-CLI](https://github.com/databricks/databricks-cli#dbfs-cli-examples) vom alten zum neuen Arbeitsbereich migriert werden. Kopieren Sie zunächst die benötigten Skripts von `dbfs:/dat abricks/init/..` auf Ihren lokalen Desktopcomputer oder auf Ihren virtuellen Computer. Kopieren Sie die Skripts anschließend an den gleichen Pfad im neuen Arbeitsbereich.

   ```bash
   // Primary to local
   dbfs cp -r dbfs:/databricks/init ./old-ws-init-scripts --profile primary

   // Local to Secondary workspace
   dbfs cp -r old-ws-init-scripts dbfs:/databricks/init --profile secondary
   ```

10. **Manuelles Konfigurieren und Anwenden der Zugriffssteuerung**

   Wenn Ihr vorhandener primärer Arbeitsbereich für die Verwendung des Premium-Tarifs (SKU) konfiguriert ist, verwenden Sie wahrscheinlich auch das [Zugriffssteuerungsfeature](https://docs.azuredatabricks.net/administration-guide/admin-settings/index.html#manage-access-control).

   Wenn Sie das Zugriffssteuerungsfeature verwenden, wenden Sie die Zugriffssteuerung manuell auf die Ressourcen (Notebooks, Cluster, Aufträge, Tabellen) an.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in der [Dokumentation zu Azure Databricks](https://docs.azuredatabricks.net/user-guide/index.html).
