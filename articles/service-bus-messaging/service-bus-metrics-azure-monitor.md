---
title: Azure Service Bus-Metriken in Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Überwachen von Service Bus-Entitäten mit Azure Monitor
services: service-bus-messaging
documentationcenter: .NET
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/24/2018
ms.author: spelluru
ms.openlocfilehash: 293cde00e53171e848263df8564ec85f273c1a40
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166332"
---
# <a name="azure-service-bus-metrics-in-azure-monitor-preview"></a>Azure Service Bus-Metriken in Azure Monitor (Vorschauversion)

Service Bus-Metriken informieren Sie über den Status der Ressourcen in Ihrem Azure-Abonnement. Mithilfe eines umfangreichen Satzes von Metrikdaten können Sie die allgemeine Integrität Ihrer Service Bus-Ressourcen nicht nur auf Namespaceebene, sondern auch auf Entitätsebene bewerten. Diese Statistiken können wichtig sein, da sie Sie beim Überwachen des Zustands von Service Bus unterstützen. Metriken können auch beim Beheben der Ursachen von Problemen helfen, ohne dass Sie sich an den Azure-Support wenden müssen.

Azure Monitor bietet einheitliche Benutzeroberflächen für die übergreifende Überwachung verschiedener Azure-Dienste. Weitere Informationen finden Sie unter [Überwachung in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview.md) und im Beispiel zum [Abrufen von Azure Monitor-Metriken mit .NET](https://github.com/Azure-Samples/monitor-dotnet-metrics-api) auf GitHub.

> [!IMPORTANT]
> Wenn seit 2 Stunden keine Interaktion mit einer Entität mehr stattgefunden hat, zeigen die Metriken solange „0“ als Wert an, bis die Entität sich nicht mehr im Leerlauf befindet.

## <a name="access-metrics"></a>Zugreifen auf Metriken

Azure Monitor bietet mehrere Möglichkeiten für den Metrikzugriff. Sie können über das [Azure-Portal](https://portal.azure.com), über die Azure Monitor-APIs (REST und .NET) sowie über Analyselösungen wie Log Analytics und Event Hubs auf Metriken zugreifen. Weitere Informationen finden Sie unter [Von Azure Monitor gesammelte Überwachungsdaten](../monitoring/monitoring-data-collection.md).

Metriken sind standardmäßig aktiviert, und es stehen Daten für die letzten 30 Tage zur Verfügung. Zur längeren Aufbewahrung können Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Dies wird in den [Diagnoseeinstellungen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#diagnostic-settings) in Azure Monitor konfiguriert.

## <a name="access-metrics-in-the-portal"></a>Zugreifen auf Metriken über das Portal

Im [Azure-Portal](https://portal.azure.com) können Sie Metriken im Zeitverlauf überwachen. Das folgende Beispiel zeigt, wie Sie die erfolgreichen und die eingehenden Anforderungen auf Kontoebene anzeigen:

![][1]

Sie können auch direkt über den Namespace auf die Metriken zugreifen. Dazu wählen Sie den Namespace aus und klicken dann auf **Metriken (Vorschau)**. Um Metriken, die auf Entitätsebene gefiltert werden, anzuzeigen, wählen Sie die Entität aus und klicken dann auf **Metriken (Vorschau)**.

![][2]

Bei Metriken mit Dimensionsunterstützung müssen Sie den gewünschten Dimensionswert als Filter verwenden.

## <a name="billing"></a>Abrechnung

Die Verwendung von Metriken in Azure Monitor ist in der Vorschauversion kostenlos. Wenn Sie allerdings zusätzliche Lösungen zur Metrikdatenerfassung verwenden, fallen unter Umständen Kosten für diese Lösungen an. So werden etwa Gebühren für Azure Storage berechnet, wenn Sie Metrikdaten in einem Azure-Speicherkonto archivieren. Kosten für Log Analytics fallen auch an, wenn Sie Metrikdaten für eine ausführlichere Analyse an Log Analytics streamen.

Die folgenden Metriken bieten Ihnen eine Übersicht über die Integrität eines Diensts. 

> [!NOTE]
> Einige Metriken werden eingestellt, da sie unter einem anderen Namen verwendet werden. Deshalb müssen Sie möglicherweise Ihre Verweise aktualisieren. Metriken, die mit dem Schlüsselwort „veraltet“ markiert sind, werden zukünftig nicht mehr unterstützt.

Alle Metrikwerte werden minütlich an Azure Monitor gesendet. Das Aggregationsintervall definiert das Zeitintervall, für das Metrikwerte dargestellt werden. Das unterstützte Zeitintervall für alle Service Bus-Metriken beträgt 1 Minute.

## <a name="request-metrics"></a>Anforderungsmetriken

Zählt die Anzahl der Anforderungen von Daten und Verwaltungsvorgängen

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
| Eingehende Anforderungen (Vorschau) | Die Anzahl der Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum <br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Erfolgreiche Anforderungen (Vorschau)|Die Anzahl der erfolgreichen Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Serverfehler (Vorschau)|Die Anzahl der aufgrund eines Fehlers nicht verarbeiteten Anforderungen an den Service Bus-Dienst in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Benutzerfehler (Vorschau – siehe folgender Unterabschnitt)|Die Anzahl der aufgrund von Benutzerfehlern nicht verarbeiteten Anforderungen in einem bestimmten Zeitraum<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Gedrosselte Anforderungen (Vorschau)|Die Anzahl der Anforderungen, die aufgrund eines überschrittenen Verbrauchs gedrosselt wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

### <a name="user-errors"></a>Benutzerfehler

Die beiden folgenden Arten von Fehlern werden als Benutzerfehler klassifiziert:

1. Clientseitige Fehler (in HTTP: Fehler vom Typ 400)
2. Fehler bei der Nachrichtenverarbeitung (beispielsweise [MessageLockLostException](/dotnet/api/microsoft.azure.servicebus.messagelocklostexception))


## <a name="message-metrics"></a>Nachrichtenmetriken

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|Eingehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum an Service Bus gesendet wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Ausgehende Nachrichten (Vorschau)|Die Anzahl von Ereignissen oder Nachrichten, die in einem bestimmten Zeitraum von Service Bus empfangen wurden<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|

## <a name="connection-metrics"></a>Verbindungsmetriken

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|ActiveConnections (Vorschau)|Die Anzahl der aktiven Verbindungen in einem Namespace und bei einer Entität<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geöffnete Verbindungen (Vorschau)|Die Anzahl der geöffneten Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName|
|Geschlossene Verbindungen (Vorschau)|Die Anzahl der geschlossenen Verbindungen<br/><br/> Einheit: Anzahl <br/> Aggregationstyp: Summe <br/> Dimension: EntityName |

## <a name="resource-usage-metrics"></a>Metriken zur Ressourcennutzung

> [!NOTE] 
> Die folgenden Metriken sind nur mit dem **Premium**-Tarif verfügbar. 

| Metrikname | BESCHREIBUNG |
| ------------------- | ----------------- |
|CPU-Auslastung pro Namespace (Vorschau)|Der Prozentsatz der CPU-Auslastung des Namespace<br/><br/> Einheit: Prozent <br/> Aggregationstyp: Maximum <br/> Dimension: EntityName|
|Arbeitsspeichernutzung pro Namespace (Vorschau)|Der Prozentsatz der Arbeitsspeichernutzung des Namespace<br/><br/> Einheit: Prozent <br/> Aggregationstyp: Maximum <br/> Dimension: EntityName|

## <a name="metrics-dimensions"></a>Metrikdimensionen

Azure Service Bus unterstützt folgende Dimensionen für Metriken in Azure Monitor. Das Hinzufügen von Dimensionen zu Ihren Metriken ist optional. Wenn Sie keine Dimensionen hinzufügen, werden Metriken auf Namespaceebene angegeben. 

|Dimensionsname|BESCHREIBUNG|
| ------------------- | ----------------- |
|EntityName| Service Bus unterstützt Messagingentitäten unter dem Namespace.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie unter [Übersicht über die Azure-Überwachung](../monitoring-and-diagnostics/monitoring-overview.md).

[1]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor1.png
[2]: ./media/service-bus-metrics-azure-monitor/service-bus-monitor2.png


