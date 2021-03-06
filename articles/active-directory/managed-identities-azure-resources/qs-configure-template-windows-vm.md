---
title: Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe einer Vorlage
description: Ausführliche Anweisungen zum Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einer Azure-VM mithilfe einer Azure Resource Manager-Vorlage
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: f50714538cdfd1a0bd258a3b78d0885e3c7beed1
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018484"
---
# <a name="configure-managed-identities-for-azure-resources-on-an-azure-vm-using-a-templates"></a>Konfigurieren von verwalteten Identitäten für Azure-Ressourcen auf einem virtuellen Azure-Computer mithilfe einer Vorlage

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Verwaltete Identitäten für Azure-Ressourcen stellen für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. 

In diesem Artikel erfahren Sie, wie Sie mithilfe der Azure Resource Manager-Bereitstellungsvorlage die folgenden Vorgänge für verwaltete Identitäten für Azure-Ressourcen auf einer Azure-VM ausführen können:

## <a name="prerequisites"></a>Voraussetzungen

- Wenn Sie nicht mit der Verwendung der Azure Resource Manager-Bereitstellungsvorlage vertraut sind, sehen Sie sich die [Übersicht](overview.md) an. **Machen Sie sich den [Unterschied zwischen einer vom System und einer vom Benutzer zugewiesenen verwalteten Identität](overview.md#how-does-it-work)** bewusst.
- Wenn Sie noch kein Azure-Konto haben, sollten Sie sich [für ein kostenloses Konto registrieren](https://azure.microsoft.com/free/), bevor Sie fortfahren.
- Um die Verwaltungsvorgänge in diesem Artikel auszuführen, benötigt Ihr Konto die folgenden Zuweisungen der rollenbasierten Azure-Zugriffssteuerung:

    > [!NOTE]
    > Es sind keine weiteren Azure AD-Verzeichnisrollenzuweisungen erforderlich.

    - [Mitwirkender für virtuelle Computer](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor), um eine VM zu erstellen und die vom System und/oder Benutzer zugewiesene verwaltete Identität für eine Azure-VM zu aktivieren bzw. zu entfernen
    - [Mitwirkender für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-contributor), um eine vom Benutzer zugewiesene verwaltete Identität zu erstellen
    - [Operator für verwaltete Identität](/azure/role-based-access-control/built-in-roles#managed-identity-operator), um eine vom Benutzer zugewiesene verwaltete Identität einer VM zuzuweisen bzw. davon zu entfernen

## <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

Wie beim Azure-Portal und der Skripterstellung bieten [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)-Vorlagen die Möglichkeit, von einer Azure-Ressourcengruppe definierte neue bzw. geänderte Ressourcen bereitzustellen. Für die Vorlagenbearbeitung und -bereitstellung sind mehrere Optionen sowohl lokal als auch portalbasiert verfügbar, einschließlich:

   - Verwenden einer [benutzerdefinierten Vorlage vom Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template). Dies ermöglicht Ihnen, eine ganz neue Vorlage zu erstellen oder eine Vorlage, die auf einer vorhandenen, oft verwendeten Vorlage oder [Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/) basiert.
   - Ableiten von einer vorhandenen Ressourcengruppe, indem eine Vorlage aus [der ursprünglichen Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) oder aus dem [aktuellen Status der Bereitstellung](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group) exportiert wird.
   - Verwenden eines lokalen [JSON-Editors (z.B. VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md) und anschließendes Hochladen und Bereitstellen mithilfe von PowerShell oder CLI.
   - Verwenden des [Azure-Ressourcengruppenprojekts](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) von Visual Studio für das Erstellen und Bereitstellen einer Vorlage.  

Unabhängig von der gewählten Option ist die Vorlagensyntax während der ursprünglichen und erneuten Bereitstellung identisch. Die Aktivierung einer vom System oder vom Benutzer zugewiesenen verwalteten Identität auf einer neuen oder vorhandenen VM erfolgt auf dieselbe Weise. Standardmäßig führt Azure Resource Manager außerdem ein [inkrementelles Update](../../azure-resource-manager/deployment-modes.md) für Bereitstellungen durch.

## <a name="system-assigned-managed-identity"></a>Systemseitig zugewiesene verwaltete Identität

In diesem Abschnitt aktivieren und deaktivieren Sie eine vom System zugewiesene verwaltete Identität mithilfe einer Azure Resource Manager-Vorlage.

### <a name="enable-system-assigned-managed-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Aktivieren einer vom System zugewiesenen verwalteten Identität beim Erstellen einer Azure-VM oder für eine vorhandene VM

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

2. Um die vom System zugewiesene verwaltete Identität zu aktivieren, laden Sie die Vorlage in einem Editor, suchen Sie die gewünschte Ressource `Microsoft.Compute/virtualMachines` im Abschnitt `resources`, und fügen Sie die `"identity"`-Eigenschaft in der gleichen Ebene wie die `"type": "Microsoft.Compute/virtualMachines"`-Eigenschaft hinzu. Verwenden Sie die folgende Syntax:

   ```JSON
   "identity": { 
       "type": "SystemAssigned"
   },
   ```

3. (Optional:) Fügen Sie die verwalteten Identitäten des virtuellen Computers für Azure-Ressourcenerweiterungen als ein `resources`-Element hinzu. Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können.  Verwenden Sie die folgende Syntax:

   >[!NOTE] 
   > Im folgenden Beispiel wird angenommen, dass eine Windows-VM-Erweiterung (`ManagedIdentityExtensionForWindows`) bereitgestellt wird. Sie können die Konfiguration auch für Linux ausführen, indem Sie stattdessen `ManagedIdentityExtensionForLinux` für die Elemente `"name"` und `"type"` verwenden. Für den VM-Erweiterungsendpunkt ist die Einstufung als veraltet für Januar 2019 geplant.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2018-06-01",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

4. Wenn Sie fertig sind, sollten die folgenden Abschnitte dem Abschnitt `resource` der Vorlage hinzugefügt worden sein, und diese sollte wie folgt aussehen:

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "SystemAssigned",
                },
            },
            {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2018-06-01",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
                }
            }
        }
    ]
   ```

### <a name="assign-a-role-the-vms-system-assigned-managed-identity"></a>Zuweisen einer Rolle zur vom System zugewiesenen verwalteten Identität der VM

Nachdem Sie die Option für eine vom System zugewiesene verwaltete Identität auf Ihrer VM aktiviert haben, möchten Sie der Identität möglicherweise eine Rolle wie **Leser** für die Ressourcengruppe gewähren, in der sie erstellt wurde.

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.
 
2. Laden Sie die Vorlage in einen [Editor](#azure-resource-manager-templates), und fügen Sie die folgenden Informationen hinzu, um Ihrer VM **Lesezugriff** für die Ressourcengruppe zu gewähren, in der sie erstellt wurde.  Ihre Vorlagenstruktur kann je nach Editor und dem von Ihnen gewählten Bereitstellungsmodell variieren.
   
   Fügen Sie unter dem Abschnitt `parameters` Folgendes hinzu:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Fügen Sie unter dem Abschnitt `variables` Folgendes hinzu:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Fügen Sie unter dem Abschnitt `resources` Folgendes hinzu:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-managed-identity-from-an-azure-vm"></a>Deaktivieren einer vom System zugewiesenen verwalteten Identität auf einer Azure-VM

Bei einem virtuellen Computer, für den die vom System zugewiesene verwaltete Identität nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

2. Laden Sie die Vorlage in einen [Editor](#azure-resource-manager-templates), und suchen Sie nach der gewünschten `Microsoft.Compute/virtualMachines`-Ressource im Abschnitt `resources`. Wenn Ihre VM nur über eine vom System zugewiesene verwaltete Identität verfügt, können Sie diese deaktivieren, indem Sie den Identitätstyp in `None` ändern.  
   
   **Microsoft.Compute/virtualMachines-API, Version 2018-06-01**

   Wenn Ihre VM sowohl vom System als auch vom Benutzer zugewiesene verwaltete Identitäten enthält, entfernen Sie `SystemAssigned` aus dem Identitätstyp und behalten `UserAssigned` zusammen mit den `userAssignedIdentities`-Wörterbuchwerten bei.

   **Microsoft.Compute/virtualMachines-API, Version 2018-06-01**
   
   Wenn `2017-12-01` als `apiVersion` verwendet wird und Ihre VM sowohl vom System als auch vom Benutzer zugewiesene verwaltete Identitäten enthält, entfernen Sie `SystemAssigned` aus dem Identitätstyp und behalten `UserAssigned` zusammen mit dem `identityIds`-Array der vom Benutzer zugewiesenen verwalteten Identitäten bei.  
   
Das folgende Beispiel zeigt, wie Sie eine vom System zugewiesene verwaltete Identität von einer VM ohne vom Benutzer zugewiesene verwaltete Identitäten entfernen:

```JSON
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[parameters('vmName')]",
    "location": "[resourceGroup().location]",
    "identity": { 
        "type": "None"
}
```

## <a name="user-assigned-managed-identity"></a>Benutzerseitig zugewiesene verwaltete Identität

In diesem Abschnitt weisen Sie einer Azure-VM mit der Azure Resource Manager-Vorlage eine vom Benutzer zugewiesene verwaltete Identität zu.

> [!Note]
> Weitere Informationen zum Erstellen einer vom Benutzer zugewiesenen verwalteten Identität mit einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen einer vom Benutzer zugewiesenen verwalteten Identität](how-to-manage-ua-identity-arm.md#create-a-user-assigned-managed-identity).

 ### <a name="assign-a-user-assigned-managed-identity-to-an-azure-vm"></a>Zuweisen einer vom Benutzer zugewiesenen verwalteten Identität zu einem virtuellen Azure-Computer

1. Fügen Sie unter dem `resources`-Element den folgenden Eintrag hinzu, um Ihrer VM eine vom Benutzer zugewiesene verwaltete Identität zuzuweisen.  Achten Sie darauf, dass Sie `<USERASSIGNEDIDENTITY>` durch den Namen der vom Benutzer zugewiesenen verwalteten Identität ersetzen, die Sie erstellt haben.

   **Microsoft.Compute/virtualMachines-API, Version 2018-06-01**

   Wenn `2018-06-01` als `apiVersion` verwendet wird, werden die vom Benutzer zugewiesenen verwalteten Identitäten im `userAssignedIdentities`-Wörterbuchformat gespeichert, und der Wert `<USERASSIGNEDIDENTITYNAME>` muss in einer im Abschnitt `variables` der Vorlage definierten Variable gespeichert werden.

   ```json
   {
       "apiVersion": "2018-06-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "userAssignedIdentities": {
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
           }
        }
   }
   ```
   
   **Microsoft.Compute/virtualMachines-API, Version 2017-12-01**
    
   Wenn `2017-12-01` als `apiVersion` verwendet wird, werden die vom Benutzer zugewiesenen verwalteten Identitäten im `identityIds`-Array gespeichert, und der Wert `<USERASSIGNEDIDENTITYNAME>` muss in einer im Abschnitt `variables` der Vorlage definierten Variable gespeichert werden.
    
   ```json
   {
       "apiVersion": "2017-12-01",
       "type": "Microsoft.Compute/virtualMachines",
       "name": "[variables('vmName')]",
       "location": "[resourceGroup().location]",
       "identity": {
           "type": "userAssigned",
           "identityIds": [
               "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
           ]
       }
   }
   ```
       

2. (Optional) Fügen Sie als Nächstes unter dem `resources`-Element den folgenden Eintrag hinzu, um Ihrem virtuellen Computer die Erweiterung der verwalteten Identität zuzuweisen (geplante Einstufung als veraltet im Januar 2019). Dieser Schritt ist optional, da Sie für den Tokenabruf auch den Azure IMDS-Identitätsendpunkt (Instance Metadata Service) verwenden können. Verwenden Sie die folgende Syntax:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
3. Wenn Sie fertig sind, sollten die folgenden Abschnitte dem Abschnitt `resource` der Vorlage hinzugefügt worden sein, und diese sollte wie folgt aussehen:
   
   **Microsoft.Compute/virtualMachines-API, Version 2018-06-01**    

   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2018-06-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "userAssignedIdentities": {
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]": {}
                }
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2018-06-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```
   **Microsoft.Compute/virtualMachines-API, Version 2017-12-01**
   
   ```JSON
   "resources": [
        {
            //other resource provider properties...
            "apiVersion": "2017-12-01",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[variables('vmName')]",
            "location": "[resourceGroup().location]",
            "identity": {
                "type": "userAssigned",
                "identityIds": [
                   "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
                ]
            }
        },
        {
            "type": "Microsoft.Compute/virtualMachines/extensions",
            "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForLinux')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
                "publisher": "Microsoft.ManagedIdentity",
                "type": "ManagedIdentityExtensionForWindows",
                "typeHandlerVersion": "1.0",
                "autoUpgradeMinorVersion": true,
                "settings": {
                    "port": 50342
            }
        }
       }
    ]
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-vm"></a>Entfernen einer vom Benutzer zugewiesenen verwalteten Identität von einer Azure-VM

Bei einem virtuellen Computer, für den die vom Benutzer zugewiesene verwaltete Identität nicht mehr benötigt wird, gehen Sie wie folgt vor:

1. Verwenden Sie unabhängig davon, ob Sie sich bei Azure lokal oder über das Azure-Portal anmelden, ein Konto, das dem Azure-Abonnement zugeordnet ist, das den virtuellen Computer enthält.

2. Laden Sie die Vorlage in einen [Editor](#azure-resource-manager-templates), und suchen Sie nach der gewünschten `Microsoft.Compute/virtualMachines`-Ressource im Abschnitt `resources`. Wenn Ihr virtueller Computer nur über eine benutzerseitig zugewiesene verwaltete Identität verfügt, können Sie diese deaktivieren, indem Sie den Identitätstyp in `None` ändern.
 
   Das folgende Beispiel zeigt, wie Sie alle vom Benutzer zugewiesenen verwalteten Identitäten von einer VM ohne vom System zugewiesene verwaltete Identitäten entfernen:
   
   ```json
    {
      "apiVersion": "2018-06-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```
   
   **Microsoft.Compute/virtualMachines-API, Version 2018-06-01**
    
   Um eine einzelne vom Benutzer zugewiesene verwaltete Identität von einem virtuellen Computer zu entfernen, entfernen Sie diese aus dem `useraAssignedIdentities`-Wörterbuch.

   Wenn Sie eine vom System zugewiesene verwaltete Identität verwenden, behalten Sie sie im Wert `type` unter dem Wert `identity` bei.
 
   **Microsoft.Compute/virtualMachines-API, Version 2017-12-01**

   Um eine einzelne benutzerseitig zugewiesene verwaltete Identität von einem virtuellen Computer zu entfernen, entfernen Sie diese aus dem `identityIds`-Array.

   Wenn Sie eine vom System zugewiesene verwaltete Identität verwenden, behalten Sie sie im Wert `type` unter dem Wert `identity` bei.
   
## <a name="next-steps"></a>Nächste Schritte

- [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](overview.md)

