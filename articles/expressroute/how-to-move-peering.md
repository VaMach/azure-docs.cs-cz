---
title: "Přesunout veřejný partnerský vztah na Azure ExpressRoute pro partnerský vztah Microsoftu | Microsoft Docs"
description: "Tento článek popisuje kroky pro přesun váš veřejný partnerský vztah Microsoftu partnerského vztahu ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/09/2017
ms.author: cherylmc
ms.openlocfilehash: f48e270054ba45211c2a1517593d5d7b22925d0a
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Přesunout veřejný partnerský vztah na partnerský vztah Microsoftu

ExpressRoute podporuje použití Microsoft partnerský vztah s filtry tras pro služby Azure PaaS, jako je Azure storage a Azure SQL Database. Teď musíte pouze jedné domény směrování pro přístup k Microsoft PaaS a služby SaaS. Můžete selektivně inzerování předpon služby PaaS pro oblasti, které chcete používat filtry tras.

Tento článek vám umožňuje přesun konfigurace veřejného partnerského vztahu Microsoft partnerský vztah bez výpadků. Další informace o směrování doménách a partnerských vztahů najdete v tématu [ExpressRoute okruhy a domény směrování](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> Chcete-li použít partnerský vztah Microsoftu musí mít doplněk ExpressRoute premium. Další informace o doplňku premium najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md#expressroute-premium).

## <a name="before"></a>Než začnete

* Pro připojení k partnerský vztah Microsoftu, budete muset nastavit a spravovat adres (NAT) Poskytovatel připojení může nastavit a spravovat NAT jako spravovanou službu. Pokud máte v úmyslu přístup k Azure PaaS a služby Azure SaaS na partnerský vztah Microsoftu, je potřeba velikost fondu IP adres NAT správně. Další informace o NAT pro ExpressRoute najdete v tématu [požadavky NAT pro partnerský vztah Microsoftu](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Pokud aktuálně máte síti seznam řízení přístupu (ACL) pro prostředek služby Azure PaaS, který již existuje v veřejný partnerský vztah Azure, musíte zajistit, že fond IP adres NAT **rozsah adres** je zahrnuta v seznamu ACL, brány Firewall pro Microsoft partnerský vztah.

* Chcete-li přesunout do bez výpadků partnerský vztah Microsoftu, postupujte podle kroků v tomto článku v pořadí, které se mají zobrazovat.

## <a name="create"></a>1. Vytvoření partnerského vztahu Microsoftu

Pokud nebyl vytvořen partnerský vztah Microsoftu, použijte kterýkoli z následujících článků vytvoření partnerského vztahu Microsoftu. Pokud se vaše připojení poskytovatele nabízí spravované vrstvy 3 služby, požádejte poskytovatele připojení umožňující partnerský vztah Microsoftu pro váš okruh.

  * [Vytvoření partnerského vztahu Microsoftu pomocí portálu Azure](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Vytvoření partnerského vztahu Microsoftu pomocí Azure Powershell](expressroute-howto-routing-arm.md#msft)
  * [Vytvoření partnerského vztahu Microsoftu pomocí rozhraní příkazového řádku Azure](howto-routing-cli.md#msft)

## <a name="validate"></a>2. Ověření Microsoft partnerský vztah je povoleno

Ověřte, zda partnerského vztahu Microsoftu je povolená a inzerované předpony veřejných jsou ve stavu nakonfigurované.

  * [Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Azure CLI](howto-routing-cli.md#getmsft)

## <a name="routefilter"></a>3. Nakonfigurujete a připojíte k okruhu filtr trasy

Ve výchozím nastavení nové partnerského vztahu Microsoftu neprovádějí ohlášení všechny předpony dokud trasy filtr je připojen k okruhu. Když vytvoříte pravidlo filtru trasy, můžete určit seznam komunit služby pro oblasti Azure, které chcete používat pro služby Azure PaaS, jak je znázorněno na následujícím snímku obrazovky:

![Sloučení veřejného partnerského vztahu](.\media\how-to-move-peering\public.png)

Nastavit filtry tras pomocí některého z následujících článků:

  * [Nastavit filtry směrování pro partnerský vztah Microsoftu pomocí portálu Azure](how-to-routefilter-portal.md)
  * [Nastavit filtry směrování pro partnerský vztah Microsoftu pomocí Azure PowerShell](how-to-routefilter-powershell.md)
  * [Nastavit filtry směrování pro partnerský vztah Microsoftu pomocí rozhraní příkazového řádku Azure](how-to-routefilter-cli.md)

## <a name="delete"></a>4. Odstranění veřejného partnerského vztahu

Po ověření, že partnerského vztahu Microsoftu je nakonfigurována a předpony, které chcete spotřebovávat jsou správně inzerované na partnerský vztah Microsoftu, můžete pak odstranění veřejného partnerského vztahu. Odstranění veřejného partnerského vztahu, použijte některou z následujících článků:

  * [Odstranění veřejného partnerského vztahu Azure pomocí portálu Azure](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Odstranění veřejného partnerského vztahu Azure pomocí Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)
  * [Odstranění veřejného partnerského vztahu Azure pomocí rozhraní příkazového řádku](howto-routing-cli.md#deletepublic)

## <a name="next-steps"></a>Další kroky

Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).