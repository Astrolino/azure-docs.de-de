---
title: Entfernen des MySQL-Ressourcenanbieters in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den MySQL-Ressourcenanbieter aus Ihrer Azure Stack-Bereitstellung entfernen können.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: dcd1c40717cb35fe4daa9ab9e2c66f334ffff5fe
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361497"
---
# <a name="remove-the-mysql-resource-provider"></a>Entfernen des MySQL-Ressourcenanbieters

Bevor Sie den MySQL-Ressourcenanbieter entfernen, müssen Sie alle Anbieterabhängigkeiten entfernen. Außerdem benötigen Sie eine Kopie des Bereitstellungspakets, mit dem der Ressourcenanbieter installiert wurde.

## <a name="dependency-cleanup"></a>Bereinigung von Abhängigkeiten

Es müssen verschiedene Bereinigungsaufgaben durchgeführt werden, bevor Sie das Skript „DeployMySqlProvider.ps1 script“ zum Entfernen des Ressourcenanbieters ausführen können.

Die Mandanten sind für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen aller ihrer Datenbanken aus dem Ressourcenanbieter (Die Daten werden dabei nicht gelöscht.)
* Aufheben der Registrierung des Anbieternamespace

Der Administrator ist für die folgenden Bereinigungsaufgaben verantwortlich:

* Löschen der Hostserver aus dem MySQL-Adapter
* Löschen aller Pläne, die auf den MySQL-Adapter verweisen
* Löschen aller Kontingente, die dem MySQL-Adapter zugeordnet sind

## <a name="to-remove-the-mysql-resource-provider"></a>So entfernen Sie den MySQL-Ressourcenanbieter

1. Vergewissern Sie sich, dass Sie alle vorhandenen Abhängigkeiten des MySQL-Ressourcenanbieters entfernt haben.

   >[!NOTE]
   >Die Deinstallation des MySQL-Ressourcenanbieters wird fortgesetzt, auch wenn abhängige Ressourcen den Ressourcenanbieter derzeit verwenden.
  
2. Rufen Sie eine Kopie der Binärdatei des MySQL-Ressourcenanbieters ab, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.
3. Rufen Sie eine Kopie der Binärdatei des SQL-Ressourcenanbieters ab, und führen Sie dann den Self-Extractor aus, um den Inhalt in ein temporäres Verzeichnis zu extrahieren.
4. Öffnen Sie ein neues PowerShell-Konsolenfenster mit erhöhten Rechten, und wechseln Sie zu dem Verzeichnis, in dem Sie die Binärdateien des MySQL-Ressourcenanbieters extrahiert haben.
5. Führen Sie das Skript „DeployMySqlProvider.ps1“ mit den folgenden Parametern aus:
    - **Deinstallieren**. Entfernt den Ressourcenanbieter und alle zugeordneten Ressourcen.
    - **PrivilegedEndpoint**. Die IP-Adresse oder der DNS-Name des privilegierten Endpunkts.
    - **AzureEnvironment**. Die Azure-Umgebung, die für die Bereitstellung von Azure Stack verwendet wird. Nur für Azure AD-Bereitstellungen erforderlich.
    - **CloudAdminCredential**. Die Anmeldeinformationen für den Cloudadministrator, die für den Zugriff auf den privilegierten Endpunkt erforderlich sind.
    - **DirectoryTenantID**
    - **AzCredential**. Die Anmeldeinformationen für das Azure Stack-Dienstadministratorkonto. Verwenden Sie die gleichen Anmeldeinformationen wie bei der Bereitstellung von Azure Stack.

## <a name="next-steps"></a>Nächste Schritte

[Anbieten von App Services als PaaS](azure-stack-app-service-overview.md)
