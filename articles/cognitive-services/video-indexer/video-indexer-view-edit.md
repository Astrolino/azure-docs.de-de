---
title: Anzeigen und Bearbeiten von Video Indexer-Auswertungen
titlesuffix: Azure Cognitive Services
description: In diesem Thema wird veranschaulicht, wie Sie Video Indexer-Erkenntnisse anzeigen und bearbeiten.
services: cognitive services
author: juliako
manager: cgronlun
ms.service: cognitive-services
ms.component: video-indexer
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: juliako
ms.openlocfilehash: c9b229e2fb3297d724ec8de02bf54e9765689ab7
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984537"
---
# <a name="view-and-edit-video-indexer-insights"></a>Anzeigen und Bearbeiten von Video Indexer-Auswertungen

In diesem Thema wird veranschaulicht, wie Sie die Video Indexer-Erkenntnisse eines Videos anzeigen und bearbeiten.

1. Navigieren Sie zur [Video Indexer](https://www.videoindexer.ai/)-Website, und melden Sie sich an.
2. Suchen Sie nach einem Video, für das Sie Ihre Video Indexer-Erkenntnisse erstellen möchten. Weitere Informationen finden Sie unter [Suchen bestimmter Momente in Videos](video-indexer-search.md).
3. Klicken Sie auf die Schaltfläche für die **Wiedergabe**.

    Auf der Seite werden die zusammengefassten Informationen des Videos dargestellt. 

    ![Einblicke](./media/video-indexer-view-edit/video-indexer-summarized-insights.png)

4. Zeigen Sie die zusammengefassten Informationen des Videos an. 

    Die Zusammenfassung der Erkenntnisse enthält eine aggregierte Ansicht der Daten: Gesichter, Schlüsselwörter, Stimmungen. Beispielsweise werden Informationen zu den Gesichtern von Personen und die zugehörigen Zeitbereiche sowie die prozentuale Sichtbarkeit der einzelnen Gesichter angegeben.

    Der Player und die Erkenntnisse werden synchronisiert. Wenn Sie beispielsweise auf ein Schlüsselwort oder die Transkriptzeile klicken, gelangen Sie im Player zum entsprechenden Moment des Videos. Sie können die Player-Ansicht bzw. die Ansicht für Erkenntnisse und die Synchronisierung in Ihrer Anwendung einrichten. Weitere Informationen finden Sie unter [Einbetten von Video Indexer-Widgets in Ihre Anwendungen](video-indexer-embed-widgets.md). 

3. Bearbeiten Sie die Video Indexer-Erkenntnisse.

    Wählen Sie unter dem Video die Schaltfläche „Bearbeiten“. Die Seite mit der vollständigen Aufstellung des Videos wird angezeigt. Die Aufstellung ist in einzelne Blöcke unterteilt. Die Blöcke sollen das Durchgehen der Daten vereinfachen. Die Unterteilung in Blöcke kann beispielsweise darauf basieren, dass sich der Sprecher ändert oder dass es zu einer längeren Pause kommt. Sie können Ihre eigene Wiedergabeliste erstellen, die nur die gewünschten Zeilen enthält. Wenn Sie nur bestimmte Teile des Quellvideos anzeigen möchten, können Sie nach Themen/Schlüsselwörtern, Stimmungen, Personen oder Sprechern filtern. Sie können auch angeben, dass nur das Transkript oder die OCR-Daten des Videos angezeigt werden sollen.  

    ![Einblicke](./media/video-indexer-view-edit/video-indexer-create-new-playlist.png)

## <a name="next-steps"></a>Nächste Schritte

[Informieren Sie sich über die Erstellung Ihrer eigenen Video Indexer-Erkenntnisse basierend auf einem anderen Video](video-indexer-create-new.md).

## <a name="see-also"></a>Weitere Informationen

[Was ist Video Indexer?](video-indexer-overview.md)

