---
title: Referenz zu wichtigen Änderungen in Azure Active Directory | Microsoft-Dokumentation
description: Hier erhalten Sie Informationen zu Änderungen an den Azure AD-Protokollen, die sich auf Ihre Anwendung auswirken können.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 266d9ff9ceb4aa71429a9afc3056d23d222a9c7b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247370"
---
# <a name="whats-new-for-authentication"></a>Neuerungen bei der Authentifizierung 

>Erhalten Sie Benachrichtigungen über Aktualisierungen dieser Seite. Fügen Sie Ihrem RSS-Feedreader einfach [diese URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) hinzu.

Für das Authentifizierungssystem werden fortlaufend Änderungen vorgenommen und Features hinzugefügt, um die Sicherheit und Einhaltung von Standards zu verbessern. Damit Sie bezüglich der aktuellen Entwicklungen auf dem neuesten Stand bleiben, bietet dieser Artikel Informationen zu Folgendem:

- Neueste Features
- Bekannte Probleme
- Protokolländerungen
- Veraltete Funktionen

> [!TIP] 
> Diese Seite wird regelmäßig aktualisiert. Daher sollten Sie die Seite oft besuchen. Sofern nicht anders angegeben, werden diese Änderungen nur für neu registrierte Anwendungen umgesetzt.  

## <a name="upcoming-changes"></a>Bevorstehende Änderungen

### <a name="authorization-codes-can-no-longer-be-reused"></a>Autorisierungscodes können nicht mehr wiederverwendet werden.

**Gültig ab:** 10. Oktober 2018 **Betroffene Endpunkte:** v1.0 und v2.0 **Betroffenes Protokoll:** [Codefluss](v2-oauth2-auth-code-flow.md)

Ab dem 10. Oktober 2018 akzeptiert Azure AD bereits zuvor verwendete Authentifizierungscodes für Apps nicht mehr. Diese Sicherheitsänderung trägt dazu bei, Azure AD an die OAuth-Spezifikation anzupassen. Sie wird auf v1- und v2-Endpunkten erzwungen.

Wenn Ihre App Autorisierungscodes zum Abrufen von Token für mehrere Ressourcen wiederverwendet, sollten Sie den Code für das Abrufen eines Aktualisierungstokens verwenden. Verwenden Sie dieses Aktualisierungstoken anschließend, um die zusätzlichen Token für andere Ressourcen abzurufen. Autorisierungscodes können nur einmal verwendet werden, Aktualisierungstoken hingegen können mehrere Male und für mehrere Ressourcen verwendet werden. Für jede neue App, die einen Authentifizierungscode im OAuth-Codefluss erneut verwenden möchte, wird ein Fehler vom Typ „invalid_grant“ zurückgegeben.

Weitere Informationen zu Aktualisierungstoken finden Sie unter [Aktualisieren der Zugriffstoken](v1-protocols-oauth-code.md#refreshing-the-access-tokens).

> [!NOTE]
> Um möglichst wenige Apps zu unterbrechen, gilt für bestehende Anwendungen, die auf diesem Feature basieren, eine Ausnahme von dieser Anforderung.  Alle Apps mit mehr als 10 Anmeldungen pro Tag, die auf diesem Muster basieren, gelten als darauf basierend.  

## <a name="may-2018"></a>Mai 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>ID-Token können nicht für den OBO-Fluss verwendet werden.

**Datum:** 1. Mai 2018 **Betroffene Endpunkte:** v1.0 und v2.0 **Betroffene Protokolle:** Impliziter Fluss und [OBO-Fluss](v1-oauth2-on-behalf-of-flow.md)

Seit dem 1. Mai 2018 können ID-Token nicht mehr als Assertion in einem OBO-Fluss für neue Anwendungen verwendet werden. Stattdessen müssen Zugriffstoken zum Schützen von APIs genutzt werden (auch zwischen einem Client und der mittleren Ebene der gleichen Anwendung). Vor dem 1. Mai 2018 registrierte Apps funktionieren weiterhin und können ID-Token gegen Zugriffstoken austauschen. Dies wird jedoch nicht empfohlen.

Sie können die folgenden Schritte ausführen, um diese Änderung zu umgehen:

1. Erstellen Sie eine Web-API für Ihre Anwendung der mittleren Ebene mit einem oder mehreren Bereichen. Dies ermöglicht eine differenziertere Kontrolle und höhere Sicherheit.
1. Vergewissern Sie sich im App-Manifest, im [Azure-Portal](https://portal.azure.com) oder im [App-Registrierungsportal](https://apps.dev.microsoft.com), dass die App Zugriffstoken über den impliziten Fluss ausgeben darf. Dies wird durch den Schlüssel `oauth2AllowImplicitFlow` gesteuert.
1. Wenn Ihre Clientanwendung ein ID-Token über `response_type=id_token` anfordert, fordern Sie zusätzlich ein Zugriffstoken (`response_type=token`) für die oben erstellte Web-API an. Daher sollte bei Verwendung des v2.0-Endpunkts der `scope`-Parameter etwa wie folgt aussehen: `api://GUID/SCOPE`. Auf dem v1.0-Endpunkt sollte der `resource`-Parameter der App-URI der Web-API sein.
1. Übergen Sie anstelle des ID-Tokens dieses Zugriffstoken an die mittlere Ebene.  
