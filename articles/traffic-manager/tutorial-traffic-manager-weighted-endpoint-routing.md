---
title: 'Tutorial: Weiterleiten von Datenverkehr an gewichtete Endpunkte mit Azure Traffic Manager | Microsoft-Dokumentation'
description: In diesem Tutorialartikel erfahren Sie, wie Sie Datenverkehr mithilfe von Traffic Manager an gewichtete Endpunkte weiterleiten.
services: traffic-manager
author: KumudD
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: kumud
ms.openlocfilehash: 171a9cfe5b5b63efde31c37cd49ef548f1e4ebcc
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649433"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-using-traffic-manager"></a>Tutorial: Steuern des Routings von Datenverkehr mit gewichteten Endpunkten unter Verwendung von Traffic Manager 

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Traffic Manager und der gewichteten Routingmethode das Routing von Benutzerdatenverkehr zwischen Endpunkten steuern. Bei dieser Routingmethode weisen Sie den einzelnen Endpunkten in der Traffic Manager-Profilkonfiguration jeweils eine Gewichtung zu, und der Benutzerdatenverkehr wird auf der Grundlage der zugewiesenen Gewichtung an die einzelnen Endpunkte weitergeleitet. Die Gewichtung ist eine Ganzzahl zwischen 1 und 1000. Je höher der Gewichtungswert, der einem Endpunkt zugewiesen ist, desto höher seine Priorität.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen von zwei VMs, die eine Standardwebsite auf IIS ausführen
> * Erstellen von zwei Test-VMs zum Anzeigen von Traffic Manager in Aktion
> * Konfigurieren des DNS-Namens für die virtuellen Computer, die IIS ausführen
> * Erstellen eines Traffic Manager-Profils
> * Erstellen von VM-Endpunkten für das Traffic Manager-Profil
> * Anzeigen von Traffic Manager in Aktion

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Um den Traffic Manager in Aktion sehen zu können, müssen Sie in diesem Tutorial Folgendes bereitstellen:
- zwei Instanzen von Standardwebsites, die in verschiedenen Azure-Regionen ausgeführt werden – **USA, Osten** und **Europa, Westen**.
- zwei Test-VMs zum Testen des Traffic Managers – ein virtueller Computer in **USA, Osten** und der zweite virtuelle Computer in **Europa, Westen**. Die virtuellen Testcomputer veranschaulichen, wie Traffic Manager Benutzerdatenverkehr an eine Website weiterleitet, deren Endpunkt eine höhere Gewichtung zugewiesen ist.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure 

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

### <a name="create-websites"></a>Erstellen von Websites

In diesem Abschnitt erstellen Sie zwei Websiteinstanzen, die die zwei Dienstendpunkte für das Traffic Manager-Profil in zwei Azure-Regionen bereitstellen. Führen Sie die folgenden Schritte aus, um die beiden Websites zu erstellen:
1. Erstellen Sie zwei VMs für die Ausführung einer Standardwebsite – eine in **USA, Osten** und die andere in **Europa, Westen**.
2. Installieren Sie den IIS-Server auf jedem virtuellen Computer und aktualisieren Sie die Standardwebsiteseite, die den Namen der VM beschreibt, mit der ein Benutzer beim Besuch der Website verbunden ist.

#### <a name="create-vms-for-running-websites"></a>Erstellen von virtuellen Computern für die Ausführung von Websites
In diesem Abschnitt erstellen Sie die beiden virtuellen Computer *myIISVMEastUS* und *myIISVMWEurope* in den Azure-Regionen **USA, Osten** und **Europa, Westen**.

1. Wählen Sie im Azure-Portal links oben **Ressource erstellen** > **Compute** > **Windows Server 2016 VM** aus.
2. Geben Sie die folgenden Informationen für **Grundlagen** ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |NAME|myIISVMEastUS|
    |Benutzername| Geben Sie den gewünschten Benutzernamen ein.|
    |Kennwort| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    |Ressourcengruppe| Klicken Sie auf **Neu**, und geben Sie *myResourceGroupTM1* ein.|
    |Standort| Wählen Sie **USA, Osten** aus.|
    |||
4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie die folgenden Werte unter **Einstellungen** aus, und wählen Sie anschließend **OK**:
    
    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk| Wählen Sie **Virtuelles Netzwerk** in **Virtuelles Netzwerk erstellen** aus, und geben Sie *myVNet1* für **Name** und *mySubnet* für das Subnetz ein.|
    |Netzwerksicherheitsgruppen (NSG)|Wählen Sie **Standard** aus, und wählen Sie in der Dropdownliste**Öffentliche Eingangsports hinzufügen** die Optionen **HTTP** und **RDP** aus. |
    |Startdiagnose|Wählen Sie **Deaktiviert** aus.|
    |||
6. Klicken Sie auf der Seite **Zusammenfassung** unter **Erstellen** auf **Erstellen**, um die Bereitstellung der VM zu starten.

7. Führen Sie die Schritte 1 bis 6 mit den folgenden Änderungen erneut aus:

    |Einstellung|Wert|
    |---|---|
    |Ressourcengruppe | Klicken Sie auf **Neu**, und geben Sie *myResourceGroupTM2* ein.|
    |Standort|Europa, Westen|
    |VM-Name | myIISVMWEurope|
    |Virtuelles Netzwerk | Wählen Sie **Virtuelles Netzwerk** in **Virtuelles Netzwerk erstellen** aus, und geben Sie *myVNet2* für **Name** und *mySubnet* für das Subnetz ein.|
    |||
8. Die Erstellung der VMs kann einige Minuten dauern. Fahren Sie mit den restlichen Schritten erst fort, nachdem beide VMs erstellt wurden.

![Erstellen einer VM](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Installieren von IIS und Anpassen der Standardwebseite

In diesem Abschnitt Installieren Sie den IIS-Server auf den beiden virtuellen Computern – *myIISVMEastUS*  & *myIISVMWEurope* – und aktualisieren dann die Standardwebsiteseite. Die benutzerdefinierte Websiteseite zeigt den Namen des virtuellen Computers an, mit dem Sie eine Verbindung herstellen, wenn Sie die Website über einen Webbrowser besuchen.

1. Wählen Sie im linken Menü **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf *myIISVMEastUS* in der Ressourcengruppe *myResourceGroupTM1*.
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, und wählen Sie dann in **Herstellen einer Verbindung mit dem virtuellen Computer** die Option **RDP-Datei herunterladen** aus. 
3. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. 
4. Klicken Sie auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.
6. Navigieren Sie auf dem Serverdesktop zu **Windows-Verwaltungsprogramme**>**Server-Manager**.
7. Starten Sie Windows PowerShell auf VM1, und verwenden Sie die folgenden Befehle, um den IIS-Server zu installieren und die HTM-Standarddatei zu aktualisieren.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Installieren von IIS und Anpassen der Webseite](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. Schließen Sie die RDP-Verbindung mit *myIISVMEastUS*.
9. Wiederholen Sie die Schritte 1 bis 8 mit dem Herstellen einer RDP-Verbindung mit dem virtuellen Computer *myIISVMWEurope* innerhalb der Ressourcengruppe *myResourceGroupTM2* zum Installieren von IIS und Anpassen seiner Standardwebseite.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Konfigurieren von DNS-Namen für die virtuellen Computer, die IIS ausführen

Basierend auf DNS-Namen der Dienstendpunkte leitet Traffic Manager den Benutzerdatenverkehr weiter. In diesem Abschnitt konfigurieren Sie die DNS-Namen für die IIS-Server – *myIISVMEastUS* und *myIISVMWEurope*.

1. Klicken Sie im linken Menü auf **Alle Ressourcen**, und wählen Sie dann in der Ressourcenliste *myIISVMEastUS* in der Ressourcengruppe *myResourceGroupTM1* aus.
2. Wählen Sie auf der Seite **Übersicht** unter **DNS-Name** die Option **Konfigurieren** aus.
3. Fügen Sie auf der Seite **Konfiguration** als DNS-Namen einen eindeutigen Namen ein, und wählen Sie dann **Speichern**.
4. Wiederholen Sie die Schritte 1 bis 3 für den virtuellen Computer mit dem Namen *myIISVMWEurope*, der sich in der Ressourcengruppe *myResourceGroupTM1* befindet.

### <a name="create-a-test-vm"></a>Erstellen eines virtuellen Testcomputers

In diesem Abschnitt erstellen Sie den virtuellen Computer *mVMEastUS*. Mit diesem virtuellen Computer testen Sie, wie Traffic Manager Datenverkehr an den Websiteendpunkt mit dem höheren Gewichtungswert weiterleitet.

1. Wählen Sie im Azure-Portal links oben **Ressource erstellen** > **Compute** > **Windows Server 2016 VM** aus.
2. Geben Sie die folgenden Informationen für **Grundlagen** ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    |Einstellung|Wert|
    |---|---|
    |NAME|myVMEastUS|
    |Benutzername| Geben Sie den gewünschten Benutzernamen ein.|
    |Kennwort| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    |Ressourcengruppe| Wählen Sie **Vorhandene** und dann *myResourceGroupTM1* aus.|
    |||

4. Wählen Sie unter **Größe auswählen** eine VM-Größe aus.
5. Wählen Sie die folgenden Werte unter **Einstellungen** aus, und wählen Sie anschließend **OK**:
    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk| Wählen Sie **Virtuelles Netzwerk** in **Virtuelles Netzwerk erstellen** aus, und geben Sie *myVNet3* für **Name** und *mySubnet* für das Subnetz ein.|
    |Netzwerksicherheitsgruppen (NSG)|Wählen Sie **Standard** aus, und wählen Sie in der Dropdownliste**Öffentliche Eingangsports hinzufügen** die Optionen **HTTP** und **RDP** aus. |
    |Startdiagnose|Wählen Sie **Deaktiviert** aus.|
    |||

6. Klicken Sie auf der Seite **Zusammenfassung** unter **Erstellen** auf **Erstellen**, um die Bereitstellung der VM zu starten.
8. Die Erstellung des virtuellen Computers dauert einige Minuten. Fahren Sie mit den restlichen Schritten erst fort, nachdem der virtuelle Computer erstellt wurde.

## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils
Erstellen Sie ein Traffic Manager-Profil auf der Grundlage der Routingmethode **Gewichtet**.

1. Klicken Sie links oben auf dem Bildschirm auf **Ressource erstellen** > **Netzwerk** > **Traffic Manager-Profil** > **Erstellen**.
2. Geben Sie unter **Traffic Manager-Profil erstellen** die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **Erstellen**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | NAME                   | Dieser Name muss innerhalb der Zone „trafficmanager.net“ eindeutig sein und ergibt den DNS-Namen „trafficmanager.net“, der für den Zugriff auf Ihr Traffic Manager-Profil verwendet wird.                                   |
    | Routingmethode          | Wählen Sie die Routingmethode **Gewichtet** aus.                                       |
    | Abonnement            | Wählen Sie Ihr Abonnement aus.                          |
    | Ressourcengruppe          | Wählen Sie **Vorhandene** und anschließend *myResourceGroupTM1* aus. |
    |        |   |

    ![Erstellen eines Traffic Manager-Profils](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Hinzufügen von Traffic Manager-Endpunkten

Fügen Sie die beiden virtuellen Computer hinzu, auf denen die IIS-Server ausgeführt werden (*myIISVMEastUS*  &  und *myIISVMWEurope*), um Benutzerdatenverkehr an sie weiterzuleiten.

1. Suchen Sie in der Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, das Sie im vorhergehenden Abschnitt erstellt haben, und wählen Sie in den angezeigten Ergebnissen das Profil aus.
2. Klicken Sie unter **Traffic Manager-Profil** im Abschnitt **Einstellungen** auf **Endpunkte** und dann auf **Hinzufügen**.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **OK**:

    | Einstellung                 | Wert                                              |
    | ---                     | ---                                                |
    | Typ                    | Azure-Endpunkt                                   |
    | NAME           | myEastUSEndpoint                                        |
    | Zielressourcentyp           | Öffentliche IP-Adresse                          |
    | Zielressource          | **Wählen Sie eine öffentliche IP-Adresse aus**, um die Liste der Ressourcen mit öffentlichen IP-Adressen im gleichen Abonnement anzuzeigen. Wählen Sie in **Ressource** die öffentliche IP-Adresse mit dem Namen *myIISVMEastUS-ip* aus. Dies ist die öffentliche IP-Adresse der IIS-Server-VM in „USA, Osten“.|
    |  Weight      | Geben Sie **100** ein.        |
    |        |           |

4. Wiederholen Sie die Schritte 2 und 3, um einen weiteren Endpunkt mit dem Namen *myWestEuropeEndpoint* für die öffentliche IP-Adresse *myIISVMWEurope-ip* hinzuzufügen, die dem virtuellen IIS-Servercomputer mit dem Namen *myIISVMWEurope* zugeordnet ist, und geben Sie unter **Gewichtung** den Wert **25** ein. 
5.  Wenn Sie das Hinzufügen beider Endpunkte abgeschlossen haben, werden diese unter **Traffic Manager-Profil** zusammen mit ihrem Überwachungsstatus als **Online** angezeigt.

## <a name="test-traffic-manager-profile"></a>Testen des Traffic Manager-Profils
Um den Traffic Manager in Aktion anzuzeigen, führen Sie die folgenden Schritte aus:
1. Bestimmen Sie den DNS-Namen des Traffic Manager-Profils.
2. Sehen Sie sich Traffic Manager in Aktion an.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Bestimmen des DNS-Namens des Traffic Manager-Profils
In diesem Tutorial verwenden Sie der Einfachheit halber den DNS-Namen des Traffic Manager-Profils, um die Websites zu besuchen. 

Sie können den DNS-Namen des Traffic Manager-Profils folgendermaßen bestimmen:

1.  Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben. Klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
1. Klicken Sie auf **Overview**.
2. Unter **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt. In Produktionsbereitstellungen konfigurieren Sie mithilfe eines DNS-CNAME-Eintrags einen benutzerdefinierten Domänennamen für den Verweis auf den Traffic Manager-Domänennamen.

   ![Traffic Manager-DNS-Name](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Anzeigen von Traffic Manager in Aktion
In diesem Abschnitt sehen Sie den Traffic Manager in Aktion. 

1. Wählen Sie im linken Menü **Alle Ressourcen** aus, und klicken Sie anschließend in der Ressourcenliste auf *myVMEastUS* in der Ressourcengruppe *myResourceGroupTM1*.
2. Klicken Sie auf der Seite **Übersicht** auf **Verbinden**, und wählen Sie dann in **Herstellen einer Verbindung mit dem virtuellen Computer** die Option **RDP-Datei herunterladen** aus. 
3. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. 
4. Klicken Sie auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren. 
6. Geben Sie in einem Webbrowser auf der VM *myVMEastUS* den DNS-Namen Ihres Traffic Manager-Profils an, um Ihre Website anzuzeigen. Sie werden zu der Website weitergeleitet, die auf dem IIS-Server *myIISVMEastUS* gehostet wird, da ihr im Vergleich zum IIS-Server *myIISVMWEurope* (Endpunktgewichtung: **25**) eine höhere Gewichtung (**100**) zugewiesen ist.

   ![Testen des Traffic Manager-Profils](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Löschen des Traffic Manager-Profils
Löschen Sie die in diesem Tutorial erstellten Ressourcengruppen, wenn Sie sie nicht mehr benötigen. Wählen Sie hierzu die Ressourcengruppe aus (**ResourceGroupTM1** oder **ResourceGroupTM2**), und wählen Sie dann **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Weiterleiten von Datenverkehr an bestimmte Endpunkte auf der Grundlage des geografischen Standorts des Benutzers](traffic-manager-configure-geographic-routing-method.md)


