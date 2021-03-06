---
title: Lesen oder Schreiben partitionierter Daten in Azure Data Factory | Microsoft-Dokumentation
description: Erfahren Sie, wie partitionierte Daten in Azure Data Factory gelesen oder geschrieben werden.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: shlo
ms.openlocfilehash: 59644f3318e2bf9c4f0ea6c3f5699fe1d19f2089
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053709"
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory"></a>Lesen oder Schreiben partitionierter Daten in Azure Data Factory
In Version 1 unterstützte Azure Data Factory das Lesen oder Schreiben von partitionierten Daten mithilfe der Systemvariablen SliceStart/SliceEnd/WindowStart/WindowEnd. In der aktuellen Version von Data Factory können Sie dieses Verhalten mithilfe eines Pipelineparameters und der Anfangszeit/geplanten Zeit eines Triggers als Wert des Parameters erreichen. 

## <a name="use-a-pipeline-parameter"></a>Verwenden eines Pipelineparameters 
In Version 1 konnten Sie die partitionedBy-Eigenschaft und die SliceStart-Systemvariable wie im folgenden Beispiel dargestellt verwenden: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/{Year}/{Month}/{Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

Weitere Informationen über die partitonedBy-Eigenschaft finden Sie im Artikel [Azure-Blob-Connector, Version 1](v1/data-factory-azure-blob-connector.md#dataset-properties). 

In der aktuellen Version von Data Factory besteht eine Möglichkeit zum Erreichen dieses Verhaltens im Ausführen der folgenden Aktionen: 

1. Definieren Sie einen **Pipelineparameter** vom Typ Zeichenfolge. Im folgenden Beispiel lautet der Name des Pipelineparameters **windowStartTime**. 
2. Legen Sie in der Datasetdefinition **folderPath** als Verweis auf den Wert des Pipelineparameters fest. 
3. Übergeben Sie den eigentlichen Wert für den Parameter beim Aufrufen der Pipeline auf Anforderung, oder übergeben Sie die Startzeit bzw. die geplante Zeit eines Triggers dynamisch zur Laufzeit. 

```json
"folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
},
```

## <a name="pass-in-value-from-a-trigger"></a>Übergeben eines Werts aus einem Trigger
In der folgenden Triggerdefinition mit rollierendem Fenster wird das Startzeitfenster des Triggers als Wert für den Pipelineparameter **windowStartTime** übergeben: 

```json
{
    "name": "MyTrigger",
    "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": "1",
            "startTime": "2018-05-15T00:00:00Z",
            "delay": "00:10:00",
            "maxConcurrency": 10
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "windowStartTime": "@trigger().outputs.windowStartTime"
            }
        }
    }
}
```

## <a name="example"></a>Beispiel

Es folgt ein Beispiel für eine Datasetdefinition:

```json
{
  "name": "SampleBlobDataset",
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
    },
    "format": {
      "type": "TextFormat",
      "columnDelimiter": ","
    }
  },
  "structure": [
    { "name": "ProfileID", "type": "String" },
    { "name": "SessionStart", "type": "String" },
    { "name": "Duration", "type": "Int32" },
    { "name": "State", "type": "String" },
    { "name": "SrcIPAddress", "type": "String" },
    { "name": "GameType", "type": "String" },
    { "name": "Multiplayer", "type": "String" },
    { "name": "EndRank", "type": "String" },
    { "name": "WeaponsUsed", "type": "Int32" },
    { "name": "UsersInteractedWith", "type": "String" },
    { "name": "Impressions", "type": "String" }
  ],
  "linkedServiceName": {
    "referenceName": "churnStorageLinkedService",
    "type": "LinkedServiceReference"
  }
}
```

Pipelinedefinition: 

```json
{
    "properties": {
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": {
                    "value": "@concat(pipeline().parameters.blobContainer, '/scripts/', pipeline().parameters.partitionHiveScriptFile)",
                    "type": "Expression"
                },
                "scriptLinkedService": {
                    "referenceName": "churnStorageLinkedService",
                    "type": "LinkedServiceReference"
                },
                "defines": {
                    "RAWINPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/', pipeline().parameters.inputRawLogsFolder, '/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'MM')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'dd')",
                        "type": "Expression"
                    }
                }
            },
            "linkedServiceName": {
                "referenceName": "HdiLinkedService",
                "type": "LinkedServiceReference"
            },
            "name": "HivePartitionGameLogs"
        }],
        "parameters": {
            "windowStartTime": {
                "type": "String"
            },
            "blobStorageAccount": {
                "type": "String"
            },
            "blobContainer": {
                "type": "String"
            },
            "inputRawLogsFolder": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Eine ausführliche exemplarische Vorgehensweise zum Erstellen einer Data Factory mit einer Pipeline finden Sie unter [Schnellstart: Erstellen einer Data Factory](quickstart-create-data-factory-powershell.md). 
