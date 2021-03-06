---
title: Vorauszahlen für Azure Cosmos DB-Ressourcen zum Einsparen von Kosten | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie reservierte Kapazitäten für Azure Cosmos DB kaufen, um Computekosten einzusparen.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 1be2d67d8a1ee51c4883ae1f50b80ad3a9691c2d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981966"
---
# <a name="prepay-for-azure-cosmos-db-resources-with-reserved-capacity"></a>Vorauszahlen für Azure Cosmos DB-Ressourcen mit reservierter Kapazität

Reservierte Cosmos DB-Kapazität hilft Ihnen dabei, Geld zu sparen, indem Sie im Voraus für einen Zeitraum von einem oder drei Jahren für Azure Cosmos DB-Ressourcen bezahlen. Reservierte Azure Cosmos DB-Kapazität bietet einen größeren Rabatt auf den für Cosmos DB-Ressourcen wie Datenbanken und Container (Tabellen/Sammlungen/Graphen) bereitgestellten Durchsatz. Reservierte Azure Cosmos DB-Kapazität kann Ihre Cosmos DB-Ausgaben erheblich reduzieren – Sie können mit einer Vorauszahlung für ein bzw. drei Jahre gegenüber den regulären Preisen bis zu 65 % sparen. Reservierte Kapazität stellt lediglich einen Abrechnungsrabatt dar, der keine Auswirkungen auf den Zustand Ihrer Cosmos DB-Ressourcen zur Laufzeit hat.

Die reservierte Kapazität von Azure Cosmos DB deckt den für Ihre Ressourcen bereitgestellten Durchsatz ab, nicht die Speicher- und Netzwerkkosten. Sobald Sie eine Reservierung gekauft haben, werden die Durchsatzgebühren, die den Reservierungsattributen entsprechen, nicht mehr zu den Preisen der nutzungsbasierten Bezahlung abgerechnet. Weitere Informationen zu Reservierungen finden Sie im Artikel [Azure-Reservierungen](../billing/billing-save-compute-costs-reservations.md). 

Sie können die reservierte Azure Cosmos DB-Kapazität über das [Azure-Portal](https://portal.azure.com) erwerben. So erwerben Sie reservierte Kapazität:

* Ihnen muss die Besitzerrolle für mindestens ein Enterprise-Abonnement oder ein Abonnement mit nutzungsbasierter Bezahlung zugeordnet sein.  
* In Enterprise-Abonnements müssen Azure-Reservierungskäufe im [EA-Portal](https://ea.azure.com/) aktiviert werden.  
* Für das Cloud Solution Provider-Programm (CSP) können nur die Administrator- oder Vertriebs-Agents reservierte Azure Cosmos DB-Kapazitäten kaufen.

## <a name="determine-the-required-throughput-before-purchase"></a>Bestimmen des erforderlichen Durchsatzes vor dem Kauf

Die Größe der Reservierung sollte sich nach dem Gesamtdurchsatz richten, der von den vorhandenen oder in Kürze bereitzustellenden Azure Cosmos DB-Ressourcen (z.B. Datenbanken oder Container – Sammlungen, Tabellen, Grafiken) verbraucht wird. Sie können den erforderlichen Durchsatz auf folgende Weise ermitteln:

* Navigieren Sie zum [Azur-Portal](https://portal.azure.com), suchen Sie Ihr Azure Cosmos DB-Konto, öffnen Sie das Blatt „Metriken“ und suchen Sie die Informationen zum durchschnittlichen Durchsatz/Sek auf der Registerkarte **Durchsatz** über einen Zeitraum von 3-6 Monaten. Geben Sie diese Größe beim Kauf als reservierte Kapazitätseinheiten an.

Wenn Sie ein Enterprise Agreement (EA)-Kunde sind, können Sie alternativ Ihre Nutzungsdatei herunterladen und über den Wert von **Diensttyp** im Abschnitt **Zusätzliche Informationen** der Nutzungsdatei die Informationen zum Azure Cosmos DB-Durchsatz ermitteln.

Sie können auch den durchschnittlichen Durchsatz für alle Workloads auf Ihren Azure Cosmos DB-Konten zusammenfassen, die Sie für die nächsten ein oder drei Jahre erwarten, und diese Menge für die Reservierung verwenden.

## <a name="buy-azure-cosmos-db-reserved-capacity"></a>Kaufen von reservierter Azure Cosmos DB-Kapazität

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.  

2. Klicken Sie auf **Alle Dienste** > **Reservierungen** > **Hinzufügen**.  

3. Wählen Sie im Bereich **Produkttyp auswählen** die Option **Azure Cosmos DB** und dann **Auswählen** aus, um eine neue Reservierung zu erwerben.  

4. Füllen Sie die Pflichtfelder aus, wie in der folgenden Tabelle beschrieben:

   ![Formular für die reservierte Kapazität ausfüllen](./media/cosmos-db-reserved-capacity/fill_reserved_capacity_form.png) 

   |Feld  |BESCHREIBUNG  |
   |---------|---------|
   |NAME   |    Name der Reservierung. Dieses Feld wird automatisch mit dem `CosmosDB_Reservation_<timeStamp>` ausgefüllt. Sie können beim Anlegen der Reservierung einen anderen Namen vergeben oder sie nach dem Anlegen der Reservierung umbenennen.      |
   |Abonnement  |   Das Abonnement, das für die Bezahlung der reservierten Azure Cosmos DB-Kapazitäten verwendet wird. Die Zahlungsmethode für das ausgewählte Abonnement wird mit Vorauszahlungen belastet. Es muss einer der folgenden Abonnementtyp vorliegen: <br/><br/>  [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/) (Angebotsnummer: MS-AZR-0017P) – Bei einem Enterprise-Abonnement werden die Gebühren vom Verpflichtungsguthaben der Reservierung abgezogen oder als Überschreitung belastet. <br/><br/> [Nutzungsbasierte Zahlung](https://azure.microsoft.com/offers/ms-azr-0003p/) (Angebotsnummer: MS-AZR-0003P) – Bei einem Abonnement mit nutzungsbasierter Zahlung wird die Kreditkarte mit den Gebühren belastet, oder die Gebühren werden für die Zahlung auf Rechnung in Rechnung gestellt.    |
   |Bereich   |   Der Reservierungsumfang bestimmt, wie viele Abonnements den mit der Reservierung verbundenen Abrechnungsvorteil nutzen können, und wie die Reservierung auf bestimmte Abonnements angewendet wird. Eine Reservierung ist entweder ein einzelnes oder ein gemeinsames Abonnement. Optionen:   <br/><br/>  **Einzelabonnement** – Der Reservierungsrabatt wird auf Azure Cosmos DB-Instanzen im ausgewählten Abonnement angewendet. <br/><br/>  **Gemeinsam** – Der Reservierungsrabatt wird auf Azure Cosmos DB-Instanzen angewendet, die in einem beliebigen Abonnement innerhalb des Abrechnungskontexts ausgeführt werden. Der Abrechnungskontext wird basierend darauf bestimmt, wie Sie sich bei Azure angemeldet haben. Für Enterprise-Kunden stellt der freigegebene Bereich die Reservierung dar und umfasst alle Abonnements (mit Ausnahme von Dev/Test-Abonnements) innerhalb der Reservierung. Für Kunden mit nutzungsbasierter Zahlung stellt der freigegebene Bereich alle Abonnements mit nutzungsbasierter Zahlung dar, die vom Kontoadministrator erstellt wurden.  <br/><br/> Sie können den Reservierungsumfang nach dem Kauf der reservierten Kapazität ändern.  |
   |Typ der reservierten Kapazität   |  Der Typ der reservierten Kapazität ist der Durchsatz, der in Form von Anforderungseinheiten bereitgestellt wird.|
   |Einheiten für die reservierte Kapazität  |      Die Menge an Durchsatz, die Sie reservieren möchten. Sie können diesen Wert berechnen, indem Sie den Durchsatz für alle Ihre Cosmos DB-Ressourcen (z.B. Datenbanken oder Container) pro Region bestimmen und ihn dann mit der Anzahl der Regionen multiplizieren, die Sie Ihrer Cosmos DB-Datenbank zuordnen werden.  <br/> Zum Beispiel: Wenn Sie fünf Regionen mit 1 Million RU/Sek. in jeder Region haben, dann wählen Sie 5 Millionen RU/Sek. für den Kauf von Reservierungskapazitäten.    |
   |Begriff  |   Ein Jahr oder drei Jahre   |

5. Überprüfen Sie den Preis der Reservierung im Abschnitt **Kosten**. Dieser Reservierungspreis gilt für Azure Cosmos DB-Ressourcen mit Durchsatz, die in allen Regionen bereitgestellt werden.  

6. Wählen Sie die Option **Kaufen**. Nach einem erfolgreichen Kauf wird der folgende Screenshot angezeigt. 

   ![Formular für die reservierte Kapazität ausfüllen](./media/cosmos-db-reserved-capacity/reserved_capacity_successful.png) 

Nachdem Sie eine Reservierung gekauft haben, wird sie sofort auf alle vorhandenen Ressourcen von Azure Cosmos DB angewendet, die den Bedingungen der Reservierung entsprechen. Wenn Sie keine vorhandenen Azure Cosmos DB-Ressourcen haben, wird die Reservierung angewendet, wenn Sie eine neue Cosmos DB-Instanz bereitstellen, die den Bedingungen der Reservierung entspricht. In beiden Fällen beginnt der Zeitraum der Reservierung unmittelbar nach dem erfolgreichen Kauf. 

Wenn Ihre Reservierung abläuft, laufen Ihre Azure Cosmos DB-Instanzen weiter und werden zu den regulären nutzungsbasierten Gebühren abgerechnet.

## <a name="next-steps"></a>Nächste Schritte

Der Reservierungsrabatt wird automatisch auf die Azure Cosmos DB-Ressourcen angewendet, die dem Reservierungsbereich und den Reservierungsattributen entsprechen. Sie können den Bereich der Reservierung über das Azure-Portal, PowerShell, die CLI oder die API aktualisieren.

*  Informationen dazu, wie Kapazitätsrabatte auf Azure Cosmos DB angewendet werden, finden Sie unter [Grundlegendes zum Rabatt für Azure-Reservierungen](../billing/billing-understand-cosmosdb-reservation-charges.md).

* Weitere Informationen zu Azure-Reservierungen finden Sie in den folgenden Artikeln:

   * [Was sind Azure-Reservierungen?](../billing/billing-save-compute-costs-reservations.md)  
   * [Verwalten von Azure-Reservierungen](../billing/billing-manage-reserved-vm-instance.md).  
   * [Grundlegendes zur Nutzung von Azure-Reservierungen für den Konzernbeitritt](../billing/billing-understand-reserved-instance-usage-ea.md)  
   * [Grundlegendes zur Nutzung von Azure-Reservierungen für das Abonnement mit nutzungsbasierter Bezahlung](../billing/billing-understand-reserved-instance-usage.md)
   * [Verkaufen von Microsoft Azure Reserved VM Instances](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Support kontaktieren

Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.

