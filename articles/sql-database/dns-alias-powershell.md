---
title: PowerShell für den DNS-Alias für Azure SQL-Datenbank | Microsoft-Dokumentation
description: Mit PowerShell-Cmdlets wie New-AzureRMSqlServerDNSAlias können Sie neue Clientverbindungen mit einem anderen Azure SQL-Datenbankserver umleiten, ohne Clientkonfigurationen vornehmen zu müssen.
keywords: dns sql database
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.devlang: PowerShell
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: genemi,amagarwa,maboja
manager: craigg
ms.date: 02/05/2018
ms.openlocfilehash: 095e6c6d59bf73bb74e2d8fbe3d1506601ab533e
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471117"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>PowerShell für den DNS-Alias für Azure SQL-Datenbank

Dieser Artikel bietet ein PowerShell-Skript, das veranschaulicht, wie Sie einen DNS-Alias für Azure SQL-Datenbank verwalten können. Das Skript führt die folgenden Cmdlets mit folgenden Aktionen aus:

Die im Codebeispiel verwendeten Cmdlets lauten wie folgt:

- [New-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Erstellt einen neuen DNS-Alias im Azure SQL-Datenbankdienstsystem. Der Alias verweist auf Azure SQL-Datenbankserver 1.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Ruft alle DNS-Aliase auf, die SQL-Datenbankserver 1 zugewiesen sind, und listet diese auf.
- [Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Ändert den Servernamen, auf dessen Verweis der Alias konfiguriert ist, von Server 1 in SQL-Datenbankserver 2.
- [Remove-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Entfernt den DNS-Alias aus SQL-Datenbankserver 2 anhand des Aliasnamens.

Die oben angegebenen PowerShell-Cmdlets wurden zum Modul **AzureRm.Sql** ab Modulversion 5.1.1 hinzugefügt.

## <a name="dns-alias-in-connection-string"></a>DNS-Alias in der Verbindungszeichenfolge

Um eine Verbindung mit einem bestimmten Azure SQL-Datenbankserver herzustellen, kann ein Client wie SQL Server Management Studio (SSMS) anstelle des echten Servernamens den DNS-Aliasnamen angeben. In der folgenden Beispielserverzeichenfolge ersetzt der Alias *any-unique-alias-name* den ersten durch Punkte getrennten Knoten in der Serverzeichenfolge mit vier Knoten:

- Beispielserverzeichenfolge: `any-unique-alias-name.database.windows.net`.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie das in diesem Artikel angegebene PowerShell-Demoskript ausführen möchten, gelten die folgenden Voraussetzungen:

- Ein Azure-Abonnement und -Konto. Eine kostenlose Testversion erhalten Sie unter [https://azure.microsoft.com/free/][https://azure.microsoft.com/free/].
- Azure PowerShell-Modul mit dem Cmdlet **New-AzureRMSqlServerDNSAlias**
  - Informationen zum Installieren oder Durchführen eines Upgrades finden Sie unter [Installieren und Konfigurieren von Azure PowerShell][install-azurerm-ps-84p].
  - Führen Sie `Get-Module -ListAvailable AzureRM;` in der Datei „powershell\_ise.exe“ aus, um die entsprechende Version zu ermitteln.
- Zwei Azure SQL-Datenbankserver

## <a name="code-example"></a>Codebeispiel

Im folgenden PowerShell-Codebeispiel werden mehreren Variablen zunächst Literalwerte zugewiesen. Um den Code auszuführen, müssen Sie zuerst alle Platzhalterwerte in die entsprechenden tatsächlichen Werten in Ihrem System ändern. Alternativ können Sie den Code einfach im vorliegenden Zustand untersuchen. Die Konsolenausgabe des Codes wird ebenfalls bereitgestellt.

```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Connect-AzureRmAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzureRmSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzureRMSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzureRMSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```

### <a name="actual-console-output-from-the-powershell-example"></a>Tatsächliche Konsolenausgabe aus dem PowerShell-Beispiel

Die folgende Konsolenausgabe wurde kopiert und in eine tatsächliche Ausführung eingefügt.

```powershell
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.

[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount :

[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName
----------------- ----------         ------------------
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>>
```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Erläuterung des DNS-Aliasfeatures für SQL-Datenbank finden Sie unter [DNS-Alias für Azure SQL-Datenbank][dns-alias-overview-37v].

<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-azurerm-ps-84p]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps

[dns-alias-overview-37v]: dns-alias-overview.md
