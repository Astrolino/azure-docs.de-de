---
title: Azure Stream Analytics-Ausgabe an Cosmos DB
description: Dieser Artikel beschreibt, wie Sie Azure Stream Analytics für die JSON-Ausgabe zum Speichern der Ausgabe in Azure Cosmos DB verwenden können, um eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz zu ermöglichen.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 95cfc7e6d9515274aa7a3c5fde382244f3b33fab
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "42143365"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Stream Analytics-Ausgabe an Azure Cosmos DB  
Stream Analytics kann für die JSON-Ausgabe auf [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) ausgerichtet werden, was eine Datenarchivierung und Abfragen unstrukturierter JSON-Daten mit geringer Latenz ermöglicht. In diesem Dokument werden einige bewährte Implementierungsmethoden für diese Konfiguration behandelt.

Falls Sie noch nicht mit Cosmos DB vertraut sind, sehen Sie sich zum Einstieg den [Lernpfad für Azure Cosmos DB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) an. 

> [!Note]
> Derzeit unterstützt Azure Stream Analytics nur die Verbindung mit Cosmos DB nur über die **SQL-API**.
> Andere Azure Cosmos DB-APIs werden noch nicht unterstützt. Wenn Sie Azure Stream Analytics auf die mit anderen APIs erstellten Azure Cosmos DB-Konten verweisen, werden die Daten unter Umständen nicht richtig gespeichert. 

## <a name="basics-of-cosmos-db-as-an-output-target"></a>Grundlagen von Cosmos DB als Ausgabeziel
Die Azure Cosmos DB-Ausgabe in Stream Analytics ermöglicht das Schreiben der Ergebnisse Ihrer Datenstromverarbeitung als JSON-Ausgabe in Ihre Cosmos DB-Sammlungen. Stream Analytics erstellt keine Sammlungen in Ihrer Datenbank, sondern fordert deren vorherige Erstellung an. Dies ist so, damit die Abrechnungskosten von Cosmos DB-Sammlungen von Ihnen gesteuert werden können und Sie die Leistung, Konsistenz und Kapazität Ihrer Sammlungen direkt mithilfe der [Cosmos DB-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx)optimieren können. 

Nachstehen sind einige der Cosmos DB-Sammlungsoptionen beschrieben.

## <a name="tune-consistency-availability-and-latency"></a>Optimieren von Konsistenz, Verfügbarkeit und Latenz
Entsprechend Ihren Anwendungsanforderungen lässt Azure Cosmos DB das Optimieren der Datenbank und Sammlungen zu und wählt Kompromisse zwischen Konsistenz, Verfügbarkeit und Latenz. Abhängig von den Lesekonsistenzebenen, die Ihr Szenario hinsichtlich Lese- und Schreiblatenz benötigt, können Sie in Ihrem Datenbankkonto eine Konsistenzebene wählen. Außerdem aktiviert Azure Cosmos DB für jeden CRUD-Vorgang in Ihrer Sammlung die synchrone Indizierung. Dies ist eine andere nützliche Option zum Steuern der Schreib-/Leseleistung in Azure Cosmos DB. Weitere Informationen finden Sie im Artikel [Ändern der Datenbank- und Abfragekonsistenzebenen](../cosmos-db/consistency-levels.md).

## <a name="upserts-from-stream-analytics"></a>Einfügen/Aktualisieren über Stream Analytics
Die Stream Analytics-Integration mit Azure Cosmos DB ermöglicht das Einfügen oder Aktualisieren von Datensätzen in Ihrer Sammlung auf der Grundlage einer angegebenen Dokument-ID-Spalte. Dies wird auch als *Upsert*(Kunstwort aus Update und Insert) bezeichnet.

Stream Analytics verwendet einen optimistischen Upsert-Ansatz, bei dem Aktualisierungen nur erfolgen, wenn eine Einfügung aufgrund eines Dokument-ID-Konflikts keinen Erfolg hat. Diese Aktualisierung wird mit einem PATCH-Befehl ausgeführt. Dadurch sind Teilaktualisierungen des Dokuments möglich, was bedeutet, dass das Hinzufügen neuer Eigenschaften oder Austauschen einer vorhandenen Eigenschaft schrittweise erfolgt. Änderungen der Werte von Arrayeigenschaften in Ihrem JSON-Dokument können jedoch dazu führen, dass das gesamte Array überschrieben wird, d.h., nicht zusammengeführt wird.

## <a name="data-partitioning-in-cosmos-db"></a>Partitionieren von Daten in CosmosDB
Azure Cosmos DB [unbegrenzt](../cosmos-db/partition-data.md) wird als Ansatz für die Partitionierung Ihrer Daten empfohlen, da Azure Cosmos DB Partitionen automatisch basierend auf Ihrer Arbeitsauslastung skaliert. Beim Schreiben in unbegrenzte Container verwendet Stream Analytics so viele parallele Writer wie im vorherigen Abfrageschritt oder im eingegebenen Partitionierungsschema.
> [!Note]
> Derzeit unterstützt Azure Stream Analytics nur unbegrenzte Sammlungen mit Partitionsschlüsseln auf der obersten Ebene. Beispielsweise wird `/region` unterstützt. Geschachtelte Partitionsschlüssel (z.B. `/region/name`) werden nicht unterstützt. 

Bei festen Azure Cosmos DB-Sammlungen lässt Stream Analytics kein zentrales oder horizontales Hochskalieren zu, sobald diese vollständig sind. Sie haben eine Obergrenze von 10 GB und einen Durchsatz von 10.000 RU/s.  Zum Migrieren der Daten aus einem festen Container zu einem unbegrenzten Container (beispielsweise mit mindestens 1.000 RU/s und einem Partitionsschlüssel) müssen Sie das [Datenmigrationstool](../cosmos-db/import-data.md) oder die [Änderungsfeedbibliothek](../cosmos-db/change-feed.md) verwenden.

Das Schreiben in mehrere feste Container ist veraltet. Von diesem Ansatz für die horizontale Skalierung Ihres Stream Analytics-Auftrags wird abgeraten. Der Artikel [Partitionieren und Skalieren von Daten in Cosmos DB](../cosmos-db/sql-api-partition-data.md) enthält weitere Details.

## <a name="cosmos-db-settings-for-json-output"></a>Cosmos DB-Einstellungen für die JSON-Ausgabe
Beim Erstellen einer Cosmos DB-Datenbank als Ausgabe in Stream Analytics wird eine Aufforderung zur Eingabe von Informationen eingeblendet. Dieser Abschnitt enthält eine Erklärung der Definition der Eigenschaften.


![Bildschirm mit DocumentDB als Stream Analytics-Ausgabe](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

Feld           | BESCHREIBUNG 
-------------   | -------------
Ausgabealias    | Ein Alias zum Verweisen auf diese Ausgabe in Ihrer ASA-Abfrage   
Kontoname    | Der Name oder Endpunkt-URI des Azure Cosmos DB-Kontos 
Kontoschlüssel     | Der Schlüssel für den gemeinsamen Zugriff für das Azure Cosmos DB-Konto
Datenbank        | Der Name der Azure Cosmos DB-Datenbank
Sammlungsname | Der Sammlungsname für die zu verwendende Sammlung. `MyCollection` ist eine gültige Beispieleingabe – es muss eine Sammlung mit dem Namen `MyCollection` vorhanden sein.  
Dokument-ID     | Optional. Der Spaltenname in Ausgabeergebnissen, der als eindeutiger Schlüssel verwendet wird, auf dem Einfüge- oder Aktualisierungsvorgänge basieren müssen. Wenn das Feld leer gelassen wird, werden sämtliche Ereignisse eingefügt, ohne Update-Option.
