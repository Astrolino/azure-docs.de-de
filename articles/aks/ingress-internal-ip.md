---
title: Erstellen eines Eingangscontrollers für ein internes Netzwerk in Azure Kubernetes Service (AKS)
description: Erfahren Sie, wie Sie einen NGINX-Eingangscontroller für ein internes privates Netzwerk in einem Azure Kubernetes Service-Cluster (AKS) installieren und konfigurieren.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: 76ad9d21f7b328e7f201d227cdd9ace51c62a3fd
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356038"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Erstellen eines Eingangscontrollers für ein internes virtuelles Netzwerk in Azure Kubernetes Service (AKS)

Ein Eingangscontroller ist eine Softwarekomponente, die einen Reverseproxy, konfigurierbare Datenverkehrsweiterleitung und TLS-Terminierung für Kubernetes-Dienste bereitstellt. Mithilfe von Ressourcen für eingehende Kubernetes-Daten werden Eingangsregeln und Routen für einzelne Kubernetes-Dienste konfiguriert. Durch die Verwendung von einem Eingangscontroller und Eingangsregeln kann eine einzelne IP-Adresse zum Weiterleiten von Datenverkehr an mehrere Dienste in einem Kubernetes-Cluster verwendet werden.

Dieser Artikel beschreibt, wie Sie den [NGINX-Eingangscontroller][nginx-ingress] in einem Azure Kubernetes Service-Cluster (AKS) bereitstellen. Der Eingangscontroller wurde für ein internes privates virtuelles Netzwerk und eine IP-Adresse konfiguriert. Externer Zugriff ist nicht zulässig. Es werden zwei Anwendungen im AKS-Cluster ausgeführt, die jeweils über eine einzelne IP-Adresse zugänglich sind.

Weitere Funktionen:

- [Erstellen eines einfachen Eingangscontrollers mit Verbindung mit einem externen Netzwerk][aks-ingress-basic]
- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
- [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Artikel wird Helm verwendet, um den NGINX-Eingangscontroller, cert-manager und eine Beispiel-Web-App zu installieren. Helm muss im AKS-Cluster initialisiert sein und ein Dienstkonto für Tiller verwenden. Weitere Informationen zum Konfigurieren und Verwenden von Helm finden Sie unter [Installieren von Anwendungen mit Helm in Azure Kubernetes Service (AKS)][use-helm].

Für den Artikel müssen Sie mindestens Version 2.0.41 der Azure-Befehlszeilenschnittstelle (Azure CLI) ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0][azure-cli-install] Informationen dazu.

## <a name="create-an-ingress-controller"></a>Erstellen eines Eingangscontrollers

Standardmäßig wird ein NGINX-Eingangscontroller mit Zuweisung einer dynamischen öffentlichen IP-Adresse erstellt. Eine verbreitete Konfigurationsanforderung stellt die Verwendung eines internen privaten Netzwerks und einer IP-Adresse dar. Dieser Ansatz ermöglicht die Beschränkung des Zugriffs auf Ihre Dienste auf interne Benutzer, sodass kein externer Zugriff möglich ist.

Erstellen Sie eine Datei mit dem Namen *internal-ingress.yaml* anhand der folgenden Beispielmanifestdatei. In diesem Beispiel wird *10.240.0.42* der Ressource *loadBalancerIP* zugewiesen. Geben Sie eine eigene interne IP-Adresse für die Verwendung für den Eingangscontroller an. Stellen Sie sicher, dass diese IP-Adresse nicht bereits in Ihrem virtuellen Netzwerk verwendet wird.

```yaml
controller:
  service:
    loadBalancerIP: 10.240.0.42
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

Nun stellen Sie das *nginx-ingress*-Diagramm mit Helm bereit. Um die im vorherigen Schritt erstellte Manifestdatei verwenden zu können, fügen Sie den Parameter `-f internal-ingress.yaml` hinzu:

> [!TIP]
> Im folgenden Beispiel wird der Eingangscontroller im `kube-system`-Namespace installiert. Sie können bei Bedarf einen anderen Namespace für Ihre eigene Umgebung angeben. Wenn in Ihrem AKS-Cluster RBAC nicht aktiviert ist, fügen Sie den Befehlen `--set rbac.create=false` hinzu.

```console
helm install stable/nginx-ingress --namespace kube-system -f internal-ingress.yaml
```

Wird der Kubernetes-Lastenausgleichsdienst für den NGINX-Eingangscontroller erstellt, wird Ihre interne IP-Adresse zugewiesen, wie in der folgenden Beispielausgabe gezeigt:

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                              TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
alternating-coral-nginx-ingress-controller        LoadBalancer   10.0.97.109   10.240.0.42   80:31507/TCP,443:30707/TCP   1m
alternating-coral-nginx-ingress-default-backend   ClusterIP      10.0.134.66   <none>        80/TCP                       1m
```

Es wurden noch keine Eingangsregeln erstellt, sodass die Standard-404-Seite des NGINX-Eingangscontrollers angezeigt wird, wenn Sie zur internen IP-Adresse navigieren. Eingangsregeln werden in den folgenden Schritten konfiguriert.

## <a name="run-demo-applications"></a>Ausführen von Demoanwendungen

Um den Eingangscontroller in Aktion zu sehen, führen wir zwei Demoanwendungen im AKS-Cluster aus. In diesem Beispiel wird Helm verwendet, um mehrere Instanzen einer einfachen Hallo-Welt-Anwendung auszuführen.

Bevor Sie Helm-Beispieldiagramme installieren können, fügen Sie das Azure-Beispielrepository wie folgt Ihrer Helm-Umgebung hinzu:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Erstellen Sie die erste Demoanwendung aus einem Helm-Diagramm mit dem folgenden Befehl:

```console
helm install azure-samples/aks-helloworld
```

Installieren Sie nun eine zweite Instanz der Demoanwendung. Geben Sie für die zweite Instanz einen neuen Titel an, sodass sich die beiden Anwendungen visuell unterscheiden. Sie geben auch einen eindeutigen Dienstnamen an:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Erstellen einer Eingangsroute

Beide Anwendungen werden jetzt in Ihrem Kubernetes-Cluster ausgeführt. Zum Weiterleiten von Datenverkehr an die einzelnen Anwendungen erstellen Sie eine Kubernetes-Eingangsressource. Die Eingangsressource konfiguriert Regeln, die den Datenverkehr an eine der beiden Anwendungen weiterleiten.

Im folgenden Beispiel wird der Datenverkehr an die Adresse `http://10.240.0.42/` an den Dienst mit dem Namen `aks-helloworld` weitergeleitet. Datenverkehr an die Adresse `http://10.240.0.42/hello-world-two` wird an den Dienst `ingress-demo` weitergeleitet.

Erstellen Sie eine Datei mit dem Namen `hello-world-ingress.yaml`, und fügen Sie den folgenden YAML-Beispielcode ein.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Erstellen Sie die Eingangsressource mit dem Befehl `kubectl apply -f hello-world-ingress.yaml`.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Testen des Eingangscontrollers

Navigieren Sie mit einem Webclient zu den beiden Anwendungen, um die Routen für den Eingangscontroller zu testen. Bei Bedarf können Sie diese ausschließlich interne Funktion schnell von einem Pod im AKS-Cluster aus testen. Erstellen Sie einen Testpod, und fügen Sie an diesen eine Terminalsitzung an:

```console
kubectl run -it --rm aks-ingress-test --image=debian
```

Installieren Sie `curl` mit `apt-get` im Pod:

```console
apt-get update && apt-get install -y curl
```

Greifen Sie nun mit `curl` auf die Adresse des Kubernetes-Eingangscontrollers zu, z.B. *http://10.240.0.42*. Geben Sie Ihre eigene interne IP-Adresse, die Sie beim Bereitstellen des Eingangscontrollers im ersten Schritt dieses Artikels verwendet haben, an.

```console
curl -L http://10.240.0.42
```

Die Adresse wurde ohne zusätzlichen Pfad angegeben, sodass der Eingangscontroller standardmäßig die Route */* verwendet. Die erste Demoanwendung wird zurückgegeben, wie in der folgenden verkürzten Beispielausgabe gezeigt:

```
$ curl -L 10.240.0.42

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Fügen Sie nun den Pfad */hello-world-two* der Adresse hinzu, z.B. *http://10.240.0.42/hello-world-two*. Die zweite Demoanwendung wird mit dem benutzerdefinierten Titel zurückgegeben, wie in der folgenden verkürzten Beispielausgabe gezeigt:

```
$ curl -L -k http://10.240.0.42/hello-world-two

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Artikel werden einige externe Komponenten in AKS berücksichtigt. Weitere Informationen zu diesen Komponenten finden Sie auf den folgenden Projektseiten:

- [Helm CLI][helm-cli]
- [NGINX-Eingangscontroller][nginx-ingress]

Weitere Funktionen:

- [Erstellen eines einfachen Eingangscontrollers mit Verbindung mit einem externen Netzwerk][aks-ingress-basic]
- [Aktivieren des Add-Ons für das HTTP-Anwendungsrouting][aks-http-app-routing]
- [Erstellen eines Eingangscontrollers mit einer dynamischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-tls]
- [Erstellen eines Eingangscontrollers mit einer statischen öffentlichen IP-Adresse und Konfigurieren von Let's Encrypt für das automatische Generieren von TLS-Zertifikaten][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
