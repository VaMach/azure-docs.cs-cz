---
title: "Připojení z virtuální sítě do Azure Data Lake Store | Microsoft Docs"
description: "Připojení k Azure Data Lake Store z virtuálních sítí Azure"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.openlocfilehash: 029939eabc7283d7591576fa1b14731a9e626a88
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a>Přístup k Azure Data Lake Store z virtuálních počítačů v rámci síť Azure
Azure Data Lake Store je PaaS služba, která běží na veřejné internetové IP adresy. Jakýkoli server, který se může připojit k veřejnému Internetu může obvykle připojit k Azure Data Lake Store také koncové body. Ve výchozím nastavení všechny virtuální počítače, které jsou v sítě Azure Vnet můžete přístup k Internetu a proto můžete přístup k Azure Data Lake Store. Nicméně je možné nakonfigurovat virtuální počítače ve virtuální síti není mít přístup k Internetu. Pro tyto virtuální počítače přístup k Azure Data Lake Store je omezený také. Blokování veřejný přístup k Internetu pro virtuální počítače ve virtuálních sítí Azure lze provést pomocí kteréhokoli následující postup.

* Podle konfigurace skupin zabezpečení sítě (NSG)
* Nakonfigurováním uživatele definované trasy (UDR)
* Při výměně tras přes protokol BGP (odvětví standardní protokol dynamického směrování) ExpressRoute je při použití tohoto bloku přístup k Internetu

V tomto článku se dozvíte, jak povolit přístup k Azure Data Lake Store z virtuálních počítačů Azure, které byly omezený přístup k prostředkům pomocí jedné ze tří metod uvedené výše.

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a>Povolení připojení k Azure Data Lake Store z virtuálních počítačů s připojením s omezeným přístupem
Pro přístup k Azure Data Lake Store z těchto virtuálních počítačů, je nutné nakonfigurovat jejich přístupu k IP adresu, kde je k dispozici účet Azure Data Lake Store. Poznáte IP adresy pro vaše účty Data Lake Store pomocí překladu názvů DNS vaše účty (`<account>.azuredatalakestore.net`). To můžete použít nástroje, jako **nslookup**. Otevřete příkazový řádek ve vašem počítači a spusťte následující příkaz.

    nslookup mydatastore.azuredatalakestore.net

Výstup vypadá přibližně takto. Hodnota proti **adresu** vlastnost je IP adresa spojená s vaším účtem Data Lake Store.

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a>Povolení připojení z virtuálních počítačů omezený pomocí NSG
Pokud pravidla NSG se používá k blokování přístupu k Internetu, můžete vytvořit další NSG, která umožňuje přístup k IP adrese Data Lake Store. Další informace o pravidla NSG je k dispozici na [co je skupina zabezpečení sítě?](../virtual-network/virtual-networks-nsg.md). Pokyny o tom, jak vytvářet skupiny Nsg najdete v tématu [Správa skupin Nsg pomocí portálu Azure](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a>Povolení připojení z virtuálních počítačů omezený pomocí UDR nebo ExpressRoute
Pokud trasy, udr nebo trasy protokolu BGP vyměňují se používá k blokování přístupu k Internetu, je potřeba nakonfigurovat tak, aby virtuální počítače v těchto podsítích přístup koncových bodů Data Lake Store speciální trasy. Další informace najdete v tématu [co jsou trasy definované uživatelem?](../virtual-network/virtual-networks-udr-overview.md). Pokyny pro vytvoření udr, najdete v části [udr vytvořit ve službě Správce prostředků](../virtual-network/virtual-network-create-udr-arm-ps.md).

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a>Povolení připojení z virtuálních počítačů omezený pomocí ExpressRoute
Když je nakonfigurovaná okruh ExpressRoute, místní servery přistupovat k Data Lake Store prostřednictvím veřejného partnerského vztahu. Další informace o konfiguraci ExpressRoute pro veřejný partnerský vztah je k dispozici na [nejčastější dotazy k ExpressRoute](../expressroute/expressroute-faqs.md).

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Store](data-lake-store-overview.md)
* [Zabezpečení dat uložených v Azure Data Lake Store](data-lake-store-security-overview.md)

