---
title: "Vytvoření sady škálování virtuálního počítače pomocí portálu Azure | Microsoft Docs"
description: "Nasazení sad škálování pomocí portálu Azure."
keywords: "Sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc52368a1a14ad094e7ab9180b90a9aa4ccdb6d8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-virtual-machine-scale-set-with-the-azure-portal"></a>Jak vytvořit sadu škálování virtuálního počítače pomocí portálu Azure
Tento kurz ukazuje, jak je snadné vytvořit sadu škálování virtuálního počítače v několika málo minut pomocí portálu Azure. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Volba image virtuálního počítače z Marketplace
Z portálu můžete snadno nasadit škálování s CentOS, CoreOS, Debian, Ubuntu Server, ostatní Linux Image a bitové kopie systému Windows Server.

První, přejděte na [portál Azure](https://portal.azure.com) ve webovém prohlížeči. Klikněte na tlačítko **nový**, vyhledejte **škálovací sadu**a pak vyberte **škálovací sadu virtuálních počítačů** položku:

![hledání portálu sady škálování virtuálního počítače Azure](./media/virtual-machine-scale-sets-portal-create/portal-search.png)

## <a name="create-the-scale-set"></a>Vytvoření škálovací sady
Teď můžete použít výchozí nastavení a rychle vytvořit byly sadou škálování.

* Zadejte název pro sadu škálování.  
Tento název bude základem plně kvalifikovaný název domény služby Vyrovnávání zatížení před byly sadou škálování, proto se ujistěte, že název je jedinečný všechny Azure.

* Vyberte požadovaný typ operačního systému.

* Zadejte své požadované uživatelské jméno a vyberte typ ověřování, která dáváte přednost.  
Pokud si zvolíte heslo, musí být nejméně 12 znaků dlouhé a splňovat tři ze čtyř následujících složitost: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Pokud se rozhodnete **veřejný klíč SSH**, nezapomeňte pouze vložení v svůj veřejný klíč, *není* privátního klíče:

* Vyberte **Ano** nebo **ne** k **povolit škálování nad rámec 100 instance**.  
Pokud ano, nastavte měřítka může mít rozsah v několika skupinách pro umístění. Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-placement-groups.md).

* Je nutné vybrat odpovídající **instance velikost**.  
Další informace o velikosti virtuálních počítačů najdete v tématu [velikosti virtuálních počítačů Windows](..\virtual-machines\windows\sizes.md) nebo [velikosti virtuálního počítače s Linuxem](..\virtual-machines\linux\sizes.md).

* Zadejte název skupiny požadovaných prostředků a umístění.  
Pokud vaší oblasti a **instance velikost** podporuje dostupnost zóny **dostupnost zóny** pole je přístupné. Další informace o dostupnosti zón najdete [přehled](../availability-zones/az-overview.md) článku.

* Zadejte popisek názvu vaší požadované domény (základ plně kvalifikovaný název domény pro vyrovnávání zatížení před byly sadou škálování).  
Tento popisek musí být jedinečný v rámci všech Azure.

* Zvolte si image disku požadovaný operační systém, počet instancí a velikost počítače.

* Vyberte typ požadovaný disk: spravované nebo nespravované.  
Další informace najdete v tématu [této dokumentace](./virtual-machine-scale-sets-managed-disks.md). Pokud jste vybrali možnost sad škálování rozmístěny v několika skupinách pro umístění, tato možnost nebude dostupná, protože je vyžadován pro sady škálování rozložit umístění skupin spravovaných disků.

* Povolit nebo zakázat automatické škálování a konfigurace, pokud je povoleno.

![portál sady škálování virtuálního počítače Azure vytvořit výzvy](./media/virtual-machine-scale-sets-portal-create/portal-create.png)

## <a name="connect-to-a-vm-in-the-scale-set"></a>Připojit k virtuálnímu počítači v sadě škálování
Pokud jste zvolili pro omezení vaší škálování nastavit do jednoho umístění skupiny, pak byly sadou škálování se nasadí s nakonfigurovaná na umožňují připojit se ke stupnici snadno nastavit (v opačném případě se připojit k virtuálním počítačům v sadě škálování, je pravděpodobně potřeba vytvořit jumpbox ve stejné pravidla NAT  virtuální síť jako sada škálování). Chcete-li vidět, přejděte na `Inbound NAT Rules` kartě Nástroje pro vyrovnávání zatížení pro škálovací sadu:

![pravidla nat portálu sady škálování virtuálního počítače Azure](./media/virtual-machine-scale-sets-portal-create/portal-nat-rules.png)

Pro každý virtuální počítač ve škálovací nastavit pomocí těchto pravidel NAT můžete připojit. Pro instanci pro sadu škálování systému Windows, pokud je pravidlo NAT na příchozí port 50000, můžete se připojit k tomuto počítači pomocí protokolu RDP na `<load-balancer-ip-address>:50000`. Pro sadu škálování Linux byste připojili pomocí příkazu `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Další kroky
Dokumentace k nasazení škálování nastaví z rozhraní příkazového řádku, najdete v části [této dokumentace](virtual-machine-scale-sets-cli-quick-create.md).

Dokumentace k nasazení škálování nastaví z prostředí PowerShell, najdete v části [této dokumentace](virtual-machine-scale-sets-windows-create.md).

Dokumentace k nasazení škálování nastaví ze sady Visual Studio, najdete v části [této dokumentace](virtual-machine-scale-sets-vs-create.md).

Pro obecnou dokumentací, podívejte se [stránky přehled dokumentace pro sady škálování](virtual-machine-scale-sets-overview.md).

Obecné informace, podívejte se [hlavní cílová stránka pro sady škálování](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

