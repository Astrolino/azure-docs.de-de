---
title: Microsoft Azure Data Box Edge – Übersicht | Microsoft-Dokumentation
description: Beschreibt Azure Data Box Edge, eine Speicherlösung, die ein physisches Gerät für die netzwerkbasierte Übertragung in Azure verwendet.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6422482a31ad8022b795cb357c37ad8560781767
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166592"
---
# <a name="what-is-azure-data-box-edge-preview"></a>Was ist Azure Data Box Edge (Vorschauversion)? 

Azure Data Box Edge ist eine Speicherlösung, mit der Sie Daten verarbeiten und über ein Netzwerk an Azure senden können. Dieser Artikel bietet Ihnen einen Überblick über die Lösung Data Box Edge, ihre Vorteile, ihre wichtigsten Funktionen und die Szenarien, in denen Sie dieses Gerät bereitstellen können. 

Data Box Edge verwendet ein von Microsoft geliefertes physisches Gerät, um die sichere Datenübertragung zu beschleunigen. Das physische Gerät befindet sich an Ihrem Standort, und Sie beschreiben es mit den Protokollen NFS und SMB mit Daten. 

Data Box Edge bietet sämtliche Gatewayfunktionen von Data Box Gateway. Die Data Box ist zusätzlich mit KI-fähigen Edge-Computingfunktionen ausgestattet, die bei der Analyse, Verarbeitung oder Filterung von Daten auf dem Weg in ein Azure-Blockblob, -Seitenblob oder Azure Files helfen.  

> [!IMPORTANT]
> - Data Box Edge befindet sich in Vorschauphase. [Registrieren](#sign-up) Sie sich für diesen Dienst!
> - Lesen Sie die [Vertragsbedingungen für die Nutzung der Vorschau](https://azure.microsoft.com/support/legal/preview-supplemental-terms/), bevor Sie diese Lösung bereitstellen.


## <a name="use-cases"></a>Anwendungsfälle

Azure Data Box Edge ist ein KI-fähiges Edge-Computing-Gerät mit Netzwerkfunktionen zur Datenübertragung. Es folgen die verschiedenen Szenarien, in denen Data Box Edge für die Datenübertragung verwendet werden kann.

- **Vorverarbeiten von Daten**: Analysieren Sie Daten von lokalen oder IoT-Geräten, um schnell zum Ergebnis zu gelangen und gleichzeitig in der Nähe dessen zu bleiben, wo Daten generiert werden. Data Box Edge überträgt das gesamte Dataset in die Cloud, um eine erweiterte Verarbeitung oder detailliertere Analysen durchzuführen.  Die Vorverarbeitung ermöglicht Folgendes: 

    - Aggregieren von Daten.
    - Ändern von Daten, um z.B. personenbezogene Informationen zu entfernen.
    - Erstellen einer Teilmenge der Daten, die für eine detailliertere Analyse in die Cloud übertragen werden.
    - Analysieren von und Reagieren auf IoT-Ereignisse. 

- **Rückschluss dank Azure Machine Learning**: Mit Data Box Edge können Sie ML-Modelle (Machine Learning) ausführen, um so schnell Ergebnisse zu erzielen, auf die Sie reagieren können, bevor die Daten in die Cloud übertragen werden. Das gesamte Dataset wird übertragen, um Ihre ML-Modelle weiter zu trainieren und zu verbessern.

- **Übertragen von Daten über das Netzwerk an Azure**: Verwenden Sie Data Box Edge, um Daten einfach und schnell in Azure zu übertragen. Dadurch werden weitere Berechnungen und Analysen oder eine Archivierung ermöglicht. 

## <a name="benefits"></a>Vorteile

Data Box Edge hat die folgenden Vorteile:

- **Einfache Datenübertragung**: Macht das Verschieben von Daten in und aus Azure Storage so einfach wie das Arbeiten mit einer lokalen Netzwerkfreigabe.  
- **Hohe Leistung**: Ermöglicht schnelle Datenübertragungen in und aus Azure. 
- **Schneller Zugriff**: Speichert die zuletzt verwendeten Dateien für einen schnellen Zugriff auf lokale Dateien im Cache zwischen.  
- **Eingeschränkte Bandbreitenauslastung**: Daten können in Azure geschrieben werden, auch wenn das Netzwerk gedrosselt ist, um die Nutzung während der Hauptgeschäftszeiten einzuschränken.  
- **Transformieren von Daten**: Ermöglicht das Analysieren, Verarbeiten oder Filtern von Daten beim Verschieben in Azure.

## <a name="key-capabilities"></a>Wichtige Funktionen

Data Box Edge bietet die folgenden Funktionen:

|Funktion |BESCHREIBUNG  |
|---------|---------|
|Hohe Leistung     | Vollständig automatisierte und stark optimierte Datenübertragung und Bandbreite.|
|Unterstützte Protokolle     | Für die Datenerfassung werden die Standardprotokolle SMB und NFS unterstützt. <br> Weitere Informationen zu unterstützten Versionen finden Sie unter [Systemanforderungen für Data Box Edge](http://aka.ms/dbe-docs).|
|Computing       |Ermöglicht das Analysieren, Verarbeiten und Filtern von Daten.|
|Datenzugriff     | Direkter Datenzugriff aus Azure Storage-Blobs und Azure Files über Cloud-APIs zur zusätzlichen Datenverarbeitung in der Cloud.|
|Schneller Zugriff     | Lokaler Cache auf dem Gerät für schnellen Zugriff auf zuletzt verwendete Dateien.|
|Hochladen von Daten im Offlinemodus     | Der Modus „Getrennt“ unterstützt Szenarien für das Hochladen von Daten im Offlinemodus.|
|Datenaktualisierung     | Möglichkeit, lokale Dateien mit den neuesten Informationen aus der Cloud zu aktualisieren.|
|Verschlüsselung    | BitLocker-Unterstützung zur lokalen Verschlüsselung von Daten und zur sicheren Datenübertragung in die Cloud über *HTTPS*.       |
|Resilienz     | Integrierte Netzwerkresilienz.        |


## <a name="features-and-specifications"></a>Funktionen und Spezifikationen

Das physische Data Box Edge-Gerät weist die folgenden Merkmale auf:

| Funktionen/Technische Daten                                          | BESCHREIBUNG              |
|---------------------------------------------------------|--------------------------|
| Dimensionen   | Breite: 43,8 cm Tiefe: 69,2 cm Höhe: 4,4 cm<br>(ohne Ösen und Netzteilgriffe)  |            
| Platzbedarf im Rack|1 HE bei Einbau im Rack|
| Kabel| 2 Netzkabel<br>2 RJ45-Kabel (1 GBit/s)<br>2 SFP+-Kupferkabel (10 GBit/s)|
| Komponenten|2 integrierte Netzteile|
| CPU|2 Intel Xeon-Prozessoren mit je 10 Kernen  |
| Arbeitsspeicher| 64 GB RAM|
| Datenträger| 8 NVMe-SSD-Datenträger mit je 1,6 TB <br> Das System fällt aus, sobald ein NVMe-SSD-Datenträger ausfällt. |
| Lokale Speicherkapazität| 12,8 TB Gesamtkapazität|
| Netzwerkschnittstellen| 2 Netzwerkschnittstellen (1 GbE), 1 davon zur Verwaltung, nicht vom Benutzer konfigurierbar, dient zur erstmaligen Einrichtung Die andere Datenschnittstelle ist vom Benutzer konfigurierbar und standardmäßig DHCP. <br>2 Netzwerkschnittstellen (25 GbE), die auch als 10-GbE-Schnittstellen betrieben werden können. Diese Datenschnittstellen können vom Benutzer als DHCP (Standard) oder statisch konfiguriert werden. <br> 2 Netzwerkschnittstellen (25 GbE): Diese Datenschnittstellen können vom Benutzer als DHCP (Standard) oder statisch konfiguriert werden.|

## <a name="components"></a>Komponenten

Die Data Box Edge-Lösung umfasst die Data Box Edge-Ressource, ein physisches Data Box Edge-Gerät und eine lokale Webbenutzeroberfläche.

* **Physisches Data Box Edge-Gerät**: Ein von Microsoft bereitgestellter 1HE-Rackserver, der konfiguriert werden kann, um Daten an Azure zu senden. 
    
* **Data Box Edge-Ressource:** Eine Ressource im Azure-Portal, mit der Sie ein Data Box Edge-Gerät auf einer zentralen Webbenutzeroberfläche verwalten können, auf die an verschiedenen geografischen Standorten zugegriffen werden kann. Verwenden Sie die Data Box Edge-Ressource, um Ressourcen zu erstellen und zu verwalten, Geräte und Warnungen anzuzeigen und zu verwalten sowie Freigaben zu verwalten.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Weitere Informationen finden Sie unter [Verwalten Ihres Data Box Edge-Geräts mithilfe des Data Box Edge-Diensts](http://aka.ms/dbe-docs).

* **Lokale Webbenutzeroberfläche für Data Box**: Auf der lokalen Webbenutzeroberfläche können Sie Diagnosen ausführen, das Data Box Edge-Gerät herunterfahren und neu starten, Kopierprotokolle anzeigen und den Microsoft-Support kontaktieren, um eine Serviceanfrage zu stellen.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Informationen zur Verwendung der webbasierten Benutzeroberfläche finden Sie unter [Verwenden der webbasierten Benutzeroberfläche zum Verwalten Ihrer Data Box](http://aka.ms/dbe-docs).


## <a name="region-availability"></a>Regionale Verfügbarkeit

Das virtuelle Data Box Gateway-Gerät, die Azure-Ressource und das Zielspeicherkonto, an das Sie Daten übertragen, müssen sich nicht alle in der gleichen Region befinden.

- **Verfügbarkeit der Ressource**: Bei dieser Version ist die Data Box Gateway-Ressource in den folgenden Regionen verfügbar:
    - **USA**: USA, Westen 2 und USA, Osten
    - **Europa**: Europa, Westen
    - **Asien-Pazifik**: Asien-Pazifik, Südosten

- **Zielspeicherkonten**: Die Speicherkonten, in denen die Daten gespeichert werden, sind in allen Azure-Regionen verfügbar. 

    Die Regionen, in denen die Speicherkonten Data Box-Daten speichern, sollten sich in der Nähe des Standorts des Geräts befinden, um eine optimale Leistung zu erzielen. Ein weit entfernt vom Gerät befindliches Speicherkonto führt zu langen Wartezeiten und einer langsameren Leistung. 


## <a name="sign-up"></a>Registrieren

Data Box Edge ist in der Vorschauphase. Sie müssen sich für den Dienst registrieren. Führen Sie die folgenden Schritte aus, um sich für Data Box Edge zu registrieren:

1. Melden Sie sich am Azure-Portal an unter: [https://aka.ms/databox-edge](https://aka.ms/databox-edge).

2. Wählen Sie das Abonnement aus, das Sie für die Vorschauversion von Data Box Edge verwenden möchten. Wählen Sie die Region aus, in der Sie die Data Box Edge-Ressource bereitstellen möchten. Klicken Sie bei der Option „Data Box Edge“ auf **Registrieren**.

    ![Data Box Edge-Registrierung 3](media/data-box-edge-overview/data-box-edge-sign-up3.png)

3.  Beantworten Sie die Fragen nach dem Land, in dem sich die Daten befinden, Zeitrahmen, Azure-Zieldienst für die Datenübertragung, der Netzwerkbandbreite und Häufigkeit der Datenübertragung. Überprüfen Sie **Datenschutz und Nutzungsrichtlinien**, und aktivieren Sie das Kontrollkästchen **Microsoft darf Ihre E-Mail-Adresse für die Kontaktaufnahme verwenden**.

    ![Data Box Edge-Registrierung 4](media/data-box-edge-overview/data-box-edge-sign-up4.png)

4. Nachdem Sie registriert und für die Vorschauversion aktiviert wurden, können Sie ein Data Box Edge-Gerät bestellen.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die [Data Box Edge-Systemanforderungen](http://aka.ms/dbe-docs) an.
- Machen Sie sich mit den [Data Box Edge-Einschränkungen](http://aka.ms/dbe-docs) vertraut.
- Stellen Sie [Azure Data Box Edge](http://aka.ms/dbe-docs) im Azure-Portal bereit.




