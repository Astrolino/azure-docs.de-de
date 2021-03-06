---
title: Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung in Azure | Microsoft-Dokumentation
description: Beschreibt das Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung in Azure
services: virtual-network
documentationcenter: virtual-network
author: genlin
manager: WillChen
editor: ''
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 02/06/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 1e95b00ea08105238a860265e46275c24ed7bfbd
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/11/2018
ms.locfileid: "29151817"
---
#  <a name="configure-reverse-lookup-zones-for-an-smtp-banner-check"></a>Konfigurieren von Reverse-Lookupzonen für eine SMTP-Bannerüberprüfung

Dieser Artikel beschreibt die Verwendung einer Reverse-Lookupzone in Azure DNS und das Erstellen eines Reverse-DNS-Eintrags (PTR) für die SMTP-Bannerüberprüfung. 

## <a name="symptom"></a>Symptom

Wenn Sie einen SMTP-Server in Microsoft Azure hosten, erhalten Sie eventuell beim Senden oder Empfangen einer Nachricht von Remotemailservern die folgende Fehlermeldung:

**554: Kein PTR-Eintrag** 

## <a name="solution"></a>Lösung

Für eine virtuelle IP-Adresse in Azure werden die Reverse-Einträge in Domänenzonen im Besitz von Microsoft erstellt, nicht in benutzerdefinierten Domänenzonen.

Um PTR-Einträge in Zonen im Besitz von Microsoft zu konfigurieren, verwenden Sie die -ReverseFqdn-Eigenschaft für die Ressource PublicIpAddress. Weitere Informationen finden Sie unter [Konfigurieren von Reverse-DNS für in Azure gehostete Dienste](../dns/dns-reverse-dns-for-azure-services.md). 

Achten Sie beim Konfigurieren der PTR-Einträge darauf, dass die IP-Adresse und der Reverse-FQDN zum Abonnement gehören. Wenn Sie versuchen, einen Reverse-FQDN festzulegen, der nicht dem Abonnement angehört, erhalten Sie die folgende Fehlermeldung:

    Set-AzureRmPublicIpAddress : ReverseFqdn mail.contoso.com that PublicIPAddress ip01 is trying to use does not belong to subscription <Subscription ID>. One of the following conditions need to be met to establish ownership: 
                        
    1) ReverseFqdn stimmt mit dem FQDN einer beliebigen Ressource für eine öffentliche IP unter dem Abonnement überein. 
    2) ReverseFqdn wird (durch eine Kette von CNAME-Einträgen) in den FQDN einer beliebigen Ressource für eine öffentliche IP unter dem Abonnement aufgelöst. 
    3) ReverseFqdn wird (durch eine Kette von CNAME-Einträgen und A-Datensätzen) in die IP-Adresse einer Ressource für eine statische öffentliche IP unter dem Abonnement aufgelöst. 

Wenn Sie Ihr SMTP-Banner manuell in unseren standardmäßigen Reverse-FQDN ändern, kann beim Remotemailserver trotzdem ein Fehler auftreten, da er möglicherweise erwartet, dass der SMTP-Bannerhost mit dem MX-Eintrag für die Domäne übereinstimmt.