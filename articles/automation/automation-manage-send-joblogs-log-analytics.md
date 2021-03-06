---
title: Weiterleiten von Azure Automation-Auftragsdaten an Log Analytics
description: In diesem Artikel wird erläutert, wie Auftragsstatus und Runbookauftrags-Datenströme an Azure Log Analytics gesendet werden, um zusätzliche Einblicke und Verwaltungsoptionen zu erhalten.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 06/12/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 13ba4d774cbc347830c32385ba4927a0df687159
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47035469"
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics"></a>Weiterleiten von Auftragsstatus und Auftragsdatenströmen von Automation an Log Analytics

Automation kann Runbookauftragsstatus und Auftragsdatenströme an Ihren Log Analytics-Arbeitsbereich senden. Dieser Prozess beinhaltet nicht die Arbeitsbereichsverknüpfung. Er ist vollkommen unabhängig. Auftragsprotokolle und -streams werden im Azure-Portal oder mit PowerShell für einzelne Aufträge angezeigt, d.h., Sie können einfache Untersuchen durchführen. Log Analytics bietet folgende Vorteile:

* Gewinnen Sie Einblicke in Ihre Automation-Aufträge.
* Lösen Sie basierend auf Ihrem Runbookauftragsstatus (beispielsweise „Fehler“ oder „Angehalten“) das Senden einer E-Mail oder einer Warnung aus.
* Schreiben Sie erweiterte Abfragen für Ihre Auftragsdatenströme.
* Korrelieren Sie Aufträge über Automation-Konten hinweg.
* Visualisieren Sie Ihren Auftragsverlauf.

## <a name="prerequisites-and-deployment-considerations"></a>Voraussetzungen und Überlegungen zur Bereitstellung

Zum Senden Ihrer Automation-Protokolle an Log Analytics benötigen Sie:

* Die [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/)-Version von November 2016 (v2.3.0) oder höher.
* Einen Log Analytics-Arbeitsbereich Weitere Informationen finden Sie unter [Erste Schritte mit Log Analytics](../log-analytics/log-analytics-get-started.md). 
* Die Ressourcen-ID für Ihr Azure Automation-Konto

So ermitteln Sie die Ressourcen-ID für Ihr Azure Automation-Konto:

```powershell-interactive
# Find the ResourceId for the Automation Account
Get-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Führen Sie zum Ermitteln der Ressourcen-ID für Ihren Log Analytics-Arbeitsbereich den folgenden PowerShell-Befehl aus:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Get-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Sollten Sie über mehrere Automation-Konten (oder über mehrere Arbeitsbereiche) verfügen, suchen Sie in der Ausgabe der vorherigen Befehle den *Namen*, den Sie konfigurieren müssen, und kopieren Sie den Wert für *ResourceId*.

Wenn Sie den *Namen* Ihres Automation-Kontos suchen müssen, wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann **Alle Einstellungen**. Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus.  Die Werte werden auf dem Blatt **Eigenschaften** angezeigt.<br> ![Automation-Konto – Eigenschaften](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Einrichten der Integration in Log Analytics

1. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm**.
2. Führen Sie das folgende PowerShell-Skript aus, und bearbeiten Sie den Wert für `[your resource id]` und `[resource id of the log analytics workspace]` mit den Werten aus dem vorherigen Schritt.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Nach dem Ausführen dieses Skripts werden in Log Analytics innerhalb von zehn Minuten Datensätze neu geschriebener Auftragsprotokolle oder -streams angezeigt.

Um die Protokolle anzuzeigen, führen Sie die folgende Abfrage in der Log Analytics-Protokollsuche durch: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="verify-configuration"></a>Überprüfen der Konfiguration
Vergewissern Sie sich, dass Ihr Automation-Konto Protokolle an Ihren Log Analytics-Arbeitsbereich sendet. Überprüfen Sie hierzu mithilfe des folgenden PowerShell-Befehls, ob die Diagnose für das Automation-Konto ordnungsgemäß konfiguriert ist:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Überprüfen Sie in der Ausgabe Folgendes:
+ Unter *Protokolle* muss *Aktiviert* den Wert *True* haben.
+ Der Wert für *WorkspaceId* muss der Ressourcen-ID Ihres Log Analytics-Arbeitsbereichs entsprechen.

## <a name="log-analytics-records"></a>Log Analytics-Datensätze

Die Diagnose von Azure Automation erstellt zwei Arten von Datensätzen in Log Analytics und kennzeichnet sie als **AzureDiagnostics**. In den folgenden Abfragen wird die aktualisierte Abfragesprache für Log Analytics verwendet. Informationen zu allgemeinen Abfragen zwischen der alten Abfragesprache und der neuen Azure Log Analytics-Abfragesprache finden Sie unter [Legacy to new Azure Log Analytics Query Language cheat sheet](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language) (Cheat Sheet: Gegenüberstellung von alter Abfragesprache und Azure Log Analytics-Abfragesprache).

### <a name="job-logs"></a>Auftragsprotokolle
| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Benutzer oder das System, von dem der Vorgang initiiert wurde. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| ResultType |Der Status des Runbookauftrags. Mögliche Werte:<br>- Neu<br>- Gestartet<br>- Beendet<br>- Ausgesetzt<br>- Fehler<br>- Abgeschlossen |
| Category (Kategorie) | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobLogs“. |
| NameVorgang | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation lautet der Wert „Job“. |
| Ressource | Name des Automation-Kontos |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResultDescription |Beschreibt den resultierenden Zustand des Runbookauftrags. Mögliche Werte:<br>- Auftrag gestartet<br>- Fehler beim Ausführen des Auftrags<br>- Auftrag abgeschlossen |
| CorrelationId |Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt. |
| ResourceId |Gibt die Ressourcen-ID des Runbooks für das Azure Automation-Konto an. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Auftragsdatenströme
| Eigenschaft | BESCHREIBUNG |
| --- | --- |
| TimeGenerated |Ausführungsdatum und -uhrzeit des Runbookauftrags. |
| RunbookName_s |Der Name des Runbooks. |
| Caller_s |Der Benutzer oder das System, von dem der Vorgang initiiert wurde. Mögliche Werte sind entweder eine E-Mail-Adresse oder, bei geplanten Aufträgen, ein System. |
| StreamType_s |Der Typ des Auftragsstreams. Mögliche Werte:<br>- Status<br>- Ausgabe<br>- Warnung<br>- Fehler<br>- Debuggen<br>- Ausführlich |
| Tenant_g | GUID, die den Mandanten für den Aufrufer identifiziert. |
| JobId_g |Die GUID, bei der es sich um die ID des Runbookauftrags handelt. |
| ResultType |Der Status des Runbookauftrags. Mögliche Werte:<br>– In Bearbeitung |
| Category (Kategorie) | Klassifizierung des Datentyps. Für Automation lautet der Wert „JobStreams“. |
| NameVorgang | Gibt den Typ des in Azure ausgeführten Vorgangs an. Für Automation lautet der Wert „Job“. |
| Ressource | Name des Automation-Kontos |
| SourceSystem | So wurden die Daten von Log Analytics gesammelt. Immer *Azure* für Azure-Diagnose. |
| ResultDescription |Enthält den Ausgabedatenstrom des Runbooks. |
| CorrelationId |Die GUID, bei der es sich um die Korrelations-ID des Runbookauftrags handelt. |
| ResourceId |Gibt die Ressourcen-ID des Runbooks für das Azure Automation-Konto an. |
| SubscriptionId | Die Azure-Abonnement-ID (GUID) für das Automation-Konto. |
| ResourceGroup | Name der Ressourcengruppe für das Automation-Konto. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Anzeigen von Automation-Protokollen in Log Analytics
Nachdem Sie damit begonnen haben, Ihre Automation-Auftragsprotokolle an Log Analytics zu senden, sehen wir uns nun an, wie Sie diese Protokolle in Log Analytics verwenden können.

Führen Sie die folgende Abfrage aus, um die Protokolle anzuzeigen: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Senden einer E-Mail, wenn ein Runbookauftrag mit einem Fehler abgebrochen oder angehalten wird
Einer der häufigsten Kundenwünsche ist die Möglichkeit, eine E-Mail oder SMS zu senden, wenn bei einem Runbookauftrag ein Problem auftritt.   

Zum Erstellen einer Warnungsregel erstellen Sie zunächst eine Protokollsuche für die Datensätze des Runbookauftrags, die die Warnung auslösen sollen. Klicken Sie auf die Schaltfläche **Warnung**, um die Warnungsregel zu erstellen und zu konfigurieren.

1. Klicken Sie auf der Seite mit der Log Analytics-Übersicht auf **Protokollsuche**.
2. Erstellen Sie eine Protokollsuchabfrage für die Warnung, indem Sie folgenden Suchtext in das Abfragefeld eingeben: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` Sie können auch nach dem Runbook-Namen gruppieren, indem Sie folgende Abfrage verwenden: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Wenn Sie Protokolle von mehreren Automation-Konten oder Abonnements in Ihrem Arbeitsbereich eingerichtet haben, können Sie Ihre Warnungen nach Abonnement und Automation-Konto gruppieren. Der Name des Automation-Kontos kann dem Ressourcenfeld in der JobLogs-Suche entnommen werden.
1. Klicken Sie oben auf der Seite auf **Neue Warnungsregel**, um den Bildschirm **Regel erstellen** zu öffnen. Weitere Informationen zu den Konfigurationsoptionen für Warnungen finden Sie unter [Protokollwarnungen in Azure Monitor – Warnungen](../monitoring-and-diagnostics/monitor-alerts-unified-log.md).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Durchführen einer Suche nach allen Aufträgen, die mit Fehlern abgeschlossen wurden
Neben dem Erstellen von Warnungen für Fehler können Sie auch ermitteln, wenn ein Runbook-Auftrag einen Fehler ohne Abbruch aufweist. In diesen Fällen erzeugt PowerShell einen Fehlerdatenstrom, die Fehler ohne Abbruch führen jedoch nicht dazu, dass der Auftrag unterbrochen wird oder fehlschlägt.    

1. Klicken Sie in Ihrem Log Analytics-Arbeitsbereich auf **Protokollsuche**.
2. Geben Sie in das Abfragefeld `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` ein, und klicken Sie dann auf die Schaltfläche **Suchen**.

### <a name="view-job-streams-for-a-job"></a>Anzeigen von Auftragsdatenströmen für einen Auftrag
Wenn Sie einen Auftrag debuggen, sollten Sie ggf. auch einen Blick in die Auftragsdatenströme werfen. Mit der folgenden Abfrage werden alle Datenströme für einen einzelnen Auftrag mit der GUID „2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0“ abgerufen:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Anzeigen des Auftragsstatusverlaufs
Abschließend möchten Sie möglicherweise Ihren Auftragsverlauf visualisieren. Sie können die folgende Abfrage verwenden, um nach dem Statusverlauf Ihrer Aufträge zu suchen.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Diagramm zum Auftragsstatusverlauf in Log Analytics](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="remove-diagnostic-settings"></a>Entfernen von Diagnoseeinstellungen

Führen Sie zum Entfernen der Diagnoseeinstellung aus dem Automation-Konto die folgenden Befehle aus:

```powershell-interactive
$automationAccountId = "[resource id of your automation account]"

Remove-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

## <a name="summary"></a>Zusammenfassung

Sie erhalten einen besseren Einblick in den Status Ihrer Automation-Aufträge, wenn Sie den Status und die Streamdaten Ihres Automation-Auftrags an Log Analytics senden und:
+ Warnungen einrichten, um bei Problemen benachrichtigt zu werden.
+ Benutzerdefinierte Ansichten und Suchabfragen zum Anzeigen von Runbook-Ergebnissen, Runbook-Auftragsstatus und anderer wichtiger Schlüsselindikatoren oder Metriken verwenden.  

Log Analytics bietet eine höhere operative Transparenz für Ihre Automation-Aufträge, sodass schneller auf Vorfälle reagiert werden kann.  

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Erstellen verschiedener Suchabfragen und zur Überprüfung der Automation-Auftragsprotokolle mit Log Analytics finden Sie unter [Protokollsuchen in Log Analytics](../log-analytics/log-analytics-log-searches.md).
* Unter [Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md) erfahren Sie, wie Sie die Ausgabe und Fehlermeldungen von Runbooks erstellen und abrufen.
* Weitere Informationen zum Ausführen von Runbooks, zum Überwachen von Runbookaufträgen sowie andere technische Details finden Sie unter [Verfolgen eines Runbookauftrags](automation-runbook-execution.md).
* Weitere Informationen zu Log Analytics und Datenerfassungsquellen finden Sie unter [Sammeln von Azure-Speicherdaten in Log Analytics – Übersicht](../log-analytics/log-analytics-azure-storage.md).
