---
title: "Přehled služby Azure Container Instances"
description: "Vysvětlení služby Azure Container Instances"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: overview
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 01e539856adbdcf02dc4e49087a3ab71b328db5a
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="azure-container-instances"></a>Azure Container Instances

Kontejnery se rychle stávají upřednostňovaným způsobem balení, nasazování a správy cloudových aplikací. Služba Azure Container Instances nabízí nejrychlejší a nejjednodušší způsob provozování kontejneru v Azure. Není přitom nutné zřizovat žádné virtuální počítače nebo využívat služby vyšší úrovně.

Azure Container Instances je skvělým řešením pro jakýkoli scénář, který může fungovat v izolovaných kontejnerech, včetně jednoduchých aplikací, automatizace úkolů a úloh sestavení. Pro scénáře, kde potřebujete úplnou orchestraci kontejnerů, včetně zjišťování služeb napříč více kontejnery, automatického škálování a koordinovaných upgradů aplikací, doporučujeme službu [Azure Container Service (AKS)](../aks/index.yml).

## <a name="fast-startup-times"></a>Rychlé časy spuštění

Kontejnery nabízejí významné výhody při spouštění oproti virtuálním počítačům. Se službou Azure Container Instances můžete v Azure spustit kontejner během několika sekund a bez nutnosti zřizovat a spravovat virtuální počítače.

## <a name="hypervisor-level-security"></a>Zabezpečení na úrovni hypervisoru

Kontejnery tradičně nabízejí izolaci závislostí aplikace a zásady správného řízení prostředků, ale nebyly považovány za dostatečně odolné pro použití v nehostinném prostředí více tenantů. Díky službě Azure Container Instances je vaše aplikace v kontejneru izolována stejně, jako by byla na virtuálním počítači.

## <a name="custom-sizes"></a>Vlastní velikosti

Kontejnery jsou obvykle optimalizované pro spouštění jenom jedné aplikace, ale konkrétní požadavky těchto aplikací se můžou značně lišit. Se službou Azure Container Instances si můžete vyžádat přesně to, co potřebujete z hlediska procesorových jader a paměti. Platíte podle toho, co si vyžádáte, a účtuje se po sekundách, takže můžete podrobně optimalizovat náklady podle vašich potřeb.

## <a name="public-ip-connectivity"></a>Připojení pomocí veřejné IP adresy

Se službou Azure Container Instances můžete své kontejnery zveřejnit přímo na internetu s použitím veřejné IP adresy. V budoucnu plánujeme možnosti sítě rozšířit o integraci s virtuálními sítěmi, nástroji pro vyrovnávání zatížení a dalšími základními částmi síťové infrastruktury Azure.

## <a name="persistent-storage"></a>Trvalé úložiště

Pro načtení a uložení stavu pomocí služby Azure Container Instances nabízíme přímé [připojení sdílených složek Azure](container-instances-mounting-azure-files-volume.md).

## <a name="linux-and-windows-containers"></a>Kontejnery Windows a Linuxu

Se službou Azure Container Instances můžete plánovat kontejnery Windows i Linuxu pomocí stejného rozhraní API. Při vytváření [skupiny kontejnerů](container-instances-container-groups.md) stačí jednoduše zadat typ operačního systému.

Některé funkce jsou momentálně omezené jenom na linuxové kontejnery. Pracujeme na tom, aby všechny funkce byly dostupné i pro kontejnery Windows. Aktuální rozdíly pro tyto platformy najdete v tématu věnovaném [kvótám a dostupnosti oblastí pro Azure Container Instances](container-instances-quotas.md).

## <a name="co-scheduled-groups"></a>Společně plánované skupiny

Azure Container Instances podporuje plánování [skupin více kontejnerů](container-instances-container-groups.md), které sdílejí hostitelský počítač, místní síť, úložiště a životní cyklus. Díky tomu můžete kombinovat hlavní aplikaci s dalšími, které zastávají podpůrnou roli, jako je například protokolování.

## <a name="next-steps"></a>Další kroky

Vyzkoušejte si nasazení kontejneru do Azure jediným příkazem s využitím naší [příručky Rychlý start](container-instances-quickstart.md).