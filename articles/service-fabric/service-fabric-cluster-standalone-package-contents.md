---
title: Eigenständiges Service Fabric-Paket für Windows Server | Microsoft Docs
description: Beschreibung und Inhalt des eigenständigen Azure Service Fabric-Pakets für Windows Server.
services: service-fabric
documentationcenter: .net
author: maburlik
manager: timlt
editor: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: maburlik
ms.openlocfilehash: dccdd6518dd97299150892a5629809ea7f708838
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209354"
---
# <a name="contents-of-service-fabric-standalone-package-for-windows-server"></a>Inhalt des eigenständigen Service Fabric-Pakets für Windows Server
Sie finden im [heruntergeladenen](http://go.microsoft.com/fwlink/?LinkId=730690), eigenständigen Service Fabric-Paket die folgenden Dateien:

| **Dateiname** | **Kurzbeschreibung** |
| --- | --- |
| CreateServiceFabricCluster.ps1 |Ein PowerShell-Skript, das den Cluster mit den Einstellungen in „ClusterConfig.json“ erstellt |
| RemoveServiceFabricCluster.ps1 |Ein PowerShell-Skript, das einen Cluster mit den Einstellungen in „ClusterConfig.json“ entfernt |
| AddNode.ps1 |PowerShell-Skript zum Hinzufügen eines Knotens zu einem vorhandenen, bereitgestellten Cluster auf dem aktuellen Computer |
| RemoveNode.ps1 |PowerShell-Skript zum Entfernen eines Knotens aus einem vorhandenen, bereitgestellten Cluster auf dem aktuellen Computer |
| CleanFabric.ps1 |PowerShell-Skript zum Entfernen einer eigenständigen Service Fabric-Installation vom aktuellen Computer. Vorherige MSI-Installationen müssen jeweils mit ihrem dazugehörigen Deinstallationsprogramm entfernt werden. |
| TestConfiguration.ps1 |PowerShell-Skript zum Analysieren der Infrastruktur auf der Grundlage von „cluster.json“. |
| DownloadServiceFabricRuntimePackage.ps1 |PowerShell-Skript zum Herunterladen des neuesten Runtimepakets für Szenarien, in denen der Bereitstellungscomputer nicht mit dem Internet verbunden ist |
| DeploymentComponentsAutoextractor.exe |Selbstextrahierendes Archiv mit Bereitstellungskomponenten für die Skripts des eigenständigen Pakets |
| EULA_ENU.txt |Die Lizenzbedingungen für die Verwendung des eigenständigen Windows Server-Pakets für Microsoft Azure Service Fabric. Sie können jetzt [eine Kopie des Endbenutzer-Lizenzvertrags herunterladen](http://go.microsoft.com/fwlink/?LinkID=733084). |
| Readme.txt |Link zu Versionshinweisen und grundlegenden Installationsanweisungen. Dies ist eine Teilmenge der Anweisungen in diesem Dokument. |
| ThirdPartyNotice.rtf |Hinweis zu im Paket enthaltener Drittanbietersoftware. |
| Tools\Microsoft.Azure.ServiceFabric.WindowsServer.SupportPackage.zip |Die Datei „StandaloneLogCollector.exe“ wird bei Bedarf ausgeführt, um Ablaufverfolgungsprotokolle zu sammeln und für Supportzwecke an Microsoft hochzuladen. |
| Tools\ServiceFabricUpdateService.zip |Ein Tool zum Aktivieren des automatischen Codeupgrades für Cluster, die nicht über Internetzugriff verfügen. Weitere Informationen finden Sie [hier](service-fabric-cluster-upgrade-windows-server.md)|

**Vorlagen** 
| **Dateiname** | **Kurzbeschreibung** |
| --- | --- |
| ClusterConfig.Unsecure.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. |
| ClusterConfig.Unsecure.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die die Einstellungen für einen ungeschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Computer im Cluster. |
| ClusterConfig.Windows.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt. |
| ClusterConfig.Windows.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen sicheren Cluster mit mehreren virtuellen oder physischen Computern und Windows-Sicherheit enthält, einschließlich der Informationen für jeden Computer, der Teil des geschützten Clusters ist. Der Cluster wird mithilfe von [Windows-Identitäten](https://msdn.microsoft.com/library/ff649396.aspx) geschützt. |
| ClusterConfig.x509.DevCluster.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für einen geschützten Entwicklungscluster mit einem einzelnen virtuellen oder physischen Computer und drei Knoten enthält, einschließlich der Informationen für jeden Knoten im Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt. |
| ClusterConfig.x509.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für den geschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Knoten im geschützten Cluster. Der Cluster wird mittels X.509-Zertifikaten geschützt. |
| ClusterConfig.gMSA.Windows.MultiMachine.json |Beispiel für eine Clusterkonfigurationsdatei, die alle Einstellungen für den geschützten Cluster mit mehreren virtuellen oder physischen Computern enthält, einschließlich der Informationen für jeden Knoten im geschützten Cluster. Der Cluster wird über [gruppenverwalteten Dienstkonten](https://technet.microsoft.com/library/jj128431(v=ws.11).aspx) geschützt. |

## <a name="cluster-configuration-samples"></a>Clusterkonfigurationsbeispiele
Die neuesten Versionen der Clusterkonfigurationsvorlagen finden Sie auf der GitHub-Seite mit [Beispielen für eigenständige Clusterkonfigurationen](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).

## <a name="independent-runtime-package"></a>Unabhängiges Laufzeitpaket
Das aktuelle Laufzeitpaket wird während der Clusterbereitstellung automatisch von [Downloadlink – Service Fabric-Runtime – Windows Server](https://go.microsoft.com/fwlink/?linkid=839354) heruntergeladen.

## <a name="related"></a>Verwandte Themen
* [Erstellen eines eigenständigen Azure Service Fabric-Clusters](service-fabric-cluster-creation-for-windows-server.md)
* [Szenarien für die Service Fabric-Clustersicherheit](service-fabric-windows-cluster-windows-security.md)
