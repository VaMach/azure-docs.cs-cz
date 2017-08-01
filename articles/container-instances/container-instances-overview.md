---
title: "Přehled služby Azure Container Instances | Dokumentace Azure"
description: "Vysvětlení služby Azure Container Instances"
services: container-service
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: a678700884b612cad6281eb8f3b74ce63a0ebb69
ms.openlocfilehash: b77ba307938b6b1f5221de40cc135d17c15c1d9b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---

# <a name="azure-container-instances"></a>Azure Container Instances

Kontejnery se rychle stávají upřednostňovaným způsobem balení, nasazování a správy cloudových aplikací. Služba Azure Container Instances nabízí nejrychlejší a nejjednodušší způsob provozování kontejneru v Azure. Není přitom nutné zřizovat žádné virtuální počítače nebo využívat služby vyšší úrovně. 

Azure Container Instances je skvělým řešením pro jakýkoli scénář, který může fungovat v izolovaných kontejnerech, včetně jednoduchých aplikací, automatizace úkolů a úloh sestavení. Pro scénáře, kde potřebujete úplnou orchestraci kontejnerů, včetně zjišťování služeb napříč více kontejnery, automatického škálování a koordinovaných upgradů aplikací, doporučujeme službu [Azure Container Service](https://docs.microsoft.com/azure/container-service/).

## <a name="fast-startup-times"></a>Rychlé časy spuštění

Kontejnery nabízejí významné výhody při spouštění oproti virtuálním počítačům. Se službou Azure Container Instances můžete v Azure spustit kontejner během několika sekund a bez nutnosti zřizovat a spravovat virtuální počítače.

## <a name="hypervisor-level-security"></a>Zabezpečení na úrovni hypervisoru

Kontejnery tradičně nabízejí izolaci závislostí aplikace a zásady správného řízení prostředků, ale nebyly považovány za dostatečně odolné pro použití v nehostinném prostředí více tenantů. Díky službě Azure Container Instances je vaše aplikace v kontejneru izolována stejně, jako by byla na virtuálním počítači.

## <a name="custom-sizes"></a>Vlastní velikosti

Kontejnery jsou obvykle optimalizované pro spouštění jenom jedné aplikace, ale konkrétní požadavky těchto aplikací se můžou značně lišit. Se službou Azure Container Instances si můžete vyžádat přesně to, co potřebujete z hlediska procesorových jader a paměti. Platíte podle toho, co si vyžádáte, a účtuje se po sekundách, takže můžete podrobně optimalizovat náklady podle vašich potřeb.

## <a name="public-ip-connectivity"></a>Připojení pomocí veřejné IP adresy

Se službou Azure Container Instances můžete své kontejnery zveřejnit přímo na internetu s použitím veřejné IP adresy. V budoucnu plánujeme možnosti sítě rozšířit o integraci s virtuálními sítěmi, nástroji pro vyrovnávání zatížení a dalšími základními částmi síťové infrastruktury Azure.

## <a name="persistent-storage"></a>Trvalé úložiště

Pro načtení a uložení stavu pomocí služby Azure Container Instances nabízíme přímé připojení sdílených složek Azure.

## <a name="linux-and-windows-containers"></a>Kontejnery Windows a Linuxu

Se službou Azure Container Instances můžete plánovat kontejnery Windows i Linuxu pomocí stejného rozhraní API. Jednoduše určíte základní typ operačního systému a všechno ostatní je stejné.

## <a name="co-scheduled-groups"></a>Společně plánované skupiny

Azure Container Instances podporuje plánování skupin více kontejnerů, které sdílejí hostitelský počítač, místní síť, úložiště a životní cyklus. Díky tomu můžete kombinovat hlavní aplikaci s dalšími, které zastávají podpůrnou roli, jako je například protokolování.

## <a name="next-steps"></a>Další kroky

Vyzkoušejte si nasazení kontejneru do Azure jediným příkazem s využitím naší [příručky Rychlý start](container-instances-quickstart.md).
