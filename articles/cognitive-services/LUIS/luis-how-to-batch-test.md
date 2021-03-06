---
title: Batchtest mit 1000 Beispieläußerungen
titleSuffix: Azure Cognitive Services
description: Verwenden Sie LUIS-Batchtestsets (Language Understanding), um Äußerungen mit falschen Absichten und Entitäten zu suchen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: b4c58a13c8b66add8ebd7e535ecac9d7b0f1075b
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032120"
---
# <a name="batch-testing-with-a-set-of-example-utterances"></a>Batchtests mit einem Satz von Beispieläußerungen
 Batchtests sind umfassende Tests Ihres aktuellen trainierten Modells zum Messen seiner Leistung in LUIS. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Importieren einer Datasetdatei für Batchtests

1. Wählen Sie in der oberen Leiste **Test** und dann **Batch testing panel** (Batchtestbereich) aus.

    ![Link zum Testen in Batches](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Wählen Sie **Dataset importieren** aus. Das Dialogfeld **Import new dataset** (Neues Dataset importieren) wird angezeigt. Wählen Sie **Datei auswählen** aus, und suchen Sie eine JSON-Datei mit dem richtigen [JSON-Format](luis-concept-batch-test.md#batch-file-format), die *höchstens 1.000* zu testende Äußerungen enthält.

    Fehler beim Importieren werden auf einer roten Benachrichtigungsleiste am oberen Rand des Browsers gemeldet. Wenn ein Import Fehler aufweist, wird kein Dataset erstellt. Weitere Informationen finden Sie unter [Häufige Fehler](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. Geben Sie im Feld **Datasetname** einen Namen für die Datasetdatei an. Die Datasetdatei enthält ein **Array von Äußerungen** mit *bezeichneter Absicht* und *Entitäten*. Überprüfen Sie die [Beispielbatchdatei](luis-concept-batch-test.md#batch-file-format) auf ihre Syntax. 

4. Wählen Sie **Fertig**aus. Die Datasetdatei wird hinzugefügt.

## <a name="run-rename-export-or-delete-dataset"></a>Ausführen, Umbenennen, Exportieren oder Löschen eines Datasets
Verwenden Sie zum Ausführen, Umbenennen, Exportieren oder Löschen des Datasets die Auslassungspunkte (***...*** ) am Ende der Datasetzeile.

![Datasetaktionen](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Ausführen eines Batchtests mit Ihrer trainierten App

Um den Test auszuführen, wählen Sie den Namen des Datasets aus. Wenn der Test abgeschlossen ist, wird in dieser Zeile das Testergebnis des Datasets angezeigt.

![Batchtestergebnisse](./media/luis-how-to-batch-test/run-test.png)

Das herunterladbare Dataset ist dieselbe Datei, die für die Batchtests hochgeladen wurde.

|Zustand|Bedeutung|
|--|--|
|![Symbol mit grünem Kreis für erfolgreichen Test](./media/luis-how-to-batch-test/batch-test-result-green.png)|Alle Äußerungen sind erfolgreich.|
|![Symbol mit rotem x für Fehler beim Test](./media/luis-how-to-batch-test/batch-test-result-red.png)|Mindestens eine Absicht einer Äußerung entsprach nicht der Vorhersage.|
|![Symbol für Testbereitschaft](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Der Test ist für die Ausführung bereit.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Anzeigen von Batchtestergebnissen 
Wählen Sie zum Überprüfen der Batchtestergebnisse **See results** (Ergebnisse anzeigen) aus.

![Batchtestergebnisse](./media/luis-how-to-batch-test/run-test-results.png)

<!-- Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. Green points indicate correct prediction, and red ones indicate incorrect prediction. The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.-->


<a name="filter-chart-results-by-intent-or-entity"></a>  
## <a name="filter-chart-results"></a>Filtern von Diagrammergebnissen

Um das Diagramm nach einer bestimmten Absicht oder Entität zu filtern, wählen Sie die Absicht bzw. die Entität im Filterbereich rechts aus. Die Datenpunkte und ihre Verteilung im Diagramm werden entsprechend Ihrer Auswahl aktualisiert. 
 
![Visualisierte Batchtestergebnisse](./media/luis-how-to-batch-test/filter-by-entity.png) 

## <a name="view-single-point-utterance-data"></a>Anzeigen von Einzelpunkt-Äußerungsdaten
Zeigen Sie im Diagramm auf einen Datenpunkt, um die Bewertung seiner Vorhersage anzuzeigen. Wählen Sie einen Datenpunkt aus, um die zugehörige Äußerung in der Liste der Äußerungen am unteren Rand der Seite abzurufen. 

![Ausgewählte Äußerung](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Anzeigen von Abschnittsdaten
Wählen Sie im Diagramm mit vier Abschnitten den Namen des Abschnitts aus, z.B. **Falsch positive Ergebnisse** rechts oben im Diagramm. Unter dem Diagramm werden alle Äußerungen in diesem Abschnitt in einer Liste angezeigt. 

![Ausgewählte Äußerungen nach Abschnitt](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Im vorhergehenden Bild ist die Äußerung `switch on` mit der Absicht „TurnAllOn“ bezeichnet, erhielt jedoch als Vorhersage die Absicht „None“. Dies ist ein Hinweis darauf, dass für die Absicht „TurnAllOn“ weitere Beispieläußerungen erforderlich sind, um die erwartete Vorhersage zu treffen. 

Die beiden Abschnitte des Diagramms in Rot weisen auf Äußerungen hin, bei denen die Vorhersage nicht mit der erwarteten übereinstimmte. Diese zeigen Äußerungen an, für die LUIS mehr Training benötigt. 

In den beiden Abschnitten des Diagramms in Grün entsprach die Vorhersage der Erwartung.

## <a name="next-steps"></a>Nächste Schritte

Wenn sich in Tests herausstellt, dass Ihre LUIS-App nicht die richtige Absichten und Entitäten erkennt, können Sie die Leistung Ihrer LUIS-App verbessern, indem Sie aktiv weitere Äußerungen bezeichnen oder Features hinzufügen. 

* [Bezeichnen von vorgeschlagenen Äußerungen mit LUIS](luis-how-to-review-endoint-utt.md) 
* [Verwenden von Features zum Verbessern der Leistung Ihrer LUIS-App](luis-how-to-add-features.md) 
* [Tutorial zu Grundlagen von Batchtests](luis-tutorial-batch-testing.md)
* [Erfahren Sie etwas über Batchtestkonzepte](luis-concept-batch-test.md).
