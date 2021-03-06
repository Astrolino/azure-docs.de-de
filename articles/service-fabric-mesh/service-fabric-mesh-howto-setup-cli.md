---
title: Einrichten der Azure Service Fabric Mesh CLI | Microsoft Docs
description: Erfahren Sie, wie die Azure Service Fabric CLI eingerichtet wird.
services: service-fabric-mesh
keywords: ''
author: tylermsft
ms.author: twhitney
ms.date: 07/26/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7e8a12a215c94102f6b08262f129faebf9cfcde9
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115623"
---
# <a name="set-up-the-service-fabric-mesh-cli"></a>Einrichten der Service Fabric Mesh CLI
Die Service Fabric Mesh-Befehlszeilenschnittstelle (Command Line Interface, CLI) ist zum Bereitstellen und Verwalten von Ressourcen in Service Fabric Mesh erforderlich. 

Für die Vorschauversion wird die Azure Service Fabric Mesh CLI als eine Erweiterung der Azure CLI bereitgestellt. Sie können sie in der Azure Cloud Shell oder in einer lokalen Installation der Azure CLI installieren. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

## <a name="install-the-service-fabric-mesh-cli-locally"></a>Lokales Installieren der Service Fabric Mesh-Befehlszeilenschnittstelle
Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie die Version 2.0.43 oder höher der Azure CLI installieren. Führen Sie `az --version` aus, um die Version zu finden. Informationen zur Installation und zu einem Upgrade auf die neueste Version der CLI finden Sie unter [Installieren der Azure CLI][azure-cli-install].

Installieren Sie das Azure Service Fabric Mesh CLI-Erweiterungsmodul mit dem folgenden Befehl. 

```azurecli-interactive
az extension add --name mesh
```

Führen Sie zum Aktualisieren eines vorhandenen Azure Service Fabric Mesh-CLI-Moduls den folgenden Befehl aus.

```azurecli-interactive
az extension update --name mesh
```

Sie können auch Ihre [Windows-Entwicklungsumgebung](service-fabric-mesh-howto-setup-developer-environment-sdk.md) einrichten.

[azure-cli-install]: /cli/azure/install-azure-cli
