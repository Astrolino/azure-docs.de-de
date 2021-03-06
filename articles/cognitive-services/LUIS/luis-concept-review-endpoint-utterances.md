---
title: Überprüfen von Endpunktäußerungen für die Verwendung des aktiven Lernens in Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Aktives Lernen ist eine von drei Strategien zur Verbesserung der Vorhersagegenauigkeit. Von diesen ist sie am einfachsten zu implementieren. Mit aktivem Lernen überprüfen Sie Endpunktäußerungen auf die Richtigkeit von Absicht und Entität. LUIS wählt Endpunktäußerungen aus, bei denen es nicht sicher ist.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 10/07/2018
ms.author: diberry
ms.openlocfilehash: 4ef6f5022b3c38eab2cda20123f179811f46390f
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637237"
---
# <a name="enable-active-learning-by-reviewing-endpoint-utterances"></a>Aktivieren des aktiven Lernens durch Überprüfen von Endpunktäußerungen
Aktives Lernen ist eine von drei Strategien zur Verbesserung der Vorhersagegenauigkeit. Von diesen ist sie am einfachsten zu implementieren. Mit aktivem Lernen überprüfen Sie Endpunktäußerungen auf die Richtigkeit von Absicht und Entität. LUIS wählt Endpunktäußerungen aus, bei denen es nicht sicher ist.

## <a name="what-is-active-learning"></a>Was ist aktives Lernen?
Aktives Lernen ist ein zweistufiger Prozess. LUIS wählt zunächst die am App-Endpunkt empfangenen Äußerungen aus, die validiert werden müssen. Der zweite Schritt wird vom App Besitzer oder Projektmitarbeiter ausgeführt. Dabei werden die ausgewählten Äußerungen [überprüft](luis-how-to-review-endoint-utt.md), um auch z.B. die richtige Absicht und eventuelle Entitäten in die Absicht zu validieren. Nach dem Überprüfen der Äußerungen trainieren und veröffentlichen Sie die App erneut. 

## <a name="which-utterances-are-on-the-review-list"></a>Äußerungen in der Prüfliste
LUIS fügt der Prüfliste Äußerungen hinzu, wenn die wichtigste ausgelöste Absicht eine niedrigere Bewertung hat oder wenn die Bewertungen der beiden wichtigsten Absichten zu ähnlich sind. 

## <a name="single-pool-for-utterances-per-app"></a>Einzelner Pool für Äußerungen pro App
Die Liste **Überprüfen von Endpunktäußerungen** ändert sich nicht je nach Version. Es muss nur ein Pool mit Äußerungen überprüft werden. Dies gilt unabhängig davon, welche Version der Äußerung Sie aktiv bearbeiten oder welche Version der App auf dem Endpunkt veröffentlicht wurde. 

## <a name="where-are-the-utterances-from"></a>Ursprung von Äußerungen
Endpunktäußerungen stammen aus Endbenutzerabfragen am HTTP-Endpunkt der Anwendung. Wenn Ihre App noch nicht veröffentlicht wurde oder noch keine Treffer empfangen hat, stehen keine Äußerungen für die Überprüfung bereit. Wenn für eine bestimmte Absicht oder Entität keine Treffer am Endpunkt empfangen werden, stehen auch keine Äußerungen zum Überprüfen bereit. 

## <a name="schedule-review-periodically"></a>Planen regelmäßiger Überprüfungen
Das Überprüfen vorgeschlagener Äußerungen muss nicht täglich erfolgen, sollte jedoch Teil Ihrer regelmäßigen Wartung von LUIS sein. 

## <a name="delete-review-items-programmatically"></a>Programmgesteuertes Löschen überprüfter Elemente
Verwenden Sie die API zum **[Löschen nicht gekennzeichneter Äußerungen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9)**. Sichern Sie diese Äußerungen vor dem Löschen, indem Sie die **[Protokolldateien exportieren](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36)**.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie, wie Sie Endpunktäußerungen [überprüfen](luis-how-to-review-endoint-utt.md).
