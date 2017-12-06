---
title: "Kurz pro Azure Container Service - Kubernetes monitorování"
description: "Kurz pro Azure Container Service – monitorování Kubernetes s Operations Management Suite (OMS)"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 948e3aeea34a0355c3d958f29008c26499e19ba4
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Monitorování Kubernetes clusteru se službou Operations Management Suite

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Monitorování Kubernetes clusteru a kontejnerů, je důležité, zejména v případě, že spravujete provozní cluster ve velkém měřítku s více aplikacemi. 

Můžete využít výhod několik Kubernetes řešení monitorování, buď od společnosti Microsoft nebo jiní poskytovatelé. V tomto kurzu můžete Kubernetes cluster monitorovat pomocí řešení kontejnery v [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), řešení správy založené na cloudu IT společnosti Microsoft. (OMS kontejnery řešení je ve verzi preview.)

V tomto kurzu součástí sedm 7, obsahuje následující úlohy:

> [!div class="checklist"]
> * Získat nastavení pracovní prostor OMS
> * Nastavit OMS agentů na uzlech Kubernetes
> * Přístup k monitorování informací portálu OMS nebo portálu Azure

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzech byla aplikace zabalené do kontejneru obrázků, tyto Image nahrané do registru kontejner Azure a cluster Kubernetes vytvořili. 

Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Získat nastavení pracovního prostoru

Když se dostanete [portálu OMS](https://mms.microsoft.com), přejděte na **nastavení** > **připojené zdroje** > **servery se systémem Linux**. Zde můžete najít *ID pracovního prostoru* a primární nebo sekundární *klíč pracovního prostoru*. Poznamenejte si tyto hodnoty, které budete muset nastavit OMS agentů v clusteru.

## <a name="set-up-oms-agents"></a>Nastavit OMS agentů

Zde je soubor YAML nastavit OMS agenty na uzly clusteru Linux. Vytvoří Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), která se spouští jednu identické pod na každém uzlu clusteru. Prostředek DaemonSet je ideální pro nasazení agenta monitorování. 

Uložte následující text do souboru s názvem `oms-daemonset.yaml`a nahraďte zástupný symbol hodnoty pro *myWorkspaceID* a *myWorkspaceKey* s ID pracovního prostoru OMS a klíč. (V produkčním prostředí, můžete zakódovat tyto hodnoty jako tajných klíčů.)

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

Vytvořte DaemonSet pomocí následujícího příkazu:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Pokud chcete zjistit, jestli je vytvořená DaemonSet, spusťte příkaz:

```azurecli-interactive
kubectl get daemonset
```

Výstup je podobný tomuto:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Po agenty běží, trvá několik minut, než OMS ingestování a zpracovat data.

## <a name="access-monitoring-data"></a>Přístup k datům monitorování

Zobrazit a analyzovat monitorování dat pomocí kontejneru OMS [kontejneru řešení](../../log-analytics/log-analytics-containers.md) v portálu OMS nebo portálu Azure. 

K instalaci kontejneru řešení pomocí [portálu OMS](https://mms.microsoft.com), přejděte na **řešení Galerie**. Pak přidejte **kontejneru řešení**. Můžete taky přidat kontejnery řešení z [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

Na portálu OMS, vyhledejte **kontejnery** souhrnné dlaždice na řídicím panelu OMS. Klikněte na dlaždici podrobnosti, včetně: události kontejneru, chyb, stav, bitové kopie inventáře a využití procesoru a paměti. Podrobnější informace, klikněte na řádek na libovolnou dlaždici nebo provádět [hledání protokolů](../../log-analytics/log-analytics-log-searches.md).

![Řídicí panel kontejnery na portálu OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Podobně v portálu Azure přejděte do **analýzy protokolů** a vyberte název pracovního prostoru. Chcete-li zobrazit **kontejnery** dlaždice souhrnu, klikněte na tlačítko **řešení** > **kontejnery**. Chcete-li zobrazit podrobnosti, klikněte na dlaždici.

Najdete v článku [dokumentace Azure Log Analytics](../../log-analytics/index.yml) obsahuje podrobné pokyny k dotazování a analýze dat monitorování.

## <a name="next-steps"></a>Další kroky

V tomto kurzu sledovat Kubernetes clusteru pomocí OMS. Úlohy popsané součástí:

> [!div class="checklist"]
> * Získat nastavení pracovní prostor OMS
> * Nastavit OMS agentů na uzlech Kubernetes
> * Přístup k monitorování informací portálu OMS nebo portálu Azure


Tento odkaz zobrazíte předdefinovaných skriptu ukázky pro službu kontejneru.

> [!div class="nextstepaction"]
> [Skript ukázek Azure Container Service](cli-samples.md)
