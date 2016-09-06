<properties
   pageTitle="Vytvoření partnerského vztahu virtuálních sítí pomocí webu Azure Portal | Microsoft Azure"
   description="Naučte se vytvořit virtuální síť pomocí webu Azure Portal v nástroji Resource Manager."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/02/2016"
   ms.author="narayanannamalai"/>

# Vytvoření partnerského vztahu virtuálních sítí pomocí webu Azure Portal

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Pokud chcete navázat partnerský vztah virtuální sítě podle výše uvedeného scénáře pomocí webu Azure Portal, použijte následující postup.

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. K navázání partnerského vztahu virtuální sítě musíte mezi virtuálními sítěmi vytvořit dvě propojení, jedno pro každý směr. Nejprve můžete vytvořit partnerské propojení virtuálních sítí ze sítě VNET1 do sítě VNET2. Na portálu klikněte na **Procházet** > **zvolte Virtuální sítě** 

    ![Vytvoření partnerského vztahu virtuální sítě na webu Azure Portal](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)

3. V okně Virtuální sítě zvolte položku VNET1, klikněte na Partnerské vztahy a potom na tlačítko Přidat.

    ![Zvolte partnerství](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)

4. V okně Přidat partnerský vztah pojmenujte partnerské propojení LinkToVnet2, vyberte předplatné a partnerskou virtuální síť VNET2 a klikněte na tlačítko OK.

    ![Propojení virtuální sítě](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)

5. Jakmile je toto partnerské propojení virtuálních sítí vytvořeno, uvidíte následující stav propojení:

    ![Stav propojení](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)

6. Jako další krok vytvořte partnerské propojení virtuálních sítí ze sítě VNET2 do sítě VNET1. V okně Virtuální sítě zvolte položku VNET2, klikněte na Partnerské vztahy a potom na tlačítko Přidat. 

    ![Partnerský vztah z jiné virtuální sítě](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)

7. V okně Přidat partnerský vztah pojmenujte partnerské propojení LinkToVnet1, vyberte předplatné a partnerskou virtuální síť a klikněte na tlačítko OK.

    ![Dlaždice Vytváření virtuální sítě](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)

8. Jakmile je toto partnerské propojení virtuálních sítí vytvořeno, uvidíte následující stav propojení:

    ![Konečný stav propojení](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)

9. Zkontrolujte stav pro propojení LinkToVnet2. Ten se teď taky změní na Připojeno.  

    ![Konečný stav propojení 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)

10. POZNÁMKA: Partnerský vztah virtuálních sítí je navázán pouze tehdy, pokud jsou připojena obě propojení. 

Pro každé propojení lze nastavit několik vlastností:

|Možnost|Popis|Výchozí|
|:-----|:----------|:------|
|AllowVirtualNetworkAccess|Určuje, jestli má být adresní prostor partnerské virtuální sítě má zahrnutý jako součást značky Virtual_network.|Ano|
|AllowForwardedTraffic|Určuje, jestli bude přijata nebo zahozena komunikace, která nemá původ přímo z partnerské virtuální sítě.|Ne|
|AllowGatewayTransit|Umožňuje určit, jestli může partnerská virtuální síť používat bránu vaší virtuální sítě.|Ne|
|UseRemoteGateways|Určuje, jestli se má používat brána partnerské virtuální sítě. Partnerská virtuální síť musí mít nakonfigurovanou bránu a dále musí být vybraná možnost AllowGatewayTransit. Pokud vaše síť již má nakonfigurovanou bránu, tuto možnost nelze použít.|Ne|

Každé propojení v partnerském vztahu virtuálních sítí má sadu výše uvedených vlastností. V prostředí portálu můžete kliknout na propojení partnerského virtuální sítě a změnit libovolné dostupné nastavení. Potom klikněte na tlačítko Uložit, aby se změny projevily.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2. V tomto příklad použijeme dvě předplatné A a B a dva uživatele UserA a UserB s odpovídajícími oprávněními v uvedeném pořadí
3. Na portálu klikněte na Procházet a zvolte Virtuální sítě. Klikněte na danou virtuální síť a pak na tlačítko Přidat.

    ![Procházení – Scénář 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)

4. V okně Přidat přístup klikněte na Vybrat roli a vyberte možnost Přispěvatel sítě. Klikněte na Přidat uživatele, zadejte přihlašovací jméno uživatele UserB a klikněte na OK.

    ![RBAC](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)

    Toto není povinné. Partnerský vztah lze navázat i v případě, že uživatelé vytvoří požadavky na partnerský vztah pro své virtuální sítě jednotlivě, pokud se jejich požadavky shodují. Když přidáte privilegované uživatele z druhé virtuální sítě jako uživatele místní virtuální sítě , budete mít nastavení na portálu snazší. 

5. Potom se přihlaste k webu Azure Portal s účtem uživatele UserB, který je privilegovaným uživatelem v předplatném SubscriptionB. Výše uvedeným postupem přidejte uživatele UserA jako Přispěvatele sítě.

    ![RBAC2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)

    POZNÁMKA: Můžete se provést odhlásit a znovu přihlásit v obou uživatelských relací v prohlížeči, abyste se ujistili, že je autorizace povolená správně.

6. Přihlaste se k portálu jako UserA, přejděte do okna VNET3, klikněte na Partnerské vztahy, zaškrtněte políčko „Znám svoje ID prostředku“ a zadejte ID prostředku pro VNET5 v následujícím formátu.

    /subscriptions/<Subscription- ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/<VNET name>

    ![ID prostředku](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)

7. Přihlaste se k portálu jako UserB a výše uvedeným postupem vytvořte partnerské propojení ze sítě VNET5 do sítě VNET3. 

    ![ID prostředku 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)

8. Partnerský vztah bude vytvořen a jakýkoli virtuální počítač v síti VNet3 by měl být schopný komunikovat s libovolným virtuálním počítačem v síti VNet5

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Jako první krok partnerství sítě virtuálních sítí propojuje ze sítě HubVnet do sítě VNET1. Povšimněte si, že pro dané propojení není vybrána možnost „Povolit přesměrovaný přenos“.

    ![Základní partnerské vztahy](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)

2. Jako další krok je možné vytvořit partnerská propojení ze sítě VNET1 do sítě HubVnet. Povšimněte si, že je vybrána možnost „Povolit přesměrovaný přenos“. 

    ![Základní partnerské vztahy](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)

3. Po navázání partnerství si můžete projít tento [článek](virtual-network-create-udr-arm-ps.md) a definovat Uživatelem definovanou trasu (UDR) pro přesměrování přenosu dat sítě VNet1 prostřednictvím virtuálního zařízení, abyste využili jeho možností. Při zadávání adresy pro Další směrování v trase můžete jako adresu nastavit IP adresu virtuálního zařízení v partnerské virtuální síti HubVNet.

## Odebrání partnerského vztahu virtuálních sítí

1.  V prohlížeči přejděte na http://portal.azure.com a v případě potřeby se přihlaste pomocí účtu Azure.
2.  Přejděte do okna virtuální sítě, klikněte na Partnerské vztahy, klikněte na propojení, které chcete odebrat, a klikněte na tlačítko Odstranit. 

    ![Delete1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)

3. Po odebrání jednoho propojení v partnerském vztahu virtuálních sítí se stav partnerského propojení se bude zobrazovat jako Odpojeno.

    ![Delete2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)

4. V tomto stavu nelze toto propojení vytvořit znovu, dokud se stav partnerského propojení nezmění Iniciováno. Před opakovaným vytvořením partnerského vztahu virtuálních sítí doporučujeme nejdříve odebrat obě propojení. 



<!--HONumber=ago16_HO5-->


