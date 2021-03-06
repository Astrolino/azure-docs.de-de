---
title: Herstellen einer Verbindung mit Azure Stack über PowerShell als Benutzer | Microsoft-Dokumentation
description: Lernen Sie die Schritte zum Verbinden mit der Azure Stack-Instanz des Benutzers kennen.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2018
ms.author: sethm
ms.reviewer: Balsu.G
ms.openlocfilehash: acdad9788737f4f552cedc1b5f42e03e2288dba8
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159078"
---
# <a name="connect-to-azure-stack-with-powershell-as-a-user"></a>Herstellen einer Verbindung mit Azure Stack über PowerShell als Benutzer

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

Dieser Artikel enthält die Schritte zum Herstellen einer Verbindung mit Ihrer Azure Stack-Instanz. Sie müssen eine Verbindung herstellen, um Azure Stack-Ressourcen mit PowerShell zu verwalten. Beispielsweise können Sie PowerShell verwenden, um Angebote zu abonnieren, virtuelle Computer zu erstellen und Azure Resource Manager-Vorlagen bereitzustellen. um PowerShell-Cmdlets auszuführen.

Bei der Einrichtung ist Folgendes zu beachten:
  - Stellen Sie sicher, dass die Voraussetzungen erfüllt sind.
  - Stellen Sie eine Verbindung mit Azure Active Directory (Azure AD) oder Active Directory-Verbunddienste (AD FS) her. 
  - Registrieren Sie Ressourcenanbieter.
  - Testen Sie die Verbindung.

## <a name="prerequisites"></a>Voraussetzungen

Sie können diese Voraussetzungen über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client konfigurieren, sofern [eine VPN-Verbindung](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) besteht:

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).
* Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md).

Sie müssen die folgenden Skriptvariablen durch Werte der Azure Stack-Konfiguration ersetzen:

- **Name des Azure AD-Mandanten**  
  Der Name des zum Verwalten von Azure Stack verwendeten Azure AD-Mandanten, z.B. ihrverzeichnis.onmicrosoft.com.
- **Azure Resource Manager-Endpunkt**  
  Beim Azure Stack Development Kit wird dieser Wert auf https://management.local.azurestack.external festgelegt. Um diesen Wert für integrierte Azure Stack-Systeme zu erhalten, wenden Sie sich an Ihren Dienstanbieter.

## <a name="connect-with-azure-ad"></a>Herstellen einer Verbindung mit Azure AD

  ```PowerShell
  $AADTenantName = "yourdirectory.onmicrosoft.com"
  $ArmEndpoint = "https://management.local.azurestack.external"

  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
  $TenantId = (invoke-restmethod "$($AuthEndpoint)/$($AADTenantName)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantId
   ```

## <a name="connect-with-ad-fs"></a>Herstellen einer Verbindung mit AD FS

  ```PowerShell  
  $ArmEndpoint = "https://management.local.azurestack.external"

  # Register an Azure Resource Manager environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  $AuthEndpoint = (Get-AzureRmEnvironment -Name "AzureStackUser").ActiveDirectoryAuthority.TrimEnd('/')
  $tenantId = (invoke-restmethod "$($AuthEndpoint)/.well-known/openid-configuration").issuer.TrimEnd('/').Split('/')[-1]

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $tenantId
  ```

## <a name="register-resource-providers"></a>Registrieren von Ressourcenanbietern

Ressourcenanbieter werden nicht automatisch für neue Benutzerabonnements registriert, für die über das Portal keine Ressourcen bereitgestellt wurden. Sie können explizit einen Ressourcenanbieter registrieren, indem Sie das folgende Skript ausführen:

```PowerShell  
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>Testen der Konnektivität

Wenn alles eingerichtet ist, können Sie mit PowerShell die Konnektivität testen, um Ressourcen in Azure Stack zu erstellen. Erstellen Sie als Test eine Ressourcengruppe für eine Anwendung, und fügen Sie einen virtuellen Computer hinzu. Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe mit dem Namen „MyResourceGroup“ zu erstellen:

```PowerShell  
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Nächste Schritte

- [Entwickeln von Vorlagen für Azure Stack](azure-stack-develop-templates.md)
- [Bereitstellen von Vorlagen mit PowerShell](azure-stack-deploy-template-powershell.md)
- Wenn Sie PowerShell für die Umgebung des Cloudbetreibers einrichten möchten, finden Sie weitere Informationen im Artikel [Konfigurieren der PowerShell-Umgebung des Azure Stack-Betreibers](../azure-stack-powershell-configure-admin.md).