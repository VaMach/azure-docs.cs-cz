---
title: "Vytvoření a úprava okruhu ExpressRoute: prostředí PowerShell: portál Azure classic | Microsoft Docs"
description: "Tento článek vás provede kroky pro vytváření a zřizování okruhu ExpressRoute. Tento článek také ukazuje, jak zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení váš okruh."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0134d242-6459-4dec-a2f1-4657c3bc8b23
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: ganesr;cherylmc
ms.openlocfilehash: 93ddc2975db34053c6a776d1c3b931536f3f8ec7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-modify-an-expressroute-circuit-using-powershell-classic"></a>Vytvoření a úprava okruhu ExpressRoute pomocí prostředí PowerShell (klasické)
> [!div class="op_single_selector"]
> * [Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure CLI](howto-circuit-cli.md)
> * [Video – portál Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (Classic)](expressroute-howto-circuit-classic.md)
>

Tento článek vás provede kroky k vytvoření okruhu Azure ExpressRoute pomocí rutin prostředí PowerShell a modelu nasazení classic. Tento článek také ukazuje, jak zkontrolovat stav, aktualizace nebo odstranění a zrušení zřízení okruhu ExpressRoute.

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]


**O modelech nasazení Azure**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Než začnete
### <a name="step-1-review-the-prerequisites-and-workflow-articles"></a>Krok 1. Přečtěte si požadavky a pracovní postup články
Ujistěte se, že jste si přečetli [požadavky](expressroute-prerequisites.md) a [pracovních](expressroute-workflows.md) před zahájením konfigurace.  

### <a name="step-2-install-the-latest-versions-of-the-azure-service-management-sm-powershell-modules"></a>Krok 2. Nainstalujte nejnovější verzi modulů prostředí PowerShell Azure Service Management (SM)
Postupujte podle pokynů v [Začínáme s rutinami prostředí Azure PowerShell](/powershell/azure/overview) podrobné pokyny ke konfiguraci počítače pro používání modulů prostředí Azure PowerShell.

### <a name="step-3-log-in-to-your-azure-account-and-select-a-subscription"></a>Krok 3. Přihlaste se k účtu Azure a vybrat odběr
1. Otevřete konzolu PowerShellu se zvýšenými oprávněními a připojte se ke svému účtu. Připojení vám usnadní následující ukázka:

        Login-AzureRmAccount

2. Zkontrolujte předplatná pro příslušný účet.

        Get-AzureRmSubscription

3. Máte-li více předplatných, vyberte předplatné, které chcete použít.

        Select-AzureSubscription -SubscriptionId "Replace_with_your_subscription_id"

4. Potvrďte, pokud u vybraného id předplatného je nastaven jako výchozí.

        Get-AzureSubscription -default

## <a name="create-and-provision-an-expressroute-circuit"></a>Vytvořit a zřídit okruhu ExpressRoute
### <a name="step-1-import-the-powershell-modules-for-expressroute"></a>Krok 1. Importovat další moduly Powershellu pro ExpressRoute
 Pokud jste tak již neučinili, je nutné naimportovat moduly Azure a ExpressRoute do relace prostředí PowerShell mohli začít používat rutiny pro ExpressRoute. Naimportovat moduly z umístění, které byly instalovány k v místním počítači. V závislosti na metodě, které jste použili pro instalaci modulů může být jiný než následující příklad ukazuje umístění. V příkladu podle potřeby upravte.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="step-2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>Krok 2. Získejte seznam podporovaných zprostředkovatelů, umístění a šířek pásma
Než vytvoříte okruh ExpressRoute, musíte seznam poskytovatelů podporovaných připojení, umístění a možnosti šířky pásma.

Rutiny prostředí PowerShell `Get-AzureDedicatedCircuitServiceProvider` vrátí tyto informace, které budete používat v dalších krocích:

    Get-AzureDedicatedCircuitServiceProvider

Zkontrolujte, pokud poskytovatel připojení se nezobrazí. Poznamenejte si následující informace vzhledem k tomu, že budete je potřebovat později při vytvoření okruhu:

* Name (Název)
* PeeringLocations
* BandwidthsOffered

Nyní jste připraveni vytvořit okruh ExpressRoute.         

### <a name="step-3-create-an-expressroute-circuit"></a>Krok 3. Vytvoření okruhu ExpressRoute
Následující příklad ukazuje, jak vytvořit 200 MB/s okruh ExpressRoute prostřednictvím Equinix ze Silicon Valley. Pokud používáte k jinému zprostředkovateli a různá nastavení, dosaďte tyto informace při zkontrolujte vaši žádost.

> [!IMPORTANT]
> Váš okruh ExpressRoute bude účtován od okamžiku, kdy se objeví klíč služby. Ujistěte se, při provádění této operace, pokud poskytovatel připojení je připraven ke zřízení okruhu.
> 
> 

Toto je požadavek příklad pro nový klíč služby:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Nebo, pokud chcete vytvořit okruh ExpressRoute s doplňkem premium, použijte následující příklad. Odkazovat [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) další podrobnosti o doplněk premium.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Odpověď bude obsahovat klíč služby. Podrobný popis všech parametrů získáte spuštěním následující:

    get-help new-azurededicatedcircuit -detailed

### <a name="step-4-list-all-the-expressroute-circuits"></a>Krok 4. Zobrazí seznam všech okruhy ExpressRoute
Můžete spustit `Get-AzureDedicatedCircuit` získat seznam všech okruhy ExpressRoute, které jste vytvořili:

    Get-AzureDedicatedCircuit

Odpověď bude vypadat přibližně jako v následujícím příkladu:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Tyto informace kdykoli můžete načíst pomocí `Get-AzureDedicatedCircuit` rutiny. Provádění volání bez parametrů jsou uvedeny všechny okruhů. Zobrazí se klíč služby v *klíč ServiceKey* pole.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Podrobný popis všech parametrů získáte spuštěním následující:

    get-help get-azurededicatedcircuit -detailed

### <a name="step-5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>Krok 5. Klíč služby poslat svého poskytovatele připojení pro zřizování
*ServiceProviderProvisioningState* poskytuje informace o aktuálním stavu zřizování na straně poskytovatele služeb. *Stav* poskytuje stav na straně společnosti Microsoft. Další informace o zřizování stavy okruhu najdete v tématu [pracovních](expressroute-workflows.md#expressroute-circuit-provisioning-states) článku.

Když vytvoříte nový okruh ExpressRoute, okruhu bude v následujícím stavu:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Okruh přejde na následující stav, pokud poskytovatel připojení probíhá povolení pro vás:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Okruh ExpressRoute musí být v následujícím stavu, abyste mohli používat ji:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="step-6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>Krok 6. Pravidelně kontrolovat stav a stav okruhu klíče
Tímto způsobem zjistíte, když má poskytovatel povoleno váš okruh. Po nakonfiguroval okruhu *ServiceProviderProvisioningState* se zobrazí jako *zajištěno* jak je znázorněno v následujícím příkladu:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="step-7-create-your-routing-configuration"></a>Krok 7. Vytvořte vlastní konfiguraci směrování
Odkazovat [konfigurace směrování pro okruh ExpressRoute (vytvoření a úprava okruhu partnerských vztahů)](expressroute-howto-routing-classic.md) podrobné pokyny najdete v článku.

> [!IMPORTANT]
> Tyto pokyny platí jenom pro okruhy vytvořené poskytovateli služeb, které nabízejí vrstvy 2 připojení služby. Pokud používáte poskytovatele služeb, který nabízí spravované vrstvy 3 služby (obvykle virtuální privátní síť IP, např. MPLS), poskytovatel připojení nakonfigurujete a správu směrování za vás.
> 
> 

### <a name="step-8-link-a-virtual-network-to-an-expressroute-circuit"></a>Krok 8. Propojení virtuální sítě k okruhu ExpressRoute
V dalším kroku propojení virtuální sítě k okruhu ExpressRoute. Odkazovat na [okruhy ExpressRoute propojování virtuálních sítí](expressroute-howto-linkvnet-classic.md) podrobné pokyny. Pokud potřebujete k vytvoření virtuální sítě pomocí modelu nasazení classic pro ExpressRoute najdete v tématu [vytvoření virtuální sítě pro ExpressRoute](expressroute-howto-vnet-portal-classic.md).

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Získávání stav okruhu ExpressRoute
Tyto informace kdykoli můžete načíst pomocí `Get-AzureCircuit` rutiny. Provádění volání bez parametrů jsou uvedeny všechny okruhů.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Informace o konkrétní okruh ExpressRoute můžete získat pomocí předání klíče služby jako parametr pro volání.

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Podrobný popis všech parametrů můžete získat spuštěním v následujícím příkladu:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Úprava okruhu ExpressRoute
Bez dopadu na připojení můžete upravit některé vlastnosti okruhu ExpressRoute.

Můžete provést následující bez výpadků:

* Povolit nebo zakázat doplněk ExpressRoute premium pro váš okruh ExpressRoute.
* Zadaný na tomto portu je dostupná kapacita, zvětšete šířku pásma okruhu ExpressRoute. Všimněte si, že šířku pásma okruhu přechod na starší verzi není podporován. 
* Změňte plán měření z – měření podle objemu dat na neomezená Data na úrovni. Všimněte si, že změna měření plánu z neomezená Data – měření podle objemu dat není podporován.
* Můžete povolit nebo zakázat *povolit klasické operace*.

Odkazovat [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) Další informace o omezení.

### <a name="to-enable-the-expressroute-premium-add-on"></a>Chcete-li povolit doplněk ExpressRoute premium
Doplněk ExpressRoute premium pro váš okruh. existující můžete povolit pomocí následující rutiny prostředí PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Funkce doplněk ExpressRoute premium povoleno bude mít teď váš okruh. Všimněte si, že jsme spustí hned, jak byl úspěšně spuštěn příkaz fakturace pro funkci rozšíření premium.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Chcete-li zakázat doplněk ExpressRoute premium
> [!IMPORTANT]
> Tato operace může selhat, pokud používáte prostředky, které jsou větší než co je povoleno pro standardní okruh.
> 
> 

#### <a name="considerations"></a>Požadavky

* Je nutné zajistit, že počet virtuální sítě propojené ke okruhu je menší než 10 před downgradovat z úrovně premium na standard. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete mít účtují zvýhodněné sazby.
* Je nutné zrušit všechny virtuální sítě v jiných geopolitické oblasti. Pokud to neuděláte, vaše žádost o aktualizaci se nezdaří a budete mít účtují zvýhodněné sazby.
* Směrovací tabulka musí být menší než 4 000 tras pro soukromý partnerský vztah. Pokud má větší než 4 000 tras velikost trasy tabulce, relaci protokolu BGP bude vyřaďte a nebude možné opětovně povolena dokud počet předpon inzerovaných přejde nižší než 4 000.

#### <a name="disable-the-premium-add-on"></a>Zakázat doplněk premium
Doplněk ExpressRoute premium pro váš okruh. existující můžete zakázat pomocí následující rutiny prostředí PowerShell:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Chcete-li aktualizovat šířku pásma okruhu ExpressRoute
Zkontrolujte [ExpressRoute – nejčastější dotazy](expressroute-faqs.md) pro podporované možnosti šířky pásma pro poskytovatele. Můžete vybrat libovolnou velikost, která je větší než velikost existujícího okruhu tak dlouho, dokud umožňuje fyzického portu (na kterém je vytvořený váš okruh).

> [!IMPORTANT]
> Možná budete muset znovu vytvořit okruh ExpressRoute, pokud je nedostatečné kapacity na existující port. Pokud v tomto umístění není k dispozici žádné další kapacitu, nelze upgradovat okruh.
>
> Nejde snížit šířku pásma okruhu ExpressRoute bez přerušení. Přechod na starší verzi šířky pásma vyžaduje zrušení zřízení okruh ExpressRoute a pak znova nezajistíte nové okruh ExpressRoute.
> 
> 

#### <a name="resize-a-circuit"></a>Změna velikosti okruhu

Jakmile se rozhodnete, jakou velikost potřebujete, můžete ke změně velikosti okruhu následující příkaz:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Váš okruh bude mít byla velikosti na straně společnosti Microsoft. Obraťte se svého poskytovatele připojení k aktualizaci konfigurace na jejich straně tak, aby odpovídaly tuto změnu. Všimněte si, že jsme spustí fakturace můžete pro možnost aktualizované šířky pásma z tohoto bodu na.

Pokud se zobrazí chybová zpráva při zvýšení šířku pásma okruhu, znamená to, existuje ponecháno žádné dostatečnou šířku pásma na fyzického portu, kde se má vytvořit existujícím okruhem. Budete muset odstranit tento okruh a vytvořit nové okruh velikosti, které potřebujete. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Zrušení zřízení a odstraňování okruhu ExpressRoute

### <a name="considerations"></a>Požadavky

* Je nutné zrušit všechny virtuální sítě od okruhu ExpressRoute pro tato operace proběhla úspěšně. Zkontrolujte, zda jsou všechny virtuální sítě, které jsou propojeny s okruh, pokud se tato operace nezdaří.
* Pokud je poskytovatel služby okruh ExpressRoute Stav zřizování **zřizování** nebo **zajištěno** , musíte pracovat se svým poskytovatelem služeb pro zrušení zřízení okruhu na jejich straně. Budeme nadále rezervovat prostředky a dokud poskytovatele služeb dokončení zrušení okruhu a upozorní nám vám účtovat.
* Pokud má poskytovatel služeb zrušit okruhu (poskytovatele služeb Stav zřizování je nastavena na **není zajišťováno**) pak můžete odstranit okruh. To se zastaví fakturace pro okruh.

#### <a name="delete-a-circuit"></a>Odstranit okruh

Váš okruh ExpressRoute můžete odstranit tak, že spustíte následující příkaz:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Další kroky
Po vytvoření okruhu, ujistěte se, že provedete následující kroky:

* [Vytvoření a úprava směrování pro okruhu ExpressRoute](expressroute-howto-routing-classic.md)
* [Propojení virtuální sítě k okruhu ExpressRoute](expressroute-howto-linkvnet-classic.md)

