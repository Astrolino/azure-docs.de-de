
## <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung
Zuallererst muss die aktuelle Version der [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> In den Beispielen in diesem Thema wird das [Azure Resource Manager-Bereitstellungsmodell](../articles/azure-resource-manager/resource-group-overview.md) verwendet. Daher kommen die [Azure Resource Manager-Cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx) zum Einsatz. 
> 
> 

Führen Sie das Cmdlet [**Connect-AzureRmAccount**](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) aus. Daraufhin wird eine Anmeldeseite angezeigt, in der Sie Ihre Anmeldeinformationen eingeben müssen. Verwenden Sie die gleichen Anmeldeinformationen wie für die Anmeldung beim Azure-Portal.

    Connect-AzureRmAccount

Wenn Sie über mehrere Abonnements verfügen, wählen Sie mit dem Cmdlet [**Set-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/set-azurermcontext) das Abonnement aus, das für die PowerShell-Sitzung verwendet werden soll. Führen Sie das Cmdlet [**Get-AzureRmContext**](https://docs.microsoft.com/powershell/module/azurerm.profile/get-azurermcontext) aus, um das Abonnement anzuzeigen, das für die aktuelle PowerShell-Sitzung verwendet wird. Um alle Ihre Abonnements anzuzeigen, führen Sie [**Get-AzureRmSubscription**](https://docs.microsoft.com/powershell/module/servicemanagement/azurerm.profile/get-azurermsubscription) aus.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'

