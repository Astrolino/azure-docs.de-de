---
title: Kopieren von Daten nach bzw. aus Azure Cosmos DB mit Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie mithilfe der Data Factory Daten von unterstützten Quelldatenspeichern nach Azure Cosmos DB oder aus Cosmos DB in unterstützte Senkenspeicher kopiert werden.
services: data-factory, cosmosdb
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: multiple
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: jingwang
ms.openlocfilehash: db2f3086da41e990e6f87d8086d6e8defa447532
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543437"
---
# <a name="copy-data-to-or-from-azure-cosmos-db-using-azure-data-factory"></a>Kopieren von Daten nach oder aus Azure Cosmos DB mithilfe von Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Version 1](v1/data-factory-azure-documentdb-connector.md)
> * [Aktuelle Version](connector-azure-cosmos-db.md)

In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus und nach Azure Cosmos DB (SQL-API) zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-capabilities"></a>Unterstützte Funktionen

Sie können Daten aus Azure Cosmos DB in einen beliebigen unterstützten Senkendatenspeicher oder Daten aus einem beliebigen unterstützten Quelldatenspeicher nach Azure Cosmos DB kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Azure Cosmos DB-Connector unterstützt insbesondere Folgendes:

- Kopieren von Daten aus der/ in die Cosmos DB [SQL-API](https://docs.microsoft.com/azure/cosmos-db/documentdb-introduction).
- Schreiben in die Cosmos DB als INSERT oder UPSERT.
- Importieren oder Exportieren von JSON-Dokumenten im jeweiligen Zustand oder Kopieren von Daten aus bzw. in ein tabellarisches Dataset (z.B. eine SQL-Datenbank, CSV-Dateien) Informationen zum Kopieren von Daten in ihrem jeweiligen Zustand in bzw. aus JSON-Dateien oder in eine andere bzw. aus einer anderen Cosmos DB-Sammlung finden Sie unter [Importieren oder Exportieren von JSON-Dokumenten](#importexport-json-documents).

Data Factory wird mit der [Cosmos DB-BulkExecutor-Bibliothek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) integriert, um beim Schreiben in Cosmos DB die beste Leistung zu erzielen.

>[!TIP]
>Sehen Sie sich [dieses Video](https://youtu.be/5-SRNiC_qOU) an, das das Kopieren von Daten aus dem Azure Blob Storage in die Cosmos DB erläutert und Überlegungen zur Leistungsoptimierung beim Einlesen von Daten in die Cosmos DB im Allgemeinen beschreibt.

## <a name="getting-started"></a>Erste Schritte

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren spezifischer Data Factory-Entitäten für Azure Cosmos DB verwendet werden:

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Azure Cosmos DB verknüpften Dienst unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft muss auf **CosmosDb** festgelegt werden. | JA |
| connectionString |Geben Sie die zum Verbinden mit der Azure Cosmos DB-Datenbank erforderlichen Informationen an. Beachten Sie, dass Sie wie im folgenden Beispiel gezeigt Datenbankinformationen in der Verbindungszeichenfolge angeben müssen. Markieren Sie dieses Feld als SecureString, um es sicher in Data Factory zu speichern, oder [verweisen Sie auf ein in Azure Key Vault gespeichertes Geheimnis](store-credentials-in-key-vault.md). |JA |
| connectVia | Die [Integrationslaufzeit](concepts-integration-runtime.md), die zum Herstellen einer Verbindung mit dem Datenspeicher verwendet werden muss. Sie können die Azure-Integrationslaufzeit oder selbstgehostete Integrationslaufzeit verwenden (sofern sich Ihr Datenspeicher in einem privaten Netzwerk befindet). Wenn keine Option angegeben ist, wird die standardmäßige Azure Integration Runtime verwendet. |Nein  |

**Beispiel:**

```json
{
    "name": "CosmosDbLinkedService",
    "properties": {
        "type": "CosmosDb",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu Datasets. Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Azure Cosmos DB-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus bzw. nach Azure Cosmos DB die type-Eigenschaft des Datasets auf **DocumentDbCollection** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft des Datasets muss auf **DocumentDbCollection** festgelegt werden. |JA |
| collectionName |Name der Cosmos DB-Dokumentsammlung |JA |

**Beispiel:**

```json
{
    "name": "CosmosDbDataset",
    "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName":{
            "referenceName": "<Azure Cosmos DB linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "collectionName": "<collection name>"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schema per Data Factory

Bei schemafreien Datenspeichern, z.B. Azure Cosmos DB, leitet die Kopieraktivität das Schema auf eine der folgenden Weisen ab: Daher besteht die bewährte Methode darin, die Struktur der Daten im Abschnitt **structure** anzugeben, es sei denn, Sie möchten [JSON-Dokumente in ihrem jeweiligen Zustand importieren oder exportieren](#importexport-json-documents).

*. Wenn Sie die Struktur der Daten mithilfe der **structure** -Eigenschaft in der Datasetdefinition angeben, berücksichtigt der Data Factory-Dienst diese Struktur als das Schema. Wenn in diesem Fall eine Zeile keinen Wert für eine Spalte enthält, wird ein NULL-Wert für sie angegeben.
*. Wenn Sie die Struktur der Daten nicht mithilfe der **structure** -Eigenschaft in der Datasetdefinition angeben, leitet der Data Factory-Dienst das Schema unter Verwendung der ersten Zeile in den Daten ab. Wenn in diesem Fall die erste Zeile nicht das vollständige Schema enthält, fehlen im Ergebnis des Kopiervorgangs einige Spalten.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Azure Cosmos DB-Quelle und -Senke unterstützt werden.

### <a name="azure-cosmos-db-as-source"></a>Azure Cosmos DB als Quelle

Legen Sie zum Kopieren von Daten aus Azure Cosmos DB den Quelltyp in der Kopieraktivität auf **DocumentDbCollectionSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Quelle der Kopieraktivität muss auf **DocumentDbCollectionSource** festgelegt werden. |JA |
| query |Geben Sie die Cosmos DB-Abfrage an, um Daten zu lesen.<br/><br/>Beispiel: `SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` |Nein  <br/><br/>Falls nicht angegeben, wird folgende SQL-Anweisung ausgeführt: `select <columns defined in structure> from mycollection` |
| nestingSeparator |Sonderzeichen, um anzugeben, dass das Dokument geschachtelt ist und wie das Resultset zu vereinfachen ist.<br/><br/>Wenn eine Cosmos DB-Abfrage beispielsweise das geschachtelte Ergebnis `"Name": {"First": "John"}` zurückgibt, identifiziert die Kopieraktivität den Spaltennamen „Name.First“ mit dem Wert „John“, wenn es sich bei „nestedSeparator“ um einen Punkt handelt. |Nein (der Standardwert ist ein Punkt `.`) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Document DB input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT c.BusinessEntityID, c.Name.First AS FirstName, c.Name.Middle AS MiddleName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\""
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-cosmos-db-as-sink"></a>Azure Cosmos DB als Senke

Legen Sie zum Kopieren von Daten in Azure Cosmos DB den Senkentyp in der Copy-Aktivität auf **DocumentDbCollectionSink** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | BESCHREIBUNG | Erforderlich |
|:--- |:--- |:--- |
| type | Die type-Eigenschaft der Senke der Kopieraktivität muss auf **DocumentDbCollectionSink** festgelegt werden. |JA |
| writeBehavior |Beschreibt, wie Daten in Cosmos DB geschrieben werden. Zulässige Werte sind: `insert` und `upsert`.<br/>Das Verhalten von **upsert** besteht darin, das Dokument zu ersetzen, wenn ein Dokument mit der gleichen ID bereits vorhanden ist. Andernfalls wird es eingefügt. ADF generiert automatisch eine ID für das Dokument, wenn sie im Originaldokument oder durch Spaltenzuordnung nicht angegeben ist. Dies bedeutet, dass Sie sicherstellen müssen, dass das Dokument eine ID aufweist, damit „upsert“ wie erwartet funktioniert. |Nein, der Standardwert ist „insert“ |
| writeBatchSize | Data Factory verwendet die [Cosmos DB-BulkExecutor-Bibliothek](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started) zum Schreiben von Daten in Cosmos DB. „writeBatchSize“ steuert die Größe der in der Bibliothek bereitgestellten Dokumente. Sie können versuchen, „writeBatchSize“ zu vergrößern, um die Leistung zu verbessern. |Nein, Standardwert ist 10.000 |
| nestingSeparator |Ein Sonderzeichen im Quellspaltennamen, um anzuzeigen, dass das geschachtelte Dokument erforderlich ist. <br/><br/>Beispielsweise generiert `Name.First` in der Struktur des Ausgabedatasets folgende JSON-Struktur im Cosmos DB-Dokument, sofern es sich bei „nestedSeparator“ um einen Punkt handelt: `"Name": {"First": "[value maps to this column from source]"}`. |Nein (der Standardwert ist ein Punkt `.`) |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyToCosmosDB",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Document DB output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DocumentDbCollectionSink",
                "writeBehavior": "upsert"
            }
        }
    }
]
```

## <a name="importexport-json-documents"></a>Importieren oder Exportieren von JSON-Dokumenten

Mit diesem Cosmos DB-Connector können Sie Folgendes problemlos durchführen:

* JSON-Dokumente aus verschiedenen Quellen in Cosmos DB importieren, z.B. aus einem Azure-Blob, Azure Data Lake Store und anderen dateibasierten Speichern, die von Azure Data Factory unterstützt werden
* JSON-Dokumente aus Cosmos DB-Sammlungen in verschiedene dateibasierte Speicher exportieren.
* Dokumente zwischen zwei Cosmos DB-Sammlungen unverändert kopieren

So erhalten Sie eine vom Schema unabhängige Kopie

* Aktivieren Sie bei Verwendung des Tools zum Kopieren von Daten die Option **Export as-is to JSON files or Cosmos DB collection** (Wie vorhanden in JSON-Dateien oder Cosmos DB-Sammlung exportieren).
* Geben Sie bei Verwendung der Aktivitätserstellung weder den Abschnitt „structure“ (das Schema) in Cosmos DB-Datasets noch die „nestingSeparator“-Eigenschaft für Cosmos DB-Quellen/-Senken in der Kopieraktivität an. Wenn Sie Daten aus JSON-Dateien importieren bzw. in JSON-Dateien exportieren, geben Sie im entsprechenden Dateispeicherdataset ordnungsgemäß als Formattyp „JsonFormat“ und als Konfiguration „filePattern“ an (siehe Abschnitt [JSON-Format](supported-file-formats-and-compression-codecs.md#json-format)). Geben Sie anschließend nicht den Abschnitt „structure“ (das Schema) an, und überspringen Sie die restlichen Formateinstellungen.

## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).
