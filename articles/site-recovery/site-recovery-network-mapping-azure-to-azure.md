---
title: "Mapování sítě mezi dvěma oblastmi Azure ve službě Azure Site Recovery | Microsoft Docs"
description: "Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů. Další informace o převzetí služeb při selhání do Azure nebo do sekundárního datacentra."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 9d6a806ec533259797080fbfee2c38f918ebd8a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Mapování sítě mezi dvěma oblastmi Azure


Tento článek popisuje, jak k mapování virtuální sítě Azure dvou oblastí Azure mezi sebou. Mapování sítě zajišťuje, když replikovaného virtuálního počítače je vytvořen v cílové oblasti Azure, je vytvořený ve virtuální síti, který je namapovaný na virtuální sítě zdrojového virtuálního počítače.  

## <a name="prerequisites"></a>Požadavky
Předtím, než je mapovat sítě, ujistěte se, jste vytvořili [virtuálních sítí Azure](../virtual-network/virtual-networks-overview.md) v obou zdroje a cíle oblastech Azure.

## <a name="map-networks"></a>Mapování sítě

Chcete-li jinou virtuální sítí v jiné oblasti mapovat virtuální síť Azure v jedné oblasti Azure, přejděte infrastruktura Site Recovery -> mapování sítě (pro virtuální počítače Azure) a vytvořit mapování sítě.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Virtuální počítač v následujícím příkladu je spuštěná ve východní Asie oblasti a je právě replikován pro jihovýchodní Asie.

Vyberte zdrojové a cílové sítě a potom klikněte na tlačítko OK vytvoření mapování sítě z východní Asie k jihovýchodní Asie.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


To samé vytvoření mapování sítě z jihovýchodní Asie k východní Asie.  
![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Mapování sítě při povolení replikace

Pokud mapování sítě neuděláte, když se replikace virtuálního počítače pro první čas formuláře jedné oblasti Azure do jiného, můžete cílové sítě v rámci stejného procesu. Site Recovery vytvoří mapování sítě z oblasti zdrojové do cílové oblasti a cílová oblast zdroj oblasti založené na tomto výběru.   

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Ve výchozím nastavení, Site Recovery vytvoří síť v cílové oblasti, kde je stejný jako ke zdrojové síti a přidáním '-automatické obnovení systému ' jako příponu na název zdrojové síti. Kliknutím na tlačítko Přizpůsobit můžete již vytvořené síťové.

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Pokud již probíhá mapování sítě, nelze změnit cíl virtuální síť při povolení replikace. Chcete-li ji změnit, upravte existující mapování sítě.  

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Mapování sítě](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Pokud změníte mapování sítě z oblasti-1 na 2 oblasti, nezapomeňte že upravit mapování sítě z oblasti 2 oblasti-1 také.
>
>


## <a name="subnet-selection"></a>Výběr podsítě
Podsíť cílového virtuálního počítače je vybrána na základě názvu podsíti zdrojového virtuálního počítače. Pokud je k dispozici v cílové síti podsíť se stejným názvem jako zdrojový virtuální počítač, pak, je zvolen pro cílový virtuální počítač. Pokud neexistuje žádná podsíť s tímto názvem v cílové síti, pak abecedně první podsíť je zvolen jako cílové podsíti. Tato podsíť můžete upravit tak, že přejdete na výpočty a síť nastavení virtuálního počítače.

![Upravit podsíť](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP adresa

IP adresa pro každé síťové rozhraní cílového virtuálního počítače je zvolen následujícím způsobem:

### <a name="dhcp"></a>DHCP
Síťové rozhraní zdrojový virtuální počítač používá protokol DHCP, pak síťové rozhraní cílového virtuálního počítače je také nastavena jako DHCP.

### <a name="static-ip"></a>Statická IP adresa
Pokud síťové rozhraní zdrojový virtuální počítač používá statickou IP adresu, pak síťové rozhraní cílový virtuální počítač také nastaven na používá statickou IP adresu. Statická IP adresa je zvolen následujícím způsobem:

#### <a name="same-address-space"></a>Stejné adresní prostor

Podsíť zdrojové a cílové podsíti mají stejné adresní prostor, cílová IP adresa je nastavit stejné jako IP adresa síťového rozhraní zdrojového virtuálního počítače. Pokud není k dispozici stejnou IP Adresou, některé dostupnou IP adresu nastavena jako cílová IP adresa.

#### <a name="different-address-space"></a>Jiným adresním prostorem

Pokud podsíť zdrojové a cílové podsíti jiným adresním prostorem, cílová IP adresa je nastaven jako dostupnou IP adresu v cílové podsíti.

Cílová IP adresa na každé rozhraní sítě můžete upravit tak, že přejdete do nastavení výpočty a síť virtuálního počítače.

## <a name="next-steps"></a>Další kroky

- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
