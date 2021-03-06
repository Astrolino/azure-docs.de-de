---
title: Verwenden der BulkExecutor-.NET-Bibliothek zum Ausführen von Massenvorgängen in Azure Cosmos DB | Microsoft-Dokumentation
description: Verwenden Sie die BulkExecutor-.NET-Bibliothek von Azure Cosmos DB, um Dokumente in Azure Cosmos DB-Containern per Massenvorgang zu importieren und zu aktualisieren.
keywords: .NET-BulkExecutor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: cc0faa44501ea130309a02bb48d02f9c5b33febd
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053379"
---
# <a name="use-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Verwenden der BulkExecutor-.NET-Bibliothek zum Ausführen von Massenvorgängen in Azure Cosmos DB

Dieses Tutorial enthält Anleitungen zum Verwenden der BulkExecutor-.NET-Bibliothek von Azure Cosmos DB zum Importieren und Aktualisieren von Dokumenten in Azure Cosmos DB-Containern. Informationen zur BulkExecutor-Bibliothek und dazu, wie Sie damit massiven Durchsatz und riesige Speichermengen nutzen können, finden Sie im Artikel [BulkExecutor-Bibliothek – Übersicht](bulk-executor-overview.md). In diesem Tutorial werden Sie durch eine .NET-Beispielanwendung geführt, die zufällig generierte Dokumente per Massenvorgang in einen Azure Cosmos DB-Container importiert. Nach dem Import wird erläutert, wie Sie die importierten Daten per Massenvorgang aktualisieren, indem Sie Patches als Vorgänge angeben, die für bestimmte Dokumentfelder ausgeführt werden sollen.

## <a name="prerequisites"></a>Voraussetzungen

* Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option „Azure-Entwicklung“.

* Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) erstellen, bevor Sie beginnen. 

* Sie können [Azure Cosmos DB ohne ein Azure-Abonnement testen](https://azure.microsoft.com/try/cosmosdb/) – kostenlos und ohne Verpflichtungen. Alternativ dazu können Sie den [Azure Cosmos DB-Emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator) mit dem URI `https://localhost:8081` verwenden. Den Primärschlüssel finden Sie unter [Authentifizieren von Anforderungen](local-emulator.md#authenticating-requests).

* Erstellen Sie ein Azure Cosmos DB-SQL-API-Konto mithilfe der Schritte, die im Abschnitt [Erstellen eines Datenbankkontos](create-sql-api-dotnet.md#create-a-database-account) des .NET-Schnellstarttutorials beschrieben werden. 

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Beginnen Sie jetzt mit der Arbeit mit Code, indem Sie einige .NET-Beispielanwendungen von GitHub herunterladen. Diese Anwendungen führen Massenvorgänge für Azure Cosmos DB-Daten aus. Um die Anwendungen zu klonen, öffnen Sie eine Eingabeaufforderung, navigieren Sie zu dem Verzeichnis, in das Sie die Anwendungen kopieren möchten, und führen Sie den folgenden Befehl aus:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

Das geklonte Repository enthält zwei Beispiele: BulkImportSample und BulkUpdateSample. Sie können jede der Beispielanwendungen öffnen, die Verbindungszeichenfolgen in der App.config-Datei auf die Verbindungszeichenfolgen Ihres Azure Cosmos DB-Kontos aktualisieren, die Lösung erstellen und sie ausführen. 

Die Anwendung BulkImportSample generiert zufällig Dokumente und importiert sie per Massenvorgang in Azure Cosmos DB. Die Anwendung BulkUpdateSample aktualisiert die importierten Dokumente per Massenvorgang, indem sie Patches als Vorgänge angibt, die für bestimmte Dokumentfelder ausgeführt werden sollen. In den nächsten Abschnitten sehen wir uns den Code in jeder dieser Beispiel-Apps an.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Massenimport von Daten in Azure Cosmos DB

1. Navigieren Sie zum Ordner „BulkImportSample“, und öffnen Sie die Datei „BulkImportSample.sln“.  

2. Die Verbindungszeichenfolgen von Azure Cosmos DB werden aus der App.config-Datei abgerufen, wie im folgenden Code gezeigt:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Die Massenimportanwendung erstellt eine neue Datenbank und eine Sammlung mit dem Datenbanknamen, dem Sammlungsname und Durchsatzwerten, die in der App.config-Datei angegeben sind. 

3. Als Nächstes wird das DocumentClient-Objekt mit dem direkten TCP-Verbindungsmodus initialisiert:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. Das BulkExecutor-Objekt wird mit hohen Wiederholungswerten für Wartezeit und gedrosselte Anforderungen initialisiert. Dann werden die Werte auf 0 festgelegt, um die Überlastungssteuerung während der Lebensdauer an BulkExecutor zu übergeben.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. Die Anwendung ruft die BulkImportAsync-API auf. Die .NET-Bibliothek stellt zwei Überladungen der Massenimport-API bereit: Die eine akzeptiert eine Liste von serialisierten JSON-Dokumenten, die andere akzeptiert eine Liste von deserialisierten POCO-Dokumenten. Informationen zu den Definitionen jeder dieser überladenen Methoden finden Sie in der [API-Dokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **Die BulkImportAsync-Methode akzeptiert die folgenden Parameter:**
   
   |**Parameter**  |**Beschreibung** |
   |---------|---------|
   |enableUpsert    |   Ein Flag zum Aktivieren von Upsert-Vorgängen für die Dokumente. Wenn bereits ein Dokument mit der angegebenen ID vorhanden ist, wird es aktualisiert. Standardmäßig ist „false“ festgelegt.      |
   |disableAutomaticIdGeneration    |    Ein Flag zum Deaktivieren der automatischen Generierung einer ID. Standardmäßig ist „true“ festgelegt.     |
   |maxConcurrencyPerPartitionKeyRange    | Der maximale Grad an Parallelität pro Partitionsschlüsselbereich. Bei einer Festlegung auf NULL verwendet die Bibliothek den Standardwert 20. |
   |maxInMemorySortingBatchSize     |  Die maximale Anzahl von Dokumenten, für die von dem Dokumentenumerator ein Pull ausgeführt wird, der in jeder Phase an den API-Aufruf übergeben wird.  Für die speicherinterne Sortierungsphase während der Vorverarbeitung vor dem Massenimport sorgt die Festlegung auf NULL dafür, dass die Bibliothek den Standardwert „min(documents.count, 1000000)“ verwendet.       |
   |cancellationToken    |    Das Abbruchtoken für ein ordnungsgemäßes Beenden des Massenimports.     |

   **Definition von Massenimport-Antwortobjekten** – das Ergebnis des Massenimport-API-Aufrufs enthält die folgenden Attribute:

   |**Parameter**  |**Beschreibung**  |
   |---------|---------|
   |NumberOfDocumentsImported (long)   |  Die Gesamtanzahl von Dokumenten, die von allen an den Massenimport-API-Aufruf gesendeten Dokumenten erfolgreich importiert wurden.       |
   |TotalRequestUnitsConsumed (double)   |   Die Gesamtanzahl von Anforderungseinheiten, die vom Massenimport-API-Aufruf verbraucht wurden.      |
   |TotalTimeTaken (TimeSpan)    |   Die Gesamtzeit, die der Massenimport-API-Aufruf benötigt, um die Ausführung abzuschließen.      |
   |BadInputDocuments (List<object>)   |     Die Liste der Dokumente mit ungültigem Format, die im Massenimport-API-Aufruf nicht erfolgreich importiert wurden. Benutzer sollten die zurückgegebenen Dokumente korrigieren und den Import wiederholen. Zu Dokumenten mit ungültigem Format gehören auch Dokumente, deren ID-Wert keine Zeichenfolge ist (NULL-Werte oder andere Datentypen werden als ungültig betrachtet).    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Massenaktualisierung von Daten in Azure Cosmos DB

Sie können vorhandene Dokumente mithilfe der BulkUpdateAsync-API aktualisieren. In diesem Beispiel legen Sie das Feld „Name“ auf einen neuen Wert fest und entfernen das Feld „Description“ aus den vorhandenen Dokumenten. Alle unterstützten Vorgänge zum Aktualisieren von Feldern finden Sie in der [API-Dokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. Navigieren Sie zum Ordner „BulkUpdateSample“, und öffnen Sie die Datei „BulkUpdateSample.sln“.  

2. Definieren Sie die Aktualisierungselemente zusammen mit den entsprechenden Vorgängen zum Aktualisieren von Feldern. In diesem Beispiel verwenden Sie SetUpdateOperation zum Aktualisieren des Felds „Name“ und UnsetUpdateOperation zum Entfernen des Felds „Description“ aus allen Dokumenten. Sie können auch andere Vorgänge ausführen, wie z.B. das inkrementelle Erhöhen eines Dokumentfelds um einen bestimmten Wert, das Verschieben bestimmter Werte in ein Arrayfeld oder das Entfernen eines bestimmten Werts aus einem Arrayfeld. Informationen zu den verschiedenen Methoden, die von der API für die Massenaktualisierung bereitgestellt werden, finden Sie in der [API-Dokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. Die Anwendung ruft die BulkUpdateAsync-API auf. Informationen zur Definition der BulkUpdateAsync-Methode finden Sie in der [API-Dokumentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **Die BulkUpdateAsync-Methode akzeptiert die folgenden Parameter:**

   |**Parameter**  |**Beschreibung** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   Der maximale Grad an Parallelität pro Partitionsschlüsselbereich. Bei einer Festlegung auf NULL verwendet die Bibliothek den Standardwert 20.   |
   |maxInMemorySortingBatchSize    |    Die maximale Anzahl von Aktualisierungselementen, für die von dem Enumerator für Aktualisierungselemente ein Pull ausgeführt wird, der in jeder Phase an den API-Aufruf übergeben wird, um während der speicherinternen Vorverarbeitung vor der Massenaktualisierung eine Sortierung durchzuführen. Bei einer Festlegung auf NULL verwendet die Bibliothek den Standardwert „min(updateItems.count, 1000000)“.     |
   | cancellationToken|Das Abbruchtoken für ein ordnungsgemäßes Beenden der Massenaktualisierung. |

   **Definition von Massenaktualisierungs-Antwortobjekten** – das Ergebnis des Massenaktualisierungs-API-Aufrufs enthält die folgenden Attribute:

   |**Parameter**  |**Beschreibung** |
   |---------|---------|
   |NumberOfDocumentsUpdated (long)    |   Die Gesamtanzahl von Dokumenten, die von allen an den Massenaktualisierungs-API-Aufruf gesendeten Dokumenten erfolgreich aktualisiert wurden.      |
   |TotalRequestUnitsConsumed (double)   |    Die Gesamtanzahl von Anforderungseinheiten, die vom Massenaktualisierungs-API-Aufruf verbraucht wurden.    |
   |TotalTimeTaken (TimeSpan)   | Die Gesamtzeit, die der Massenaktualisierungs-API-Aufruf benötigt, um die Ausführung abzuschließen. |
    
## <a name="performance-tips"></a>Leistungstipps 

Berücksichtigen Sie bei der Verwendung der BulkExecutor-Bibliothek die folgenden Punkte, um die Leistung zu verbessern:

* Um die beste Leistung zu erzielen, führen Sie Ihre Anwendung auf einem virtuellen Azure-Computer in der Region aus, die Sie für Ihre Cosmos DB-Kontoschreibvorgänge verwenden.  

* Es empfiehlt sich, ein einzelnes BulkExecutor-Objekt für die gesamte Anwendung auf einem einzelnen virtuellen Computer zu instanziieren, das einem bestimmten Cosmos DB-Container entspricht.  

* Eine einzelne Ausführung einer Massenvorgang-API verbraucht eine große Menge an CPU- und Netzwerk-E/A-Ressourcen des Clientcomputers. Dies wird erreicht, indem mehrere Tasks intern erzeugt werden. Vermeiden Sie das Erzeugen mehrerer gleichzeitiger Tasks in Ihrem Anwendungsprozess, von denen jeder Massenvorgang-API-Aufrufe ausführt. Wenn ein einzelner Massenvorgang-API-Aufruf, der auf einem einzelnen virtuellen Computer ausgeführt wird, nicht den gesamten Durchsatz Ihres Containers verbrauchen kann (wenn der Durchsatz mehr als 1 Million Anforderungseinheiten pro Sekunde beträgt), ist es besser, separate virtuelle Computer zu erstellen, um Massenvorgang-API-Aufrufe gleichzeitig auszuführen.  

* Stellen Sie sicher, dass „InitializeAsync()“ aufgerufen wird, nachdem ein BulkExecutor-Objekt instanziiert wurde, um die Zielpartitionszuordnung für den Cosmos DB-Container abzurufen.  

* Stellen Sie in der App.config-Datei Ihrer Anwendung sicher, dass **gcServer** aktiviert ist, um eine bessere Leistung zu erzielen.
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* Die Bibliothek gibt Ablaufverfolgungen aus, die entweder in einer Protokolldatei oder in der Konsole gesammelt werden können. Um beides zu aktivieren, fügen Sie folgendes zur App.config-Datei Ihrer Anwendung hinzu.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu NuGet-Paketen und Versionsanmerkungen zur BulkExecutor-.NET-Bibliothek finden Sie unter [Details zum BulkExecutor-SDK](sql-api-sdk-bulk-executor-dot-net.md). 
