---
title: Erstellen eines öffentlichen Standard-Lastenausgleichs mit einem Zonen-Front-End mit einer öffentlichen IP-Adresse mithilfe von Azure PowerShell | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Azure PowerShell einen öffentlichen Standard-Lastenausgleich mit einem Zonen-Front-End mit öffentlicher IP-Adresse erstellen.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: dbb4176ac61cf707b28cddc98db80a1188be3cc8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
ms.locfileid: "31592129"
---
#  <a name="create-a-public-load-balancer-standard-with-zonal-frontend-using-azure-powershell"></a>Erstellen eines öffentlichen Standard-Lastenausgleichs mit einem Zonen-Front-End mithilfe von Azure PowerShell

In diesem Artikel wird die Erstellung eines öffentlichen [Standard-Lastenausgleichs](https://aka.ms/azureloadbalancerstandard) mit einem Zonen-Front-End mithilfe einer öffentlichen Standard-IP-Adresse erläutert. Um zu verstehen, wie Verfügbarkeitszonen mit dem Standard-Lastenausgleich arbeiten, lesen Sie [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md). 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

> [!NOTE]
> Unterstützung für Verfügbarkeitszonen ist für ausgewählte Azure-Ressourcen und -Regionen sowie VM-Größenkategorien verfügbar. Weitere Informationen zu den ersten Schritten sowie zu den Azure-Ressourcen, -Regionen und VM-Größenkategorien, die mit Verfügbarkeitszonen verwendet werden können, finden Sie unter [Overview of Availability Zones in Azure (Preview) (Übersicht über Verfügbarkeitszonen in Azure (Vorschauversion))](https://docs.microsoft.com/azure/availability-zones/az-overview). Wenn Sie Unterstützung benötigen, können Sie über [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) Kontakt aufnehmen oder [ein Azure-Supportticket erstellen](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich mit dem Befehl `Connect-AzureRmAccount` bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Ressourcengruppe erstellen

Erstellen Sie mithilfe des folgenden Befehls eine Ressourcengruppe:

```powershell
New-AzureRmResourceGroup -Name myResourceGroupZLB -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Erstellen eines öffentlichen IP-Standards 
Erstellen Sie mithilfe des folgenden Befehls einen öffentlichen IP-Standard:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupZLB -Name 'myPublicIPZonal' `
  -Location westeurope -AllocationMethod Static -Sku Standard -zone 1
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Erstellen einer Front-End-IP-Konfiguration für die Website

Erstellen Sie mithilfe des folgenden Befehls eine Front-End-IP-Konfiguration:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEnd' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Erstellen des Back-End-Adresspools

Erstellen Sie mithilfe des folgenden Befehls einen Back-End-Adresspool:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Erstellen eines Lastenausgleichstests auf Port 80

Erstellen Sie für den Lastenausgleich mithilfe des folgenden Befehls einen Integritätstest auf Port 80:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
 Erstellen Sie mithilfe des folgenden Befehls eine Lastenausgleichsregel:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie mithilfe des folgenden Befehls einen Load Balancer Standard:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroupZLB -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr zu [Standard-Lastenausgleich und Verfügbarkeitszonen](load-balancer-standard-availability-zones.md).



