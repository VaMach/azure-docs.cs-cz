---
title: "OpenShift na Přehled služby Azure | Microsoft Docs"
description: "OpenShift na Přehled služby Azure."
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
ms.openlocfilehash: f9641b52db91a4356f6d5789a8cd78a6bb3da02b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="openshift-overview"></a>Přehled OpenShift

OpenShift je otevřít a rozšiřitelné kontejneru aplikační platforma, která přináší docker a Kubernetes k podnikové síti.  

OpenShift zahrnuje Kubernetes pro správu a kontejner orchestration. Přidá developer a zaměřené na operace nástroje, které umožňují:

- Rychlý vývoj aplikací
- Snadné nasazení a škálování
- Dlouhodobé údržby životního cyklu pro týmy a aplikace

Existuje více nabídek OpenShift, z nichž dva jsou k dispozici pro spuštění v Azure.

- OpenShift Origin
- OpenShift Container Platform
- OpenShift Online
- OpenShift vyhrazené

Čtyři nabídek zahrnutých, dva jsou dostupné pro zákazníky, k nasazení do Azure na své vlastní - OpenShift původ a OpenShift kontejneru platformy.

## <a name="openshift-origin"></a>OpenShift Origin

[S otevřeným zdrojem](https://www.openshift.org/) nadřazený projekt OpenShift, který je komunita podporována. Původ můžete nainstalovat na CentOS nebo RHEL.

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Enterprise ready ([komerční nabídky](https://www.openshift.com)) verze z Red Hat, kterou podporuje Red Hat. Zákazník nakoupí potřebná oprávnění pro platformu OpenShift kontejneru a je odpovědná za instalaci a správu celé infrastruktury.

Vzhledem k tomu, že zákazník "vlastní" celý platformy, se můžete nainstalovat v jejich místní datacentra, veřejného cloudu (Azure, AWS, Google atd.), atd.

## <a name="openshift-online"></a>OpenShift Online

Red Hat spravované **víceklientské** OpenShift (pomocí platformy kontejneru). Red Hat spravuje všechny základní infrastruktury (virtuální počítače, OpenShift clusteru, sítě, úložiště, atd.). 

Zákazník nasadí kontejnery, ale nemá žádnou kontrolu, na kterých hostitelích kontejnery spustit. Vzhledem k tomu, že je více klientů, může kontejnery společně umístěný na stejném hostitelé virtuálních počítačů jako kontejnery od ostatních zákazníků. Je náklady na kontejner.

## <a name="openshift-dedicated"></a>OpenShift vyhrazené

Red Hat spravované **jednoho klienta** OpenShift (pomocí platformy kontejneru). Red Hat spravuje všechny základní infrastruktury (virtuální počítače, OpenShift clusteru, sítě, úložiště, atd.). Cluster je specifická pro jedno zákazníka a spustí ve veřejném cloudu (Azure AWS, Google, - brzo časná 2018). Počáteční clusteru zahrnuje čtyři uzly aplikace $48 kb/rok (předem platbu celý rok).

## <a name="next-steps"></a>Další kroky

- [Konfigurace běžné požadavků pro OpenShift v Azure](./openshift-prerequisites.md)
- [Nasazení OpenShift původu](./openshift-origin.md)
- [Nasazení OpenShift kontejneru platformy](./openshift-container-platform.md)
- [Úlohy nasazení POST](./openshift-post-deployment.md)
- [Řešení potíží s nasazením OpenShift](./openshift-troubleshooting.md)
