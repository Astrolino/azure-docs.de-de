---
title: Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
description: Informationen zum Erstellen und Verwenden einer statischen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 8aab091ed992a946cd78ecf4f0c8fdfff4185a08
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47407551"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Verwenden einer statischen öffentlichen IP-Adresse mit dem Lastenausgleich von Azure Kubernetes Service (AKS)

Standardmäßig ist die öffentliche IP-Adresse, die einer Lastenausgleichsressource zugeordnet ist, die von einem AKS-Cluster erstellt wurde, nur gültig solange die Ressource existiert. Wenn Sie den Kubernetes-Dienst löschen, werden auch der zugehörige Lastenausgleich und die zugehörige IP-Adresse gelöscht. Wenn Sie eine bestimmte IP-Adresse für einen Kubernetes-Dienst zuweisen oder beibehalten möchten, können Sie eine statische öffentliche IP-Adresse erstellen und verwenden.

In diesem Artikel wird erläutert, wie Sie eine statische öffentliche IP-Adresse erstellen und Ihrem Kubernetes-Dienst zuweisen.

## <a name="before-you-begin"></a>Voraussetzungen

Es wird vorausgesetzt, dass Sie über ein AKS-Cluster verfügen. Wenn Sie noch einen AKS-Cluster benötigen, erhalten Sie weitere Informationen im AKS-Schnellstart. Verwenden Sie dafür entweder die [Azure CLI][aks-quickstart-cli] oder das [Azure-Portal][aks-quickstart-portal].

Außerdem muss mindestens Version 2.0.46 oder höher der Azure CLI installiert und konfiguriert sein. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][install-azure-cli] Informationen dazu.

## <a name="create-a-static-ip-address"></a>Erstellen einer statischen IP-Adresse

Wenn Sie eine statische öffentliche IP-Adresse zur Verwendung mit AKS erstellen, muss die Ressource der IP-Adresse im **Knoten** „Ressourcengruppe“ erstellt werden. Rufen Sie den Namen der Ressourcengruppe mit dem Befehl [az aks show][az-aks-show] ab, und fügen Sie den Abfrageparameter `--query nodeResourceGroup` hinzu. Im folgenden Beispiel wird der Knoten „Ressourcengruppe“ für den AKS-Clusternamen *myAKSCluster* in der Ressourcengruppe *myResourceGroup* abgerufen:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Erstellen Sie dann über den Befehl [az network public-ip create][az-network-public-ip-create] eine statische öffentliche IP-Adresse. Geben Sie den Knoten „Ressourcengruppe“ an, den Sie über den vorherigen Befehl erhalten haben, und fügen Sie anschließend einen Namen für die Ressource der IP-Adresse hinzu, z.B. *myAKSPublicIP*:

```azurecli
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

Die IP-Adresse wird ähnlich wie in der folgenden gekürzten Beispielausgabe angezeigt:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
````

Anschließend können Sie die öffentliche IP-Adresse über den Befehl [az network public-ip list][az-network-public-ip-list] abrufen. Geben Sie den Namen des Knotens „Ressourcengruppe“ an, und fragen Sie anschließend wie im folgenden Beispiel gezeigt die *IP-Adresse* ab:

```azurecli
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Erstellen eines Diensts mithilfe der statischen IP-Adresse

Fügen Sie die `loadBalancerIP`-Eigenschaft und den Wert der öffentlichen statischen IP-Adresse dem YAML-Manifest hinzu, um mithilfe der statischen öffentlichen IP-Adresse einen Dienst zu erstellen. Erstellen Sie eine Datei namens „`load-balancer-service.yaml`“, und fügen Sie den folgenden YAML-Code ein. Geben Sie Ihre eigene öffentliche IP-Adresse an, die Sie im vorherigen Schritt erstellt haben:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Erstellen Sie den Dienst und die Bereitstellung mit dem Befehl `kubectl apply`.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="troubleshoot"></a>Problembehandlung

Wenn es die in der *loadBalancerIP*-Eigenschaft des Dienstmanifests für Kubernetes angegebene statische IP-Adresse nicht gibt oder nicht in dem Knoten „Ressourcengruppe“ erstellt wurde, kann der Lastenausgleichsdienst nicht erstellt werden. Prüfen Sie die Ereignisse, die zur Erstellung des Diensts geführt haben, über den Befehl [kubectl describe][kubectl-describe], um das Problem zu lösen. Geben Sie den Namen des Diensts wie folgt genauso wie in dem YAML-Manifest an:

```console
kubectl describe service azure-load-balancer
```

Es werden Informationen zur Dienstressource für Kubernetes angezeigt. Die *Ereignisse*, die am Ende der folgenden Beispielausgabe angezeigt werden, deuten darauf hin, dass die vom *Benutzer angegebene IP-Adresse nicht gefunden wurde*. Überprüfen Sie dann, ob Sie tatsächlich eine statische öffentliche IP-Adresse im Knoten „Ressourcengruppe“ erstellt haben und ob die im Dienstmanifest für Kubernetes angegebene IP-Adresse stimmt.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie mehr Kontrolle über den Netzwerkdatenverkehr wünschen, der an Ihre Anwendungen gesendet wird, sollten Sie stattdessen einen [Eingangscontroller erstellen][aks-ingress-basic]. Außerdem können Sie einen [Eingangscontroller mit einer statischen öffentlichen IP-Adresse erstellen][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
