---
title: SDK für die Bing-News-Suche
titleSuffix: Azure Cognitive Services
description: SKD für die Bing-News-Suche für Anwendungen, die das Internet durchsuchen.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: b1d9eaa35416adfa11647f2116171256f82fe095
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801232"
---
# <a name="bing-search-sdk"></a>SKD für die Bing-Suche
Die Beispiele zur Bing-News-Suche-API beinhalten folgende Szenarios:
1. Fragen Sie Neuigkeiten mithilfe der Parameter `market` und `count` nach Suchbegriffen ab, überprüfen Sie die Anzahl der Ergebnisse, und geben Sie `totalEstimatedMatches`, den Namen, die URL, die Beschreibung, das Veröffentlichungsdatum und den Anbieternamen für das erste Neuigkeitenergebnis aus.
2. Fragen Sie die aktuellsten Neuigkeiten mithilfe der Parameter `freshness` und `sortBy` nach Suchbegriffen ab, überprüfen Sie die Anzahl der Ergebnisse, und geben Sie `totalEstimatedMatches`, die URL, die Beschreibung, das Veröffentlichungsdatum und den Anbieternamen für das erste Neuigkeitenergebnis aus.
3. Fragen Sie die Kategorie „Neuigkeiten“ mithilfe der Parameter `movie` und `TV entertainment` über eine sichere Suche ab, überprüfen Sie die Anzahl der Ergebnisse, und geben Sie die Kategorie, den Namen, die URL, die Beschreibung, das Veröffentlichungsdatum und den Anbieternamen für das erste Neuigkeitenergebnis aus.
4. Fragen Sie beliebte Neuigkeiten in Bing ab, überprüfen Sie die Anzahl der Ergebnisse, und geben Sie den Namen, den Abfragetext, `webSearchUrl`, `newsSearchUrl` und die Bild-URL des ersten Neuigkeitenergebnis aus.

Durch die SDKs für die Bing-Suche werden Websuchfunktionen in den folgenden Programmiersprachen leicht zugänglich:
* Erste Schritte mit [.NET-Beispielen](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * In den [.NET-Bibliotheken](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [Node.js-Beispielen](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * In den [Node.js-Bibliotheken](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [Java-Beispielen](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * In den [Java-Bibliotheken](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.
* Erste Schritte mit [Python-Beispielen](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * In den [Python-Bibliotheken](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) finden Sie Definitionen und Informationen zu Abhängigkeiten.

SDK-Beispiele für jede Sprache enthalten eine Infodatei mit Details zu Voraussetzungen und zum Installieren und Ausführen der Beispiele.