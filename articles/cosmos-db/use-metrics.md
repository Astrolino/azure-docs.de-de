---
title: Überwachen und Debuggen mit Metriken in Azure Cosmos DB | Microsoft-Dokumentation
description: Verwenden Sie Metriken in Azure Cosmos DB zum Beheben häufiger Probleme und Überwachen der Datenbank.
keywords: Metriken
services: cosmos-db
author: kanshiG
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: govindk
ms.openlocfilehash: 5c9dded95fe3ae36a716544368e3dc44c9b86afe
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49365491"
---
# <a name="monitoring-and-debugging-with-metrics-in-azure-cosmos-db"></a>Überwachen und Debuggen mit Metriken in Azure Cosmos DB

Azure Cosmos DB bietet Metriken für Durchsatz, Speicher, Konsistenz, Verfügbarkeit und Latenz. Das [Azure-Portal](https://portal.azure.com) bietet eine Gesamtansicht dieser Metriken. Für detailliertere Metriken sind das Client-SDK und die [Diagnoseprotokolle](./logging.md) verfügbar.

Dieser Artikel behandelt häufige Anwendungsfälle und zeigt, wie Azure Cosmos DB-Metriken zum Analysieren und Beheben der Probleme verwendet werden können. Metriken werden alle fünf Minuten erfasst und sieben Tage lang gespeichert.

## <a name="understanding-how-many-requests-are-succeeding-or-causing-errors"></a>Ermitteln der Anzahl von Anforderungen, die erfolgreich ausgeführt werden oder Fehler verursachen

Rufen Sie zunächst das [Azure-Portal](https://portal.azure.com) auf, und navigieren Sie zum Blatt **Metriken**. Suchen Sie im Blatt das Diagramm **Anzahl von Anforderungen pro 1 Minute, die die Kapazität überschritten haben**. In diesem Diagramm wird die Summe der nach Statuscode segmentierten Anforderungen für jede einzelne Minute angezeigt. Weitere Informationen zu HTTP-Statuscodes finden Sie unter [HTTP Status Codes for Azure Cosmos DB](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) (HTTP-Statuscodes für Azure Cosmos DB, in englischer Sprache).

Der häufigste Fehlerstatuscode lautet 429 (Ratenbegrenzung/Drosselung). Dieser besagt, dass Anforderungen von Azure Cosmos DB den angegebenen Durchsatz überschreiten. Die häufigste Lösung hierfür ist das [zentrale Hochskalieren der RUs](./set-throughput.md) für die angegebene Sammlung.

![Anzahl von Anforderungen pro Minute](media/use-metrics/metrics-12.png)

## <a name="determining-the-throughput-distribution-across-partitions"></a>Partitionsübergreifendes Bestimmen des Durchsatzes

Für jede skalierbare Anwendung ist eine gute Kardinalität der Partitionsschlüssel von wesentlicher Bedeutung. Um die nach Partitionen aufgeschlüsselte Verteilung des Durchsatzes jedes partitionierten Containers zu bestimmen, navigieren Sie im [Azure-Portal](https://portal.azure.com) zum Blatt **Metriken**. Auf der Registerkarte **Durchsatz** wird im Diagramm **Maximal genutzte RU/Sekunde je physische Partition** die Speicheraufschlüsselung angezeigt. Die folgende Abbildung zeigt ein Beispiel für eine schlechte Verteilung der Daten, die sich anhand der extremen Partition am linken Rand erkennen lässt. 

![Einzelne Partition mit starker Auslastung um 15:05 Uhr](media/use-metrics/metrics-17.png)

Eine ungleichmäßige Verteilung des Durchsatzes kann *Hot*-Partitionen verursachen, die zu gedrosselten Anforderungen führen können und möglicherweise eine Neupartitionierung erfordern. Weitere Informationen zum Partitionieren in Azure Cosmos DB finden Sie unter [Partitionieren und Skalieren in Azure Cosmos DB](./partition-data.md).

## <a name="determining-the-storage-distribution-across-partitions"></a>Partitionsübergreifendes Bestimmen der Speicherverteilung

Für jede skalierbare Anwendung ist eine gute Kardinalität der Partition von wesentlicher Bedeutung. Um die nach Partitionen aufgeschlüsselte Verteilung des Durchsatzes jedes partitionierten Containers zu bestimmen, wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Blatt „Metriken“. Auf der Registerkarte „Durchsatz“ wird im Diagramm „Maximal genutzte RU/Sekunde je physische Partition“ die Speicheraufschlüsselung angezeigt. Die folgende Abbildung veranschaulicht eine schlechte Verteilung der Daten, die sich anhand der extremen Partition am linken Rand erkennen lässt. 

![Beispiel für eine schlechte Datenverteilung](media/use-metrics/metrics-07.png)

Sie können bestimmen, welcher Partitionsschlüssel die Verteilung verzerrt, indem Sie auf die Partition im Diagramm klicken. 

![Partitionsschlüssel verzerrt die Verteilung](media/use-metrics/metrics-05.png)

Nachdem Sie den Partitionsschlüssel identifiziert haben, der die verzerrte Verteilung verursacht, müssen Sie möglicherweise den Container mit einem Partitionsschlüssel für eine gleichmäßigere Verteilung neu partitionieren. Weitere Informationen zum Partitionieren in Azure Cosmos DB finden Sie unter [Partitionieren und Skalieren in Azure Cosmos DB](./partition-data.md).

## <a name="comparing-data-size-against-index-size"></a>Vergleichen der Datengröße mit der Indexgröße

In Azure Cosmos DB ist die Summe des Speicherverbrauchs die Kombination von Datengröße und Indexgröße. In der Regel ist die Indexgröße ein Bruchteil der Datengröße. Im [Azure-Portal](https://portal.azure.com) wird im Blatt „Metriken“ auf der Registerkarte „Speicher“ die Aufschlüsselung des Speicherverbrauchs auf der Grundlage von Daten und Index angezeigt. 

```csharp
// Measure the document size usage (which includes the index size)  
ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll")); 
 Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);
``` 
Wenn Sie Indexspeicher sparen möchten, können Sie die [Indizierungsrichtlinie](./indexing-policies.md) anpassen.

## <a name="debugging-why-queries-are-running-slow"></a>Ermitteln, warum Abfragen langsam ausgeführt werden

Azure Cosmos DB bietet in den SDKs der SQL-API Statistiken zur Ausführung von Abfragen. 

```csharp
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
 UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
 "SELECT * FROM c WHERE c.city = 'Seattle'", 
 new FeedOptions 
 { 
 PopulateQueryMetrics = true, 
 MaxItemCount = -1, 
 MaxDegreeOfParallelism = -1, 
 EnableCrossPartitionQuery = true 
 }).AsDocumentQuery();
FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id 
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;
```

*QueryMetrics* liefert Details zur Ausführungsdauer jeder Komponente der Abfrage. Die häufigste Ursache für die langsame Ausführung von Abfragen sind Überprüfungen (die Abfrage konnte die Indizes nicht nutzen). Dies kann durch eine bessere Filterbedingung behoben werden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie jetzt erfahren haben, wie Sie mit den Metriken im Azure-Portal Problembereiche überwachen und Probleme beheben, sollten Sie die folgenden Artikel lesen, um weitere Informationen zum Verbessern der Datenbankleistung zu erhalten:

* [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md)
* [Leistungstipps für Azure Cosmos DB](performance-tips.md)
