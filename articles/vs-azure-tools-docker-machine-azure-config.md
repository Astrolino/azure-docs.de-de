---
title: Erstellen von Docker-Hosts in Azure mit Docker Machine | Microsoft Docs
description: Beschreibt die Verwendung von Docker Machine zum Erstellen von Docker-Hosts in Azure.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: ''
ms.assetid: 7a3ff6e1-fa93-4a62-b524-ab182d2fea08
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 766d327a87ed13e04166d71c3d9ae0a1e7a66d19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
ms.locfileid: "23124498"
---
# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Erstellen von Docker-Hosts in Azure mit dem Befehl „docker-machine“
Das Ausführen von [Docker](https://www.docker.com/) -Containern erfordert eine Host-VM, in der der Docker-Daemon ausgeführt wird.
In diesem Thema wird beschrieben, wie Sie den Befehl [docker-machine](https://docs.docker.com/machine/) zum Erstellen neuer Linux-VMs verwenden, die mit dem Docker-Daemon konfiguriert sind und in Azure ausgeführt werden. 

**Hinweis:** 

* *Dieser Artikel setzt mindestens die Version 0.9.0-rc2 von docker-machine voraus.*
* *In naher Zukunft werden auch Windows-Container von docker-machine unterstützt.*

## <a name="create-vms-with-docker-machine"></a>Erstellen von virtuellen Computern mit dem Befehl „docker- machine“
Erstellen Sie Docker-Host-VMs in Azure mit dem Befehl `docker-machine create` unter Verwendung des Treibers `azure`. 

Für den Azure-Treiber wird Ihre Abonnement-ID benötigt. Sie können die [Azure-Befehlszeilenschnittstelle](cli-install-nodejs.md) oder das [Azure-Portal](https://portal.azure.com) verwenden, um Ihr Azure-Abonnement abzurufen. 

**Verwenden des Azure-Portals**

* Wählen Sie auf der linken Seite **Abonnements** aus, und kopieren Sie die Abonnement-ID.

**Verwenden der Azure-Befehlszeilenschnittstelle**

* Geben Sie ```azure account list``` ein, und kopieren Sie die Abonnement-ID.

Geben Sie `docker-machine create --driver azure` ein, um die Optionen und ihre Standardwerte anzuzeigen.
In der [Dokumentation für den Azure-Treiber für Docker](https://docs.docker.com/machine/drivers/azure/) finden Sie weitere Informationen. 

Im folgenden Beispiel werden die [Standardwerte](https://github.com/docker/machine/blob/master/drivers/azure/azure.go#L22) verwendet, es werden jedoch optional die folgenden Werte festgelegt: 

* azure-dns für den Namen, der der öffentlichen IP-Adresse und den generierten Zertifikaten zugeordnet wird. Dies ist der DNS-Name Ihres virtuellen Computers. Es ist dann sicher, den virtuellen Computer zu beenden, die dynamische IP-Adresse freizugeben und eine erneute Verbindung zu erlauben, nachdem der virtuelle Computer mit einer neuen IP-Adresse neu gestartet wurde. Das Namenspräfix muss für die Region „UNIQUE_DNSNAME_PREFIX.westus.cloudapp.azure.com“ eindeutig sein.
* Öffnen von Port 80 auf dem virtuellen Computer für ausgehenden Internetzugriff
* Größe des virtuellen Computers für schnelleres Storage Premium
* Storage Premium wird für den VM-Datenträger verwendet

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-dns <Your UNIQUE_DNSNAME_PREFIX> --azure-open-port 80 --azure-size Standard_DS1_v2 --azure-storage-type "Premium_LRS" mydockerhost 
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Wählen eines Docker-Hosts mit docker-maschine
Sobald Sie in docker-maschine über einen Eintrag für Ihren Host verfügen, können Sie beim Ausführen von Docker-Befehlen den Standardhost festlegen.

## <a name="using-powershell"></a>Verwenden von PowerShell
```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

## <a name="using-bash"></a>Verwenden von Bash
```bash
eval $(docker-machine env MyDockerHost)
```

Sie können jetzt Docker-Befehle auf den angegebenen Host anwenden.

```
docker ps
docker info
```

## <a name="run-a-container"></a>Ausführen eines Containers
Bei konfiguriertem Host können Sie nun einen einfachen Webserver ausführen, um zu testen, ob der Host ordnungsgemäß konfiguriert ist.
Hier verwenden wir ein standardmäßiges nginx-Image, geben an, dass es an Port 80 lauschen soll, und dass beim Neustart der Host-VM der Container ebenfalls neu starten soll (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Die Ausgabe sollte etwa folgendermaßen aussehen:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testen des Containers
Untersuchen Sie ausgeführte Container mit `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Zum Anzeigen des ausgeführten Containers geben Sie außerdem `docker-machine ip <VM name>` ein, um die IP-Adresse zu suchen, die in den Browser eingegeben werden soll:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Ausführen des ngnix-Containers](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

## <a name="summary"></a>Zusammenfassung
Mit „docker-machine“ können Sie Docker-Hosts mühelos für einzelne Überprüfungen von Docker-Hosts bereitstellen.
Informationen zum Hosten von Containern in der Produktion finden Sie unter [Azure-Containerdienst](http://aka.ms/AzureContainerService)

Informationen zum Entwickeln von .NET Core-Anwendungen mit Visual Studio finden Sie unter [Docker-Tools für Visual Studio](http://aka.ms/DockerToolsForVS)

