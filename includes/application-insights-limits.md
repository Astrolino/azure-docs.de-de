---
title: Includedatei
description: Includedatei
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/21/2018
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 90de751f416ca611f3c674232c224199ad7af717
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310166"
---
Es gibt einige Beschränkungen hinsichtlich der Anzahl von Metriken und Ereignissen pro Anwendung (d. h. pro Instrumentationsschlüssel). Die Beschränkungen hängen von dem von Ihnen ausgewählten [Tarif](https://azure.microsoft.com/pricing/details/application-insights/) ab.

| Ressource | Standardlimit | Hinweis
| --- | --- | --- |
| Gesamtdaten pro Tag | 100 GB | Die Datenmenge kann durch Festlegen einer Obergrenze reduziert werden. Wird eine höhere Datenmenge benötigt, können Sie den Grenzwert im Portal auf bis zu 1.000 GB erhöhen. Bei Kapazitäten über 1.000 GB senden Sie eine E-Mail an AIDataCap@microsoft.com.
| Drosselung | 32.000 Ereignisse/Sekunde | Das Limit wird eine Minute lang gemessen.
| Beibehaltung von Daten | 90 Tage | Hierbei handelt es sich um eine Ressource für [Suche](../articles/application-insights/app-insights-diagnostic-search.md), [Analyse](../articles/application-insights/app-insights-analytics.md) und [Metrik-Explorer](../articles/application-insights/app-insights-metrics-explorer.md).
| [Webtests in mehreren Schritten](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) mit detaillierter Ergebnisaufbewahrung | 90 Tage | Diese Ressource liefert detaillierte Ergebnisse der einzelnen Schritte.
| Maximale Ereignisgröße | 64 K | 
| Eingenschaft und Länge der Namen von Metriken | 150 | Siehe [Typschemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Zeichenfolgenlänge des Eigenschaftswerts | 8.192 | Siehe [Typschemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Länge von Ablaufverfolgungs- und Ausnahmebenachrichtigungen | 10.000 | Siehe [Typschemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| [Verfügbarkeitstests](../articles/application-insights/app-insights-monitor-web-app-availability.md) Anzahl pro App | 100 |
| Vermerkdauer von [Profiler](../articles/application-insights/app-insights-profiler.md)-Daten | 5 Tage |
| Pro Tag gesendete [Profiler](../articles/application-insights/app-insights-profiler.md)-Daten | 10 GB |

Weitere Informationen zu Preisen und Kontingenten für Application Insights finden Sie [hier](../articles/application-insights/app-insights-pricing.md).