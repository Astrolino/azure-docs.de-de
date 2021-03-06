---
title: Übersicht über das Azure DevOps-Projekt | Microsoft-Dokumentation
description: Grundlegendes zum Nutzen für das Azure DevOps-Projekt
services: devops-project
documentationcenter: ''
author: mlearned
manager: douge
editor: mlearned
ms.assetid: ''
ms.service: devops-project
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 05/03/2018
ms.author: mlearned
ms.openlocfilehash: 39dffad597b8382dea4df6fa1b0726d9582d67d1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/10/2018
ms.locfileid: "44293625"
---
# <a name="overview-of-azure-devops-project"></a>Übersicht über das Azure DevOps-Projekt

Das Azure DevOps-Projekt erleichtert die ersten Schritte mit Azure. Mit der DevOps-Projektressource können Sie Ihren bevorzugten App-Typ unter dem Azure-Dienst Ihrer Wahl in wenigen kurzen Schritten über das Azure-Portal starten. Das DevOps-Projekt richtet alles ein, was Sie zum Entwickeln, Bereitstellen und Überwachen Ihrer Anwendung benötigen.
Über das Dashboard des DevOps-Projekts können Sie Codecommits, Buildvorgänge und Bereitstellungen in einer zentralen Ansicht im Azure-Portal überwachen.

## <a name="why-should-i-use-the-azure-devops-project"></a>Gründe für die Verwendung des Azure DevOps-Projekts

Das Azure DevOps-Projekt automatisiert die Einrichtung einer gesamten CI- und CD-Pipeline (Continuous Integration und Continuous Delivery) für Azure.  Sie können mit bereits vorhandenem Code beginnen oder eine der bereitgestellten Beispielanwendungen verwenden und anschließend schnell die jeweilige Anwendung für verschiedene Azure-Dienste wie Virtual Machines, App Service, Azure Container Service, Azure SQL-Datenbank und Azure Service Fabric bereitstellen.  

Das Azure DevOps-Projekt nimmt Ihnen die gesamte Erstkonfiguration einer DeOps-Pipeline ab – von der Einrichtung des anfänglichen Git-Repositorys und der Konfiguration der CI/CD-Pipeline über die Erstellung einer Application Insights-Ressource für die Überwachung bis hin zur Bereitstellung einer zentralen Ansicht der gesamten Lösung durch Erstellung eines Azure DevOps-Projektdashboards im Azure-Portal.

Das Azure DevOps-Projekt ermöglicht Folgendes:

* Schnelles Bereitstellen Ihrer Anwendung in Azure
* Automatisieren der Einrichtung einer CI/CD-Pipeline
* Verwenden des DevOps-Projekts als Vorlage, anhand der Sie die ordnungsgemäße Einrichtung von CI/CD für Azure mit Azure DevOps betrachten und nachvollziehen können
* Ausführen erster Schritte mit der CI/CD-Pipeline für Azure und weiteres Anpassen der Releasepipeline auf der Grundlage Ihrer spezifischen Szenarien

## <a name="how-do-i-use-the-azure-devops-project"></a>Verwenden des Azure DevOps-Projekts

Das Azure DevOps-Projekt steht über das Azure-Portal zur Verfügung.  Eine Azure DevOps-Projektressource wird im Portal auf die gleiche Weise erstellt wie jede andere Azure-Ressource.  Das DevOps-Projekt bietet einen Assistenten zum Durchlaufen der verschiedenen Konfigurationsoptionen.  

Bei der Ersteinrichtung müssen mehrere Konfigurationsoptionen ausgewählt werden.  Die Optionen umfassen:

* Verwenden der bereitgestellten Beispiel-App oder Ihres eigenen Codes
* Auswählen einer App-Sprache
* Auswählen eines App-Frameworks auf der Grundlage der Sprache
* Auswählen eines Azure-Diensts (Bereitstellungsziel)
* Azure DevOps-Organisation (neu oder bereits vorhanden)
* Wählen Sie Ihr Azure-Abonnement aus.
* Auswählen des Standorts von Azure-Diensten
* Auswählen eines Tarifs für Azure-Dienste

Nach Verwendung des Azure DevOps-Projekts können die Ressourcen auch alle von einem zentralen Ort aus über das Azure DevOps-Projektdashboard im Azure-Portal gelöscht werden.

## <a name="azure-devops-project-and-azure-devops-integration"></a>Azure DevOps-Projekt und Azure DevOps-Integration

DevOps-Projekte basieren auf Azure DevOps.  Das DevOps-Projekt automatisiert sämtliche Arbeiten, die in Azure DevOps zum Einrichten von CI/CD für Azure erforderlich sind.  Ein Git-Repository wird unter einer neuen oder vorhandenen Azure DevOps-Organisation erstellt.  Das DevOps-Projekt committet eine Beispielanwendung oder Ihren vorhandenen Code in einem neuen Git-Repository.  Die Automatisierung richtet auch einen CI-Trigger für den Build ein, sodass nach jedem neuen Codecommit ein Buildvorgang initiiert wird.  Darüber hinaus erstellt das DevOps-Projekt einen CD-Trigger und stellt jeden neuen erfolgreichen Build für den Azure-Dienst Ihrer Wahl bereit.  Build- und Releasepipelines können für zusätzliche Szenarien angepasst werden.  Darüber hinaus können Sie die Build- und die Releasepipelines für die Verwendung in anderen Projekten klonen.

Nach der Erstellung des DevOps-Projekts haben Sie folgende Möglichkeiten:

* Anpassen der Build- und Releasepipeline
* Verwenden von Pullanforderungen, um den Codefluss zu verwalten und eine hohe Qualität zu gewährleisten
* Testen und Erstellen jedes Commits vor dem Zusammenführen Ihres Codes zur Erhöhung der Qualität
* Direktes Nachverfolgen von Projektbacklog und Problemen zusammen mit Ihrer Anwendung

## <a name="how-do-i-start-using-the-azure-devops-project"></a>Informationen zum Einstieg in die Verwendung des Azure DevOps-Projekts

* [Create a CI/CD pipeline for your existing code with the Azure DevOps Project](https://docs.microsoft.com/azure/devops-project/azure-devops-project-github) (Erstellen einer CI/CD-Pipeline für Ihren vorhandenen Code mit dem Azure DevOps-Projekt)

## <a name="azure-devops-project-videos"></a>Videos zum Azure DevOps-Projekt

* [Creating your CI/CD Pipeline with VSTS into Azure](https://channel9.msdn.com/Events/Connect/2017/T174/player/) (Erstellen Ihrer CI/CD-Pipeline mit VSTS für Azure)
