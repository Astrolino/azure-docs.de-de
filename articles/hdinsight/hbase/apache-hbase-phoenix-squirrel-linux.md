---
title: Verwenden von Apache Phoenix und SQLLine mit HBase in Azure HDInsight
description: Informationen zum Verwenden von Apache Phoenix in HDInsight. Darüber hinaus erhalten Sie Informationen zum Installieren und Einrichten von SQLLine für die Verbindung mit einem HBase-Cluster in HDInsight auf Ihrem Computer.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: jasonh
ms.openlocfilehash: 92e6dcf7bf467bac31798df994e7efa8da40f035
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045657"
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Verwenden von Apache Phoenix mit Linux-basierten HBase-Clustern in HDInsight
Erfahren Sie, wie Sie [Apache Phoenix](http://phoenix.apache.org/) in Azure HDInsight und wie Sie SQLLine verwenden können. Weitere Informationen zu Phoenix finden Sie unter [Phoenix innerhalb von 15 Minuten oder weniger](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Eine Beschreibung der Phoenix-Grammatik finden Sie unter [Phoenix Grammar](http://phoenix.apache.org/language/index.html) (Phoenix-Grammatik).

> [!NOTE]
> Versionshinweise für Phoenix finden Sie in HDInsight unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>Verwendung von SQLLine
[SQLLine](http://sqlline.sourceforge.net/) ist ein Befehlszeilenprogramm zum Ausführen von SQL.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie SQLLine verwenden können, benötigen Sie folgende Elemente:

* **Ein HBase-Cluster in HDInsight**. Eine Beschreibung der Erstellung finden Sie unter [Erste Schritte mit Apache HBase in HDInsight](./apache-hbase-tutorial-get-started-linux.md).

Beim Herstellen der Verbindung mit einem HBase-Cluster müssen Sie die Verbindung mit einer der Zookeeper-VMs herstellen. Jeder HDInsight-Cluster verfügt über drei Zookeeper-VMs.

**So rufen Sie den Hostnamen des Zookeepers ab**

1. Öffnen Sie Ambari, indem Sie zu **https://\<Clustername\>.azurehdinsight.net** navigieren.
2. Geben Sie zum Anmelden den HTTP-Benutzernamen (Clusternamen) und das Kennwort ein.
3. Wählen Sie im Menü auf der linken Seite die Option **Zookeeper** aus. Drei **Zookeeper Server**-Instanzen sind aufgeführt.
4. Wählen Sie eine der **Zookeeper Server**-Instanzen aus. Suchen Sie im Bereich **Summary** (Zusammenfassung) den **Hostname** (Hostnamen). Er entspricht *zk1-jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**Verwendung von SQLLine**

1. Die Verbindungsherstellung mit dem Cluster erfolgt über SSH. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Führen Sie in SSH die folgenden Befehle aus, um SQLLine auszuführen:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Um eine HBase-Tabelle zu erstellen und Daten einzufügen, führen Sie die folgenden Befehle aus:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Weitere Informationen finden Sie im [SQLLine-Handbuch](http://sqlline.sourceforge.net/#manual) sowie unter [Phoenix Grammar](http://phoenix.apache.org/language/index.html) (Phoenix-Grammatik).

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie Apache Phoenix in HDInsight verwenden. Weitere Informationen finden Sie in diesen Artikeln:

* [Übersicht über HDInsight HBase][hdinsight-hbase-overview].
  HBase ist eine Open Source-NoSQL-Datenbank von Apache, die auf Hadoop basiert und wahlfreien Zugriff und starke Konsistenz für große Mengen unstrukturierter und teilstrukturierter Daten bietet.
* [Erstellen von HBase-Clustern in HDInsight in Azure Virtual Network][hdinsight-hbase-provision-vnet].
  Mit der Integration in virtuelle Netzwerke können HBase-Cluster im selben virtuellen Netzwerk bereitgestellt werden wie Ihre Anwendungen, sodass Anwendungen direkt mit HBase kommunizieren können.
* [Konfigurieren der HBase-Replikation in HDInsight](apache-hbase-replication.md). Erfahren Sie, wie Sie HBase-Replikation zwischen zwei Azure-Rechenzentren einrichten.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md


