---
title: Bereinigen und Vorbereiten von Daten für Azure Machine Learning | Microsoft-Dokumentation
description: Vorverarbeiten und Bereinigen von Daten als Vorbereitung für das Machine Learning.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: bdf659ec-4881-4324-8b9c-747cbfa0c3cd
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: 127c51b9a2617c6b8520d972a3cd4b6c3bbcddd1
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34837693"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Aufgaben zur Vorbereitung von Daten für erweitertes Machine Learning
Die Vorverarbeitung und Bereinigung von Daten ist eine wichtige Aufgabe, die vor dem effektiven Verwenden eines Datasets für Machine Learning durchgeführt werden muss. Unformatierte Daten enthalten oft unnötige bzw. fehlende Werte und sind unzuverlässig. Die Verwendung dieser Daten für die Modellierung kann zu falschen Ergebnissen führen. Diese Aufgaben sind Teil des Team Data Science-Prozesses (TDSP). Normalerweise wird zuerst eine Untersuchung eines Datasets durchgeführt, um die erforderliche Vorverarbeitung zu ermitteln und zu planen. Eine ausführliche Anleitung zum TDSP-Prozess finden Sie in den Schritten, die unter [Team Data Science-Prozess](overview.md) beschrieben sind.

Aufgaben der Vorverarbeitung und Bereinigung, z. B. die Datenuntersuchung, können in vielen verschiedenen Umgebungen ausgeführt werden, z. B. SQL oder Hive oder Azure Machine Learning Studio, sowie mit unterschiedlichen Tools und Sprachen, z. B. R oder Python. Dies hängt davon ab, wo die Daten gespeichert sind und wie sie formatiert sind. Da der TDSP iterativ ist, können diese Aufgaben im Workflow des Prozesses in verschiedenen Schritten erfolgen.

In diesem Artikel werden verschiedene Konzepte und Aufgaben zur Datenverarbeitung beschrieben, die vor oder nach der Erfassung von Daten in Azure Machine Learning durchgeführt werden können.

Ein Beispiel für die Datenuntersuchung und Vorverarbeitung in Azure Machine Learning Studio ist im Video [Vorverarbeitung von Daten in Azure Machine Learning Studio](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) enthalten.

## <a name="why-pre-process-and-clean-data"></a>Warum müssen Daten vorverarbeitet und bereinigt werden?
Echte Daten stammen aus verschiedenen Quellen und Abläufen und können daher Unregelmäßigkeiten oder beschädigte Daten enthalten, die die Qualität des DataSets beeinträchtigen. Folgende Qualitätsprobleme treten bei Daten häufiger auf:

* **Unvollständige Informationen**: Den Daten fehlen Attribute oder Werte.
* **Überflüssige Informationen**: Die Daten enthalten fehlerhafte Datensätze oder Ausreißer.
* **Inkonsistente Informationen**: Die Daten enthalten widersprüchliche Datensätze oder Abweichungen.

Daten von hoher Qualität sind eine wichtige Voraussetzung für die Qualität von Vorhersagemodellen. Um schlechte Ergebnisse aufgrund schlechter Ausgangsdaten zu vermeiden und die Datenqualität und damit die Leistung des Modells zu verbessern, ist es unerlässlich, die Datenintegrität zu untersuchen, um Probleme mit den Daten frühzeitig zu erkennen und geeignete Schritte zur Vorverarbeitung und Bereinigung durchführen zu können.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Welche Methoden zur Überprüfung der Datenintegrität werden am häufigsten eingesetzt?
Sie können die allgemeine Qualität der Daten überprüfen, indem Sie Folgendes prüfen:

* Die Anzahl der **Datensätze**.
* Die Anzahl der **Attribute** (oder **Features**).
* Die Attribut- **Datentypen** (nominal, ordinal oder fortlaufend).
* Die Anzahl der **fehlenden Werte**.
* **Wohlgeformtheit** der Daten.
  * Wenn die Daten in TSV- oder CSV-Dateien gespeichert sind, sollten Sie prüfen, ob die Spalten- und Zeilentrennzeichen die Spalten und Zeilen auch immer richtig trennen.
  * Bei Daten im HTML- oder XML-Format überprüfen Sie, ob die Daten gemäß den jeweiligen Standards wohlgeformt sind.
  * Es ist möglicherweise eine Analyse erforderlich, um strukturierte Informationen aus teilweise strukturierten oder unstrukturierten Daten zu extrahieren.
* **Inkonsistente Datensätze**. Überprüfen Sie den zulässigen Wertebereich. Wenn die Daten z.B. die Notendurchschnitte von Schülern enthalten, sollte der Durchschnitt im festgelegten Bereich liegen, also z.B. 0 bis 4.

Wenn Sie Probleme für die Daten ermitteln, sind **Verarbeitungsschritte** erforderlich. Dazu gehören häufig die Bereinigung fehlender Werte, die Datennormalisierung, die Diskretisierung, die Textverarbeitung zum Entfernen und/oder Ersetzen eingebetteter Zeichen, die Einfluss auf die Datenausrichtung haben können, das Bereinigen gemischter Datentypen in gemeinsamen Feldern u. a.

**In Azure Machine Learning werden wohlgeformte Tabellendaten verarbeitet.**  Wenn die Daten bereits in tabellarischer Form vorliegen, kann die Datenvorverarbeitung direkt in Azure Machine Learning in Machine Learning Studio ausgeführt werden.  Wenn Daten nicht in tabellarischer Form vorliegen, sondern z. B. als XML, ist eine Analyse erforderlich, um die Daten in eine tabellarische Form zu konvertieren.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Welches sind die wichtigsten Vorgänge bei der Datenvorverarbeitung?
* **Datenbereinigung**: Auffüllen fehlender Werte, Erkennen und Entfernen überflüssiger Daten und Ausreißer.
* **Datentransformation**: Normalisierung der Daten, um Umfang und Störungen zu verringern.
* **Datenreduzierung**: Erstellen von Stichproben aus den Datensätzen oder Attributen zur einfacheren Datenverarbeitung.
* **Datendiskretisierung**: Konvertieren kontinuierlicher Attribute in kategorische Attribute zur einfacheren Verwendung in bestimmten Machine Learning-Methoden.
* **Textbereinigung**: Entfernen eingebetteter Zeichen, die zu einer falschen Datenausrichtung führen können, z. B. eingebetteter Tabstopps in tabstoppgetrennten Dateien oder eingebetteter Zeilenumbrüche, die Datensätze unterbrechen könnten, usw.

In den folgenden Abschnitten werden einige dieser Schritte zur Datenverarbeitung beschrieben.

## <a name="how-to-deal-with-missing-values"></a>Wie werden fehlende Daten behandelt?
Bei fehlenden Werten empfiehlt es sich, zunächst den Grund für die fehlenden Werte zu ermitteln, um das Problem besser angehen zu können. Folgende Vorgehensweisen werden bei fehlenden Werten häufig angewendet:

* **Löschen**: Entfernen Sie Datensätze mit fehlenden Werten.
* **Ersetzen durch Platzhalterwerte**: Ersetzen Sie fehlende Werte durch Platzhalterwerte: z.B. *unbekannt* bei kategorischen oder „0“ bei numerischen Werten.
* **Ersetzen durch Mittelwerte**: Ersetzen Sie fehlende numerische Daten durch Mittelwerte.
* **Ersetzen durch häufige Werte**: Ersetzen Sie fehlende kategorische Daten durch den häufigsten Eintrag.
* **Ersetzen durch Regressionswerte**: Verwenden Sie ein Regressionsverfahren, um fehlende Werte durch Regressionswerte zu ersetzen.  

## <a name="how-to-normalize-data"></a>Wie werden Daten normalisiert?
Bei der Datennormalisierung werden numerische Werte in einen angegebenen Bereich neu skaliert. Folgende Normalisierungsverfahren werden häufig angewendet:

* **Min-Max-Normalisierung**: Transformieren Sie die Daten linear in einen Bereich, z. B. zwischen 0 und 1. Der Mindestwert wird auf 0 skaliert und der Höchstwert auf 1.
* **Z-Wert-Normalisierung**: Skalieren Sie die Daten basierend auf Mittelwert und Standardabweichung: Teilen Sie die Differenz aus Daten und Mittelwert durch die Standardabweichung.
* **Dezimalskalierung**: Skalieren Sie die Daten durch Verschieben des Dezimaltrennzeichens des Attributwerts.  

## <a name="how-to-discretize-data"></a>Wie werden Daten diskretisiert?
Daten können durch die Konvertierung kontinuierlicher Werte in nominale Attribute oder Intervalle diskretisiert werden. Dazu gibt es u. a. folgende Möglichkeiten:

* **Festbreitengruppierung**: Teilen Sie den Bereich aller möglichen Werte eines Attributs in N Gruppen derselben Größe auf, und weisen Sie den Werten in einer Gruppe die Gruppennummer zu.
* **Festhöhengruppierung**Teilen Sie den Bereich aller möglichen Werte eines Attributs in N Gruppen mit derselben Anzahl von Instanzen auf, und weisen Sie den Werten in einer Gruppe die Gruppennummer zu.  

## <a name="how-to-reduce-data"></a>Wie werden Daten reduziert?
Es gibt verschiedene Methoden zum Reduzieren der Größe zur einfacheren Datenverarbeitung. Je nach Größe und Inhalt der Daten können folgende Verfahren angewendet werden:

* **Datensatzstichproben**: Erstellen Sie Stichproben aus den Datensätzen, und wählen Sie nur eine repräsentative Teilmenge von Daten aus.
* **Attributstichproben**: Wählen Sie nur eine Teilmenge der wichtigsten Attribute aus den Daten aus.  
* **Aggregation**: Unterteilen Sie die Daten in Gruppen, und speichern Sie die Zahlen der einzelnen Gruppen. Beispielsweise können die Tageseinnahmen einer Restaurant-Kette aus den letzten 20 Jahren im monatlichen Umsatz zusammengefasst werden, um die Größe der Daten zu verringern.  

## <a name="how-to-clean-text-data"></a>Wie werden Textdaten bereinigt?
**Textfelder in Tabellendaten** können Zeichen enthalten, die sich auf die Spaltenausrichtung und/oder die Datensatzgrenzen auswirken. Eingebettete Tabstopps in einer tabstoppgetrennten Datei verursachen z. B. Fehlausrichtungen von Spalten, während eingebettete Zeilenumbrüche Datensatzzeilen beschädigen. Eine fehlerhafte Verarbeitung von Textcodierungen beim Schreiben oder Lesen von Text kann zu Datenverlusten und unbeabsichtigten Einfügungen von unlesbaren Zeichen, z. B. NULL-Werten, führen und möglicherweise auch Auswirkungen auf die Textanalyse haben. Es ist möglicherweise eine sorgfältige Analyse und Bearbeitung erforderlich, um Textfelder für die korrekte Ausrichtung zu bereinigen und um strukturierte Daten aus unstrukturierten oder teilweise strukturierten Textdaten zu extrahieren.

**Durchsuchen von Daten** ermöglicht einen frühzeitigen Einblick in die Daten. Während dieses Schritts können bereits einige Probleme mit den Daten aufgedeckt und entsprechende Methoden angewendet werden, um diese Probleme zu beheben.  Es ist wichtig, Fragen wie die zur Ursache des Problems und dessen Ursprung zu stellen. Dadurch können Sie auch bessere Entscheidungen für die Verarbeitungsschritte treffen, die zum Lösen der Probleme erforderlich sind. Die Einblicke, die aus den Daten gewonnen werden sollen, können auch zum Festlegen von Prioritäten für die Datenverarbeitung verwendet werden.

## <a name="references"></a>Referenzen
> *Data Mining: Concepts and Techniques*, 3. Auflage, Morgan Kaufmann, 2011, Jiawei Han, Micheline Kamber und Jian Pei
> 
> 

