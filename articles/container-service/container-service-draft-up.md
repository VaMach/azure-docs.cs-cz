---
title: "Použití nástroje Draft se službami Azure Container Service a Azure Container Registry | Dokumentace Microsoftu"
description: "Vytvořte cluster ACS Kubernetes a službu Azure Container Registry, abyste mohli vytvořit první aplikaci v Azure pomocí nástroje Draft."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: Docker, Containers, microservices, Kubernetes, Draft, Azure
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: dc3ae52b1ec6717c7e19a160e3e7ea5d211f1f5f
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017



---

<a id="use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes" class="xliff"></a>

# Použití nástroje Draft se službami Azure Container Service a Azure Container Registry k sestavení a nasazení aplikace do Kubernetes

[Draft](https://aka.ms/draft) je nový opensourcový nástroj usnadňující vývoj kontejnerových aplikací a jejich nasazení do clusterů Kubernetes, který nevyžaduje téměř žádné znalosti Dockeru ani Kubernetes – dokonce je nemusíte ani instalovat. Používání nástrojů, jako je Draft, umožňuje vám a vašim týmům zaměřit se na vytváření aplikací pomocí Kubernetes, aniž byste se museli tolik zabývat infrastrukturou.

Draft můžete používat s libovolným registrem imagí Dockeru a jakýmkoli clusterem Kubernetes, a to i místně. Tento kurz ukazuje použití služby ACS s Kubernetes, službou Azure Container Registry a Azure DNS k vytvoření živého vývojářského kanálu pro průběžnou integraci a doručování pomocí nástroje Draft.


<a id="create-an-azure-container-registry" class="xliff"></a>

## Vytvoření služby Azure Container Registry
Můžete snadno [vytvořit novou službu Azure Container Registry](../container-registry/container-registry-get-started-azure-cli.md), ale postup je následující:

1. Vytvořte skupinu prostředků Azure pro správu registru Azure Container Registry a clusteru Kubernetes ve službě ACS.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. Vytvořte registr imagí Azure Container Registry pomocí příkazu [az acr create](/cli/azure/acr#create).
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


<a id="create-an-azure-container-service-with-kubernetes" class="xliff"></a>

## Vytvoření služby Azure Container Service pomocí Kubernetes

Nyní jste připraveni použít příkaz [az acs create](/cli/azure/acs#create) k vytvoření clusteru ACS s použitím Kubernetes jako hodnoty `--orchestrator-type`.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> Vzhledem k tomu, že Kubernetes není výchozím typem orchestrátoru, nezapomeňte použít přepínač `--orchestrator-type kubernetes`.

Výstup po úspěšném provedení je podobný následujícímu.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Když teď máte cluster, můžete importovat přihlašovací údaje pomocí příkazu [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials). Nyní máte místní konfigurační soubor pro váš cluster, který pro svou práci potřebuje Helm a Draft.

<a id="install-and-configure-draft" class="xliff"></a>

## Instalace a konfigurace nástroje Draft
Pokyny k instalaci nástroje Draft najdete v [úložišti Draft](https://github.com/Azure/draft/blob/master/docs/install.md). I když jsou poměrně jednoduché, vyžadují určitou konfiguraci, protože instalace je závislá na [Helmu](https://aka.ms/helm), který musí vytvořit diagram Helmu a nasadit ho do clusteru Kubernetes.

1. [Stáhněte a nainstalujte Helm](https://aka.ms/helm#install).
2. Pomocí Helmu vyhledejte a nainstalujte `stable/traefik`, kontroler příchozího přenosu dat pro umožnění příchozích požadavků ve vašich buildech.
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    Nyní nastavte na kontroleru `ingress` sledování, aby při nasazení zachytil hodnotu externí IP adresy. Tato IP adresa bude v další části [namapovaná na vaši doménu nasazení](#wire-up-deployment-domain).

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    V tomto případě je externí IP adresa pro doménu nasazení `13.64.108.240`. Nyní můžete namapovat doménu na tuto IP adresu.

<a id="wire-up-deployment-domain" class="xliff"></a>

## Napojení domény nasazení

Draft vytvoří vydanou verzi pro každý diagram Helmu, který vytvoří – pro každou aplikaci, na které pracujete. Každá z nich dostane vygenerovaný název, který Draft používá jako _subdoménu_ kořenové _domény nasazení_, nad kterou máte kontrolu. (V tomto příkladu používáme jako doménu nasazení `squillace.io`.) K povolení tohoto chování subdomén musíte v záznamech DNS pro vaši doménu nasazení vytvořit záznam A pro `'*'`, aby se každá vygenerovaná subdoména přesměrovala do kontroleru příchozího přenosu clusteru Kubernetes.

Váš poskytovatel domény má vlastní způsob přiřazování serverů DNS. Pokud chcete [delegovat názvové servery vaší domény do Azure DNS](../dns/dns-delegate-domain-azure-dns.md), postupujte následovně:

1. Vytvořte skupinu prostředků pro vaši zónu DNS.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Vytvořte zónu DNS pro vaši doménu.
Pomocí příkazu [az network dns zone create](/cli/azure/network/dns/zone#create) získejte názvové servery pro delegování řízení DNS do Azure DNS pro vaši doménu.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Přidejte získané servery DNS k poskytovateli vaší domény nasazení – to vám umožní použít Azure DNS ke směrování vaší domény, jak budete chtít.
4. Vytvořte položku sady záznamů A pro mapování domény nasazení na ID adresu `ingress` z kroku 2 v předchozí části.
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
Výstup by měl vypadat asi takto:
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. Nakonfigurujte Draft pro použití vašeho registru a vytvoření subdomény pro každý diagram Helmu, který vytvoří. Ke konfiguraci nástroje Draft potřebujete:
  - Název služby Azure Container Registry (v tomto příkladu `draft`).
  - Klíč registru nebo heslo získané příkazem `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.
  - Kořenovou doménu nasazení, u které jste nakonfigurovali mapování na externí IP adresu příchozího přenosu dat Kubernetes (v tomto příkladu `squillace.io`).

  Zavolejte `draft init` a proces konfigurace vás vyzve k zadání výše uvedených hodnot. Proces při prvním spuštění vypadá asi takto:
    ```
    draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order to install Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

Nyní jste připraveni nasadit aplikaci.


<a id="build-and-deploy-an-application" class="xliff"></a>

## Sestavení a nasazení aplikace

V úložišti Draft najdete [šest jednoduchých ukázkových aplikací](https://github.com/Azure/draft/tree/master/examples). Naklonujte úložiště a použijte [příklad v Pythonu](https://github.com/Azure/draft/tree/master/examples/python). Přejděte do adresáře examples/Python a zadáním příkazu `draft create` sestavte aplikaci. Mělo by to vypadat asi jako v následujícím příkladu.
```bash
$ draft create
--> Python app detected
--> Ready to sail
```

Výstup zahrnujte soubor Dockerfile a digram Helmu. K sestavení a nasazení stačí zadat `draft up`. Výstup je rozsáhlý, ale začíná podobně jako následující příklad.
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

A v případě úspěšného provedení končí podobně jako následující příklad.
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying to Kubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io to access your application

Watching local files for changes...
```

Bez ohledu na název vašeho diagramu teď můžete spustit příkaz `curl http://gangly-bronco.squillace.io` a obdržet odpověď `Hello World!`.

<a id="next-steps" class="xliff"></a>

## Další kroky

Když teď máte cluster ACS Kubernetes, můžete prozkoumat používání služby [Azure Container Registry](../container-registry/container-registry-intro.md), abyste mohli vytvářet další a jiná nasazení tohoto scénáře. Můžete například vytvořit sadu záznamů DNS domény draft._základní_doména.doména_nejvyšší_úrovně_, která bude pro specifická nasazení ACS řídit vše pro subdoménu na nižší úrovni.







