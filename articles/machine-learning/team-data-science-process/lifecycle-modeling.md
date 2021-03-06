---
title: Modellierungsphase des Team Data Science-Prozess-Lebenszyklus – Azure | Microsoft-Dokumentation
description: Die Ziele, Aufgaben und Projektleistungen für die Modellierungsphase Ihrer Data Science-Projekte
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: ff7434e84473fd68153be8116899ecdfb6a6a831
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837479"
---
# <a name="modeling"></a>Modellierung

In diesem Artikel werden die Ziele, Aufgaben und Projektleistungen im Zusammenhang mit der Modellierungsphase des Team Data Science-Prozesses (TDSP) behandelt. Dieser Prozess bietet einen empfohlenen Lebenszyklus, mit dem Sie Ihre Data Science-Projekte strukturieren können. Der Lebenszyklus beschreibt die wichtigsten Phasen, die Projekte typischerweise, oft iterativ, durchlaufen:

   1. **Geschäftliche Aspekte**
   2. **Datenerfassung und -auswertung**
   3. **Modellierung**
   4. **Bereitstellung**
   5. **Kundenakzeptanz**

Dies ist eine visuelle Darstellung des TDSP-Lebenszyklus:

![TDSP-Lebenszyklus](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Ziele
* Ermitteln der optimalen Datenfeatures für das Machine Learning-Modell
* Erstellen eines informativen Machine Learning-Modells, mit dem das Ziel am genauesten vorhergesagt wird
* Erstellen eines Machine Learning-Modells, das für die Produktion geeignet ist

## <a name="how-to-do-it"></a>Vorgehensweise
In dieser Phase werden drei Hauptaufgaben durchgeführt:

  * **Featureentwicklung:** Erstellen Sie Datenfeatures aus den Rohdaten, um das Trainieren des Modells zu ermöglichen.
  * **Modelltraining:** Ermitteln Sie das Modell, mit dem die Frage am genauesten beantwortet wird, indem Sie die Erfolgsmetriken vergleichen.
  * Bestimmen Sie, ob Ihr Modell **für die Produktion geeignet** ist.

### <a name="feature-engineering"></a>Featureentwicklung
Die Featureentwicklung umfasst die Einbeziehung, Aggregation und Transformation von Rohvariablen zum Erstellen der Features, die in der Analyse verwendet werden. Wenn Sie einen Einblick in die Grundlagen eines Modells erhalten möchten, müssen Sie verstehen, wie Features miteinander in Beziehung stehen und wie die Algorithmen für maschinelles Lernen diese Features nutzen sollen. 

Dieser Schritt erfordert eine kreative Kombination von Sachkenntnis mit den im Datenuntersuchungsschritt gewonnenen Erkenntnissen. Die Featureentwicklung ist ein Balanceakt, weil Variablen mit hohem Informationsgehalt gesucht und eingefügt werden, während gleichzeitig vermieden werden muss, zu viele irrelevante Variablen einzubeziehen. Mit informativen Variablen verbessern Sie Ihr Ergebnis, aber mit wenig relevanten Variablen überladen Sie das Modell unnötig. Sie müssen diese Features auch für alle neuen Daten generieren, die während der Bewertung ermittelt werden. Deshalb kann die Generierung dieser Features nur von Daten abhängen, die zum Zeitpunkt der Bewertung verfügbar sind. 

Eine technische Anleitung zur Featureentwicklung bei Verwendung verschiedener Azure-Datentechnologien finden Sie unter [Featureentwicklung im Data Science-Prozess](create-features.md). 

### <a name="model-training"></a>Modelltraining
Je nach Art der Frage, die beantwortet werden soll, stehen gegebenenfalls relativ viele Algorithmen für die Modellierung zur Verfügung. Eine Anleitung zur Auswahl der Algorithmen finden Sie unter [Auswählen von Algorithmen für Microsoft Azure Machine Learning](../studio/algorithm-choice.md). Dieser Artikel verwendet zwar Azure Machine Learning, aber die darin enthaltenen Informationen sind für alle Machine Learning-Projekte nützlich. 

Der Prozess für das Modelltraining umfasst die folgenden Schritte: 

   * Sie können die Eingabedaten beliebig für die Modellierung eines Trainingsdatasets und eines Testdatasets **aufteilen**.
   * **Erstellen Sie die Modelle** mit dem Trainingsdataset.
   * Führen Sie eine **Auswertung** von Trainings- und Testdataset durch. Sie können eine Reihe von konkurrierenden Algorithmen for maschinelles Lernen zusammen mit den verschiedenen zugehörigen Optimierungsparametern (als *Parameter-Sweeping* bezeichnet) verwenden, die auf die Beantwortung der jeweiligen Fragen mit den aktuellen Daten ausgerichtet sind.
   * **Ermitteln Sie die „beste“ Lösung** für die Beantwortung der Frage, indem Sie die Erfolgsmetrik für die alternativen Methoden vergleichen.

> [!NOTE]
> **Vermeiden von Datenlecks:** Datenlecks können dadurch verursacht werden, dass Daten von außerhalb des Trainingsdatasets einbezogen werden, sodass ein Modell oder ein Algorithmus für maschinelles Lernen unrealistisch gute Vorhersagen ergibt. Datenlecks sind häufig der Grund dafür, warum Data Scientists nervös werden, wenn die Vorhersageergebnisse zu schön sind, um wahr zu sein. Es kann schwierig sein, diese Abhängigkeiten zu erkennen. Um Datenlecks zu vermeiden, ist häufig das wiederholte Durchlaufen der Schritte zur Erstellung eines Analyse-DataSets, Erstellung eines Modells und Auswertung der Genauigkeit der Ergebnisse erforderlich. 
> 
> 

Wir stellen für TDSP ein [Tool für die Automatisierung von Modellierung und Berichterstellung](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) bereit, mit dem mehrere Algorithmen und Parameter-Sweeping-Vorgänge durchlaufen werden können, um ein Grundmodell zu erhalten. Außerdem wird ein Basisbericht für die Modellierung erstellt, in dem die Leistung der einzelnen Modell-Parameter-Kombinationen, einschließlich Variablenwichtigkeit, zusammengefasst wird. Dieser Prozess ist ebenfalls iterativ, da er weitere Maßnahmen zur Featureentwicklung nach sich ziehen kann. 

## <a name="artifacts"></a>Artefakte
In dieser Phase werden die folgenden Artefakte produziert:

   * [Funktionssätze:](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets) Die für die Modellierung entwickelten Funktionen werden im Abschnitt **Feature Sets** (Featuresätze) des Berichts **Datendefinition** beschrieben. Er enthält Verknüpfungen zum Code, mit dem die Features generiert werden, und eine Beschreibung zur Generierung der Features.
   * [Model Report](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md) (Modellbericht): Für jedes getestete Modell wird ein vorlagenbasierter Standardbericht mit Details zu den einzelnen Experimenten erstellt.
   * **Prüfpunktentscheidung:** Bewerten Sie, ob das Modell gut genug funktioniert, um in einem Produktionssystem bereitgestellt zu werden. Einige wichtige Fragen lauten:
     * Kann die Frage mit dem Modell basierend auf den Testdaten ausreichend beantwortet werden? 
     * Sollten Alternativen ausprobiert werden? Sollten weitere Daten erfasst, weitere Features entwickelt oder Experimente mit anderen Algorithmen durchgeführt werden?

## <a name="next-steps"></a>Nächste Schritte

Hier finden Sie Links zu jedem Schritt im Lebenszyklus des TDSP:

   1. [Geschäftliche Aspekte](lifecycle-business-understanding.md)
   2. [Datenerfassung und -auswertung](lifecycle-data.md)
   3. [Modellierung](lifecycle-modeling.md)
   4. [Bereitstellung](lifecycle-deployment.md)
   5. [Kundenakzeptanz](lifecycle-acceptance.md)

Vollständige exemplarische Vorgehensweisen, in denen sämtliche Schritte im Prozess für bestimmte Szenarien gezeigt werden, sind ebenfalls verfügbar. Der Artikel mit [exemplarischen Vorgehensweisen](walkthroughs.md) enthält eine Liste der Szenarien mit Links und Kurzbeschreibungen. Die exemplarischen Vorgehensweisen zeigen auch, wie Cloud- und lokale Tools sowie Dienste in einem Workflow oder einer Pipeline zum Erstellen einer intelligenten Anwendung kombiniert werden. 

Beispiele für die Ausführung der Schritte in TDSPs mit Azure Machine Learning Studio finden Sie unter [Verwenden des TDSP mit Azure Machine Learning](http://aka.ms/datascienceprocess). 
