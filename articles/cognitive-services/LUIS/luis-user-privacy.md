---
title: 'Exportieren und Löschen von Kundendaten: Language Understanding – LUIS'
titleSuffix: Azure Cognitive Services
description: Language Understanding Intelligent Service (LUIS) speichert zwar Kundeninhalte, damit der Dienst ausgeführt werden kann, aber der LUIS-Benutzer hat die vollständige Kontrolle darüber, welche Daten er abrufen, exportieren oder löschen will. Dies kann er entweder über das LUIS-Webportal oder die programmgesteuerten LUIS-APIs tun.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: bfd168d5c56bc800831c402a23bb1a506fa15221
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883889"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Exportieren und Löschen Ihrer Kundendaten aus LUIS in Cognitive Services

## <a name="summary-of-customer-data-request-features"></a>Zusammenfassung der Features für Kundendatenanforderungen
Language Understanding Intelligent Service (LUIS) speichert zwar Kundeninhalte, damit der Dienst ausgeführt werden kann, aber der LUIS-Benutzer hat die vollständige Kontrolle darüber, welche Daten er abrufen, exportieren oder löschen will. Dies kann er entweder über das LUIS-[Webportal](luis-reference-regions.md) oder die [programmgesteuerten LUIS-APIs](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) tun.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Kundeninhalte werden verschlüsselt im regionalen Microsoft Azure-Speicher gespeichert und umfassen Folgendes:

- Inhalte zum Benutzerkonto, die bei der Registrierung erfasst wurden
- Trainingsdaten, die zum Erstellen der Modelle erforderlich sind (z.B. Absichten und Entitäten)
- Benutzerabfragen, die zur Laufzeit protokolliert werden, um die Benutzerprotokolle verbessern zu können
  - Benutzer können die Abfrageprotokollierung deaktivieren, indem sie `&log=false` an die Anforderungen anfügen. Weitere Informationen finden Sie [hier](luis-resources-faq.md#how-can-i-disable-the-logging-of-utterances).

## <a name="deleting-customer-data"></a>Löschen von Kundendaten
LUIS-Benutzer haben die vollständige Kontrolle und können sämtliche Benutzerinhalte entweder über das LUIS-Webportal oder die programmgesteuerten LUIS-APIs löschen. In der folgenden Tabelle werden Links zu Hilfen für beide Möglichkeiten angezeigt.

| | **Benutzerkonto** | **Anwendung** | **Äußerung(en)** | **Abfragen von Endbenutzern** |
| --- | --- | --- | --- | --- |
| **Portal** | [Link](luis-how-to-account-settings.md) | [Link](luis-how-to-start-new-app.md#delete-app) | [Link](luis-how-to-start-new-app.md#delete-app) | [Link](luis-how-to-start-new-app.md#delete-app) |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Exportieren von Kundendaten
LUIS-Benutzer haben zwar die vollständige Kontrolle und können die Daten im Portal abrufen, sie müssen sie jedoch über die programmgesteuerte LUIS-APIs exportieren. In der folgenden Tabelle werden Links aufgeführt, über die Sie Unterstützung beim Exportieren von Daten über die programmgesteuerte LUIS-API erhalten.

| | **Benutzerkonto** | **Anwendung** | **Äußerung(en)** | **Abfragen von Endbenutzern** |
| --- | --- | --- | --- | --- |
| **APIs** | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Link](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |


## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [LUIS regions reference (Referenz zu LUIS-Regionen)](./luis-reference-regions.md)
