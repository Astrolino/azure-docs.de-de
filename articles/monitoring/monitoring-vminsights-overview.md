---
title: Was ist Azure Monitor for VMs? | Microsoft-Dokumentation
description: Azure Monitor for VMs ist ein Feature von Azure Monitor, das Integritäts- und Leistungsüberwachung des Azure VM-Betriebssystems mit der automatischen Erkennung von Anwendungskomponenten und Abhängigkeiten mit anderen Ressourcen kombiniert und die Kommunikation unter ihnen als Zuordnung darstellt. Dieser Artikel bietet eine Übersicht.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: magoedte
ms.openlocfilehash: 26fcc3eb78af53360cca57382b4c06b017f36c0e
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063267"
---
# <a name="what-is-azure-monitor-for-vms"></a>Was ist Azure Monitor for VMs?

Azure Monitor for VMs überwacht Ihre Azure-VMs nach Maß durch Analysieren der Leistung und Integrität der Windows- und Linux-VMs, einschließlich ihrer verschiedenen Prozesse und miteinander verbundenen Abhängigkeiten von anderen Ressourcen und externen Prozessen. Die Lösung umfasst Unterstützung für die Überwachung von Leistung und Anwendungsabhängigkeiten für VMs, die lokal oder bei einem anderen Cloudanbieter gehostet sind.  Sie beinhaltet drei wichtige Features, um diesen tiefgreifenden Einblick zu ermöglichen:

* Logische Komponenten von Azure-VMs, die die Betriebssysteme Windows und Linux ausführen, werden basierend auf einer Reihe vorkonfigurierter Integritätskriterien gemessen, und es werden Warnungen ausgegeben, wenn die bewertete Bedingung erfüllt wird.  
* Die Kernleistungsmetriken von Prozessor, Arbeitsspeicher, Datenträger und Netzwerkadapter des Betriebssystems der Gast-VM werden gesammelt und in vordefinierten Leistungstrenddiagrammen dargestellt.
* Abhängigkeitszuordnung, die die erkannten, mit mehreren Ressourcengruppen und Abonnements verbundenen Komponenten der VM darstellt.  

Diese Features sind in Form von drei Perspektiven geordnet:

* Health
* Leistung
* Map

>[!NOTE]
>Aktuell wird das Integritätsfeature nur für Azure-VMs angeboten.
>

Die Integration in Log Analytics ermöglicht leistungsstarke Aggregation, Filterung und die Möglichkeit zum Durchführen von Trendanalysen der Daten im zeitlichen Verlauf. Die umfassende Überwachung Ihrer Workloads kann nicht mit Azure Monitor, der Dienstzuordnung oder Log Analytics allein erreicht werden.  

Sie können diese Daten im Kontext der einzelnen VM direkt in der VM anzeigen. Alternativ steht in Azure Monitor eine zusammengefasste Ansicht Ihrer VMs auf der Grundlage der folgenden Perspektive für die einzelnen Features zur Verfügung:

* Integrität – mit einer Ressourcengruppe zusammenhängende VMs
* Zuordnung und Leistung – VMs, die für das Berichten an einen bestimmten Log Analytics-Arbeitsbereich konfiguriert sind

![VM-Insights-Perspektive im Portal](./media/monitoring-vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps kann effektive Vorhersagen zu Leistung und Verfügbarkeit wichtiger Anwendungen liefern – dazu werden kritische Betriebssystemereignisse, Leistungsengpässe und Netzwerkprobleme identifiziert und analysiert, ob ein Problem mit anderen Abhängigkeiten zusammenhängt.  

## <a name="data-usage"></a>Datennutzung 

Sobald Sie das Onboarding von Azure Monitor for VMs ausführen, werden die von Ihren VMs gesammelten Daten in Azure Monitor erfasst und gespeichert.  Azure Monitor for VMs wird auf der Grundlage der erfassten und aufbewahrten Daten, der Anzahl der überwachten Zeitreihendaten von Integritätskriteriumsmetriken, der erstellten Warnungsregeln und der gesendeten Benachrichtigungen abgerechnet, gemäß der auf der Seite [Preise](https://azure.microsoft.com/pricing/details/monitor/) veröffentlichten Preise für Azure Monitor.

Die Protokollgröße unterscheidet sich je nach der Zeichenfolgenlänge von Indikatoren und kann mit der Anzahl der logischen Datenträger und Netzwerkadapter anwachsen.  Wenn Sie bereits über einen Arbeitsbereich verfügen und diese Indikatoren sammeln, erfolgt keine doppelte Belastung.  Wenn Sie bereits die Dienstzuordnung verwenden, besteht die einzige für Sie sichtbare Veränderung in den zusätzlichen Verbindungsdaten, die an Azure Monitor gesendet werden.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Anforderungen und Methoden für die Aktivierung Ihrer virtuellen Computer finden Sie unter [Führen Sie das Onboarding von Azure Monitor for VMs durch](monitoring-vminsights-onboard.md).
