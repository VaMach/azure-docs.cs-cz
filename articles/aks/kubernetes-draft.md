---
title: "Použít koncept s AKS a kontejner Azure registru"
description: "Použít koncept s AKS a kontejner Azure registru"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 312f8d6038718991a563dcf6214aaab4c5f2cd9a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="use-draft-with-azure-container-service-aks"></a>Použít koncept s Azure Container Service (AKS)

Koncept je otevřený nástroj, který pomáhá balíček a spuštění kódu v clusteru s podporou Kubernetes. Koncept je zaměřená na vývoj iterační cyklus; jako kód je vyvíjen, ale před potvrzením do správy verzí. S koncept můžete rychle znovu nasadit aplikace do Kubernetes jsou prováděny změny kódu. Další informace o návrhu najdete v tématu [koncept dokumentaci na Githubu][draft-documentation].

Tento dokument údaje pomocí clusteru Kubernetes AKS koncept.

## <a name="prerequisites"></a>Požadavky

Podrobně popsané kroky v tomto dokumentu předpokládají, že jste vytvořili cluster AKS a navázali s ním připojení přes kubectl. Pokud budete potřebovat tyto položky, najdete v článku [rychlý start AKS][aks-quickstart].

Budete také potřebovat privátní registru Docker v registru kontejner Azure (ACR). Pokyny pro nasazení ACR instance najdete v tématu [Azure kontejneru registru rychlý start] [acr rychlý start].

## <a name="install-helm"></a>Nainstalujte Helm

Rozhraní příkazového řádku Helm je klient, který běží ve vývojovém systému a umožňuje spuštění, zastavení a správu aplikací s Helm grafy.

Chcete-li nainstalovat rozhraní příkazového řádku Helm na Macu, použijte `brew`. Možnosti Další informace najdete v tématu [instalace Helm][install-helm].

```console
brew install kubernetes-helm
```

Výstup:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="install-draft"></a>Nainstalujte konceptu

Rozhraní příkazového řádku koncept je klient, který běží ve vývojovém systému a umožňuje že vám quicky nasazení kódu do clusteru s podporou Kubernetes.

Instalace rozhraní příkazového řádku koncept na použití Mac `brew`. Další informace o instalaci naleznete v části, [koncept nainstalovat Průvodce][install-draft].

```console
brew install draft
```

Výstup:

```
==> Installing draft from azure/draft
==> Downloading https://azuredraft.blob.core.windows.net/draft/draft-v0.7.0-darwin-amd64.tar.gz
Already downloaded: /Users/neilpeterson/Library/Caches/Homebrew/draft-0.7.0.tar.gz
==> /usr/local/Cellar/draft/0.7.0/bin/draft init --client-only
🍺  /usr/local/Cellar/draft/0.7.0: 6 files, 61.2MB, built in 1 second
```

## <a name="configure-draft"></a>Konfigurace konceptu

Při konfiguraci koncept, je třeba zadat kontejner registru. V tomto příkladu se používá Azure kontejneru registru.

Spusťte následující příkaz získat název a název serveru přihlášení vaší instance ACR. Aktualizujte příkaz s názvem skupiny prostředků obsahující instanci ACR.

```console
az acr list --resource-group <resource group> --query "[].{Name:name,LoginServer:loginServer}" --output table
```

Také je potřeba heslo instance ACR.

Spusťte následující příkaz, který vrátí ACR heslo. Aktualizujte příkaz s názvem instance ACR.

```console
az acr credential show --name <acr name> --query "passwords[0].value" --output table
```

Inicializace koncept s `draft init` příkaz.

```console
draft init
```

Během tohoto procesu zobrazí se výzva pro pověření registru kontejneru. Při použití služby Azure kontejneru registru, adresa URL registru je název ACR přihlášení serveru, uživatelské jméno je název instance ACR a heslo je heslo ACR.

```console
1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): <ACR Login Server>
2. Enter your username: <ACR Name>
3. Enter your password: <ACR Password>
```

Po dokončení koncept je v clusteru Kubernetes nakonfigurovaný a připravený k použití.

```
Draft has been installed into your Kubernetes Cluster.
Happy Sailing!
```

## <a name="run-an-application"></a>Spuštění aplikace

Koncept úložiště zahrnuje několik ukázkové aplikace, které lze použít pro ukázkové konceptu. Vytvoření klonovaného kopie úložišti.

```console
git clone https://github.com/Azure/draft
```

Přejděte do adresáře příklady Java.

```console
cd draft/examples/java/
```

Použití `draft create` příkaz ke spuštění procesu. Tento příkaz vytvoří artefakty, které se používají ke spuštění aplikace v clusteru s podporou Kubernetes. Soubor Docker graf Helm, zahrnout tyto položky a `draft.toml` souboru, který je v souboru konfigurace konceptu.

```console
draft create
```

Výstup:

```
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Ke spuštění aplikace v clusteru s podporou Kubernetes, použijte `draft up` příkaz. Tento příkaz odešle do clusteru Kubernetes kódu a konfigurační soubory aplikace. Potom spustí soubor Docker vytvoření bitové kopie kontejneru, nabízených oznámení bitovou kopii do kontejneru registru a nakonec spouští Helm grafu a spusťte aplikaci.

```console
draft up
```

Výstup:

```
Draft Up Started: 'open-jaguar'
open-jaguar: Building Docker Image: SUCCESS ⚓  (28.0342s)
open-jaguar: Pushing Docker Image: SUCCESS ⚓  (7.0647s)
open-jaguar: Releasing Application: SUCCESS ⚓  (4.5056s)
open-jaguar: Build ID: 01BW3VVNZYQ5NQ8V1QSDGNVD0S
```

## <a name="test-the-application"></a>Testování aplikace

K otestování aplikace, použijte `draft connect` příkaz. Tento příkaz proxy připojení k pod Kubernetes povolení zabezpečené místní připojení. Po dokončení aplikace jsou přístupné na zadané adrese URL.

V některých případech může trvat několik minut pro bitovou kopii kontejner ke stažení a spuštění aplikace. Pokud obdržíte chybu při přístupu k aplikaci, pokus o připojení.

```console
draft connect
```

Výstup:

```
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Po dokončení testování aplikace pomocí `Control+C` zastavit připojení k proxy serveru.

## <a name="expose-application"></a>Zveřejnit aplikaci

Při testování aplikace v Kubernetes, můžete chtít zpřístupnění aplikace v síti internet. To lze provést pomocí Kubernetes služby s typem [nástroj pro vyrovnávání zatížení] [ kubernetes-service-loadbalancer] nebo [příjem příchozích dat řadič][kubernetes-ingress]. Tento dokument údaje pomocí Kubernetes služby.


Nejdřív na konceptu pack musí aktualizovat určíte, že služby s typem `LoadBalancer` by měl být vytvořen. Uděláte to tak, aktualizujte typ služby v `values.yaml` souboru.

```console
vi chart/java/values.yaml
```

Vyhledejte `service.type` vlastnost a aktualizujte hodnotu z `ClusterIP` k `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Spustit `draft up` a znovu spusťte aplikaci.

```console
draft up
```

Může trvat několik minut, než službu, kterou chcete vrátit na veřejnou IP adresu. Ke sledování průběhu použijte `kubectl get service` s sledovat.

```console
kubectl get service -w
```

Standardně *externí IP* pro služby se zobrazí jako `pending`.

```
deadly-squid-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

Jakmile externí IP adresu se změnil z hodnoty `pending` k `IP address`, použijte `Control+C` kubectl sledovat proces zastavíte.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Informace o aplikaci, přejděte na externí IP adresu.

```console
curl 52.175.224.118
```

Výstup:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Iterace v aplikaci

Teď, když byl nakonfigurován koncept a aplikace běží v Kubernetes, se nastavují pro iterace kódu. Pokaždé, když chcete otestovat aktualizované kódu, spusťte `draft up` příkaz k aktualizaci běžící aplikaci.

V tomto příkladu aktualizujte aplikaci Java hello world.

```console
vi src/main/java/helloworld/Hello.java
```

Aktualizace textu Hello World.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java - Draft Rocks!");
    }
}
```

Spustit `draft up` příkaz k opětovnému nasazení aplikace.

```console
draft up
```

Výstup

```
Draft Up Started: 'deadly-squid'
deadly-squid: Building Docker Image: SUCCESS ⚓  (18.0813s)
deadly-squid: Pushing Docker Image: SUCCESS ⚓  (7.9394s)
deadly-squid: Releasing Application: SUCCESS ⚓  (6.5005s)
deadly-squid: Build ID: 01BWK8C8X922F5C0HCQ8FT12RR
```

Nakonec zobrazte aplikaci zobrazíte aktualizace.

```console
curl 52.175.224.118
```

Výstup:

```
Hello World, I'm Java - Draft Rocks!
```

## <a name="next-steps"></a>Další kroky

Další informace o používání koncept naleznete v dokumentaci k koncept na Githubu.

> [!div class="nextstepaction"]
> [Koncept dokumentace][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[install-helm]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[kubernetes-ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quicstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md