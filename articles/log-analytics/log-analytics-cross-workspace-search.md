---
title: Ressourcenübergreifende Suche mit Azure Log Analytics | Microsoft Docs
description: In diesem Artikel wird beschrieben, wie Sie Abfragen für Ressourcen aus mehreren Arbeitsbereichen und für Daten aus einer Application Insights-App in Ihrem Abonnement ausführen können.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: cc7bcefe43eed524f50fcad09d7fb31a80d22b83
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50025802"
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Ausführen ressourcenübergreifender Protokollsuchen in Log Analytics  

Mit Azure Log Analytics konnten Sie bislang nur Daten innerhalb des aktuellen Arbeitsbereichs analysieren. Abfragen für mehrere in Ihrem Abonnement definierte Arbeitsbereiche waren nicht möglich.  Darüber hinaus konnten Sie Telemetrieelemente, die von Ihrer webbasierten Anwendung mit Application Insights gesammelt wurden, nur direkt in Application Insights oder über Visual Studio suchen.  Dadurch wurde auch die gemeinsame native Analyse von Betriebs- und Anwendungsdaten zu einer Herausforderung.   

Sie können jetzt nicht nur Abfragen über mehrere Log Analytics-Arbeitsbereiche ausführen, sondern auch Daten aus einer bestimmten Application Insights-App in der gleichen Ressourcengruppe, einer anderen Ressourcengruppe oder einem anderen Abonnement in Abfragen einbeziehen. Dies bietet Ihnen eine systemweite Ansicht Ihrer Daten.  Sie können diese Arten von Abfragen nur in [Log Analytics](log-analytics-log-search-portals.md#log-analytics-page) ausführen. Die Anzahl der Ressourcen (Log Analytics-Arbeitsbereiche und Application Insights-App), die Sie in eine einzelne Abfrage einschließen können, ist auf 100 beschränkt. 

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Abfragen über mehrere Log Analytics-Arbeitsbereiche und mit Application Insights
Um einen anderen Arbeitsbereich in der Abfrage anzugeben, verwenden Sie die [*Arbeitsbereichs*](https://docs.microsoft.com/azure/log-analytics/query-language/workspace-expression)-ID, und verwenden Sie für eine App in Application Insights die [*App*](https://docs.microsoft.com/azure/log-analytics/query-language/app-expression)-ID.  

### <a name="identifying-workspace-resources"></a>Identifizieren von Arbeitsbereichsressourcen
In den folgenden Beispielen werden Abfragen für Log Analytics-Arbeitsbereiche gezeigt, mit denen die zusammengefasste Anzahl von Protokollen aus der Tabelle „Update“ für einen Arbeitsbereich namens *contosoretail-it* zurückgegeben wird. 

Ein Arbeitsbereich kann auf mehrere Arten identifiziert werden:

* Ressourcenname: Ein für Benutzer lesbarer Name des Arbeitsbereichs (manchmal auch *Komponentenname* genannt). 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Bei der Identifizierung eines Arbeitsbereichs über seinen Namen wird vorausgesetzt, dass er in allen zugänglichen Abonnements eindeutig ist. Sollten mehrere Anwendungen mit dem angegebenen Namen vorhanden sein, ist die Abfrage aufgrund der Mehrdeutigkeit nicht erfolgreich. In diesem Fall muss einer der anderen Bezeichner verwendet werden.

* Qualifizierter Name: Der vollständige Name des Arbeitsbereichs, bestehend aus Abonnementname, Ressourcengruppe und Komponentenname im folgenden Format: *Abonnementname/Ressourcengruppe/Komponentenname*. 

    `workspace('contoso/contosoretail/contosoretail-it').Update | count `

    >[!NOTE]
    >Da die Namen von Azure-Abonnements nicht eindeutig sind, ist dieser Bezeichner unter Umständen mehrdeutig. 
    >

* Arbeitsbereichs-ID: Bei einer Arbeitsbereichs-ID handelt es sich um den eindeutigen, unveränderlichen Bezeichner, der jedem Arbeitsbereich zugewiesen ist und als GUID (Globally Unique Identifier) dargestellt wird.

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure-Ressourcen-ID: Die von Azure definierte, eindeutige Identität des Arbeitsbereichs. Verwenden Sie die Ressourcen-ID, wenn der Ressourcenname nicht eindeutig ist.  Das Format für Arbeitsbereiche lautet wie folgt: */subscriptions/<Abonnement-ID>/resourcegroups/<Ressourcengruppe>/providers/Microsoft.OperationalInsights/workspaces/<Komponentenname>*.  

    Beispiel: 
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Update | count
    ```

### <a name="identifying-an-application"></a>Identifizieren einer Anwendung
In den folgenden Beispielen wird die zusammengefasste Anzahl der Anforderungen für eine App namens *fabrikamapp* in Application Insights zurückgegeben. 

Eine Anwendung in Application Insights kann mit dem Ausdruck *app(Identifier)* identifiziert werden.  Das *Identifier*-Argument gibt die App mit einem der folgenden Namen an:

* Ressourcenname: Ein für Benutzer lesbarer Name der App (manchmal auch *Komponentenname* genannt).  

    `app("fabrikamapp")`

* Qualifizierter Name: Der vollständige Name der App, bestehend aus Abonnementname, Ressourcengruppe und Komponentenname im folgenden Format: *Abonnementname/Ressourcengruppe/Komponentenname*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Da die Namen von Azure-Abonnements nicht eindeutig sind, ist dieser Bezeichner unter Umständen mehrdeutig. 
    >

* ID: Die App-GUID der Anwendung.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure-Ressourcen-ID: Die von Azure definierte, eindeutige Identität der App. Verwenden Sie die Ressourcen-ID, wenn der Ressourcenname nicht eindeutig ist. Das Format ist: */subscriptions/<Abonnement-ID>/resourcegroups/<Ressourcengruppe>/providers/microsoft.OperationalInsights/components/<Komponentenname>*.  

    Beispiel: 
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

### <a name="performing-a-query-across-multiple-resources"></a>Durchführen einer mehrere Ressourcen übergreifenden Abfrage
Sie können mehrere Ressourcen aus beliebigen Ihrer Ressourceninstanzen abfragen, dabei können Arbeitsbereiche und Apps kombiniert werden.
    
Beispiel für eine zwei Arbeitsbereiche übergreifende Abfrage:    

```
union Update, workspace("contosoretail-it").Update, workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update
| where TimeGenerated >= ago(1h)
| where UpdateState == "Needed"
| summarize dcount(Computer) by Classification
```

## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich in der [Referenz zur Log Analytics-Suche](https://docs.microsoft.com/azure/log-analytics/query-language/kusto) über alle Optionen der Abfragesyntax, die in Log Analytics zur Verfügung stehen.    
