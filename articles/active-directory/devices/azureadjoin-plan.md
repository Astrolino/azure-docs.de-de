---
title: Verwendungsszenarios und Bereitstellungsaspekte für Azure AD Join| Microsoft Docs
description: Erläutert, wie Administratoren Azure AD Join für Ihre Endbenutzer (Mitarbeiter, Studenten und andere Benutzer) einrichten können. Zudem werden die unterschiedlichen Praxis-Szenarios, die bei der Verwendung von Azure AD Join möglich sind, erläutert.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
tags: azure-classic-portal
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: markvi
ms.openlocfilehash: a145b4184fbebd9c4f447face1c47bec20c0ec32
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415633"
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Verwendungsszenarios und Bereitstellungsaspekte für Azure AD Join
## <a name="usage-scenarios-for-azure-ad-join"></a>Verwendungsszenarios für Azure AD Join
### <a name="scenario-1-businesses-largely-in-the-cloud"></a>Szenario 1: Unternehmen, die größtenteils in der Cloud sind
Azure Active Directory Join (Azure AD Join) kann davon profitieren, wenn Sie aktuell die Identitäten für Ihr Unternehmen in der Cloud ausführen und verwalten, oder diese in Kürze in die Cloud umziehen. Sie können ein Konto verwenden, das Sie in Azure AD erstellt haben, um sich bei Windows 10 anzumelden. Ihre Benutzer können ihre Computer entweder über [den Prozess des Eindrucks beim ersten Ausführen (First Run Experience, FRX)](azuread-joined-devices-frx.md) oder über das [Menü „Einstellungen“](../user-help/device-management-azuread-joined-devices-setup.md) mit Azure AD verknüpfen.  Ihre Benutzer kommen auch in den Genuss eines SSO-Zugriffs (Single Sign-On, einmaliges Anmelden) auf Cloudressourcen wie Office 365, und zwar entweder über ihre Browser oder in Office-Anwendungen.

### <a name="scenario-2-educational-institutions"></a>Szenario 2: Bildungseinrichtungen
Bildungseinrichtungen weisen in der Regel zwei Benutzertypen auf: Lehrkräfte und Schüler. Lehrkräfte werden als längerfristige Mitglieder der Organisation betrachtet. Erstellen von lokalen Konten für sie ist wünschenswert. Aber Schüler und Studenten sind für kürzere Zeiten Mitglieder der Organisation, und ihre Konten können daher in Azure AD verwaltet werden. Dies bedeutet, dass die Verzeichnisse in der Cloud abgelegt werden können und nicht lokal gespeichert werden müssen. Es bedeutet auch, dass Studenten sich mit ihren Azure AD-Konten bei Windows anmelden und Zugriff auf Office 365-Ressourcen in Office-Anwendungen erhalten können.

### <a name="scenario-3-retail-businesses"></a>Szenario 3: Einzelhandelsunternehmen
Einzelhandelsunternehmen haben sowohl saisonbedingte als auch feste Mitarbeiter. Für feste Mitarbeiter in Vollzeit erstellen Sie in der Regel lokale Konten und verwenden Computer, die in die Domäne eingebunden sind. Da Saisonkräfte nur für kürzere Zeit Teil der Organisation sind, werden ihre Konten meist an einem Ort verwaltet, an dem Benutzerlizenzen leichter verschoben werden können. Wenn Sie ihre Benutzerkonten in der Cloud mit Office 365-Lizenzen erstellen, profitieren diese Benutzer von der Anmeldung bei Windows- und Office-Anwendungen mit einem Azure AD-Konto. Gleichzeitig können Sie ihre Lizenzen flexibler handhaben, nachdem diese Benutzer das Unternehmen verlassen haben.

### <a name="scenario-4-additional-scenarios"></a>Szenario 4: Zusätzliche Szenarien
Zusammen mit den oben beschriebenen Szenarien profitieren Sie davon, dass Ihre Benutzer ihre Geräte mit Azure AD verbinden, da dadurch eine vereinfachte Verknüpfung, effiziente Geräteverwaltung, Registrierung bei der automatischen mobilen Geräteverwaltung und SSO bei Azure AD und lokalen Ressourcen ermöglicht wird.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Bereitstellungsaspekte für Azure AD Join
### <a name="enable-your-users-to-join-a-company-owned-device-directly-to-azure-ad"></a>Aktivieren Ihrer Benutzer für das direkte Verbinden eines Unternehmensgeräts mit Azure AD
Unternehmen können Nur-Cloud-Konten für Partnerunternehmen und Organisationen bereitstellen. Diese Partner können dann einfach auf Unternehmens-Apps und Ressourcen mit einmaligem Anmelden zugreifen. Dieses Szenario gilt für Benutzer, die auf Ressourcen in erster Linie in der Cloud zugreifen, wie z. B. Office 365 oder SaaS-Apps, die Azure AD für die Authentifizierung benötigen.

### <a name="prerequisites"></a>Voraussetzungen
**Auf Unternehmensebene (Administrator)**

* Azure-Abonnement mit Azure Active Directory  

**Auf Benutzerebene**

* Windows 10 (Professional- und Enterprise-Editionen)

### <a name="administrator-tasks"></a>Administratoraufgaben
* [Einrichten der Geräteregistrierung](device-management-azure-portal.md)

### <a name="user-tasks"></a>Benutzeraufgaben
* [Einrichten eines neuen Windows 10-Geräts mit Azure AD während des Setups](azuread-joined-devices-frx.md)
* [Einrichten eines Windows 10-Geräts mit Azure AD im Einstellungenmenü](../user-help/device-management-azuread-registered-devices-windows10-setup.md)
* [Verknüpfen eines persönlichen Windows 10-Geräts mit Ihrer Organisation](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>Aktivieren von BYOD (Bring Your Own Device, BYOD) in Ihrer Organisation für Windows 10
Sie können Ihre Benutzer und Mitarbeiter so einrichten, dass sie ihre persönlichen Windows-Geräte (BYOD) für den Zugriff auf Unternehmens-Apps und -Ressourcen verwenden. Ihre Benutzer können ihre Azure AD-Konten (Geschäfts- oder Schulkonten) einem persönlichen Windows-Gerät hinzufügen, um in einer sicheren und kompatiblen Art auf Arbeitsressourcen zuzugreifen.

### <a name="prerequisites"></a>Voraussetzungen
**Auf Unternehmensebene (Administrator)**

* Azure AD-Abonnement

**Auf Benutzerebene**

* Windows 10 (Professional- und Enterprise-Editionen)

### <a name="administrator-tasks"></a>Administratoraufgaben
* [Einrichten der Geräteregistrierung](device-management-azure-portal.md)

### <a name="user-tasks"></a>Benutzeraufgaben
* [Verknüpfen eines persönlichen Windows 10-Geräts mit Ihrer Organisation](../user-help/device-management-azuread-joined-devices-setup.md)

## <a name="next-steps"></a>Nächste Schritte

- [Geräteverwaltung](overview.md)

