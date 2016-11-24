---
title: "Síťová rozhraní | Dokumentace Microsoftu"
description: "Přečtěte si o síťových rozhraních Azure v Azure Resource Manageru."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: f58b503f-18bf-4377-aa63-22fc8a96e4be
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 6e96471c4f61e1ebe15c23f87ac646001d8e30ee
ms.openlocfilehash: 586877781f4b74c49936a8129db49a67018bab65


---
# <a name="network-interfaces"></a>Síťová rozhraní
Síťové rozhraní (NIC) je propojení mezi virtuálním počítačem a základní softwarovou sítí. Tento článek vysvětluje, co je síťové rozhraní a jak se používá v modelu nasazení Azure Resource Manager.

Společnost Microsoft doporučuje nasazení nových prostředků pomocí modelu nasazení Resource Manager, ale virtuální počítače s připojením k síti můžete nasadit i v modelu nasazení [Classic](virtual-network-ip-addresses-overview-classic.md). Pokud již znáte model Classic, v modelu nasazení Resource Manager existují důležité rozdíly, co se týče sítí virtuálních počítačů. Další informace o těchto rozdílech najdete v článku [Sítě virtuálních počítačů – Classic](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

Síťové rozhraní v Azure:

1. Je prostředek, který lze vytvořit, odstranit a který má vlastní konfigurovatelné nastavení.
2. Musí být při vytvoření připojené k jedné podsíti v jedné virtuální síti Azure. Pokud jsou pro vás virtuální sítě novinkou, přečtěte si článek [Přehled virtuálních síti](virtual-networks-overview.md), kde o nich zjistíte více. Síťové rozhraní musí být připojené k virtuální síti, která je ve stejném[ umístění](https://azure.microsoft.com/regions) a [předplatném](../azure-glossary-cloud-terminology.md#subscription) Azure jako síťové rozhraní. Po vytvoření síťového rozhraní můžete změnit podsíť, ke které je připojené, ale nelze změnit virtuální síť, ke které je připojené.
3. Má přiřazený název, který po vytvoření síťového rozhraní již nelze změnit. Název musí být jedinečný v rámci [skupiny prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) Azure, ale nemusí být jedinečný v rámci předplatného, umístění Azure, v němž je vytvořené, ani virtuální sítě, ke které je připojené. V rámci předplatného Azure je obvykle vytvořeno několik síťových rozhraní. Místo používání výchozích názvů se doporučuje navrhnout zásady vytváření názvů pro usnadnění správy více síťových rozhraní. Návrhy najdete v článku [Doporučené zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).
4. Může být připojené k virtuálnímu počítači, ale pouze k jednomu, který je ve stejném umístění jako síťové rozhraní.
5. Má adresu MAC, která přetrvává v síťovém rozhraní po celou dobu jeho připojení k virtuálnímu počítači. Adresa MAC přetrvává i v případě restartování virtuálního počítače (z operačního systému) nebo jeho zastavení (zrušení přidělení) a spuštění pomocí webu Azure Portal, Azure PowerShellu nebo rozhraní příkazového řádku Azure (Azure CLI). V případě odpojení síťového rozhraní od virtuálního počítače a připojení k jinému virtuálnímu počítači obdrží jinou adresu MAC. Pokud je síťové rozhraní odstraněno, adresa MAC se přiřadí jiným síťovým rozhraním.
6. Musí mít přiřazenou jednu primární **privátní** statickou nebo dynamickou IP adresu *IPv4*.
7. Může mít přiřazen jeden prostředek veřejné IP adresy.
8. Podporuje urychlení sítí pomocí rozhraní SR-IOV (single-root I/O virtualization) pro určité velikosti virtuálních počítačů, na kterých běží určité verze operačního systému Microsoft Windows Server. Další informace o této funkci ve verzi PREVIEW najdete v článku [Urychlení sítí pro virtuální počítač](virtual-network-accelerated-networking-powershell.md).
9. Může přijímat provoz, který není určen pro jemu přiřazené privátní IP adresy, pokud je pro dané síťové rozhraní povolené předávání IP adres. Pokud například na virtuálním počítači běží software brány firewall, směruje pakety, které nejsou určené pro jeho IP adresy. Na virtuálním počítači musí běžet software, který dokáže směrovat nebo předávat provoz, ale aby to mohl provádět, musí pro síťové rozhraní být povolené předávání IP adres.
10. Je často vytvořené ve stejné skupině prostředků jako virtuální počítač, k němuž je připojené, nebo jako virtuální síť, k níž je připojené, přestože to není nutné.

## <a name="vms-with-multiple-network-interfaces"></a>Virtuální počítače s několika síťovými rozhraními
K virtuálnímu počítači může být připojeno více síťových rozhraní, ale je při tom třeba vzít v úvahu následující skutečnosti:  

* Velikost virtuálního počítače musí podporovat více síťových rozhraní. Další informace o tom, které velikosti virtuálních počítačů podporují více síťových rozhraní, najdete v článcích [Velikosti virtuálních počítačů s Windows Serverem](../virtual-machines/virtual-machines-windows-sizes.md) a [Velikosti virtuálních počítačů s Linuxem](../virtual-machines/virtual-machines-linux-sizes.md).
* Virtuální počítač je nutné vytvořit alespoň se dvěma síťovými rozhraními. Pokud je virtuální počítač vytvořen pouze s jedním síťovým rozhraním, nebude k němu po vytvoření možné připojit další síťová rozhraní, a to ani v případě, že velikost virtuálního počítače podporuje více než jedno síťové rozhraní. Pokud byl virtuální počítač vytvořen alespoň se dvěma síťovými rozhraními a velikost virtuálního počítače podporuje více než dvě síťová rozhraní, můžete k němu po vytvoření připojit další síťová rozhraní.  
* Pokud jsou k virtuálnímu počítači připojena alespoň tři síťová rozhraní, můžete od něj odpojit sekundární síťová rozhraní (primární síťové rozhraní nelze odpojit). Pokud jsou k virtuálnímu počítači připojena dvě síťová rozhraní nebo méně, nelze od něj síťová rozhraní odpojit.  
* Pořadí síťových rozhraní ve virtuálním počítači je náhodné a může se také měnit následkem aktualizací infrastruktury Azure. IP adresy a odpovídající ethernetové adresy MAC však zůstanou stejné. Předpokládejme například, že operační systém identifikuje síťové rozhraní Azure NIC1 jako Eth1. Eth1 má IP adresu 10.1.0.100 a adresu MAC 00-0D-3A-B0-39-0D. Po aktualizaci nebo restartování infrastruktury Azure může operační systém najednou identifikovat síťové rozhraní Azure NIC1 jako Eth2, ale IP adresa a adresa MAC budou stejné, jako když operační systém identifikoval Azure NIC1 jako Eth1. Pokud restartování spustí zákazník, pořadí síťových rozhraní v operačním systému se nezmění.  
* Pokud je virtuální počítač členem [skupiny dostupnosti](../azure-glossary-cloud-terminology.md#availability-set), všechny virtuální počítače v rámci této skupiny dostupnosti musí mít alespoň jedno síťové rozhraní. Pokud má virtuální počítač více síťových rozhraní, není nutné, aby všechny virtuální počítače měly stejný počet síťových rozhraní, pokud má každý z nich alespoň dvě.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak vytvořit virtuální počítač s jedním síťovým rozhraním v článku [Vytvoření virtuálního počítače](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
* Zjistěte, jak vytvořit virtuální počítač s více síťovými rozhraními v článku [Nasazení virtuálního počítače s několika síťovými rozhraními](virtual-network-deploy-multinic-arm-ps.md).
* Zjistěte, jak vytvořit síťové rozhraní s několika konfiguracemi protokolu IP v článku [Virtuální počítače Azure s několika IP adresami](virtual-network-multiple-ip-addresses-powershell.md).




<!--HONumber=Nov16_HO3-->


