---
title: Was ist die gruppenbasierte Lizenzierung in Azure Active Directory? | Microsoft-Dokumentation
description: Lernen Sie mehr über die gruppenbasierte Lizenzierung in Azure Active Directory, ihre Funktionsweise und die entsprechenden Best Practices.
services: active-directory
keywords: Azure AD-Lizenzierung
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.topic: conceptual
ms.workload: identity
ms.date: 06/13/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 7848b52bcf5204a871920cbfab8a0e95223654d4
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735900"
---
# <a name="what-is-group-based-licensing-in-azure-active-directory"></a>Was ist die gruppenbasierte Lizenzierung in Azure Active Directory?

Für kostenpflichtige Microsoft-Clouddienste wie Office 365, Enterprise Mobility + Security, Dynamics 365 und ähnliche Produkte werden Lizenzen benötigt. Diese Lizenzen werden jedem Benutzer zugewiesen, der Zugriff auf diese Dienste benötigt. Administratoren verwalten Lizenzen über eines der Verwaltungsportale (Office, Azure) und PowerShell-Cmdlets. Azure Active Directory (Azure AD) ist die zugrunde liegende Infrastruktur, die die Identitätsverwaltung aller Microsoft-Clouddienste unterstützt. Azure AD speichert Informationen zum Lizenzzuweisungsstatus für Benutzer.

Bislang konnten Lizenzen nur auf Ebene einzelner Benutzer zugewiesen werden, was die umfassende Verwaltung erschweren kann. Um Benutzerlizenzen basierend auf Organisationsänderungen zu vergeben oder zu entziehen, wenn beispielsweise Benutzer der Organisation oder Abteilung beitreten oder diese verlassen, muss ein Administrator häufig ein komplexes PowerShell-Skript schreiben. Dieses Skript richtet einzelne Aufrufe an den Clouddienst.

Um diese Probleme zu beheben, enthält Azure AD jetzt die gruppenbasierte Lizenzierung. Sie können einer Gruppe eine oder mehrere Produktlizenzen zuweisen. Azure AD stellt sicher, dass die Lizenzen allen Mitgliedern der Gruppe zugewiesen werden. Allen neuen Mitgliedern, die der Gruppe beitreten, werden die entsprechenden Lizenzen zugewiesen. Wenn sie die Gruppe verlassen, werden diese Lizenzen entfernt. Dadurch ist keine automatisierte Lizenzverwaltung über PowerShell mehr erforderlich, um Änderungen in der Organisations- und Abteilungsstruktur benutzerbezogen widerzuspiegeln.

>[!Note]
>Die gruppenbasierte Lizenzierung ist ein Feature der öffentlichen Vorschauversion von Azure Active Directory (Azure AD) und mit jedem kostenpflichtigen Azure AD-Lizenzplan verfügbar. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="features"></a>Features

Hier die Hauptmerkmale der gruppenbasierten Lizenzierung:

- Lizenzen können beliebigen Sicherheitsgruppen in Azure AD zugewiesen werden. Sicherheitsgruppen können mithilfe von Azure AD Connect lokal synchronisiert werden. Sie können Sicherheitsgruppen auch direkt in Azure AD Connect (auch als reine Cloudgruppen bezeichnet) oder automatisch über das Azure AD-Feature „Dynamische Gruppe“ erstellen.

- Wenn eine Produktlizenz einer Gruppe zugewiesen wird, kann der Administrator einen oder mehrere Servicepläne im Produkt deaktivieren. In der Regel erfolgt dies, wenn die Organisation noch nicht für die Verwendung eines Dienstes in einem Produkt bereit ist. Beispielsweise könnte der Administrator Office 365 einer Abteilung zuweisen, aber den Yammer-Dienst vorübergehend deaktivieren.

- Alle Microsoft-Clouddienste, die eine Lizenzierung auf Benutzerebene erfordern, werden unterstützt. Dazu zählen alle Office 365-Produkte, Enterprise Mobility + Security und Dynamics 365.

- Die gruppenbasierte Lizenzierung ist derzeit nur über [das Azure-Portal](https://portal.azure.com) verfügbar. Wenn Sie in erster Linie andere Verwaltungsportale für die Benutzer- und Gruppenverwaltung nutzen, z.B. das Office 365-Portal, können Sie dies weiterhin tun. Jedoch sollten Sie das Azure-Portal zum Verwalten von Lizenzen auf Gruppenebene verwenden.

- Azure AD verwaltet Lizenzänderungen, die sich aus Änderungen an der Gruppenmitgliedschaft ergeben, automatisch. In der Regel erfolgen Änderungen des Lizenzstatus binnen Minuten nach einer Mitgliedschaftsänderung.

- Ein Benutzer kann Mitglied mehrerer Gruppen mit angegebenen Lizenzrichtlinien sein. Ein Benutzer kann auch über Lizenzen verfügen, die außerhalb von Gruppen zugewiesen wurden. Der resultierende Benutzerstatus ist eine Kombination aller zugewiesenen Produkt- und Dienstlizenzen.

- In manchen Fällen können Benutzern keine Lizenzen zugewiesen werden. Mögliche Gründe sind das Fehlen verfügbarer Lizenzen im Mandanten oder in Konflikt stehende Dienste, die gleichzeitig zugewiesen wurden. Administratoren haben Zugriff auf Informationen zu Benutzern, für die Azure AD Gruppenlizenzen nicht vollständig verarbeiten konnte. Sie können anhand dieser Informationen Korrekturmaßnahmen vornehmen.

- Während der öffentlichen Vorschauphase ist im Mandanten ein kostenpflichtiges oder Testabonnement für Azure AD Basic oder Premium Edition erforderlich, um die gruppenbasierte Lizenzverwaltung nutzen zu können.

## <a name="your-feedback-is-welcome"></a>Wir freuen uns auf Ihr Feedback!

Wenn Sie Feedback hinterlassen oder Features vorschlagen möchten, nutzen Sie [das Azure AD-Administratorforum](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die gruppenbasierte Lizenzverwaltung finden Sie unter:

* [Zuweisen von Lizenzen zu einer Gruppe in Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Migrieren einzelner lizenzierter Benutzer zur gruppenbasierten Lizenzierung in Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](../users-groups-roles/licensing-group-advanced.md)
