---
title: "Nakonfigurujte příchozí s clusterem Azure Container Service (AKS)"
description: "Nainstalujte a nakonfigurujte řadič NGINX příjem příchozích dat na clusteru Azure Container Service (AKS)."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/21/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c25a0171bd412050a7c94e9b077436cd1ebe893b
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="https-ingress-on-azure-container-service-aks"></a>Příchozí přenos HTTPS na Azure Container Service (AKS)

Řadič příjem příchozích dat je softwarového produktu, které poskytuje reverzní proxy server, směrování provozu konfigurovatelné a ukončení TLS pro Kubernetes služby. Prostředky Kubernetes příjem příchozích dat se používají ke konfiguraci směrování pro jednotlivé služby Kubernetes a příchozího pravidla. Pomocí řadič příjem příchozích dat a příchozího pravidla, jedna externí adresa slouží ke směrování provozu na více služeb v clusteru s podporou Kubernetes.

Tento dokument vás provede ukázkové nasazení [NGINX příjem příchozích dat řadič] [ nginx-ingress] na clusteru Azure Container Service (AKS). Kromě toho [KUBE LEGO] [ kube-lego] projektu se používá k automatickému generování a konfigurace [umožňuje šifrování] [ lets-encrypt] certifikáty. Nakonec několik aplikací se spouštějí v clusteru AKS, z nichž každý je přístupný prostřednictvím jednu adresu.

## <a name="install-an-ingress-controller"></a>Nainstalovat řadič příjem příchozích dat

K instalaci řadičem NGINX příjem příchozích dat použijte Helm. Viz řadič příjem příchozích dat NGINX [dokumentace] [ nginx-ingress] nasazení podrobné informace. 

```
helm install stable/nginx-ingress
```

Během instalace se vytvoří Azure veřejnou IP adresu pro příjem příchozích dat řadič. Chcete-li získat veřejnou IP adresu, použijte příkaz kubectl get service. Může trvat nějakou dobu IP adresu pro přiřazení ke službě.

```console
$ kubectl get service

NAME                                          TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
kubernetes                                    ClusterIP      10.0.0.1       <none>           443/TCP                      3d
toned-spaniel-nginx-ingress-controller        LoadBalancer   10.0.236.223   52.224.125.195   80:30927/TCP,443:31144/TCP   18m
toned-spaniel-nginx-ingress-default-backend   ClusterIP      10.0.5.86      <none>           80/TCP                       18m
```

Protože žádná pravidla příjem příchozích dat byly vytvořeny, pokud přejdete na veřejnou IP adresu, jsou směrovány na NGINX příjem příchozích dat řadiče výchozí 404 stránku.

![Výchozí NGINX back-end](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Konfigurace názvu DNS

Protože se používají certifikáty protokolu HTTPS, musíte nakonfigurovat název plně kvalifikovaný název domény pro příjem příchozích dat řadiče IP adresu. V tomto příkladu se vytvoří plně kvalifikovaný název domény Azure pomocí Azure CLI. Aktualizujte skript s IP adresou řadiče pro příjem příchozích dat a název, který chcete používat ve plně kvalifikovaný název domény.

```
#!/bin/bash

# Public IP address
IP="52.224.125.195"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get resource group and public ip name
RESOURCEGROUP=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[resourceGroup]" --output tsv)
PIPNAME=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[name]" --output tsv)

# Update public ip address with dns name
az network public-ip update --resource-group $RESOURCEGROUP --name  $PIPNAME --dns-name $DNSNAME
```

V případě potřeby spusťte následující příkaz k načtení plně kvalifikovaný název domény. Aktualizujte hodnotu IP adresu s třídou řadiči příjem příchozích dat.

```azurecli
az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '52.224.125.195')].[dnsSettings.fqdn]" --output tsv
```

Řadičem příjem příchozích dat je nyní přístupné prostřednictvím plně kvalifikovaný název domény.

## <a name="install-kube-lego"></a>Nainstalujte KUBE LEGO

Příjem příchozích dat řadičem NGINX podporuje TLS ukončení. Existuje několik způsobů, jak načíst a nakonfigurovat certifikáty pro protokol HTTPS, tento dokument ukazuje, jak pomocí [KUBE LEGO][kube-lego], který poskytuje automatické [umožňuje šifrování] [ lets-encrypt] funkce generování a správu certifikátů.

Instalace řadiče KUBE LEGO, následujícím příkazem Helm instalace. Aktualizujte e-mailovou adresu s jedním z vaší organizace.

```
helm install stable/kube-lego \
  --set config.LEGO_EMAIL=user@contoso.com \
  --set config.LEGO_URL=https://acme-v01.api.letsencrypt.org/directory
```

Další informace o konfiguraci KUBE LEGO, najdete v článku [KUBE LEGO dokumentaci][kube-lego].

## <a name="run-application"></a>Spuštění aplikace

V tomto okamžiku byly nakonfigurovány řadič příjem příchozích dat a řešení pro správu certifikát. Nyní můžete spusťte několik aplikací v clusteru AKS.

V tomto příkladu Helm slouží ke spouštění více instancí aplikace jednoduché hello world.

Než spustíte aplikaci, přidejte úložiště Helm ukázek Azure ve vývojovém systému.

```
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

 Spusťte grafu AKS hello world pomocí následujícího příkazu:

```
helm install azure-samples/aks-helloworld
```

Nyní instalaci druhé instance aplikace hello world.

Pro druhou instanci zadejte nový název tak, aby dvě možná využití jsou vizuálně jedinečné. Budete taky muset zadat jedinečný název služby. Tyto konfigurace můžete vidět v následujícím příkazu.

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Vytvoření příchozího směrování

Obě aplikace je nyní spuštěna v clusteru Kubernetes, ale jsou nakonfigurované s službu typu `ClusterIP`. Aplikace jako takový nejsou přístupné z Internetu. Aby bylo možné k dispozici, vytvořte prostředek příjem příchozích dat Kubernetes. Vstupní prostředek nakonfiguruje pravidla, která směrovat provoz mezi těmito dvěma aplikacemi.

Vytvořte název souboru `hello-world-ingress.yaml` a zkopírujte následující YAML.

Všimněte si, že provoz na adresu `https://demo-aks-ingress.eastus.cloudapp.azure.com/` se směruje na služby s názvem `aks-helloworld`. Provoz na adresu `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` se směruje na `ingress-demo` služby.

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/tls-acme: "true"
    ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
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

Vytvoření vstupní zdroj s `kubectl apply` příkaz.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Otestujte konfiguraci příjem příchozích dat

Přejděte do plně kvalifikovaný název domény řadiči Kubernetes příjem příchozích dat, měli byste vidět aplikace hello world.

![Aplikace – příklad jednoho](media/ingress/app-one.png)

Nyní přejděte na plně kvalifikovaný název domény řadiče příjem příchozích dat s `/hello-world-two` cestu, měli byste vidět aplikace hello world pomocí vlastní název.

![Aplikace – příklad dvou](media/ingress/app-two.png)

Všimněte si také, že připojení je šifrovaná a že se používá certifikát vydaný umožňuje šifrování.

![Umožňuje šifrování certifikátu](media/ingress/certificate.png)

## <a name="next-steps"></a>Další postup

Další informace o softwaru ukázáno v tomto dokumentu. 

- [Řadič NGINX příjem příchozích dat][nginx-ingress]
- [KUBE-LEGO][kube-lego]

<!-- LINKS - external -->
[kube-lego]: https://github.com/jetstack/kube-lego
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
