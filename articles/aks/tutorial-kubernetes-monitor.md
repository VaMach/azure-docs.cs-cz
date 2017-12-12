---
title: "Kubernetes na kurz pro Azure – Kubernetes monitorování"
description: "Kurz AKS – monitorování Kubernetes s Operations Management Suite (OMS)"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d10f9ce965e832c826e2b27b4746231b47be83d0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-azure-container-service-aks"></a>Monitorování Azure Container Service (AKS)

Monitorování Kubernetes clusteru a kontejnerů, je důležité, zejména v případě, že spuštění clusteru výroby, ve velkém měřítku, s více aplikacemi.

V tomto kurzu můžete nakonfigurovat monitorování pomocí AKS clusteru [kontejnery řešení pro analýzu protokolu][log-analytics-containers].

V tomto kurzu součástí sedm osm, obsahuje následující úlohy:

> [!div class="checklist"]
> * Konfigurace monitorování řešení kontejneru
> * Konfigurace monitorování agentů
> * Přístup k monitorování informací na portálu Azure

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzech byla aplikace zabalené do kontejneru obrázků, tyto Image nahrané do registru kontejner Azure a cluster Kubernetes vytvořili.

Pokud se ještě provést tyto kroky a chcete sledovat, vrátit [kurzu 1 – Vytvoření kontejneru image][aks-tutorial-prepare-app].

## <a name="configure-the-monitoring-solution"></a>Konfigurace řešení monitorování

Na portálu Azure vyberte **nový** a vyhledejte řetězec `Container Monitoring Solution`. Jakmile se nachází, vyberte **vytvořit**.

![Přidat řešení](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Vytvořit nový pracovní prostor OMS, nebo vyberte nějaký existující. Pracovní prostor OMS formuláře vás provede tento proces.

Při vytváření pracovního prostoru, vyberte **připnout na řídicí panel** lze snadno načíst.

![Pracovní prostor OMS](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Až budete hotoví, vyberte **OK**. Po dokončení ověření vyberte **vytvořit** vytvoření kontejneru řešení monitorování.

Po vytvoření pracovního prostoru, zobrazí se vám na portálu Azure.

## <a name="get-workspace-settings"></a>Získat nastavení pracovního prostoru

Analýzy protokolů ID a klíč jsou potřeba ke konfiguraci řešení agent na uzlu Kubernetes.

K načtení těchto hodnot, vyberte **pracovním prostorem OMS** z kontejneru řešení nabídky na levé straně. Vyberte **upřesňující nastavení** a poznamenejte si **ID pracovního prostoru** a **primární klíč**.

## <a name="configure-monitoring-agents"></a>Konfigurovat monitorovací agenty

Následující soubor manifestu Kubernetes slouží ke konfiguraci monitorování prostředků v clusteru s podporou Kubernetes kontejneru. Vytvoří Kubernetes [DaemonSet][kubernetes-daemonset], která se spouští jeden pod na každém uzlu clusteru.

Uložte následující text do souboru s názvem `oms-daemonset.yaml`a nahraďte zástupný symbol hodnoty pro `WSID` a `KEY` s ID pracovního prostoru analýzy protokolů a klíč.

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
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
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
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
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
   nodeSelector:
    beta.kubernetes.io/os: linux
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
   volumes:
    - name: docker-sock
      hostPath:
       path: /var/run/docker.sock
    - name: container-hostname
      hostPath:
       path: /etc/hostname
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

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Po agenty běží, trvá několik minut, než OMS ingestování a zpracovat data.

## <a name="access-monitoring-data"></a>Přístup k datům monitorování

Na portálu Azure vyberte pracovní prostor analýzy protokolů, které je připnutá na řídicí panel portálu. Klikněte na **řešení monitorování kontejneru** dlaždici. Tady najdete informace o clusteru AKS a kontejnery z clusteru.

![Řídicí panel](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Najdete v článku [dokumentace Azure Log Analytics] [ log-analytics-docs] obsahuje podrobné pokyny k dotazování a analýze dat monitorování.

## <a name="next-steps"></a>Další kroky

V tomto kurzu sledovat Kubernetes clusteru pomocí OMS. Úlohy popsané součástí:

> [!div class="checklist"]
> * Konfigurace monitorování řešení kontejneru
> * Konfigurace monitorování agentů
> * Přístup k monitorování informací na portálu Azure

Přechodu na v dalším kurzu se dozvíte o upgradu Kubernetes na novou verzi.

> [!div class="nextstepaction"]
> [Upgrade Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml