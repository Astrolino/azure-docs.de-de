---
title: Häufige Diagnoseszenarien in Azure Service Fabric | Microsoft-Dokumentation
description: Lernen Sie die Problembehandlung bei häufigen Szenarien mit Azure Service Fabric kennen
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/16/2018
ms.author: srrengar
ms.openlocfilehash: bd7a7e0288ced0219a0600034b273d1acba6b09b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659639"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Diagnostizieren häufiger Szenarien mit Service Fabric

Dieser Artikel veranschaulicht allgemeine Szenarien, die bei Benutzern im Bereich der Überwachung und Diagnose mit Service Fabric aufgetreten sind. Die beschriebenen Szenarien beziehen sich auf alle drei Service Fabric-Ebenen: Anwendung, Cluster und Infrastruktur. Bei jeder Lösung kommen Application Insights und Log Analytics sowie Azure-Überwachungstools zum Einsatz, um die einzelnen Szenarien abzuschließen. Die Schritte in den einzelnen Lösungen geben dem Benutzer eine Einführung in die Verwendung von Application Insights und Log Analytics im Zusammenhang mit Service Fabric.

## <a name="prerequisites-and-recommendations"></a>Voraussetzungen und Empfehlungen

Für die Lösungen in diesem Artikel werden die folgenden Tools verwendet. Es wird empfohlen, diese Tools vorher einzurichten und zu konfigurieren:

* [Application Insights mit Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Aktivieren der Azure-Diagnose in Ihrem Cluster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Einrichten eines OMS Log Analytics-Arbeitsbereichs](service-fabric-diagnostics-oms-setup.md)
* [OMS-Agent zum Nachverfolgen von Leistungsindikatoren](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>Wie kann ich nicht behandelte Ausnahmen in meiner Anwendung anzeigen?

1. Navigieren Sie zu Ihrer Application Insights-Ressource, mit der die Anwendung konfiguriert ist.
2. Klicken Sie oben links auf *Suchen*. Klicken Sie dann im nächsten Fenster auf „Filter“.

    ![AI-Übersicht](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Viele Arten von Ereignissen (Ablaufverfolgungen, Anforderungen, benutzerdefinierte Ereignisse) werden angezeigt. Wählen Sie „Ausnahme“ als Filter aus.

    ![AI-Filterliste](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Durch Klicken auf eine Ausnahme in der Liste können Sie weitere Einzelheiten, einschließlich des Dienstkontexts anzeigen, wenn Sie das Service Fabric Application Insights SDK verwenden.

    ![AI-Ausnahme](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Wie kann ich sehen, welche HTTP-Aufrufe in meinen Diensten verwendet werden?

1. In der gleichen Application Insights-Ressource können Sie anstatt nach „Ausnahme“ nach „Anforderungen“ filtern und alle vorgenommenen Anforderungen anzeigen.
2. Wenn Sie das Service Fabric Application Insights SDK verwenden, können Sie eine visuelle Darstellung der miteinander verbundenen Dienste sowie die Anzahl der erfolgreichen und fehlgeschlagenen Anforderungen anzeigen. Klicken Sie auf der linken Seite auf „App-Übersicht“.

    ![Blatt „AI-App-Übersicht“](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI-App-Übersicht](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Weitere Informationen zur Anwendungsübersicht finden Sie in der [Dokumentation zur Anwendungsübersicht](../application-insights/app-insights-app-map.md).

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Wie erstelle ich eine Warnung, wenn ein Knoten ausfällt?

1. Knotenereignisse werden von Ihrem Service Fabric-Cluster nachverfolgt. Navigieren Sie zur Service Fabric Analytics-Lösungsressource mit dem Namen **ServiceFabric(Name_der_Ressourcengruppe)**.
2. Klicken Sie unten auf dem Blatt auf das Diagramm mit dem Titel „Zusammenfassung“.

    ![OMS-Lösung](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Hier finden Sie viele Diagramme und Kacheln, die verschiedene Metriken anzeigen. Wenn Sie auf eines der Diagramms klicken, gelangen Sie zur Protokollsuche. Hier können Sie alle Clusterereignisse oder Leistungsindikatoren abfragen.
4. Geben Sie die folgende Abfrage ein. Diese Ereignis-IDs finden Sie in der [Referenz zu Knotenereignissen](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId >= 25623 or EventId <= 25626
    ```

5. Klicken Sie oben auf „Neue Warnungsregel“. Ab jetzt erhalten Sie jedes Mal, wenn ein Ereignis basierend auf dieser Abfrage eingeht, eine Warnung mit der von Ihnen ausgewählten Kommunikationsmethode.

    ![Neue OMS-Warnung](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Wie kann ich Warnungen zu Rollbacks von Anwendungsupgrades erhalten?

1. Geben Sie im gleichen Fenster der Protokollsuche, das Sie zuvor verwendet haben, die folgende Abfrage nach Upgraderollbacks ein. Diese Ereignis-IDs finden Sie in der [Referenz zu Anwendungsereignissen](service-fabric-diagnostics-event-generation-operational.md#application-events).

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. Klicken Sie oben auf „Neue Warnungsregel“. Ab jetzt erhalten Sie jedes Mal, wenn ein Ereignis basierend auf dieser Abfrage eingeht, eine Warnung.

## <a name="how-do-i-see-container-metrics"></a>Wie zeige ich Containermetriken an?

In der gleichen Ansicht mit allen Diagrammen sehen Sie einige Kacheln für die Leistung Ihrer Container. Sie benötigen den OMS-Agent und die [Containerüberwachungslösung](service-fabric-diagnostics-oms-containers.md), damit diese Kacheln aufgefüllt werden.

![OMS-Containermetriken](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Um Telemetriedaten von **innerhalb** Ihres Containers zu instrumentieren, müssen Sie das [Application Insights NuGet-Paket für Container](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios) hinzufügen.

## <a name="how-can-i-monitor-performance-counters"></a>Wie kann ich Leistungsindikatoren überwachen?

1. Nachdem Sie den OMS-Agent zum Cluster hinzugefügt haben, müssen Sie die speziellen Leistungsindikatoren hinzufügen, die Sie nachverfolgen möchten. Navigieren Sie im Portal zu der Seite des OMS-Arbeitsbereichs – von der Lösungsseite aus gesehen befindet sich die Registerkarte für den Arbeitsbereich im linken Menü.

    ![Registerkarte „OMS-Arbeitsbereich“](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Sobald Sie die Seite des Arbeitsbereichs aufgerufen haben, klicken Sie im gleichen linken Menü auf „Erweiterte Einstellungen“.

    ![OMS – Erweiterte Einstellungen](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Klicken Sie auf „Daten“ > „Windows-Leistungsindikatoren“ (bzw. „Daten“ > „Linux-Leistungsindikatoren“ bei Linux-Computern), um mit dem Sammeln bestimmter Leistungsindikatoren von den Knoten über den OMS-Agent zu beginnen. Es folgen Beispiele für das Format, in dem Leistungsindikatoren hinzugefügt werden müssen

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`
    * `Service Fabric Service(*)\\Average milliseconds per request`

    In der Schnellstartanleitung werden VotingData und VotingWeb als Prozessnamen verwendet. Die Nachverfolgung dieser Leistungsindikatoren würde wie folgt aussehen:

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![OMS-Leistungsindikatoren](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Anhand dieser Indikatoren können Sie sehen, wie Ihre Infrastruktur die Arbeitslasten verarbeitet, und anhand der Ressourcenauslastung entsprechende Warnungen festlegen. Beispiel: Sie möchten eine Warnung festlegen, wenn die Prozessorgesamtauslastung über 90 % steigt oder unter 5 % fällt. Der Name des Leistungsindikators, den Sie dafür verwenden, lautet „% Prozessorzeit“. Dazu können Sie eine Warnungsregel für die folgende Abfrage erstellen:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Wie kann ich die Leistung meiner Reliable Services und Reliable Actors nachverfolgen?

Um die Leistung von Reliable Services oder Reliable Actors in Ihren Anwendungen nachzuverfolgen, sollten Sie auch die Leistungsindikatoren für Service Fabric Actor, Actor-Methode, Dienst und Dienstmethode hinzufügen. Sie können diese Leistungsindikatoren in ähnlicher Weise hinzufügen wie im oben beschriebenen Szenario. Es folgen Beispiele für Leistungsindikatoren für Reliable Services und Reliable Actors, die in OMS hinzugefügt werden können:

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Nutzen Sie diese Links, um die vollständige Liste der Leistungsindikatoren für Reliable [Services](service-fabric-reliable-serviceremoting-diagnostics.md) und Reliable [Actors](service-fabric-reliable-actors-diagnostics.md) abzurufen.

## <a name="next-steps"></a>Nächste Schritte

* [Richten Sie Warnungen in AI ein](../application-insights/app-insights-alerts.md), um Benachrichtigungen zu Änderungen der Leistung oder Nutzung zu erhalten.
* Die [intelligente Erkennung in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) führt eine proaktive Analyse der an AI gesendeten Telemetriedaten aus, um Sie vor potenziellen Leistungsproblemen zu warnen.
* Erfahren Sie mehr über die [Warnungen](../log-analytics/log-analytics-alerts.md) von OMS Log Analytics, die bei Erkennung und Diagnose hilfreich sein können.
* Für lokale Cluster bietet die OMS ein Gateway (HTTP-Weiterleitungsproxy), über das Daten an die OMS gesendet werden können. Weitere Informationen dazu finden Sie unter [Verbinden von Computern ohne Internetzugriff mit der OMS über das OMS-Gateway](../log-analytics/log-analytics-oms-gateway.md).
* Machen Sie sich mit den Features zur [Protokollsuche und -abfrage](../log-analytics/log-analytics-log-searches.md) in Log Analytics vertraut.
* Eine ausführlichere Übersicht über Log Analytics und die zugehörigen Optionen finden Sie unter [Was ist Log Analytics?](../operations-management-suite/operations-management-suite-overview.md)
