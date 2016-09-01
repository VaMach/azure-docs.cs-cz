<properties
   pageTitle="Postup konfigurace směrování pro okruh ExpressRoute pomocí webu Azure Portal | Microsoft Azure"
   description="Tento článek vás provede kroky pro vytváření a zřizování soukromého a veřejného partnerského vztahu a partnerského vztahu Microsoftu okruhu ExpressRoute. Tento článek také ukazuje, jak kontrolovat stav partnerských vztahů pro váš okruh, aktualizovat je nebo je odstranit."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/19/2016"
   ms.author="cherylmc"/>

# Vytvoření a úprava směrování pro okruh ExpressRoute



> [AZURE.SELECTOR]
[Azure Portal – Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell – Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell – Classic](expressroute-howto-routing-classic.md)



Tento článek vás provede kroky k vytvoření a správě konfigurace směrování pro okruhu ExpressRoute pomocí webu Azure Portal a modelu nasazení Resource Manager.

**O modelech nasazení Azure**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Předpoklady konfigurace

- Před zahájením konfigurace se ujistěte, že jste si přečetli stránku s [předpoklady](expressroute-prerequisites.md), stránku s [požadavky směrování](expressroute-routing.md) a stránku s [pracovními postupy](expressroute-workflows.md).
- Musí mít aktivní okruh ExpressRoute. Než budete pokračovat, podle pokynů [vytvořte okruh ExpressRoute](expressroute-howto-circuit-arm.md) a mějte ho povolený vaším poskytovatelem připojení. Abyste mohli spouštět rutiny popsané dál, musí být okruh ExpressRoute zřízený a povolený.

Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb nabízejícími služby připojení vrstvy 2. Pokud používáte poskytovatele služeb nabízejícího spravované služby vrstvy 3 (obvykle IPVPN, např. MPLS), poskytovatel připojení provede konfiguraci a správu směrování za vás. 


>[AZURE.IMPORTANT] Aktuálně prostřednictvím portálu pro správu služeb neinzerujeme partnerské vztahy nakonfigurované poskytovateli služeb. Pracujeme na tom, aby tato možnost brzy fungovala. Před konfigurací partnerských vztahů protokolu BGP se poraďte se svým poskytovatelem služeb.

Můžete nakonfigurovat jeden, dva nebo všechny tři partnerské vztahy (soukromý Azure, veřejný Azure a Microsoft) pro okruh ExpressRoute. Partnerské vztahy můžete konfigurovat v libovolném pořadí. Musíte se ale přesvědčit, že jste vždy konfiguraci každého partnerského vztahu dokončili. 

## Soukromý partnerský vztah Azure

Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci soukromého partnerského vztahu Azure pro okruh ExpressRoute. 

### Vytvoření soukromého partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute. Než budete pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Nakonfigurujte soukromý partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:

    - Podsíť /30 pro primární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
    - Podsíť /30 pro sekundární propojení. Nesmí být součástí žádného adresního prostor vyhrazeného pro virtuální sítě.
    - Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
    - Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS. Pro tento partnerský vztah můžete použít soukromé číslo AS. Zkontrolujte, že nepoužíváte 65515.
    - Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**


3. Vyberte řádek pro soukromý partnerský vztah Azure, jak je zobrazeno dál.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
    

4. Nakonfigurujte soukromý partnerský vztah. Následující obrázek ukazuje příklad konfigurace.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

    
5. Po zadání všech parametrů uložte konfiguraci. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)
    

### Zobrazení podrobností soukromého partnerského vztahu Azure

Vlastnosti soukromého partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)


### Aktualizace konfigurace soukromého partnerského vztahu Azure

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### Odstranění soukromého partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)


## Veřejný partnerský vztah Azure

Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci veřejného partnerského vztahu Azure pro okruh ExpressRoute. 

### Vytvoření veřejného partnerského vztahu Azure

1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Nakonfigurujte veřejný partnerský vztah Azure pro okruh. Před zahájením dalších kroků se ujistěte, že máte k dispozici následující položky:

    - Podsíť /30 pro primární propojení. 
    - Podsíť /30 pro sekundární propojení. 
    - Všechny IP adresy použité k nastavení tohoto partnerského vztahu musí být platné IPv4 adresy.
    - Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
    - Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
    - Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**

3. Vyberte řádek pro veřejný partnerský vztah Azure, jak je zobrazeno dál.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
    

4. Nakonfigurujte veřejný partnerský vztah. Následující obrázek ukazuje příklad konfigurace.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

    
5. Po zadání všech parametrů uložte konfiguraci. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)
    

### Zobrazení podrobností veřejného partnerského vztahu Azure

Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)


### Aktualizace konfigurace veřejného partnerského vztahu Azure

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### Odstranění veřejného partnerského vztahu Azure

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)


## Partnerský vztah Microsoftu

Tato část obsahuje pokyny, jak vytvořit, získat, aktualizovat a odstranit konfiguraci partnerského vztahu Microsoftu pro okruh ExpressRoute. 

### Vytvoření partnerského vztahu Microsoftu

1. Nakonfigurujte okruh ExpressRoute. Než budete dál pokračovat, ujistěte se, že je okruh poskytovatelem připojení plně zřízený.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Nakonfigurujte partnerský vztah Microsoftu pro okruh. Před pokračováním se ujistěte, že máte k dispozici následující informace.

    - Podsíť /30 pro primární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
    - Podsíť /30 pro sekundární propojení. Musí se jednat o platnou předponu veřejné IPv4 adresy, kterou vlastníte a která je registrovaná u RIR/IRR.
    - Platné ID sítě VLAN, na kterém se má partnerský vztah vytvořit. Zajistěte, aby žádný jiný partnerský vztah v okruhu nepoužíval stejné ID sítě VLAN.
    - Číslo AS pro partnerský vztah. Můžete použít 2bajtová i 4bajtová čísla AS.
    - **Inzerované předpony:** Musíte poskytnout seznam všech předpon, které plánujete inzerovat přes relaci protokolu BGP. Přijímají se jenom předpony veřejných IP adres. Pokud chcete odeslat sadu předpon, můžete odeslat seznam oddělený čárkami. Tyto předpony musí být v RIR/IRR zaregistrované na vás.
    - **Zákaznické číslo ASN:** Pokud inzerujete předpony, které nejsou registrované na číslo AS partnerského vztahu, můžete zadat číslo AS, na které jsou registrované. **Tato položka je nepovinná.**
    - **Název registru směrování:** Můžete zadat RIR/IRR, kde jsou předpony a číslo AS registrované. **Tato položka je nepovinná.**
    - Hodnota hash MD5, pokud se ji rozhodnete použít. **Tato položka je nepovinná.**
    
3. Můžete vybrat partnerský vztah, který chcete nakonfigurovat, jak je zobrazeno dál. Vyberte řádek partnerského vztahu Microsoftu.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
    

4.  Nakonfigurujte partnerský vztah Microsoftu. Následující obrázek ukazuje příklad konfigurace.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)

    
5. Po zadání všech parametrů uložte konfiguraci. 

    Pokud se váš okruh dostane do stavu potřeby ověření (jak je zobrazeno dál), musíte otevřít lístek podpory, abyste ukázali důkaz vlastnictví předpon našemu týmu podpory.  
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)


    Lístek podpory můžete otevřít přímo z portálu, jak je zobrazeno dál.   
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


6. Jakmile je konfigurace úspěšně přijatá, zobrazí se něco podobného jako v příkladu dál.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)
    

### Zobrazení podrobností partnerského vztahu Microsoftu

Vlastnosti veřejného partnerského vztahu Azure můžete zobrazit výběrem partnerského vztahu.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)


### Aktualizace konfigurace partnerského vztahu Microsoftu

Můžete vybrat řádek pro partnerský vztah a upravit vlastnosti partnerského vztahu. 


![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)


### Odstranění partnerského vztahu Microsoftu

Konfiguraci partnerského vztahu můžete odebrat výběrem ikony odstranění podle uvedeného obrázku.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)



## Další kroky

Dalším krokem je [Propojení virtuální sítě s okruhem ExpressRoute](expressroute-howto-linkvnet-arm.md).

-  Další informace o pracovních postupech ExpressRoute najdete v tématu [Pracovní postupy ExpressRoute](expressroute-workflows.md).

-  Další informace o partnerském vztahu okruhu najdete v tématu [Okruhy ExpressRoute a domény směrování](expressroute-circuit-peerings.md).

-  Další informace o práci s virtuálními sítěmi najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md).




<!---HONumber=Aug16_HO4-->


