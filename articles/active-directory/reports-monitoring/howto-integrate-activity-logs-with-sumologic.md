---
title: Integrieren von Azure Active Directory-Protokollen mit SumoLogic mit Azure Monitor (Vorschauversion) | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Azure Active Directory-Protokolle mit Azure Monitor (Vorschauversion) in SumoLogic integrieren.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d13eb22bd58dc7e680a27738549665bc2b691898
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392209"
---
# <a name="integrate-azure-ad-logs-with-sumologic-by-using-azure-monitor-preview"></a>Integrieren von Azure AD-Protokollen in SumoLogic mit Azure Monitor (Vorschauversion)

In diesem Artikel erfahren Sie, wie Sie Azure Active Directory-Protokolle (Azure AD) mit Azure Monitor in SumoLogic integrieren. Zunächst leiten Sie die Protokolle an einen Azure Event Hub weiter und integrieren dann den Event Hub in SumoLogic.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Folgendes, um dieses Feature verwenden zu können:
* Ein Azure Event Hub, der Azure AD-Aktivitätsprotokolle enthält. Erfahren Sie, [wie Sie Aktivitätsprotokolle an einen Event Hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Ein SumoLogic-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Schritte zum Integrieren von Azure AD-Protokollen in SumoLogic 

1. Beginnen Sie mit dem [Streamen von Azure AD-Protokollen an einen Azure Event Hub](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Konfigurieren Sie Ihre SumoLogic-Instanz für das [Sammeln von Protokollen für Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Installieren Sie die Azure AD-App für SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards), um die vorkonfigurierten Dashboards für die Echtzeitanalyse Ihrer Umgebung zu verwenden.

 ![Dashboard](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

* [Interpretieren des Überwachungsprotokollschemas in Azure Monitor](reference-azure-monitor-audit-log-schema.md)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretieren des Anmeldeprotokollschemas in Azure Monitor)
* [Häufig gestellte Fragen und bekannte Probleme](concept-activity-logs-azure-monitor.md#frequently-asked-questions)
