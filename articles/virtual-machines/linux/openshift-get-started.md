---
title: "OpenShift v Azure přehled | Microsoft Docs"
description: "Přehled OpenShift v Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 5d154a1c860a5ebd7af2efd55b470bb14efe6c67
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="openshift-in-azure"></a>OpenShift v Azure

OpenShift je otevřít a rozšiřitelné kontejneru aplikační platforma, která přináší Docker a Kubernetes k podnikové síti.  

OpenShift zahrnuje Kubernetes pro správu a kontejner orchestration. Přidá developer a operace-zaměřená na nástroje, které umožňují:

- Rychlý vývoj aplikací.
- Snadné nasazení a škálování.
- Dlouhodobé údržby životního cyklu pro týmy a aplikace.

Existuje více verzí OpenShift, z nichž dva jsou k dispozici pro spuštění v Azure:

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift vyhrazené

Čtyři verzí popsaná v tomto článku, dva jsou dostupné pro zákazníky nasadit v Azure na své vlastní: OpenShift původ a OpenShift kontejneru platformy.

## <a name="openshift-origin"></a>OpenShift Origin

Původ [open-source](https://www.openshift.org/) nadřazený projekt OpenShift, který je komunita podporována. Původ můžete nainstalovat na CentOS nebo Red Hat Enterprise Linux (RHEL).

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Kontejner platformy je připravené pro podniky [komerční verze](https://www.openshift.com) z a Red Hat podporována. S touto verzí zákazníci zakoupit potřebná oprávnění pro platformu OpenShift kontejneru a jsou zodpovědní za instalaci a správu celé infrastruktury.

Vzhledem k tomu, že zákazníci "vlastní" celý platformy, se ho instalovat ve svém datovém centru, místní nebo ve veřejném cloudu (například Azure, AWS nebo Google).

## <a name="openshift-online"></a>OpenShift Online

Online je Red Hat spravovaná *víceklientské* OpenShift používající kontejneru platformy. Red Hat spravuje všechny základní infrastruktury (třeba virtuální počítače, OpenShift clusteru, sítě a úložiště). 

S touto verzí zákazník nasadí kontejnery, ale nemá žádnou kontrolu, přes které hostitele kontejnery spustit. Protože Online je více klientů, kontejnery může nacházet na stejné hostitelé virtuálních počítačů jako kontejnery od ostatních zákazníků. Je náklady na kontejner.

## <a name="openshift-dedicated"></a>OpenShift vyhrazené

Vyhrazené je Red Hat spravovaná *jednoho klienta* OpenShift používající kontejneru platformy. Red Hat spravuje všechny základní infrastruktury (virtuální počítače, OpenShift clusteru, sítě, úložiště, atd.). Cluster je specifická pro jedno zákazníka a spustí ve veřejném cloudu (například AWS nebo Google s Azure brzo časná 2018). Cluster s podporou počáteční zahrnuje čtyři uzly aplikace pro 48,000 za jeden rok. (placené předem).

## <a name="next-steps"></a>Další kroky

- [Konfigurace běžné požadavků pro OpenShift v Azure](./openshift-prerequisites.md)
- [Nasazení OpenShift původ v Azure](./openshift-origin.md)
- [Nasazení OpenShift kontejneru platformy v Azure](./openshift-container-platform.md)
- [Úlohy po nasazení](./openshift-post-deployment.md)
- [Řešení potíží s OpenShift nasazení](./openshift-troubleshooting.md)
