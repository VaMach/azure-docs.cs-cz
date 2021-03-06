---
title: "Kurz Kubernetes v Azure – Monitorování Kubernetes"
description: "Kurz AKS – Monitorování Kubernetes pomocí Microsoft Operations Management Suite (OMS)"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2fedd615733e3bf51469d3b69d5fe51e3e99087e
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="monitor-azure-container-service-aks"></a>Monitorování služby Azure Container Service (AKS)

Monitorování clusteru a kontejnerů Kubernetes je důležité, zejména pokud provozujete produkční cluster ve velkém měřítku a s několika aplikacemi.

V tomto kurzu nakonfigurujete monitorování clusteru AKS pomocí [řešení kontejnerů pro Log Analytics][log-analytics-containers].

V tomto kurzu, který je sedmou částí osmidílné série, se probírají následující úlohy:

> [!div class="checklist"]
> * Konfigurace řešení pro monitorování kontejnerů
> * Konfigurace agentů monitorování
> * Přístup k informacím o monitorování na webu Azure Portal

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzech se aplikace zabalila do imagí kontejneru, tyto image se odeslaly do Azure Container Registry a vytvořil se cluster Kubernetes.

Pokud jste tyto kroky neprovedli a chcete si je projít, vraťte se ke [kurzu 1 – Vytváření imagí kontejneru][aks-tutorial-prepare-app].

## <a name="configure-the-monitoring-solution"></a>Konfigurace řešení pro monitorování

Na webu Azure Portal vyberte **Vytvořit prostředek** a vyhledejte řešení `Container Monitoring Solution`. Jakmile řešení najdete, vyberte **Vytvořit**.

![Přidání řešení](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Vytvořte nový pracovní prostor OMS nebo vyberte existující. Tímto procesem vás provede formulář Pracovní prostor OMS.

Při vytváření pracovního prostoru vyberte **Připnout na řídicí panel**, abyste k němu měli snadný přístup.

![Pracovní prostor OMS](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Až budete hotovi, vyberte **OK**. Po dokončení ověřování vyberte **Vytvořit** a vytvořte řešení pro monitorování kontejnerů.

Po vytvoření se pracovní prostor zobrazí na webu Azure Portal.

## <a name="get-workspace-settings"></a>Získání nastavení pracovního prostoru

Ke konfiguraci agenta řešení na uzlech Kubernetes se vyžaduje ID a klíč pracovního prostoru Log Analytics.

Tyto hodnoty načtete výběrem možnosti **Pracovní prostor Log Analytics** v levé nabídce řešení kontejnerů. Vyberte **Upřesnit nastavení** a poznamenejte si **ID PRACOVNÍHO PROSTORU** a **PRIMÁRNÍ KLÍČ**.

## <a name="configure-monitoring-agents"></a>Konfigurace agentů monitorování

Ke konfiguraci agentů monitorování kontejnerů v clusteru Kubernetes můžete použít následující soubor manifestu Kubernetes. Ten vytvoří [DaemonSet][kubernetes-daemonset] Kubernetes, který na každém uzlu clusteru spustí jeden pod.

Uložte následující text do souboru `oms-daemonset.yaml` a nahraďte zástupné hodnoty `WSID` a `KEY` svým ID a klíčem pracovního prostoru Log Analytics.

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
        - mountPath: /var/lib/docker/containers/
          name: container-log
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
    - name: container-log
      hostPath:
       path: /var/lib/docker/containers/
```

Pomocí následujícího příkazu vytvořte DaemonSet:

```azurecli
kubectl create -f oms-daemonset.yaml
```

Vytvoření DaemonSet můžete ověřit spuštěním následujícího příkazu:

```azurecli
kubectl get daemonset
```

Výstup je podobný tomuto:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Po spuštění agentů trvá OMS ingestování a zpracování dat několik minut.

## <a name="access-monitoring-data"></a>Přístup k datům monitorování

Na webu Azure Portal vyberte pracovní prostor Log Analytics, který se připnul na řídicí panel portálu. Klikněte na dlaždici **Řešení pro monitorování kontejnerů**. Tady najdete informace o clusteru AKS a jeho kontejnerech.

![Řídicí panel](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Podrobné pokyny k dotazování a analýze dat monitorování najdete v [dokumentaci k Azure Log Analytics][log-analytics-docs].

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste monitorovali svůj cluster Kubernetes pomocí OMS. Mezi probírané úlohy patří:

> [!div class="checklist"]
> * Konfigurace řešení pro monitorování kontejnerů
> * Konfigurace agentů monitorování
> * Přístup k informacím o monitorování na webu Azure Portal

Přejděte k dalšímu kurzu, kde se seznámíte s upgradem Kubernetes na novou verzi.

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
