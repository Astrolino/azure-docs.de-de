---
title: Verwenden von Bedingungen in Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie Azure-Ressourcen basierend auf Bedingungen bereitstellen.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/18/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 552b39c520396942fa81f963c0cfa1c8c7b47db4
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456965"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Tutorial: Verwenden von Bedingungen in Azure Resource Manager-Vorlagen

Es wird beschrieben, wie Sie Azure-Ressourcen basierend auf Bedingungen bereitstellen. 

Das in diesem Tutorial verwendete Szenario ähnelt dem Szenario unter [Tutorial: Erstellen von Azure Resource Manager-Vorlagen mit abhängigen Ressourcen](./resource-manager-tutorial-create-templates-with-dependent-resources.md). In diesem Tutorial erstellen Sie einen virtuellen Computer, ein virtuelles Netzwerk und einige andere abhängige Ressourcen, z.B. ein Speicherkonto. Anstatt jedes Mal ein neues Speicherkonto zu erstellen, können Benutzer zwischen dem Erstellen eines neuen Speicherkontos und dem Verwenden eines vorhandenen Speicherkontos wählen. Sie definieren einen zusätzlichen Parameter, um dieses Ziel zu erreichen. Wenn der Wert des Parameters „new“ lautet, wird ein neues Speicherkonto erstellt.

Dieses Tutorial enthält die folgenden Aufgaben:

> [!div class="checklist"]
> * Öffnen einer Schnellstartvorlage
> * Ändern der Vorlage
> * Bereitstellen der Vorlage
> * Bereinigen von Ressourcen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:

* [Visual Studio Code](https://code.visualstudio.com/) mit der [Erweiterung „Azure Resource Manager-Tools“](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Öffnen einer Schnellstartvorlage

„Azure-Schnellstartvorlagen“ ist ein Repository für Resource Manager-Vorlagen. Statt eine Vorlage von Grund auf neu zu erstellen, können Sie eine Beispielvorlage verwenden und diese anpassen. Die in diesem Tutorial verwendete Vorlage heißt [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Bereitstellen eines einfachen virtuellen Windows-Computers).

1. Wählen Sie in Visual Studio Code **Datei**>**Datei öffnen** aus.
2. Fügen Sie in **Dateiname** die folgende URL ein:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Wählen Sie **Öffnen** aus, um die Datei zu öffnen.
4. Wählen Sie **Datei**>**Speichern unter** aus, um eine Kopie der Datei als **azuredeploy.json** auf dem lokalen Computer zu speichern.

## <a name="modify-the-template"></a>Ändern der Vorlage

Nehmen Sie zwei Änderungen an der vorhandenen Vorlage vor:

* Fügen Sie einen Parameter für den Speicherkontonamen hinzu. Benutzer können entweder einen neuen oder einen vorhandenen Speicherkontonamen angeben.
* Fügen Sie einen neuen Parameter mit dem Namen **newOrExisting** hinzu. Die Bereitstellung nutzt diesen Parameter, um zu ermitteln, wann ein neues Speicherkonto erstellt oder ein vorhandenes Speicherkonto verwendet werden soll.

1. Öffnen Sie **azuredeploy.json** in Visual Studio Code.
2. Ersetzen Sie **variables('storageAccountName')** in der gesamten Vorlage durch **parameters('storageAccountName')**.  Es gibt drei Darstellungen von **variables('storageAccountName')**.
3. Entfernen Sie die folgende Variablendefinition:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Fügen Sie der Vorlage die folgenden beiden Parameter hinzu:

    ```json
    "storageAccountName": {
      "type": "string"
    },    
    "newOrExisting": {
      "type": "string", 
      "allowedValues": [
        "new", 
        "existing"
      ]
    },
    ```
    Die aktualisierte Parameterdefinition sieht wie folgt aus:

    ![Resource Manager-Nutzungsbedingung](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Fügen Sie am Anfang der Speicherkontodefinition die folgende Zeile hinzu.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    ```

    Mit der Bedingung wird der Wert des Parameters **newOrExisting** überprüft. Wenn der Parameterwert **new** lautet, erstellt die Bereitstellung das Speicherkonto.

    Die aktualisierte Speicherkontodefinition sieht wie folgt aus:

    ![Resource Manager-Nutzungsbedingung](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)
6. Aktualisieren Sie **storageUri** mit dem folgenden Wert:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Diese Änderung ist erforderlich, wenn Sie ein vorhandenes Speicherkonto unter einer anderen Ressourcengruppe verwenden.

7. Speichern Sie die Änderungen.

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Stellen Sie die Vorlage gemäß der Anleitung unter [Bereitstellen der Vorlage](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) bereit.

Beim Bereitstellen der Vorlage mit Azure PowerShell müssen Sie einen zusätzlichen Parameter angeben:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password"
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
$vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
New-AzureRmResourceGroupDeployment -Name mydeployment1018 -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin -adminPassword $vmPW `
    -dnsLabelPrefix $dnsLabelPrefix -storageAccountName $storageAccountName -newOrExisting $newOrExisting `
    -TemplateFile azuredeploy.json
```

> [!NOTE]
> Für die Bereitstellung tritt ein Fehler auf, wenn **newOrExisting** auf **new** festgelegt, aber das Speicherkonto mit dem angegebenen Speicherkontonamen bereits vorhanden ist.

Versuchen Sie, eine andere Bereitstellung zu erstellen, bei der **newOrExisting** auf „existing“ festgelegt ist, und geben Sie ein vorhandenes Speicherkonto an. Informationen zum Erstellen eines Speicherkontos im Voraus finden Sie unter [Speicherkonto erstellen](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie die Azure-Ressourcen nicht mehr benötigen, löschen Sie die Ressourcengruppe, um die bereitgestellten Ressourcen zu bereinigen.

1. Wählen Sie im Azure-Portal im linken Menü die Option **Ressourcengruppe** aus.
2. Geben Sie den Namen der Ressourcengruppe in das Feld **Nach Name filtern** ein.
3. Wählen Sie den Namen der Ressourcengruppe aus.  Es werden insgesamt sechs Ressourcen in der Ressourcengruppe angezeigt.
4. Wählen Sie **Ressourcengruppe löschen** aus dem Menü ganz oben aus.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial entwickeln Sie eine Vorlage, bei der Benutzer zwischen dem Erstellen eines neuen Speicherkontos und dem Verwenden eines vorhandenen Speicherkontos wählen können. Für den in diesem Tutorial erstellten virtuellen Computer sind ein Administratorbenutzername und ein Kennwort erforderlich. Anstatt das Kennwort während der Bereitstellung zu übergeben, können Sie es mit Azure Key Vault vorab speichern und während der Bereitstellung abrufen. Informationen zum Abrufen von Geheimnissen aus Azure Key Vault und Verwenden der Geheimnisse in der Vorlagenbereitstellung finden Sie hier:

> [!div class="nextstepaction"]
> [Tutorial: Integrate Azure Key Vault in Resource Manager Template deployment](./resource-manager-tutorial-use-key-vault.md) (Tutorial: Integrieren von Azure Key Vault in die Resource Manager-Vorlagenbereitstellung)
