---
title: "Vytvoření, změnit nebo odstranit partnerský vztah virtuální síti Azure | Microsoft Docs"
description: "Zjistěte, jak vytvářet, měnit nebo odstranit partnerský vztah virtuální sítě."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial;anavin
ms.openlocfilehash: 9fcfca3de6204581936a2bacfd86e84fd373190a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Vytvoření, jejich změny nebo odstranění virtuální sítě partnerského vztahu

Zjistěte, jak vytvářet, měnit nebo odstranit partnerský vztah virtuální sítě. Partnerský vztah virtuální sítě umožňuje připojení virtuální sítě přes Azure páteřní síti. Jakmile peered, virtuální sítě se pořád spravují jako samostatné prostředky. Pokud si nejste obeznámeni s partnerský vztah virtuální sítě, doporučujeme, abyste čtení [partnerského vztahu Přehled virtuálních sítí](virtual-network-peering-overview.md) a dokončení [vytvoření partnerského vztahu kurzu virtuální sítě](virtual-network-create-peering.md), před dokončením úlohy v tomto článku.

Vytváření partnerských vztahů virtuálních sítí ve stejné oblasti je všeobecně dostupné. Vytváření partnerských vztahů virtuálních sítí v různých oblastech je aktuálně ve verzi Preview v oblastech USA – středozápad, Střední Kanada a USA – západ 2. Můžete [zaregistrovat své předplatné pro verzi Preview](virtual-network-create-peering.md).

> [!WARNING]
> Partnerské vztahy virtuálních sítí vytvořené v tomto scénáři nemusí dosahovat stejné úrovně dostupnosti a spolehlivosti jako scénáře ve verzi všeobecné dostupnosti. Partnerské vztahy virtuálních sítí můžou mít omezené možnosti a nemusí být dostupné ve všech oblastech Azure. Nejaktuálnější oznámení o dostupnosti a stavu této funkce najdete na stránce [Aktualizace služby Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).
>

## <a name="before-you-begin"></a>Než začnete

Před dokončením kroků v žádné části tohoto článku dokončete následující úlohy:

- Zkontrolujte [Azure omezuje](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) článku se dozvíte o omezeních pro partnerský vztah.
- Přihlaste se k portálu Azure, rozhraní příkazového řádku Azure (CLI) nebo Azure PowerShell s účet Azure. Pokud nemáte účet Azure, si zaregistrovat [Bezplatný zkušební účet](https://azure.microsoft.com/free).
- Pokud pomocí příkazů prostředí PowerShell k dokončení úloh v tomto článku [instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Ujistěte se, že máte nejnovější verzi rutin prostředí Azure PowerShell nainstalovaný. Chcete-li získat nápovědu pro příkazy prostředí PowerShell s příklady, zadejte `get-help <command> -full`.
- Pokud používáte rozhraní příkazového řádku Azure (CLI) příkazy k dokončení úloh v tomto článku [instalace a konfigurace rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zajistěte, že abyste měli nejnovější verzi rozhraní příkazového řádku Azure, který je nainstalovaný. Chcete-li získat nápovědu pro příkazy rozhraní příkazového řádku, zadejte `az <command> --help`. Místo instalace rozhraní příkazového řádku a jeho požadavky, můžete použít prostředí cloudové služby Azure. Služba Azure Cloud Shell je volně dostupné prostředí Bash, které můžete spustit přímo z portálu Azure Portal. Cloudové prostředí má Azure CLI předem nainstalován a nakonfigurován pro použití s vaším účtem. Chcete-li použít cloudové prostředí, klikněte na tlačítko prostředí cloudu **> _** tlačítka v horní části [portál](https://portal.azure.com). 

## <a name="create-a-peering"></a>Vytvoření partnerského vztahu

>[!NOTE]
>Před vytvořením partnerský vztah, ujistěte se, které jste seznámili s [požadavky a omezení](#requirements-and-constraints) a [požadovaná oprávnění](#permissions).
>

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen nezbytné [role nebo oprávnění](#permissions).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání klikněte na něj. Nevybírejte **virtuální sítě (klasické)** -li se zobrazí se v seznamu, nelze vytvořit partnerský vztah z virtuální sítě nasazené prostřednictvím modelu nasazení classic.
3. V **virtuální sítě** okno, které se zobrazí, klikněte na virtuální síť, kterou chcete vytvořit pro partnerský vztah.
4. V podokně se zobrazí pro vybrané virtuální síť, klikněte na tlačítko **partnerských vztahů** v **nastavení** části.
5. Klikněte na tlačítko **+ přidat**. 
6. <a name="add-peering"></a>V **přidat partnerský vztah** okno, zadejte nebo vyberte hodnoty pro následující nastavení:
    - **Název:** název partnerského vztahu musí být jedinečný v rámci virtuální sítě.
    - **Virtuální síť modelu nasazení:** vyberte model nasazení, které virtuální síť, kterou chcete partnerský vztah nasazená prostřednictvím.
    - **Vím Moje ID prostředku:** Pokud máte přístup pro čtení k virtuální síti, kterou chcete partnerský vztah, nechte toto políčko nezaškrtnuté. Pokud nemáte přístup pro čtení k virtuální síti nebo předplatné, které chcete partnerský vztah, zaškrtněte toto políčko. Zadejte je úplné ID prostředku ve virtuální síti, kterou chcete partnerský vztah v **ID prostředku** pole, které se zobrazily při zaškrtnuté políčko. ID prostředku, který zadáte, musí být pro virtuální síť, která existuje v stejné Azure [oblast](https://azure.microsoft.com/regions) jako této virtuální síti. Pokud chcete vybrat virtuální síť v jiné oblasti, [registrace předplatného pro verzi preview.](virtual-network-create-peering.md) Je úplné ID prostředku bude vypadat podobně jako /subscriptions/<Id>/providers/Microsoft.Network/virtualNetworks/ /resourceGroups/ <-název skupiny prostředků-> <-název virtuální sítě->. ID prostředku pro virtuální síť můžete získat zobrazením vlastností pro virtuální síť. Další informace zobrazíte vlastnosti pro virtuální síť, v tématu [spravovat virtuální sítě](virtual-network-manage-network.md#view-vnet).
    - **Předplatné:** vyberte [předplatné](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve virtuální síti, kterou chcete partnerský vztah. Jeden nebo více odběrů nejsou uvedeny, v závislosti na tom, kolik odběry má váš účet přístup pro čtení. Pokud je zaškrtnuta možnost **ID prostředku** zaškrtávací políčko, toto nastavení není k dispozici. Virtuální sítě v různých předplatných mohou párově tak dlouho, dokud obě virtuální sítě byly vytvořeny prostřednictvím Správce prostředků. Možnost rovnocenných počítačů napříč vytvořena prostřednictvím různé modely nasazení odběrů je ve verzi preview. Zaregistrovat verzi Preview před vytvořením partnerský vztah mezi virtuálními sítěmi nasadit v rámci různých nasazení modely, které existují v různých předplatných. Další informace o tom, jak zaregistrovat verzi Preview a [peer virtuální sítě vytvořené pomocí různé modely nasazení v různých předplatných](create-peering-different-deployment-models-subscriptions.md).
    - **Virtuální sítě:** vyberte virtuální síť, kterou chcete partnerský vztah. Můžete vybrat virtuální síti vytvořené prostřednictvím buď modelu nasazení Azure. Pokud chcete vybrat virtuální síť v jiné oblasti, [registrace předplatného pro verzi preview.](virtual-network-create-peering.md) Musíte mít přístup pro čtení k virtuální síti mohla být zobrazená v seznamu. Pokud je virtuální sítě uvedené, ale je zobrazena šedě, může být protože adresní prostor virtuální sítě se překrývá s adresním prostorem pro tuto virtuální síť. Pokud adresu virtuální sítě prostory překrývají, nemůže být peered. Pokud je zaškrtnuta možnost **ID prostředku** zaškrtávací políčko, toto nastavení není k dispozici.
    - **Povolit přístup k virtuální síti:** vyberte **povoleno** (výchozí), pokud chcete povolit komunikaci mezi dvěma virtuálními sítěmi. Povolení komunikace mezi virtuálními sítěmi umožňuje prostředky připojenými k buď virtuální sítě pro komunikaci mezi sebou stejným šířky pásma a latence, jako kdyby byly připojené ke stejné virtuální síti. Veškerá komunikace mezi prostředky v dvě virtuální sítě je prostřednictvím privátní síť Azure. **Virtuální síť** výchozí značka pro skupiny zabezpečení sítě zahrnuje virtuální sítě a peered virtuální sítě. Další informace o značkách výchozí skupiny zabezpečení sítě, najdete [přehled skupin zabezpečení sítě](virtual-networks-nsg.md#default-tags) článku.  Vyberte **zakázané** Pokud nechcete, aby provoz mají být předány peered virtuální sítě. Můžete vybrat **zakázané** Pokud jste peered virtuální síť s jinou virtuální sítí, ale někdy chcete zakázat tok přenosů mezi dvě virtuální sítě. Můžete zjistit, že povolení nebo zákaz je vhodnější než odstranit a znovu vytvořit partnerských vztahů. Pokud je toto nastavení zakázáno, nebude přenos mezi peered virtuální sítě.
    - **Povolit přesměrovaných přenosů:** zaškrtnutím tohoto políčka Povolit provoz předávat peered virtuální síti (provoz není pocházející z peered virtuální sítě) do toku do této virtuální sítě. Předávání dál provoz je běžné, když máte ukázku nasazenou zařízení virtuální sítě ve virtuální síti se partnerského vztahu a vytvořit uživateli definované trasy pro přenos dat přes virtuální síťové zařízení. Ponecháte-li tento není zaškrtnuto políčko (výchozí), nelze do této virtuální sítě tok přenosů, které jsou předávány z peered virtuální sítě. Při povolení této funkce umožňuje přesměrovaných přenosů prostřednictvím partnerského vztahu, nevytvoří všechny trasy definované uživatelem nebo virtuální síťová zařízení. Trasy definované uživatelem a virtuální zařízení sítě se vytvářejí zvlášť. Další informace o [trasy definované uživatelem](virtual-networks-udr-overview.md).
    - **Povolit bránu přenosu:** toto políčko zaškrtněte, pokud máte bránu virtuální sítě připojený k této virtuální síti a chcete povolit přenosy z peered virtuální sítě, abyste procházet skrz bránu. Například tato virtuální síť může připojit k místní síti prostřednictvím brány virtuální sítě. Zaškrtnutím tohoto políčka umožňuje přenos ze peered virtuální sítě, abyste procházet skrz bránu připojené k této virtuální síti. Pokud zaškrtnete toto políčko, peered virtuální síť nemůže mít brána nakonfigurovaná. Peered virtuální síť musí mít **používání služby Brána vzdálené** zaškrtnuto při nastavování partnerského vztahu z virtuální sítě do této virtuální sítě. Necháte-li tento není zaškrtnuto políčko (výchozí), provoz z stále toků peered virtuální sítě do této virtuální sítě, ale nelze procházet skrz bránu virtuální sítě připojený k této virtuální síti. Další informace o [brány virtuální sítě](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). 
    
        Tuto možnost nelze povolit, pokud jste partnerský vztah virtuální síť (Resource Manager) s virtuální sítě (klasické). I když přenosy mezi dvěma virtuálními sítěmi, virtuální síti (klasické) nelze procházet skrz bránu sítě, který je připojený k virtuální síti (Resource Manager).

    - **Používat vzdálený brány:** zaškrtnutím tohoto políčka Povolit provoz z této virtuální sítě procházet skrz bránu virtuální sítě připojený k virtuální síti, se partnerský vztah s. Například virtuální síť, kterou jste partnerský vztah s má bránu VPN, který je připojen umožňující komunikaci s místní sítě.  Zaškrtnutím tohoto políčka umožňuje provoz z této virtuální sítě procházet skrz bránu VPN, který je připojený k peered virtuální síti. Pokud zaškrtnete toto políčko, peered virtuální síť musí mít bránu virtuální sítě je připojený a musí mít **povolit brány přenosu** políčko zaškrtnuto. Pokud necháte tento není zaškrtnuto políčko (výchozí), z peered virtuální sítě můžete stále přenos do této virtuální sítě, ale nemůže probíhaly přes bránu virtuální sítě připojen ke tento virtuální sítě. 
    
        Tuto možnost nelze povolit, pokud jste partnerský vztah virtuální síť (Resource Manager) s virtuální sítě (klasické). I když přenosy mezi dvěma virtuálními sítěmi, přenosy virtuální sítě (Resource Manager) nelze procházet skrz bránu sítě, který je připojený k virtuální síti (klasické).
7. Klikněte **OK** tlačítko Přidat podsíť virtuální sítě, které jste vybrali.

### <a name="commands"></a>Příkazy

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[virtuální síť az sítě partnerský vztah vytvořit.](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Přidat AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Scénáře

Partnerský vztah virtuálních sítí se vytváří mezi virtuálními sítěmi vytvořenými prostřednictvím stejného modelu nebo různých modelů nasazení, které existují ve stejném předplatném nebo různých předplatných. Proveďte podrobný kurz pro jednu z následujících scénářů:
 
|Model nasazení Azure  | Předplatné  |
|---------|---------|
|Obě Resource Manager |[Stejné](virtual-network-create-peering.md)|
| |[Různé](create-peering-different-subscriptions.md)|
|Jedna Resource Manager, druhá Classic     |[Stejné](create-peering-different-deployment-models.md)|
| |[Různé](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Zobrazení nebo změna nastavení partnerského vztahu

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen nezbytné [role nebo oprávnění](#permissions).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **virtuální sítě** okno, které se zobrazí, klikněte na virtuální síť, kterou chcete vytvořit pro partnerský vztah.
4. V podokně se zobrazí pro vybrané virtuální síť, klikněte na tlačítko **partnerských vztahů** v **nastavení** části.
5. Klikněte na tlačítko partnerský vztah, který chcete zobrazit nebo změnit nastavení pro.
6. Změňte příslušná nastavení. Přečtěte si informace o možnosti pro každé nastavení v [krok 6](#add-peering) Create partnerského vztahu části tohoto článku. 

    >[!NOTE]
    >Před vytvořením partnerský vztah, ujistěte se, které jste seznámili s [požadavky a omezení](#requirements-and-constraints) a [požadovaná oprávnění](#permissions).
    >

7. Klikněte na **Uložit**.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[partnerského vztahu seznam az sítě vnet](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) do seznamu partnerských vztahů pro virtuální síť, [az sítě vnet partnerského vztahu zobrazit](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) zobrazíte nastavení pro konkrétní partnerský vztah, a [az sítě vnet partnerského vztahu aktualizace](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) změnit nastavení partnerského vztahu.|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) k načtení nastavení zobrazení partnerského vztahu a [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) Chcete-li změnit nastavení.|

## <a name="delete-a-peering"></a>Odstranit partnerský vztah
Při odstranění partnerský vztah z virtuální sítě už přenosy dat do peered virtuální sítě. Při nasazení prostřednictvím Resource Manager virtuální sítě se kterými mají partnerský, každá virtuální síť má partnerský vztah k jiné virtuální síti. I když odstranění partnerského vztahu z jedné virtuální sítě zakáže komunikace mezi virtuálními sítěmi, ale neodstraníte partnerského vztahu z virtuální sítě. Stav partnerského vztahu pro partnerský vztah, který již existuje ve virtuální síti je **odpojeno**. Nelze znovu vytvořit partnerského vztahu dokud znovu vytvořit partnerský vztah v první virtuální síť a stav partnerského vztahu i změny virtuální sítě *připojeno*. 

Pokud chcete, aby virtuální sítě pro komunikaci někdy, ale ne vždy místo odstraněním partnerský vztah, můžete nastavit **povolit přístup k virtuální síti** nastavení **zakázané** místo. Další informace, jak číst kroku 6 postupu [partnerský vztah vytvořit](#create-peering) tohoto článku. Můžete zjistit, zakázání a povolení přístupu k síti jednodušší než odstranit a znovu vytvořit partnerských vztahů.

1. Přihlaste se k [portál](https://portal.azure.com) pomocí účtu, který je přiřazen nezbytné [role nebo oprávnění](#permissions).
2. Do pole, která obsahuje text *vyhledávání prostředků* v horní části portálu Azure, zadejte *virtuální sítě*. Když **virtuální sítě** se zobrazí ve výsledcích hledání klikněte na něj.
3. V **virtuální sítě** okno, které se zobrazí, klikněte na virtuální síť, kterou chcete odstranit partnerský vztah z.
4. V zobrazeném okně pro virtuální síť, která jste vybrali, klikněte na **partnerských vztahů** pod **nastavení**.
5. V seznamu partnerských vztahů, které se zobrazí v okně partnerských vztahů, klikněte pravým tlačítkem na vztahy, které chcete odstranit, klikněte na **odstranit**, pak **Ano** odstranění partnerského vztahu z první virtuální sítě.
6. Předchozí kroky odstranění partnerského vztahu z jiné virtuální sítě v partnerském vztahu.

**Příkazy**

|Nástroj|Příkaz|
|---|---|
|Rozhraní příkazového řádku|[AZ síť vnet partnerského vztahu odstranit](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Odebrat AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Požadavky a omezení 

- Virtuální sítě, ke kterým jste partnerský uzel musí mít-překrývající se adresní prostory IP adres.
- Nelze přidejte adresní prostory k, nebo odstraňte adresní prostory z virtuální sítě, jakmile je peered virtuální sítě s jinou virtuální sítí. Přidat nebo odebrat adresní prostory, odstranit partnerský vztah, přidat nebo odebrat adresní prostory, potom je znovu vytvořte partnerského vztahu. Přidejte adresní prostory k nebo odebrat adresní prostory virtuálních sítí, přečtěte si [Create, změn nebo odstranění virtuální sítě](virtual-network-manage-network.md#add-address-spaces) článku.
- Mohou párově dvě virtuální sítě nasazené prostřednictvím Resource Manager nebo virtuální síť s virtuální sítí nasazené prostřednictvím modelu nasazení classic nasazení prostřednictvím Resource Manager. Nelze peer dvě virtuální sítě vytvořené pomocí modelu nasazení classic. Pokud si nejste obeznámeni s modelech nasazení Azure, přečtěte si [modelech nasazení Azure pochopit](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) článku. K propojení dvou virtuálních sítí vytvořených prostřednictvím modelu nasazení Classic můžete použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
- Pokud navazujete partnerský vztah mezi dvěma virtuálními sítěmi vytvořenými prostřednictvím Resource Manageru, v partnerském vztahu musí být nakonfigurován partnerský vztah pro každou virtuální síť. 
    - *Získané:* při vytváření partnerského vztahu druhý virtuální sítě z první virtuální sítě, je stav partnerského vztahu *získaných*. 
    - *Připojeno:* při vytváření partnerského vztahu z druhé virtuální sítě na první virtuální síť, jeho stav partnerského vztahu je *připojeno*. Pokud můžete zobrazit stav partnerského vztahu pro první virtuální síť, zobrazí se jeho stav se změnil z *získaných* k *připojeno*. Partnerského vztahu není úspěšně vytvořeno, dokud je stav partnerského vztahu pro partnerské vztahy obě virtuální sítě *připojeno*.
- Pokud partnerský vztah virtuální síti vytvořené pomocí Správce prostředků s virtuální síti vytvořené pomocí modelu nasazení classic, můžete pouze nakonfigurovat partnerský vztah pro virtuální síť nasazení prostřednictvím Resource Manager. Nelze nakonfigurovat partnerský vztah pro virtuální sítě (klasické), nebo mezi dvěma virtuálními sítěmi nasazené prostřednictvím modelu nasazení classic. Při vytváření partnerského vztahu z virtuální sítě (Resource Manager) do virtuální sítě (klasické), je stav partnerského vztahu *aktualizace*, krátce změní na *připojeno*.
- Mezi dvěma virtuálními sítěmi je navázat partnerský vztah. Partnerských vztahů nejsou přechodné. Pokud vytvoříte partnerských vztahů mezi:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Není žádný partnerský vztah mezi VirtualNetwork1 a VirtualNetwork3 prostřednictvím VirtualNetwork2. Pokud chcete vytvořit virtuální síť partnerský vztah mezi VirtualNetwork1 a VirtualNetwork3, budete muset vytvoření partnerského vztahu mezi VirtualNetwork1 a VirtualNetwork3.
- Nelze přeložit názvy v peered virtuální sítě pomocí výchozí Azure překlad. Překládat názvy v jiných virtuálních sítí, musíte použít vlastní server DNS. Další postup nastavení serveru DNS, najdete [překladu IP adresy serveru DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) článku.
- Prostředky v obě virtuální sítě v partnerském vztahu může komunikovat s navzájem stejné šířky pásma a latenci jako kdyby byly ve stejné virtuální síti. Velikost pro všechny virtuální počítače ale má vlastní maximální šířku pásma sítě. Další informace o maximální šířce pásma pro různé velikosti virtuálních počítačů najdete v článcích o velikosti virtuálních počítačů pro [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Mohou párově virtuální sítě nasazení prostřednictvím Resource Manager, které jsou ve stejné nebo různých předplatných.
- Mohou párově virtuálních sítí nasadit v rámci různých nasazení modelů, které jsou ve stejné nebo různých předplatných (preview). 
- Odběry, které jsou obě virtuální sítě v musí být přidruženy ke stejné klienta Azure Active Directory. Pokud ještě nemáte klient služby AD, můžete rychle [vytvořit](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Můžete použít [brány VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) připojení dvě virtuální sítě, které existují v různých předplatných, které jsou přidružené k různými klienty služby Active Directory.
- Virtuální sítě můžete peered s jinou virtuální sítí a také být připojen k jiné virtuální síti s bránu virtuální sítě Azure. Pokud virtuální sítě jsou připojené prostřednictvím partnerského vztahu a bránu, provoz mezi virtuálními sítěmi toky prostřednictvím konfigurace partnerského vztahu, nikoli brány.
- Za příchozí a výchozí přenos dat využívající partnerský vztah virtuálních sítí se účtuje nominální poplatek. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Oprávnění

Účty, které použijete k vytvoření virtuální sítě partnerského vztahu musí mít potřebné role nebo oprávnění. Například pokud dvě virtuální sítě s názvem myVnetA a myVnetB byly partnerský vztah, musí váš účet se přiřazenou následující minimální role nebo oprávnění pro každou virtuální síť:
    
|Virtuální síť|Model nasazení|Role|Oprávnění|
|---|---|---|---|
|myVnetA|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Není k dispozici|
|myVnetB|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Další informace o [předdefinované role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) a přiřazení specifické oprávnění pro [vlastní role](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (pouze Resource Manager).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak vytvořit [rozbočovač a uvedenou síťovou topologii](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering). 
