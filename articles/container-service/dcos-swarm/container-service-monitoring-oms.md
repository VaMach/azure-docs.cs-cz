---
title: "Cluster Azure DC/OS monitorování - Operations Management | Microsoft Docs"
description: "Monitorování clusteru Azure Container Service DC/OS s Microsoft Operations Management Suite."
services: container-service
documentationcenter: 
author: keikhara
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 9b8f96b34b53982c469273a3df9751ceb7930d60
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-operations-management-suite"></a>Monitorování clusteru Azure Container Service DC/OS s Operations Management Suite

Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur. Kontejner řešení je řešení v OMS analýzy protokolů, který umožňuje zobrazit inventář kontejneru, výkonu a protokoly na jednom místě. Můžete auditovat, řešení potíží s kontejnery zobrazením protokoly v centrálním umístění a najít aktivní využívání nadbytečné kontejneru na hostiteli.

![](media/container-service-monitoring-oms/image1.png)

Další informace o kontejneru řešení, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

## <a name="setting-up-oms-from-the-dcos-universe"></a>Nastavení OMS z universe DC/OS


Tento článek předpokládá, že jste nastavili DC/OS a nasadili jednoduchého webového kontejneru aplikace v clusteru.

### <a name="pre-requisite"></a>Předpoklad
- [Předplatné služby Microsoft Azure](https://azure.microsoft.com/free/) -vám to zdarma.  
- Instalační program pracovní prostor Microsoft OMS – najdete v části "Krok 3" pod
- [Rozhraní příkazového řádku DC/OS](https://dcos.io/docs/1.8/usage/cli/install/) nainstalována.

1. Na řídicím panelu DC/OS klikněte na Universe a vyhledejte "OMS, jak je uvedeno níže.

![](media/container-service-monitoring-oms/image2.png)

2. Klikněte na **Nainstalovat**. Uvidíte pop až s informace o verzi OMS a **instalovat balíček** nebo **rozšířený instalace** tlačítko. Když kliknete na tlačítko **rozšířený instalace**, což vede k **vlastnosti konkrétní konfigurace OMS** stránky.

![](media/container-service-monitoring-oms/image3.png)

![](media/container-service-monitoring-oms/image4.png)

3. Zde, zobrazí se výzva k zadání `wsid` (ID pracovního prostoru OMS) a `wskey` (OMS primární klíč pro id pracovního prostoru). Chcete-li získat i `wsid` a `wskey` je potřeba vytvořit na účet OMS <https://mms.microsoft.com>. Postupujte podle kroků pro vytvoření účtu. Po dokončení vytváření účtu, je nutné získat vaše `wsid` a `wskey` kliknutím **nastavení**, pak **připojené zdroje**a potom **servery se systémem Linux**, jak je uvedeno níže.

 ![](media/container-service-monitoring-oms/image5.png)

4. Vyberte číslo vám OMS instance a klikněte na tlačítko 'Zkontrolovat a nainstalovat'. Obvykle můžete získat počet instancí OMS rovná počtu Virtuálního počítače budete mít v clusteru agenta. Agent OMS pro Linux je nainstaluje jako jednotlivé kontejnery pro každý virtuální počítač, který chce shromažďovat informace o monitorování a informace o protokolování.

## <a name="setting-up-a-simple-oms-dashboard"></a>Nastavení jednoduchý řídicí panel OMS

Po instalaci agenta OMS pro Linux na virtuálních počítačích, dalším krokem je nastavit řídicím panelu OMS. Existují dva způsoby, jak to udělat: OMS portál nebo portál Azure.

### <a name="oms-portal"></a>Portálu OMS 

Přihlaste se k portálu OMS (<https://mms.microsoft.com>) a přejděte na **řešení Galerie**.

![](media/container-service-monitoring-oms/image6.png)

Jakmile jste na **řešení Galerie**, vyberte **kontejnery**.

![](media/container-service-monitoring-oms/image7.png)

Po dokončení výběru řešení kontejneru, zobrazí se na dlaždici na stránce Přehled OMS řídicí panel. Jakmile je indexovaný ingestovaný kontejner dat, zobrazí se dlaždici naplněný informace o řešení dlaždice zobrazení.

![](media/container-service-monitoring-oms/image8.png)

### <a name="azure-portal"></a>Azure Portal 

Přihlášení k portálu Azure v <https://portal.microsoft.com/>. Přejděte na **Marketplace**, vyberte **monitorování + správu** a klikněte na tlačítko **najdete v článku všechny**. Pak zadejte `containers` ve vyhledávání. Zobrazí se "kontejner" ve výsledcích hledání. Vyberte **kontejnery** a klikněte na tlačítko **vytvořit**.

![](media/container-service-monitoring-oms/image9.png)

Po kliknutí na tlačítko **vytvořit**, se zobrazí výzvu k pracovního prostoru. Vyberte pracovní prostor nebo pokud jeden nemáte, vytvořte nový pracovní prostor.

![](media/container-service-monitoring-oms/image10.PNG)

Po dokončení výběru pracovního prostoru, klikněte na tlačítko **vytvořit**.

![](media/container-service-monitoring-oms/image11.png)

Další informace o řešení kontejneru OMS, naleznete [analýzy protokolů řešení kontejneru](../../log-analytics/log-analytics-containers.md).

### <a name="how-to-scale-oms-agent-with-acs-dcos"></a>Postup škálování agenta OMS s ACS DC/OS 

V případě, že je potřeba mít nainstalovaný agent OMS souborem uzlu skutečný počet nebo jsou vertikálním navýšení kapacity VMSS přidáním více virtuálních počítačů, můžete tak učinit pomocí příjmu `msoms` služby.

Můžete přejít na Marathon nebo na kartě služeb uživatelského rozhraní DC/OS a škálovat vaše počet uzlů.

![](media/container-service-monitoring-oms/image12.PNG)

To nasadí do dalších uzlů, které ještě nebyly nasadit agenta OMS.

## <a name="uninstall-ms-oms"></a>Odinstalujte MS OMS

Chcete-li odinstalovat MS OMS zadejte následující příkaz:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Dejte nám vědět!
Co funguje? Co je chybějící? Co je potřeba pro to pro vás užitečné? Dejte nám vědět v <a href="mailto:OMSContainers@microsoft.com">OMSContainers</a>.

## <a name="next-steps"></a>Další kroky

 Teď, když jste nastavili OMS monitorování kontejnerů,[najdete v části řídicího panelu kontejneru](../../log-analytics/log-analytics-containers.md).
