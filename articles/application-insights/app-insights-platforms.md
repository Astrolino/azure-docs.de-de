---
title: 'Application Insights: Programmiersprachen, Plattformen und Integrationsmöglichkeiten | Microsoft Docs'
description: Verfügbare Programmiersprachen, Plattformen und Integrationsmöglichkeiten für Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 09/01/2016
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: 4f474ad234c80a0dcb5a9f704a263a97e7df0cc1
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2018
ms.locfileid: "39174231"
---
# <a name="developer-analytics-languages-platforms-and-integrations"></a>Entwickleranalysen: Programmiersprachen, Plattformen und Integrationsmöglichkeiten
Bei diesen Elementen handelt es sich um Implementierungen von [Application Insights](app-insights-overview.md), von denen wir gehört haben. Einige davon stammen von Drittanbietern.

## <a name="languages---officially-supported-by-application-insights-team"></a>Programmiersprachen – offiziell vom Application Insights-Team unterstützt
* [C#|VB (.NET)](app-insights-asp-net.md)
* [Java](app-insights-java-get-started.md)
* [JavaScript-Webseiten](app-insights-javascript.md)
* [Node.JS](app-insights-nodejs.md)

## <a name="languages---community-supported"></a>Programmiersprachen – von der Community unterstützt
* [F#](https://safe-stack.github.io/docs/template-azure-ai/)
* [PHP](https://github.com/Microsoft/ApplicationInsights-PHP)
* [Python](https://pypi.python.org/pypi/applicationinsights/0.1.0)
* [Ruby](https://rubygems.org/gems/application_insights)
* [Alles andere](#projects)

## <a name="platforms-and-frameworks"></a>Plattformen und Frameworks
* [ASP.NET](app-insights-asp-net.md)
* [ASP.NET – Für Apps, die bereits aktiv sind.](app-insights-monitor-performance-live-website-now.md)
* [ASP.NET Core](app-insights-asp-net-core.md)
* [Android](app-insights-mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [Angular](https://github.com/MarkPieszak/angular-application-insights)
* [Azure-Web-Apps](app-insights-azure-web-apps.md)
* [Azure Cloud Services](app-insights-cloudservices.md): Einschließlich Web- und Workerrollen
* [Azure-Funktionen](https://github.com/christopheranderson/azure-functions-app-insights-sample)
* [Docker](app-insights-docker.md)
* [Glimpse](https://azure.microsoft.com/blog/glimpse-application-insights/)
* [iOS](app-insights-mobile-center-quickstart.md) (App Center)
* [Ionic](https://github.com/SoftwarePioniere/ionic-application-insights)
* [iOS](https://github.com/Microsoft/ApplicationInsights-iOS) (App Center)
* [J2EE](app-insights-java-get-started.md)
* [J2EE – Für Apps, die bereits aktiv sind.](app-insights-java-live.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [OSX](https://github.com/Microsoft/ApplicationInsights-OSX)
* [SAFE Stack](https://safe-stack.github.io/docs/template-azure-ai/)
* [Spring](http://joe.blog.freemansoft.com/2015/12/enabling-microsoft-application-insight.html)
* [Universelle Windows-App](app-insights-mobile-center-quickstart.md) (App Center)
* [WCF](https://github.com/Microsoft/ApplicationInsights-SDK-Labs/blob/master/WCF/readme.md)
* [Windows-Desktopanwendungen, -Dienste und -Workerrollen](app-insights-windows-desktop.md)
* [Alles andere](#projects)

## <a name="logging-frameworks"></a>Protokollierungsframeworks
* [Log4Net, NLog oder System.Diagnostics.Trace](app-insights-asp-net-trace-logs.md)
* [Java, Log4J oder Logback](app-insights-java-trace-logs.md)
* [Semantic Logging (SLAB):](https://github.com/fidmor89/SLAB_AppInsights) In [Semantic Logging Application Block](https://msdn.microsoft.com/library/dn440729.aspx) integriert
* [Cloudbasierte Auslastungstests](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/30/getting-application-insights-counters-with-cloud-based-load-testing.aspx)
* [LogStash-Plug-in](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Log Analytics](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)
* [Logary](https://www.nuget.org/packages/Logary.Targets.AppInsights/)
* [Logrus](https://github.com/jjcollinge/logrus-appinsights)

## <a name="content-management-systems"></a>Content Management-Systeme
* [Concrete](https://github.com/fidmor89/appInsights-Concrete)
* [Drupal](https://github.com/fidmor89/AppInsights-Drupal)
* [Joomla](https://github.com/fidmor89/AppInsights-Joomla)
* [Orchard](https://azure.microsoft.com/blog/integrating-application-insights-into-a-modular-cms-and-a-multi-tenant-public-saas/preview/)
* [SharePoint](app-insights-sharepoint.md)
* [WordPress](https://wordpress.org/plugins/application-insights/)

## <a name="export-and-data-analysis"></a>Export und Datenanalyse
* [Alooma](https://www.alooma.com/blog/application-insights-amazon-redshift)
* [Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](app-insights-export-power-bi.md)

## <a name="projects"></a> Erstellen eines eigenen SDK
Wenn für Ihre Programmiersprache oder Plattform noch kein SDK vorhanden ist, können Sie eins erstellen. Sehen Sie sich den Code der vorhandenen SDKs an, die im [Application Insights-SDK-Projekt auf GitHub](https://github.com/Microsoft/AppInsights-Home)aufgeführt sind.
