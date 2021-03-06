---
title: Erweiterte Azure Stack-Evaluierungsaufgaben | Microsoft-Dokumentation
description: Dieser Artikel beschreibt erweiterte Azure Stack-Evaluierungsaufgaben.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0b65f37310b911184c05fd8fb8554eeb824814b8
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364811"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Erweiterte Azure Stack Development Kit-Evaluierungsaufgaben
Wenn Sie sich mit den grundlegenden Dienstfeatures und -funktionen des Azure Stack Development Kits (ASDK) vertraut gemacht haben, können Sie Ihre Kenntnisse zu Azure Stack vertiefen, indem Sie erweiterte Szenarien ausprobieren. Diese erweiterten Evaluierungsaufgaben sind vollständig in der Dokumentation für Azure Stack-Bediener beschrieben.

> [!NOTE]
> Viele Aufgaben für Bediener werden sowohl für das ASDK als auch für Azure Stack-Produktionsbereitstellungen mit mehreren Knoten unterstützt, es werden aber nicht alle Nutzungsszenarien für ASDK-Bereitstellungen unterstützt. Weitere Informationen finden Sie unter [Unterschiede zwischen ASDK und Azure Stack mit mehreren Knoten](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences).

## <a name="delegate-offers-in-azure-stack"></a>Delegieren von Angeboten in Azure Stack
Als Azure Stack-Bediener müssen Sie häufig andere Personen in die Lage versetzen, Angebote zu erstellen und Benutzer zu registrieren. Als Dienstanbieter z.B. müssen Sie Ihren Vertriebspartnern ermöglichen können, in Ihrem Auftrag Kunden zu registrieren und diese zu verwalten. Wenn Sie einer zentralen IT-Gruppe in einem Unternehmen angehören, möchten Sie vielleicht, dass Niederlassungen ohne Ihr Eingreifen Benutzer registrieren können.

Bei diesen Aufgaben ist die [Delegierung von Angeboten in Azure Stack](.\.\azure-stack-delegated-provider.md) eine nützliche Funktion, mit der mehr Benutzer erreicht und verwaltet werden können als bei einer direkten Bearbeitung. 

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>Verfügbarmachen von SQL-Datenbanken für Ihre Azure Stack-Benutzer
Als Azure Stack-Bediener können Sie Angebote erstellen, die es Ihren Benutzern (Mandanten) ermöglichen, SQL-Datenbanken zu erstellen, die sie mit ihren cloudbasierten Apps, Websites und Workloads verwenden können. Die Bereitstellung dieser benutzerdefinierten, bedarfsgesteuerten, cloudbasierten Datenbanken für Ihre Benutzer spart diesen Zeit und Ressourcen. 

Verwenden Sie den SQL Server-Ressourcenanbieteradapter, um [Ihren Azure Stack-Benutzern SQL-Datenbanken als Azure Stack-Dienst zur Verfügung zu stellen](.\.\azure-stack-tutorial-sql-server.md). Nachdem Sie den Ressourcenanbieter installiert haben, verbinden Sie ihn mit mindestens einer SQL Server-Instanz.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Verfügbarmachen von Web- und API-Apps für Ihre Azure Stack-Benutzer
Als Azure Stack-Bediener können Sie Angebote erstellen, die es Ihren Benutzern (Mandanten) ermöglichen, Azure Functions sowie Web- und API-Anwendungen zu erstellen. Die Bereitstellung des Zugriffs auf diese bedarfsgesteuerten, cloudbasierten Apps für Ihre Benutzer spart diesen Zeit und Ressourcen.

Stellen Sie den App Service-Ressourcenanbieter bereit, um [Ihren Azure Stack-Benutzern Web- und API-Apps zur Verfügung zu stellen](.\.\azure-stack-tutorial-app-service.md).

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Anbieten von Diensten mit integrierten Azure Stack-Systemen](.\.\azure-stack-offer-services-overview.md)