---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer Sicherung | Microsoft-Dokumentation
description: Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage).
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 10/08/2018
ms.openlocfilehash: ad82ae6158bbf343dd84be125a8839e992ad3634
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868155"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen

Standardmäßig werden Sicherungen von SQL-Datenbank im georeplizierten Blobspeicher (RA-GRS) gespeichert. Die folgenden Optionen stehen für die Datenbankwiederherstellung mithilfe [automatisierter Datenbanksicherungen](sql-database-automated-backups.md) zur Verfügung:

- Erstellen einer neuen Datenbank auf dem gleichen logischen Server, die auf den Zustand zu einem angegebenen Zeitpunkt innerhalb der Aufbewahrungsdauer wiederhergestellt wird.
- Erstellen einer Datenbank auf dem gleichen logischen Server, die auf den Zustand zum Zeitpunkt des Löschens einer gelöschten Datenbank wiederhergestellt wird.
- Erstellen einer neuen Datenbank auf einem beliebigen logischen Server in einer beliebigen Region, die auf den Zustand zum Zeitpunkt der neuesten Sicherung wiederhergestellt wird.

Wenn Sie die [langfristige Aufbewahrung von Sicherungen](sql-database-long-term-retention.md) konfiguriert haben, können Sie auch eine neue Datenbank aus jeder LTR-Sicherung auf einem beliebigen logischen Server in einer beliebigen Region erstellen.  

> [!IMPORTANT]
> Sie können eine vorhandene Datenbank während der Wiederherstellung nicht überschreiben.

Bei Verwendung der Standard- oder Premium-Dienstebene ist eine wiederhergestellte Datenbank unter folgenden Bedingungen mit zusätzlichen Speicherkosten verbunden:

- Wiederherstellung von P11–P15 bis S4–S12 oder P1–P6, wenn die maximale Datenbankgröße 500 GB überschreitet.
- Wiederherstellung von P1–P6 in S4–S12, wenn die maximale Datenbankgröße 250 GB überschreitet.

Die zusätzlichen Kosten entstehen, weil die maximale Größe der wiederhergestellten Datenbank größer als die Menge des in der Computegröße enthaltenen Speichers ist und jeglicher über der enthalten Menge zusätzlich bereitgestellter Speicherplatz extra in Rechnung gestellt wird.  Ausführliche Informationen zu den Preisen für zusätzlichen Speicherplatz siehe [SQL-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-database/).  Wenn die tatsächlich verwendete Speichermenge kleiner als die enthaltene Speichermenge ist, können Sie diese zusätzlichen Kosten vermeiden, indem Sie die maximale Datenbankgröße auf die enthaltene Menge reduzieren.  

> [!NOTE]
> [Automatische Datenbanksicherungen](sql-database-automated-backups.md) werden verwendet, wenn Sie eine [Datenbankkopie](sql-database-copy.md) erstellen.

## <a name="recovery-time"></a>Wiederherstellungszeit

Die Zeit, die zum Wiederherstellen einer Datenbank mit automatisierten Sicherungen benötigt wird, hängt von mehreren Faktoren ab:

- Größe der Datenbank
- Computegröße der Datenbank
- Anzahl der beteiligten Transaktionsprotokolle
- Menge der erneut auszuführenden Aktivitäten, um den Wiederherstellungspunkt wiederherzustellen
- Netzwerkbandbreite, sofern die Wiederherstellung in einer anderen Region erfolgt
- Anzahl der gleichzeitigen Wiederherstellungsanforderungen, die aktuell in der Zielregion verarbeitet werden
  
Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Wenn es sich um einen längeren Ausfall in einer Region handelt, müssen möglicherweise andere Regionen eine große Anzahl von Geowiederherstellungsanforderungen verarbeiten. Wenn viele Anforderungen vorliegen, erhöht sich dadurch unter Umständen die Wiederherstellungsdauer für Datenbanken in dieser Region. Der Großteil der Datenbankwiederherstellungen erfolgt innerhalb von 12 Stunden.

Für ein Einzelabonnement gibt es einige Einschränkungen für die Anzahl der gleichzeitigen Wiederherstellungsanforderungen (einschließlich Point-in-Time-Wiederherstellung, Geowiederherstellung und Wiederherstellung aus einer langfristig aufbewahrten Sicherung), die übermittelt und verarbeitet werden:

|  | **Max. Anzahl gleichzeitiger Anforderungen, die verarbeitet werden** | **Max. Anzahl gleichzeitiger Anforderungen, die übermittelt werden** |
| :--- | --: | --: |
|Einzeldatenbank (pro Abonnement)|10|60|
|Pool für elastische Datenbanken (pro Pool)|4|200|
||||

Es gibt keine integrierte Funktion für Massenwiederherstellungen. Das Skript [Azure SQL-Datenbank: Vollständige Serverwiederherstellung](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) ist ein Beispiel für eine Möglichkeit, diese Aufgabe auszuführen.

> [!IMPORTANT]
> Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ im Abonnement oder Besitzer des Abonnements sein. Siehe dazu [RBAC: Integrierte Rollen](../role-based-access-control/built-in-roles.md). Sie können das Azure-Portal, PowerShell oder die REST-API zur Wiederherstellung verwenden. Die Nutzung von Transact-SQL ist nicht möglich.

## <a name="point-in-time-restore"></a>Point-in-Time-Wiederherstellung

Sie können eine vorhandene Datenbank mit dem Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase) oder der [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) zu einem früheren Zeitpunkt als eine neue Datenbank auf dem gleichen logischen Server wiederherstellen.

> [!TIP]
> Ein PowerShell-Beispielskript, das zeigt, wie eine Point-in-Time-Wiederherstellung einer Datenbank durchgeführt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](scripts/sql-database-restore-database-powershell.md).

Die Datenbank kann in allen Diensttarifen und Computegrößen als Einzeldatenbank oder in einem Pool für elastische Datenbanken wiederhergestellt werden. Stellen Sie sicher, dass Sie auf dem logischen Server oder im Pool für elastische Datenbanken, in dem Sie die Datenbank wiederherstellen, über ausreichend Ressourcen verfügen. Sobald der Vorgang abgeschlossen ist, ist die wiederhergestellte Datenbank eine normale, vollständig erreichbare Onlinedatenbank. Die wiederhergestellte Datenbank wird mit normalen Tarifen basierend auf dem Diensttarif und der Computegröße in Rechnung gestellt. Kosten entstehen erst, wenn die Datenbankwiederherstellung abgeschlossen ist.

Im Allgemeinen wird beim Wiederherstellen der Datenbank der Zustand zu einem früheren Zeitpunkt wiederhergestellt. Sie können die wiederhergestellte Datenbank entweder als Ersatz für die ursprüngliche Datenbank verwenden oder Daten daraus abrufen und die ursprüngliche Datenbank damit aktualisieren.

- **Ersetzung der Datenbank**

   Wenn die wiederhergestellte Datenbank als Ersatz für die ursprüngliche Datenbank fungieren soll, vergewissern Sie sich, dass Computegröße und/oder Diensttarif angemessen sind, und skalieren Sie die Datenbank bei Bedarf entsprechend. Sie können die ursprüngliche Datenbank umbenennen und dann die wiederhergestellte Datenbank mit dem Namen der ursprünglichen Datenbank versehen. Verwenden Sie hierzu den Befehl [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) in T-SQL.

- **Wiederherstellung der Daten**

   Wenn Sie Daten aus der wiederhergestellten Datenbank abrufen möchten, um einen Benutzer- oder Anwendungsfehler zu korrigieren, müssen Sie die erforderlichen Datenwiederherstellungsskripts schreiben und ausführen, um die Daten aus der wiederhergestellten Datenbank zu extrahieren und sie in der ursprünglichen Datenbank wiederherzustellen. Der Wiederherstellungsvorgang kann zwar eine ganze Weile dauern, die wiederherstellende Datenbank wird jedoch während des Wiederherstellungsvorgangs in der Datenbankliste angezeigt. Wenn Sie diese Datenbank während der Wiederherstellung löschen, wird der Wiederherstellungsvorgang abgebrochen, und es fallen keine Kosten für die Datenbank an, für die die Wiederherstellung nicht abgeschlossen wurde.

### <a name="recover-to-a-point-in-time-using-azure-portal"></a>Wiederherstellen bis zu einem Zeitpunkt im Azure-Portal

Öffnen Sie zum Wiederherstellen bis zu einem Zeitpunkt im Azure-Portal die Seite Ihrer Datenbank, und klicken Sie auf der Symbolleiste auf **Wiederherstellen**.

![Point-in-Time-Wiederherstellung](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Wiederherstellen einer gelöschten Datenbank

Sie können eine gelöschte Datenbank mit dem Azure-Portal, [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase) oder [REST](https://msdn.microsoft.com/library/azure/mt163685.aspx) (createMode=Restore) in dem Zustand wiederherstellen, den sie zum Zeitpunkt des Löschens hatte. Die Wiederherstellung erfolgt dabei auf dem gleichen logischen Server. Sie können eine gelöschte Datenbank während der Aufbewahrung für einen früheren Zeitpunkt wiederherstellen, indem Sie [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase) verwenden.

> [!Note]
> Das Wiederherstellen der gelöschten Datenbank steht in einer verwalteten Instanz nicht zur Verfügung.
> [!TIP]
> Ein PowerShell-Beispielskript, das zeigt, wie eine gelöschten Datenbank wiederhergestellt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Wenn Sie eine Azure SQL-Datenbank-Serverinstanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden. Derzeit wird das Wiederherstellen gelöschter Server nicht unterstützt.

### <a name="recover-a-deleted-database-using-the-azure-portal"></a>Wiederherstellen einer gelöschten Datenbank im Azure-Portal

Wenn Sie im Azure-Portal eine gelöschte Datenbank während ihrer [Beibehaltungsdauer im DTU-basierten Modell](sql-database-service-tiers-dtu.md) oder ihrer [Beibehaltungsdauer im V-Kern-basierten Modell](sql-database-service-tiers-vcore.md) wiederherstellen möchten, öffnen Sie die Seite für Ihren Server, und klicken Sie dann im Bereich „Vorgänge“ auf **Gelöschte Datenbanken**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Geografische Wiederherstellung

Sie können eine SQL-Datenbank auf einem beliebigen Server in einer beliebigen Azure-Region aus der letzten georeplizierten vollständigen und differenziellen Sicherung wiederherstellen. Die Geowiederherstellung verwendet eine georedundante Sicherung als Quelle und kann selbst dann zum Wiederherstellen einer Datenbank verwendet werden, wenn die Datenbank oder das Rechenzentrum aufgrund eines Ausfalls nicht mehr verfügbar ist.

> [!Note]
> Die Geowiederherstellung steht in einer verwalteten Instanz nicht zur Verfügung.

Die Geowiederherstellung ist die Standardoption für die Wiederherstellung, wenn eine Datenbank aufgrund eines Incidents in der Region, in der die Datenbank gehostet wird, nicht verfügbar ist. Wenn Ihre Datenbankanwendung durch einen umfangreichen Incident in einer Region nicht mehr verfügbar ist, können Sie eine Datenbank aus den georeplizierten Sicherungen auf einem Server in einer beliebigen anderen Region wiederherstellen. Es gibt eine Verzögerung zwischen der Erstellung einer differenziellen Sicherung und der Georeplikation in einem Azure-Blob in einer anderen Region. Diese Verzögerung kann bis zu einer Stunde betragen. Folglich kann bei einem Notfall ein Datenverlust von bis zu einer Stunde auftreten. Die folgende Abbildung zeigt die Wiederherstellung der Datenbank aus der letzten verfügbaren Sicherung in einer anderen Region.

![Geowiederherstellung](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Ein PowerShell-Beispielskript, das zeigt, wie eine Geowiederherstellung durchgeführt wird, finden Sie unter [Wiederherstellen einer Azure SQL-Datenbank mit automatisierten Datenbanksicherungen](scripts/sql-database-restore-database-powershell.md).

Point-in-Time-Wiederherstellung für geosekundäre Datenbanken wird derzeit nicht unterstützt. Point-in-Time-Wiederherstellung kann nur für eine primäre Datenbank erfolgen. Ausführliche Informationen zum Verwenden der Geowiederherstellung nach einem Ausfall finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Die Wiederherstellung aus Sicherungen ist die elementarste Notfallwiederherstellungslösung, die in SQL-Datenbank verfügbar ist. Sie weist den längsten Recovery Point Objective-Wert (RPO) und die längste geschätzte Wiederherstellungszeit (ERT) auf. Für Lösungen mit kleinen Datenbanken (z.B. auf der Basic-Dienstebene oder kleine Mandantendatenbanken in Pools für elastische Datenbanken) stellt die Georeplikation häufig eine sinnvolle Lösung für die Notfallwiederherstellung mit einer ERT von 12 Stunden dar. Für Lösungen mit großen Datenbanken, die kürzere Wiederherstellungszeiten erfordern, sollten Sie die Verwendung von [Failovergruppen und aktiver Georeplikation](sql-database-geo-replication-overview.md) in Erwägung ziehen. Die aktive Georeplikation bietet eine niedrigere RPO und ERT, da sie nur das Einleiten eines Failovers auf eine kontinuierlich replizierte sekundäre Datenbank erfordert. Weitere Informationen zur Optionen für Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Durchführen einer Geowiederherstellung über das Azure-Portal

Wenn Sie im Azure-Portal eine Geowiederherstellung für eine Datenbank während ihrer [Beibehaltungsdauer im DTU-basierten Modell](sql-database-service-tiers-dtu.md) oder ihrer [Beibehaltungsdauer im V-Kern-basierten Modell](sql-database-service-tiers-vcore.md) ausführen möchten, öffnen Sie die Seite „SQL-Datenbanken“, und klicken Sie dann auf **Hinzufügen**. Wählen Sie im Textfeld **Quelle auswählen** die Option **Sicherung** aus. Geben Sie die Sicherung an, aus der die Wiederherstellung in der Region und auf dem Server Ihrer Wahl erfolgen soll.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Programmgesteuerte Wiederherstellung mit automatisierten Sicherungen

Wie bereits erwähnt, kann die Datenbankwiederherstellung nicht nur über das Azure-Portal, sondern auch programmgesteuert mit Azure PowerShell oder mit der REST-API ausgeführt werden. Die folgenden Tabellen beschreiben den verfügbaren Satz von Befehlen.

### <a name="powershell"></a>PowerShell

| Cmdlet | BESCHREIBUNG |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Ruft mindestens eine Datenbank ab. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Ruft eine gelöschte Datenbank ab, die Sie wiederherstellen können. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Ruft eine georedundante Sicherung einer Datenbank ab. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |Stellt eine SQL-Datenbank wieder her. |
|  | |

### <a name="rest-api"></a>REST-API

| API | BESCHREIBUNG |
| --- | --- |
| [REST (createMode=Recovery)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Stellt eine Datenbank wieder her. |
| [Get Create or Update Database Status](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Ruft den Status während eines Wiederherstellungsvorgangs ab. |
|  | |

## <a name="summary"></a>Zusammenfassung

Automatische Sicherungen schützen Ihre Datenbanken vor Benutzer- und Anwendungsfehlern, versehentlichen Datenbanklöschungen und längeren Ausfällen. Diese integrierte Funktion ist für alle Diensttarife und Computegrößen verfügbar.

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Aufbewahrung finden Sie im Artikel [Langfristiges Aufbewahren](sql-database-long-term-retention.md).
- Informationen zu schnelleren Wiederherstellungsoptionen finden Sie unter [Failovergruppen und aktive Georeplikation](sql-database-geo-replication-overview.md).  
