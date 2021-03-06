---
title: Zugreifen per SSH auf Azure Kubernetes Service-Clusterknoten (AKS)
description: Erfahren Sie, wie Sie eine SSH-Verbindung mit Azure Kubernetes Service-Clusterknoten (AKS) zur Problembehandlung und für Wartungsaufgaben erstellen.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/21/2018
ms.author: iainfou
ms.openlocfilehash: f0097c2ba42378c66bedd614032b63c23a2483dd
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447277"
---
# <a name="connect-with-ssh-to-azure-kubernetes-service-aks-cluster-nodes-for-maintenance-or-troubleshooting"></a>Herstellen einer SSH-Verbindung mit Azure Kubernetes Service-Clusterknoten (AKS) zur Wartung oder Problembehandlung

Während des Lebenszyklus des Azure Kubernetes Service-Clusters (AKS) müssen Sie möglicherweise auf einen AKS-Knoten zugreifen. Dieser Zugriff kann zur Wartung, Protokollsammlung oder für andere Vorgänge der Problembehandlung erforderlich sein. Die AKS-Knoten sind virtuelle Linux-Computer, sodass über SSH darauf zugegriffen werden kann. Aus Sicherheitsgründen werden die AKS-Knoten nicht im Internet verfügbar gemacht.

In diesem Artikel wird gezeigt, wie Sie eine SSH-Verbindung mit einem AKS-Knoten über die privaten IP-Adressen erstellen.

## <a name="add-your-public-ssh-key"></a>Hinzufügen Ihres öffentlichen SSH-Schlüssels

Standardmäßig werden SSH-Schlüssel beim Erstellen eines AKS-Clusters generiert. Wenn Sie beim Erstellen Ihres AKS-Clusters keine eigenen SSH-Schlüssel angegeben haben, fügen Sie den AKS-Knoten Ihre öffentlichen SSH-Schlüssel hinzu. 

Führen Sie die folgenden Schritte aus, um einem AKS-Knoten Ihren SSH-Schlüssel hinzuzufügen:

1. Rufen Sie den Ressourcengruppennamen für Ihre AKS-Clusterressourcen mit [az aks show][az-aks-show] ab. Geben Sie Ihre eigene Hauptressourcengruppe und den Namen Ihres AKS-Clusters an:

    ```azurecli
    az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv
    ```

1. Listen Sie die virtuellen Computer in der AKS-Clusterressourcengruppe mit dem Befehl [az vm list][az-vm-list] auf. Diese virtuellen Computer sind Ihre AKS-Knoten:

    ```azurecli
    az vm list --resource-group MC_myResourceGroup_myAKSCluster_eastus -o table
    ```

    Die folgende Beispielausgabe zeigt die AKS-Knoten:

    ```
    Name                      ResourceGroup                                  Location
    ------------------------  ---------------------------------------------  ----------
    aks-nodepool1-79590246-0  MC_myResourceGroupAKS_myAKSClusterRBAC_eastus  eastus
    ```

1. Zum Hinzufügen Ihrer SSH-Schlüssel zum Knoten verwenden Sie den Befehl [az vm user update][az-vm-user-update]. Geben Sie den Ressourcengruppennamen und dann einen der AKS-Knoten an, die Sie im vorherigen Schritt abgerufen haben. Der Benutzername für die AKS-Knoten lautet standardmäßig *azureuser*. Geben Sie den Speicherort Ihres eigenen öffentlichen SSH-Schlüssels an, z.B. *~/.ssh/id_rsa.pub*, oder fügen Sie den Inhalt Ihres öffentlichen SSH-Schlüssels ein:

    ```azurecli
    az vm user update \
      --resource-group MC_myResourceGroup_myAKSCluster_eastus \
      --name aks-nodepool1-79590246-0 \
      --username azureuser \
      --ssh-key-value ~/.ssh/id_rsa.pub
    ```

## <a name="get-the-aks-node-address"></a>Abrufen der AKS-Knotenadresse

Die AKS-Knoten werden nicht im Internet öffentlich verfügbar gemacht. Zum Herstellen einer SSH-Verbindung mit den AKS-Knoten verwenden Sie die private IP-Adresse.

Zeigen Sie die private IP-Adresse eines AKS-Clusterknotens mit dem Befehl [az vm list-ip-addresses][az-vm-list-ip-addresses] an. Geben Sie den Namen Ihrer eigenen AKS-Clusterressourcengruppe an, den Sie in einem vorherigen Schritt mit [az-aks-show][az-aks-show] abgerufen haben:

```azurecli
az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table
```

Die folgende Beispielausgabe zeigt die privaten IP-Adressen der AKS-Knoten:

```
VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-79590246-0  10.240.0.4
```

## <a name="create-the-ssh-connection"></a>Erstellen der SSH-Verbindung

Zum Erstellen einer SSH-Verbindung mit einem AKS-Knoten führen Sie einen Hilfspod in Ihrem AKS-Cluster aus. Dieser Hilfspod bietet SSH-Zugriff auf den Cluster und dann zusätzlichen SSH-Knotenzugriff. Zum Erstellen und Verwenden dieses Hilfspods führen Sie die folgenden Schritte aus:

1. Führen Sie ein `debian`-Containerimage aus, und fügen Sie eine Terminalsitzung daran an. Dieser Container kann zum Erstellen einer SSH-Sitzung mit einem beliebigen Knoten im AKS-Cluster verwendet werden:

    ```console
    kubectl run -it --rm aks-ssh --image=debian
    ```

1. Das Debian-Basisimage enthält keine SSH-Komponenten. Sobald die Terminalsitzung mit dem Container verbunden ist, installieren Sie einen SSH-Client mit `apt-get` wie folgt:

    ```console
    apt-get update && apt-get install openssh-client -y
    ```

1. Listen Sie in einem neuen Terminalfenster, das nicht mit Ihrem Container verbunden ist, die Pods in Ihrem AKS-Cluster mit dem Befehl [kubectl get pods][kubectl-get] auf. Der im vorherigen Schritt erstellte Pod beginnt mit dem Namen *aks-ssh*, wie es im folgenden Beispiel gezeigt wird:

    ```
    $ kubectl get pods
    
    NAME                       READY     STATUS    RESTARTS   AGE
    aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
    ```

1. Im ersten Schritt dieses Artikels haben Sie dem AKS-Knoten Ihren öffentlichen SSH-Schlüssel hinzugefügt. Nun kopieren Sie Ihren privaten SSH-Schlüssel in den Pod. Dieser private Schlüssel wird verwendet, um die SSH-Verbindung mit den AKS-Knoten zu erstellen.

    Geben Sie Ihren eigenen *aks-ssh*-Podnamen an, den Sie im vorherigen Schritt erhalten haben. Ändern Sie bei Bedarf *~/.ssh/id_rsa* in den Speicherort Ihres privaten SSH-Schlüssels:

    ```console
    kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
    ```

1. Wenn Sie sich wieder in der Terminalsitzung für Ihren Container befinden, aktualisieren Sie die Berechtigungen für den kopierten privaten SSH-Schlüssel `id_rsa`, sodass er für Benutzer schreibgeschützt ist:

    ```console
    chmod 0600 id_rsa
    ```

1. Erstellen Sie jetzt eine SSH-Verbindung mit Ihrem AKS-Knoten. Auch hier lautet der Standardbenutzername für AKS-Knoten *azureuser*. Bestätigen Sie die Aufforderung zum Fortsetzen der Verbindung, da zuerst die Vertrauenswürdigkeit des SSH-Schlüssels überprüft wird. Anschließend wird die Bash-Eingabeaufforderung Ihres AKS-Knotens angezeigt:

    ```console
    $ ssh -i id_rsa azureuser@10.240.0.4
    
    ECDSA key fingerprint is SHA256:A6rnRkfpG21TaZ8XmQCCgdi9G/MYIMc+gFAuY9RUY70.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '10.240.0.4' (ECDSA) to the list of known hosts.
    
    Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-1018-azure x86_64)
    
     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage
    
      Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud
    
    [...]
    
    azureuser@aks-nodepool1-79590246-0:~$
    ```

## <a name="remove-ssh-access"></a>Entfernen des SSH-Zugriffs

Wenn Sie fertig sind, führen Sie `exit` für die SSH-Sitzung und dann `exit` für die interaktive Containersitzung aus. Beim Schließen dieser Containersitzung wird der Pod, der für den SSH-Zugriff vom AKS-Cluster verwendet wurde, gelöscht.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie zusätzliche Problembehandlungsdaten benötigen, können Sie [die Kubelet-Protokolle anzeigen][view-kubelet-logs] oder [die Kubernetes-Masterknotenprotokolle anzeigen][view-master-logs].

<!-- EXTERNAL LINKS -->
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get

<!-- INTERNAL LINKS -->
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-vm-list]: /cli/azure/vm#az-vm-list
[az-vm-user-update]: /cli/azure/vm/user#az-vm-user-update
[az-vm-list-ip-addresses]: /cli/azure/vm#az-vm-list-ip-addresses
[view-kubelet-logs]: kubelet-logs.md
[view-master-logs]: view-master-logs.md
