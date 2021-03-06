---
title: Verwenden von Aktionen mit und ohne Wartezeit in einem Unterhaltungslernmodell – Microsoft Cognitive Services | Microsoft-Dokumentation
titleSuffix: Azure
description: Erfahren Sie, wie Sie Aktionen mit und ohne Wartezeit in einem Unterhaltungslernmodell verwenden.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f349dab23b9156d3a5656e8275533ebe6a82cdf9
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45540912"
---
# <a name="wait-and-non-wait-actions"></a>Aktionen mit und ohne Wartezeit

In diesem Tutorial wird der Unterschied zwischen Aktionen mit und ohne Wartezeit im Unterhaltungslernmodul aufgezeigt.

## <a name="video"></a>Video

[![Tutorial 2 – Vorschau](http://aka.ms/cl-tutorial-02-preview)](http://aka.ms/blis-tutorial-02)

## <a name="requirements"></a>Requirements (Anforderungen)
Für dieses Tutorial muss der allgemeine Tutorialbot ausgeführt werden.

    npm run tutorial-general

## <a name="details"></a>Details

- Aktion mit Wartezeit: Nachdem das System eine Aktion mit Wartezeit ausgeführt hat, führt es keine weiteren Aktionen aus und wartet auf Benutzereingaben.
- Aktion ohne Wartezeit: Nachdem das System eine Aktion ohne Wartezeit ausgeführt hat, wählt es sofort eine andere Aktion aus (ohne zuerst auf Benutzereingaben zu warten).

## <a name="steps"></a>Schritte

### <a name="create-a-new-model"></a>Erstellen Sie ein neues Modell.

1. Klicken Sie in der Webbenutzeroberfläche auf „Neues Modell“.
2. Geben Sie im Feld „Name“ die Zeichenfolge „WaitNonWait“ ein. Klicken Sie dann auf „Erstellen“.

### <a name="create-the-first-wait-action"></a>Erstellen der ersten Aktion mit Wartezeit

1. Klicken Sie auf „Aktionen“ und dann auf „Neue Aktion“.
2. Geben Sie für „Antwort“ die Frage „Welches Tier möchten Sie?“ ein.
    - Dies ist eine Aktion mit Wartezeit. Lassen Sie daher das Kontrollkästchen „Auf Antwort warten“ aktiviert.
3. Klicken Sie auf „Erstellen“.

### <a name="create-a-non-wait-action"></a>Erstellen einer Aktion ohne Wartezeit

1. Klicken Sie auf „Neue Aktion“.
2. Geben Sie für „Antwort“ den Satz „Kühe sagen muh“ ein.
3. Deaktivieren Sie das Kontrollkästchen „Auf Antwort warten“.
4. Klicken Sie auf „Erstellen“.

### <a name="create-a-second-non-wait-action"></a>Erstellen einer zweiten Aktion ohne Wartezeit

1. Klicken Sie auf „Neue Aktion“.
2. Geben Sie für „Antwort“ den Satz „Enten sagen quak“ ein.
3. Deaktivieren Sie das Kontrollkästchen „Auf Antwort warten“.
4. Klicken Sie auf „Erstellen“.

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Trainieren des Bots

1. Klicken Sie auf „Trainingsdialoge“ und dann auf „Neuer Trainingsdialog“.
2. Geben Sie „hallo“ ein.
3. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Welches Tier möchten Sie?“ aus.
4. Geben Sie „Kuh“ ein.
5. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Kühe sagen muh“ aus.
    - Der Bot wartet nicht auf eine Eingabe und führt die nächste Aktion aus.
2. Wählen Sie „Welches Tier möchten Sie?“ aus.
3. Geben Sie „Ente“ ein.
5. Klicken Sie auf „Bewertungsaktionen“, und wählen Sie „Enten sagen quak“ aus.

![](../media/tutorial2_dialogs.PNG)

> [!NOTE]
> Beachten Sie die Reihenfolge der Botantworten im Hinblick auf die Aktionen mit und ohne Wartezeit.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Einführung in Entitäten](./3-introduction-to-entities.md)
