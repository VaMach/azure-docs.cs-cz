---
title: "Cluster Azure Kubernetes monitorování - Operations Management | Microsoft Docs"
description: "Monitorování Kubernetes clusteru v Azure Container Service pomocí služby Microsoft Operations Management Suite"
services: container-service
documentationcenter: 
author: bburns
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: c402d81841ed5a79df8a84d1f69f984d010dbe0d
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/27/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-microsoft-operations-management-suite-oms"></a>Monitorování clusteru Azure Container Service s Operations Management Suite (OMS)

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Požadavky
Tento návod předpokládá, že máte [vytvořit Kubernetes clusteru Azure Container Service pomocí](container-service-kubernetes-walkthrough.md).

Předpokládá také, abyste měli `az` rozhraní příkazového řádku Azure a `kubectl` nástroje nainstalované.

Můžete otestovat, pokud máte `az` nainstalovaná, spuštěním nástroje:

```console
$ az --version
```

Pokud nemáte `az` nástroj nainstalovali, jsou k dispozici pokyny [zde](https://github.com/azure/azure-cli#installation).  
Alternativně můžete použít [prostředí cloudu Azure](https://docs.microsoft.com/azure/cloud-shell/overview), který má `az` rozhraní příkazového řádku Azure a `kubectl` nástroje pro je již nainstalována.  

Můžete otestovat, pokud máte `kubectl` nainstalovaná, spuštěním nástroje:

```console
$ kubectl version
```

Pokud nemáte `kubectl` nainstalován, můžete spustit:
```console
$ az acs kubernetes install-cli
```

K testování, pokud máte nainstalovaný ve vaší kubectl nástroj, který můžete spustit klíče kubernetes:
```console
$ kubectl get nodes
```

Pokud výše uvedené chyby příkazu si musíte nainstalovat kubernetes clusteru klíče do vaší kubectl nástroje. Můžete to udělat pomocí následujícího příkazu:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-operations-management-suite-oms"></a>Monitorování kontejnery s služby Operations Management Suite (OMS)

Microsoft Operations Management (OMS) je společnosti Microsoft založená na cloudu IT řešení správy, které pomáhá spravovat a chránit místní a cloudové infrastruktury. Kontejner řešení je řešení v OMS analýzy protokolů, který umožňuje zobrazit inventář kontejneru, výkonu a protokoly na jednom místě. Můžete auditovat, řešení potíží s kontejnery zobrazením protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.

![](media/container-service-monitoring-oms/image1.png)

Další informace o kontejneru řešení, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

## <a name="installing-oms-on-kubernetes"></a>Instalace OMS na Kubernetes

### <a name="obtain-your-workspace-id-and-key"></a>Získání ID a klíč
Agent komunikovat službu pro OMS je potřeba nakonfigurovat s id pracovního prostoru a klíč pracovního prostoru. Id pracovního prostoru a klíč, je potřeba vytvořit na účet OMS <https://mms.microsoft.com>. Postupujte podle kroků pro vytvoření účtu. Po dokončení vytváření účtu, je nutné získat id a klíč kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-oms-agent-using-a-daemonset"></a>Instalace agenta OMS pomocí DaemonSet
DaemonSets Kubernetes používá ke spuštění jednu instanci kontejner na jednotlivých hostitelích v clusteru.
Jsou ideální pro spuštění monitorovací agenty.

Tady je [DaemonSet YAML soubor](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Uložit do souboru s názvem `oms-daemonset.yaml` a nahraďte zástupný symbol hodnoty pro `WSID` a `KEY` s vaše id a klíč v souboru.

Po přidání vaše ID a klíč v konfiguraci DaemonSet, můžete nainstalovat agenta OMS na cluster s `kubectl` nástroj pro příkazový řádek:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-oms-agent-using-a-kubernetes-secret"></a>Instalace agenta OMS pomocí Kubernetes tajný klíč
K ochraně vašeho ID pracovního prostoru OMS a klíče, které můžete použít Kubernetes tajný klíč jako součást DaemonSet YAML souboru.

 - Zkopírujte skript, soubor tajný šablonu a soubor DaemonSet YAML (z [úložiště](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) a ujistěte se, že jsou na stejném adresáři. 
      - Generování skriptu - tajný klíč gen.sh tajný klíč
      - Šablona tajné – template.yaml tajný klíč
   - Soubor DaemonSet YAML - omsagent – ds-secrets.yaml
 - Spusťte skript. Skript vyzve pro ID pracovního prostoru OMS a primární klíč. Vložte který. skript se vytvoří soubor tajný yaml, můžete ji spustit.   
   ```
   #> sudo bash ./secret-gen.sh 
   ```

   - Vytvoření tajných klíčů pod spuštěním následujícího:``` kubectl create -f omsagentsecret.yaml ```
 
   - Pokud chcete zkontrolovat, spusťte následující: 

   ``` 
   root@ubuntu16-13db:~# kubectl get secrets
   NAME                  TYPE                                  DATA      AGE
   default-token-gvl91   kubernetes.io/service-account-token   3         50d
   omsagent-secret       Opaque                                2         1d
   root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
   Name:           omsagent-secret
   Namespace:      default
   Labels:         <none>
   Annotations:    <none>

   Type:   Opaque

   Data
   ====
   WSID:   36 bytes
   KEY:    88 bytes 
   ```
 
  - Vytvoření vaší omsagent démon set spuštěním``` kubectl create -f omsagent-ds-secrets.yaml ```

### <a name="conclusion"></a>Závěr
A to je vše! Po několika minutách byste měli vidět dat odesílaných do řídicího panelu OMS.
