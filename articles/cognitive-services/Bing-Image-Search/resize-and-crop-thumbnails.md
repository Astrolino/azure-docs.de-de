---
title: Ändern der Größe und Zuschneiden von Bing-Miniaturbildern – Bing-Bildersuche-API
description: Erfahren Sie, wie Sie die Größe von Miniaturbildern ändern und sie zuschneiden, die in einer Antwort der Bing-Bildersuche-API enthalten sind.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: F4FFAE91-A003-4F7C-8E60-83A142485E28
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: de82cc5554af91294dda3826dfb394cc94dbf3d0
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296226"
---
# <a name="resizing-and-cropping-thumbnail-images"></a>Ändern der Größe und Zuschneiden von Miniaturbildern

Bei der Verarbeitung einer Suchabfrage generiert Bing Miniaturansichtsinformationen für alle Bilder in der [Antwort](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images#bing-image-search-response-format). Diese Informationen können zum Anzeigen aller oder eine Teilmenge der zurückgegebenen Miniaturansichten verwendet werden. Wenn Sie eine Teilmenge anzeigen, stellen Sie eine Möglichkeit bereit, die restlichen Bilder anzuzeigen.


<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Wenn der Benutzer auf die Miniaturansicht klickt, können Sie die Inhalts-URL ([contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl)) verwenden, um dem Benutzer das Bild in voller Größe anzuzeigen. Vergessen Sie nicht, die Quelle anzugeben.

Wenn `shoppingSourcesCount` oder `recipeSourcesCount` größer Null ist, fügen Sie der Miniaturansicht Badging (beispielsweise einen Einkaufswagen) hinzu, um anzugeben, dass für den Artikel auf dem Bild Einkaufsmöglichkeiten oder Rezepte verfügbar sind.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Verwenden Sie [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken), um Insights für das Bild abzurufen (etwa Webseiten, die das Bild enthalten, oder Personen, die auf dem Bild erkannt wurden). Ausführliche Informationen finden Sie unter [Get insights about an image](image-insights.md) (Abrufen von Insights zu einem Bild).

## <a name="resizing-and-cropping-thumbnails"></a>Ändern der Größe und Zuschneiden von Miniaturbildern

Sie können auch die Größe von Miniaturbildern ändern und diese erweitern, z.B. wenn der Cursor darüber bewegt wird.
> [!NOTE]
> Wenn das Bild erweitert wird, muss die Quelle angegeben werden. Hierzu können Sie beispielsweise den Host aus [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) extrahieren und unter dem Bild anzeigen.

[!INCLUDE [cognitive-services-bing-resize-crop-thumbnails](../../../includes/cognitive-services-bing-resize-crop-thumbnails.md)]
