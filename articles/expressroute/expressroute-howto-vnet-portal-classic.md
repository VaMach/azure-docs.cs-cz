---
title: "Konfigurace virtuální sítě a brány na portálu classic pro ExpressRoute | Microsoft Docs"
description: "Tento článek vás provede procesem nastavení virtuální sítě pro ExpressRoute pomocí modelu nasazení classic a portálu classic."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: f62254b2a7df50aa55a2a49009702848a9aecebd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Vytvoření virtuální sítě pro ExpressRoute na portálu classic
Kroky v tomto článku vás provede konfiguraci virtuální sítě a brány virtuální sítě pro použití s ExpressRoute pomocí modelu nasazení classic a portálu classic.

Pokud hledáte pokyny k modelu nasazení Resource Manager, můžete použít v následujících článcích: [vytvoření virtuální sítě pomocí prostředí PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) a [přidat bránu VPN do virtuální sítě Resource Manageru pro ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Vytvoření klasické virtuální sítě a brány
Následující postup vytvoření klasické virtuální sítě a brány virtuální sítě. Pokud již máte klasické virtuální síti, najdete v článku [nakonfigurovat existující klasické virtuální síti](#config) v tomto článku.

1. Přihlaste se do [portálu Azure Classic](http://manage.windowsazure.com).
2. V levém dolním rohu obrazovky klikněte na **nový**. V podokně navigace klikněte na **Network Services**, a poté klikněte na **Virtual Network**. Kliknutím na **Vytvořit vlastní** spustíte průvodce konfigurací.
3. Na **podrobnosti virtuální sítě** stránky, zadejte následující:
   
   * **Název** – název virtuální sítě. Tento název virtuální sítě budete používat při nasazení vaší virtuálních počítačů a instancí PaaS, proto nemusí chcete nenastavujte příliš komplikovaný.
   * **Umístění** – umístění přímo souvisí s fyzickým umístěním (oblastí), kde chcete prostředky (virtuální počítače) jsou umístěny. Například pokud chcete, aby virtuální počítače, které do této virtuální sítě nasadíte, byly fyzicky umístěné ve východní USA, vyberte toto umístění. Oblast přidruženou k virtuální síti po jejím vytvoření nedá změnit.
4. Na stránce **Servery DNS a připojení VPN** zadejte následující informace, a poté klikněte na šipku Další vpravo dole. 
   
   * **Servery DNS** – zadejte název serveru DNS a IP adresu, nebo vyberte dříve zaregistrovaný server DNS z místní nabídky. Toto nastavení neslouží k vytvoření serveru DNS. Umožňuje určit server DNS, který chcete použít pro překlad názvů pro tuto virtuální síť.
   * **Připojení Site-to-Site** – zaškrtávací políčko pro **konfigurace VPN typu site-to-site**.
   * **ExpressRoute** – zaškrtávací políčko **pomocí ExpressRoute**. Tato možnost se zobrazí, pouze pokud jste vybrali **konfigurace VPN typu Site-to-Site**.
   * **Místní sítě** -je nutné mít místní síťový web pro ExpressRoute. Ale v případě připojení ExpressRoute, předpony zadané pro místní síťový web, bude ignorována. Místo toho se použije pro účely směrování předpony inzerované Microsoftu prostřednictvím okruhu ExpressRoute.<BR>Pokud již máte místní síť vytvořenou pro připojení ExpressRoute, můžete ji vyberte z rozevíracího seznamu. Pokud ne, vyberte **zadejte novou místní síť**.
5. **Připojení Site-to-Site** stránka se zobrazí, pokud jste vybrali v předchozím kroku zadat novou místní síť. Ke konfiguraci vaší místní síti, zadejte následující informace a klikněte na šipku Další. 
   
   * **Název** -název, který chcete použít na místní (místní) Síťová lokalita.
   * **Adresní prostor** – včetně počáteční IP adresu a CIDR (počet adres). Můžete zadat libovolný rozsah adres, tak dlouho, dokud se nepřekrývá s rozsah adres pro virtuální síť. Obvykle to by zadejte rozsahy adres pro místní sítě, ale v případě ExpressRoute, nejsou tato nastavení použít. Toto nastavení je však vyžadováno k vytvoření místní sítě, při použití portálu classic.
   * **Přidání adresního prostoru** – toto nastavení není relevantní pro ExpressRoute.
6. Na **adresní prostory virtuální sítě** stránky, zadejte následující informace a klikněte na značku zaškrtnutí vpravo dole nakonfigurujete svoji síť. 
   
   * **Adresní prostor** – včetně počáteční IP a počet adres. Ověřte, že zadané adresní prostory nepřekrývají s adresní prostory, které máte v místní síti.
   * **Přidání podsítě** – včetně počáteční IP adresu a počet adres. Dodatečné podsítě nejsou vyžadovány.
   * **Přidání podsítě brány** – klikněte na tlačítko Přidat podsíť brány. Podsíť brány se používá pouze pro bránu virtuální sítě a pro tuto konfiguraci je vyžadována.<BR>Podsíť brány CIDR (počet adres) pro ExpressRoute musí být velikosti/28 nebo větší (/ 27, / 26 atd.). To umožňuje dost IP adres v této podsíti umožňující konfigurace fungovat. Na portálu classic Pokud jste zaškrtli políčko použít ExpressRoute, portálu určuje podsíť brány s velikosti/28.  Nelze upravit počet adres CIDR portálu classic. Podsíť brány se zobrazí jako **brány** portálu classic, i když skutečné název podsítě brány, který se vytvoří ve skutečnosti je **GatewaySubnet**. Tento název můžete zobrazit pomocí prostředí PowerShell nebo portálu Azure.
7. Kliknutím na značku zaškrtnutí v dolní části stránky zahájíte vytváření virtuální sítě. Po dokončení se zobrazí **vytvořen** uvedené v části **stav** na **sítě** na portálu classic.

## <a name="gw"></a>Vytvoření brány
1. Na **sítě** stránky, klikněte na virtuální síť, kterou jste právě vytvořili a pak klikněte na tlačítko **řídicí panel** v horní části stránky.
2. V dolní části **řídicí panel** klikněte na tlačítko **vytvořit bránu** a vyberte **dynamické směrování**. Klikněte na tlačítko **Ano** potvrďte, že chcete vytvořit bránu.
3. Při spuštění vytváření brány se zobrazí zpráva informací, víte, že byla spuštěna brána. To může trvat až 45 minut pro vytvoření brány.
4. Odkaz sítě k okruhu. Postupujte podle pokynů v článku [způsob propojení virtuálních sítí pro okruhy ExpressRoute](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurace existující klasické virtuální sítě pro ExpressRoute
Pokud již máte klasické virtuální sítě, můžete nakonfigurovat na portálu classic připojení expressroute. Nastavení stejné jako v částech výše, jsou, takže pročtěte těchto částí se seznámit s požadovaná nastavení. Pokud chcete vytvořit koexistujících připojení ExpressRoute nebo Site-to-Site, přečtěte si téma [v tomto článku](expressroute-howto-coexist-classic.md) kroky. Jsou jiné než kroky v tomto článku.

1. Budete muset vytvořit místní sítě, než aktualizujete zbývající nastavení virtuální sítě. Chcete-li vytvořit novou místní síť, která se vyžaduje při konfiguraci ExpressRoute prostřednictvím portálu classic, klikněte na tlačítko **nový**  **>**  **síťové služby**  **>**  **virtuální sítě**  **>**  **místní sítě přidat**. Postupujte podle kroků v průvodci vytvořit místní sítě.
2. Použití **konfigurace** stránku aktualizovat zbytek nastavení pro virtuální síť a přidružte virtuální sítě k místní síti.
3. Po konfiguraci nastavení, přejděte na [vytvoření brány](#gw) tohoto článku, chcete-li vytvořit bránu.

## <a name="next-steps"></a>Další kroky
* Pokud chcete přidat virtuální počítače k virtuální síti, najdete v části [virtuální počítače kurzů](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Pokud chcete získat další informace o ExpressRoute, přečtěte si téma [přehled ExpressRoute](expressroute-introduction.md).

