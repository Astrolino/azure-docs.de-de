---
title: Azure Security Center-Tutorial – Definieren und Bewerten von Sicherheitsrichtlinien | Microsoft-Dokumentation
description: Azure Security Center-Tutorial – Definieren und Bewerten von Sicherheitsrichtlinien
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: fcd3c2a95cea0a838fc16149a0a74fad95ea3300
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027060"
---
# <a name="tutorial-define-and-assess-security-policies"></a>Tutorial: Definieren und Bewerten von Sicherheitsrichtlinien
Mit Security Center kann sichergestellt werden, dass die Sicherheitsanforderungen von Unternehmen oder Behörden erfüllt werden, indem Sicherheitsrichtlinien zum Definieren der gewünschten Konfiguration Ihrer Workloads verwendet werden. Nachdem Sie die Richtlinien für Ihre Azure-Abonnements definiert und an den Workloadtyp bzw. die Empfindlichkeit Ihrer Daten angepasst haben, kann Security Center Sicherheitsempfehlungen für Ihre Compute-, Anwendungs-, Netzwerk-, Daten- und Speicher- sowie Identitäts- und Zugriffsressourcen bereitstellen. In diesem Lernprogramm lernen Sie Folgendes:

> [!div class="checklist"]
> * Konfigurieren der Sicherheitsrichtlinie
> * Bewerten der Sicherheit Ihrer Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchlaufen der in diesem Tutorial behandelten Features müssen Sie den Tarif „Standard“ von Security Center verwenden. Sie können Security Center Standard die ersten 60 Tage kostenlos testen. Unter [Schnellstarthandbuch zu Azure Security Center](security-center-get-started.md) wird Schritt für Schritt beschrieben, wie Sie das Upgrade auf den Tarif „Standard“ durchführen.

## <a name="configure-security-policy"></a>Konfigurieren der Sicherheitsrichtlinie
Security Center erstellt für jedes Ihrer Azure-Abonnements automatisch eine Standardsicherheitsrichtlinie. Sicherheitsrichtlinien umfassen Empfehlungen, die Sie gemäß den Sicherheitsanforderungen des jeweiligen Abonnements aktivieren oder deaktivieren können. Wenn Sie Änderungen an der Standardsicherheitsrichtlinie vornehmen möchten, müssen Sie ein Besitzer, Mitwirkender oder Sicherheitsadministrator des Abonnements sein.

1. Wählen Sie im Hauptmenü von Security Center die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, das Sie verwenden möchten.

  ![Sicherheitsrichtlinie](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

3. Klicken Sie unter **Compute und Apps** auf **Netzwerk** und auf **Daten**, und **aktivieren** Sie jede Sicherheitskonfiguration, die Sie überwachen müssen. Security Center bewertet ständig die Konfiguration Ihrer Umgebung und generiert eine Sicherheitsempfehlung, falls ein Sicherheitsrisiko besteht. Wählen Sie **Aus**, falls die Sicherheitskonfiguration nicht empfehlenswert oder relevant ist. In einer Entwicklungs- oder Testumgebung ist beispielsweise nicht der gleiche Grad an Sicherheit wie in einer Produktionsumgebung erforderlich. Klicken Sie nach dem Auswählen der Richtlinien, die für Ihre Umgebung gelten, auf **Speichern**.

  ![Sicherheitskonfiguration](./media/tutorial-security-policy/tutorial-security-policy-fig6.png)  

Warten Sie, bis Security Center diese Richtlinien verarbeitet und Empfehlungen generiert hat. Einige Konfigurationen, z.B. Systemupdates und Betriebssystemkonfigurationen, können bis zu zwölf Stunden dauern, während Netzwerksicherheitsgruppen und Verschlüsselungskonfigurationen nahezu sofort bewertet werden können. Wenn die Empfehlungen im Security Center-Dashboard angezeigt werden, können Sie mit dem nächsten Schritt fortfahren.

## <a name="assess-security-of-resources"></a>Bewerten der Sicherheit von Ressourcen
1. Basierend auf den aktivierten Sicherheitsrichtlinien stellt Security Center je nach Bedarf eine Gruppe von Sicherheitsempfehlungen bereit. Sie sollten beginnen, indem Sie den virtuellen Computer und die Computerempfehlungen prüfen. Klicken Sie im Security Center-Dashboard auf **Übersicht** und dann auf **Compute und Apps**.

  ![Compute](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Überprüfen Sie die einzelnen Empfehlungen, und sehen Sie sich dabei die Empfehlungen in Rot (hohe Priorität) zuerst an. Einige dieser Empfehlungen verfügen über Lösungen, die direkt über Security Center implementiert werden können, z.B. [Endpoint Protection-Probleme](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection). Andere Empfehlungen enthalten nur Richtlinien zur Anwendung einer Lösung, z.B. bei der Empfehlung zur fehlenden Datenträgerverschlüsselung.

2. Nachdem Sie alle relevanten Computeempfehlungen abgearbeitet haben, können Sie mit dem nächsten Bereich fortfahren: Netzwerk. Klicken Sie im Security Center-Dashboard auf **Übersicht** und dann auf **Netzwerk**.

  ![Netzwerk](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  Die Seite mit den Netzwerkempfehlungen enthält eine Liste mit Sicherheitsproblemen für die Bereiche Netzwerkkonfiguration, Endpunkte mit Internetzugriff und Netzwerktopologie. Wie bei **Compute und Apps** verfügen einige Netzwerkempfehlungen über integrierte Lösungen, während dies für andere nicht der Fall ist.

3. Nachdem Sie alle relevanten Netzwerkempfehlungen abgearbeitet haben, können Sie mit dem nächsten Bereich fortfahren: Speicher und Daten. Klicken Sie im Security Center-Dashboard auf **Übersicht** und dann auf **Daten und Speicher**.

  ![Datenressourcen](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  Die Seite **Datenressourcen** enthält Empfehlungen zur Aktivierung der Überwachung für Azure SQL-Server und -Datenbanken sowie der Verschlüsselung für SQL-Datenbanken und für Ihr Azure Storage-Konto. Falls diese Workloads bei Ihnen nicht vorhanden sind, werden keine Empfehlungen angezeigt. Wie bei **Compute und Apps** verfügen einige Empfehlungen für Daten und Speicher über integrierte Lösungen, während dies für andere nicht der Fall ist.

4. Nachdem Sie alle relevanten Empfehlungen für Daten und Speicher abgearbeitet haben, können Sie mit der nächsten Workload fortfahren: Identität und Zugriff. Klicken Sie im Security Center-Dashboard auf **Übersicht** und dann auf **Identity & access** (Identität und Zugriff).

  ![Identität und Zugriff](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  Die Seite **Identity & Access** (Identität und Zugriff) enthält Empfehlungen, wie z.B.:

   - MFA für privilegierte Konten in Ihrem Abonnement aktivieren
   - Externe Konten mit Schreibberechtigungen aus Ihrem Abonnement entfernen
   - Privilegierte externe Konten aus Ihrem Abonnement entfernen

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen
Andere Schnellstartanleitungen und Tutorials in dieser Sammlung bauen auf dieser Schnellstartanleitung auf. Wenn Sie planen, mit den nachfolgenden Schnellstartanleitungen und Tutorials fortzufahren, sollten Sie weiter den Tarif „Standard“ nutzen und die automatische Bereitstellung aktiviert lassen. Gehen Sie wie folgt vor, falls Sie nicht fortfahren oder zum Free-Tarif zurückkehren möchten:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement oder die Richtlinie aus, für das bzw. die Sie zu „Free“ zurückwechseln möchten. Der Bereich **Sicherheitsrichtlinie** wird geöffnet.
3. Wählen Sie unter **RICHTLINIENKOMPONENTEN** die Option **Tarif**.
4. Wählen Sie **Free**, um für das Abonnement vom Tarif „Standard“ zu „Free“ zu wechseln.
5. Wählen Sie **Speichern**aus.

Gehen Sie wie folgt vor, um die automatische Bereitstellung zu deaktivieren:

1. Kehren Sie zum Hauptmenü von Security Center zurück, und wählen Sie die Option **Sicherheitsrichtlinie**.
2. Wählen Sie das Abonnement aus, für das Sie die automatische Bereitstellung deaktivieren möchten.
3. Wählen Sie im Bereich **Sicherheitsrichtlinie – Datensammlung** unter **Onboarding** die Option **Aus**, um die automatische Bereitstellung zu deaktivieren.
4. Wählen Sie **Speichern**aus.

>[!NOTE]
> Wenn Sie die automatische Bereitstellung deaktivieren, wird Microsoft Monitoring Agent nicht von virtuellen Azure-Computern entfernt, auf denen der Agent bereitgestellt wurde. Wenn Sie die automatische Bereitstellung deaktivieren, schränkt dies die Sicherheitsüberwachung für Ihre Ressourcen ein.
>

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial wurden die Schritte der grundlegenden Richtliniendefinition und Sicherheitsbewertung Ihrer Workload mit Security Center beschrieben, z.B.:

> [!div class="checklist"]
> * Konfiguration von Sicherheitsrichtlinien zur Sicherstellung der Konformität mit Ihren unternehmensspezifischen oder gesetzlichen Sicherheitsvorschriften
> * Sicherheitsbewertung für Ihre Compute-, Netzwerk-, SQL-/Speicher- und Anwendungsressourcen

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie Security Center zum Schützen Ihrer Ressourcen einsetzen.

> [!div class="nextstepaction"]
> [Schützen von Ressourcen](tutorial-protect-resources.md)
