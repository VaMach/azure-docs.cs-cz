---
title: "Instalace repliky řadiče domény služby Active Directory v Azure | Microsoft Docs"
description: "Kurz, který vysvětluje, jak nainstalovat řadič domény z doménové struktury služby Active Directory v místě na virtuální počítač Azure."
services: virtual-network
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8c9ebf1b-289a-4dd6-9567-a946450005c0
ms.service: virtual-network
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: f36a78fb8f8712ae8bb0ed6b5b8b081867198687
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Instalace repliky řadiče domény služby Active Directory v virtuální sítě Azure
Toto téma ukazuje, jak instalovat další řadiče domény (také označované jako repliku řadiče domény) pro místní doménu služby Active Directory na virtuálních počítačích Azure (VM) ve virtuální sítě Azure.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek.

Může být také zájem o tyto související témata:

* Volitelně můžete nainstalovat novou doménovou strukturu služby Active Directory na virtuální síť Azure. Tyto pokyny najdete v části [nainstalovat novou doménovou strukturu služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md).
* Koncepční informace o instalaci služby Active Directory Domain Services (AD DS) na virtuální síť Azure, najdete v části [pokyny pro nasazení systému Windows Server Active Directory ve virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Diagram scénáře
V tomto scénáři externí uživatelé potřebovat přístup k aplikacím, které běží na servery připojené k doméně. Virtuální počítače, na kterých běží na aplikační servery a řadičů domény repliky jsou nainstalovány ve virtuální sítě Azure. Virtuální síť může být připojen k místní síti pomocí [site-to-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) připojení, jak je znázorněno v následujícím diagramu, nebo můžete použít [ExpressRoute](../expressroute/expressroute-locations-providers.md) pro rychlejší připojení.

Aplikační servery a řadiče domény jsou nasazeny v rámci samostatné cloudové služby k distribuci výpočetní zpracování a v rámci [skupiny dostupnosti](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) pro lepší odolnost proti chybám.
Řadiče domény replikovat mezi sebou a se místní řadiče domény pomocí replikace služby Active Directory. Žádné nástrojů pro synchronizaci, je potřeba.

![Diagram pf repliky řadiče domény služby Active Directory síť Azure][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Vytvoření lokality služby Active Directory pro virtuální síť Azure
Je vhodné pro vytvoření webu ve službě Active Directory, představující oblast sítě odpovídající virtuální síť. Který pomáhá optimalizovat ověřování, replikace a další operace umístění řadiče domény. Následující kroky popisují, jak vytvořit web a pro další informace viz [přidání nového serveru](https://technet.microsoft.com/library/cc781496.aspx).

1. Otevřete lokality a služby Active Directory: **správce serveru** > **nástroje** > **lokality a služby Active Directory**.
2. Vytvořit web představující oblast, kde jste vytvořili virtuální sítě Azure: klikněte na tlačítko **lokality** > **akce** > **nové lokality** > zadejte název nové lokality, jako je například Azure USA – západ > vyberte propojení lokalit > **OK**.
3. Vytvořte podsíť a přidružit nové lokality: dvakrát klikněte na **lokality** > klikněte pravým tlačítkem na **podsítě** > **novou podsíť** > zadejte rozsah IP adres ve virtuální síti (například 10.1.0.0/16 v diagramu scénář) > vyberte nový web Azure > **OK**.

## <a name="create-an-azure-virtual-network"></a>Vytvoření virtuální sítě Azure
1. V [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **síťové služby** > **virtuální sítě** > **vytvořit vlastní** a použijte následující hodnoty a dokončete průvodce.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Podrobnosti virtuální sítě** |<p>Název: Zadejte název virtuální sítě, jako je například WestUSVNet.</p><p>Oblast: Vyberte nejbližší oblast.</p> |
   |  **DNS a síť VPN připojení** |<p>Servery DNS: Zadejte název a IP adresu jeden nebo více serverů DNS na místě.</p><p>Připojení: Vyberte **konfigurace VPN typu site-to-site**.</p><p>Místní sítě: Zadejte novou místní síť.</p><p>Pokud používáte místo VPN ExpressRoute, přečtěte si téma [nakonfigurujte připojení ExpressRoute prostřednictvím poskytovatele Exchange](../expressroute/expressroute-locations-providers.md).</p> |
   |  **Připojení Site-to-site** |<p>Název: Zadejte název do místní sítě.</p><p>IP adresa zařízení VPN: zadejte veřejnou IP adresu zařízení, které se budou připojovat k virtuální síti. Zařízení VPN nesmí být umístěné za překladem adres (NAT).</p><p>Adresa: Zadejte rozsahy adres ve vaší místní síti (například 192.168.0.0/16 v diagramu scénář).</p> |
   |  **Adresní prostory virtuální sítě** |<p>Adresní prostor: Zadejte rozsah IP adres pro virtuální počítače, které chcete spustit ve virtuální síti Azure (například 10.1.0.0/16 v diagramu scénář). Tento rozsah adres se nesmí překrývat s rozsahy adres místní sítě.</p><p>Podsítě: Zadejte název a adresu pro podsíť pro aplikační servery (například front-endu, 10.1.1.0/24) a pro řadiče domény (například back-end, 10.1.2.0/24).</p><p>Klikněte na tlačítko **přidat podsíť brány**.</p> |
2. Dále nakonfigurujete bránu virtuální sítě k vytvoření bezpečného připojení site-to-site VPN. V tématu [konfigurovat bránu virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) pokyny.
3. Vytvořte připojení site-to-site VPN mezi nové virtuální sítě a místní zařízení VPN. V tématu [konfigurovat bránu virtuální sítě](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) pokyny.

## <a name="create-azure-vms-for-the-dc-roles"></a>Vytvořit virtuální počítače Azure pro role řadiče domény
Opakujte tyto kroky k vytvoření virtuálních počítačů k hostování role řadiče domény, podle potřeby. Měli byste nasadit aspoň dva virtuální řadiče domény zajistit odolnost proti chybám a redundance. Pokud virtuální síť Azure zahrnuje aspoň dva řadiče domény, které jsou nakonfigurované podobně (to znamená, že jsou oba GC spuštění serveru DNS, a ani jeden z nich obsahuje všechny FSMO role a tak dále) pak umístit virtuální počítače, které běží ty řadiče domény v sadu dostupnosti pro lepší odolnost proti chybám.
Vytvoření virtuálních počítačů místo uživatelského rozhraní pomocí prostředí Windows PowerShell naleznete v tématu [použití Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

1. V [portál Azure classic](https://manage.windowsazure.com), klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**. Použijte následující hodnoty a dokončete průvodce. Přijměte výchozí hodnotu pro nastavení, není-li navrhované nebo vyžaduje jinou hodnotu.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Vyberte bitovou kopii** |Windows Server 2012 R2 Datacenter |
   |  **Konfigurace virtuálního počítače** |<p>Název virtuálního počítače: Zadejte název bez přípony (například AzureDC1).</p><p>Nové uživatelské jméno: Zadejte název uživatele. Tento uživatel bude členem místní skupiny Administrators na virtuálním počítači. Je nutné tento název se přihlásit k virtuálnímu počítači poprvé. Předdefinovaný účet s názvem správce nebude fungovat.</p><p>Nové heslo nebo potvrďte: Zadejte heslo</p> |
   |  **Konfigurace virtuálního počítače** |<p>Cloudové služby: Zvolte <b>vytvořte novou cloudovou službu</b> pro první virtuální počítač, vyberte tento stejný název cloudové služby při vytváření další virtuální počítače, který bude hostitelem role řadiče domény.</p><p>Název DNS cloudové služby: Zadejte globálně jedinečného názvu</p><p>Oblasti nebo skupiny vztahů nebo virtuální sítě: Zadejte název virtuální sítě (například WestUSVNet).</p><p>Účet úložiště: Zvolte <b>použít účet úložiště automaticky generované</b> pro první virtuální počítač a potom vyberte tento stejný název účtu úložiště při vytváření další virtuální počítače, který bude hostitelem role řadiče domény.</p><p>Skupinu dostupnosti: Zvolit <b>vytvořit skupinu dostupnosti</b>.</p><p>Název sady dostupnosti: Zadejte název pro dostupnost nastavit při vytváření první virtuální počítač a potom vyberte stejný název, když vytvoříte další virtuální počítače.</p> |
   |  **Konfigurace virtuálního počítače** |<p>Vyberte <b>nainstalujte agenta virtuálního počítače</b> a další rozšíření, budete potřebovat.</p> |
2. Připojte disk pro každý virtuální počítač, který se spustí roli serveru řadiče domény. Další disk je nutný k uložení databáze AD, protokoly a adresáře SYSVOL. Zadejte velikost disku (například 10 GB) a nechte **předvoleb mezipaměti hostitele** nastavena na **žádné**. Pokyny najdete v tématu [jak připojit datový Disk do virtuálního počítače s Windows](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
3. Po prvním přihlášení k virtuálnímu počítači, otevřete **správce serveru** > **Souborová služba a služba úložiště** k vytvoření svazku na tento disk pomocí systému souborů NTFS.
4. Vyhradit statickou IP adresu pro virtuální počítače, které budou spouštět roli řadiče domény. Můžete vyhradit statickou IP adresu, stažení služby instalace webové platformy společnosti Microsoft a [nainstalovat Azure PowerShell](/powershell/azure/overview) a spusťte rutinu Set-AzureStaticVNetIP. Například:

    ' Get-AzureVM - ServiceName AzureDC1-název AzureDC1 | Set-AzureStaticVNetIP - IPAddress 10.0.0.4 | Update-AzureVM

Další informace o nastavení statické IP adresy najdete v tématu [nakonfigurovat statické interní IP adresy pro virtuální počítač](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Nainstalovat službu AD DS na virtuálních počítačích Azure
Přihlaste se k virtuálnímu počítači a ověřte, že máte připojení site-to-site VPN nebo ExpressRoute připojení k prostředkům ve vaší místní síti. Potom nainstalujte službu AD DS na virtuálních počítačích Azure. Můžete použít stejný postup, který použijete k instalaci dalšího řadiče domény v místní síti (uživatelského rozhraní, prostředí Windows PowerShell nebo pomocí souboru odpovědí). Při instalaci služby AD DS, zkontrolujte, zda že zadáte nový svazek pro umístění databáze AD, protokoly a adresáře SYSVOL. Pokud potřebujete aktualizačnímu programu na instalaci služby AD DS, najdete v části [instalace služby Active Directory Domain Services (úroveň 100)](https://technet.microsoft.com/library/hh472162.aspx) nebo [instalace řadiče domény repliky Windows serveru 2012 v existující doméně (úroveň 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>Znovu nakonfigurujte server DNS pro virtuální síť
1. V [portál Azure](https://portal.azure.com)v **vyhledávání prostředků** zadejte *virtuální sítě*, pak klikněte na tlačítko **virtuální sítě (klasické)** ve výsledcích hledání. Klikněte na název virtuální sítě a potom [změnit konfiguraci IP adresy serverů DNS pro vaši virtuální síť](../virtual-network/virtual-network-manage-network.md#dns-servers) k používání statických IP adres přiřazené místo IP adresy serverů DNS místní repliku řadiče domény.
2. Chcete-li zajistit, že všechny repliky řadiče domény virtuální počítače ve virtuální síti je nakonfigurována pro použití serverů DNS ve virtuální síti, klikněte na tlačítko **virtuální počítače**, klikněte ve sloupci Stav pro každý virtuální počítač a pak klikněte na tlačítko **restartujte**. Počkejte, dokud je virtuální počítač zobrazuje **systémem** stavu, než se pokusíte se přihlásit do ní.

## <a name="create-vms-for-application-servers"></a>Vytvořit virtuální počítače pro aplikační servery
1. Opakujte tyto kroky k vytvoření virtuálních počítačů do spustit jako aplikační servery. Přijměte výchozí hodnotu pro nastavení, není-li navrhované nebo vyžaduje jinou hodnotu.

   | Na této stránce průvodce... | Zadejte tyto hodnoty |
   | --- | --- |
   |  **Vyberte bitovou kopii** |Windows Server 2012 R2 Datacenter |
   |  **Konfigurace virtuálního počítače** |<p>Název virtuálního počítače: Zadejte název bez přípony (například AppServer1).</p><p>Nové uživatelské jméno: Zadejte název uživatele. Tento uživatel bude členem místní skupiny Administrators na virtuálním počítači. Je nutné tento název se přihlásit k virtuálnímu počítači poprvé. Předdefinovaný účet s názvem správce nebude fungovat.</p><p>Nové heslo nebo potvrďte: Zadejte heslo</p> |
   |  **Konfigurace virtuálního počítače** |<p>Cloudové služby: Zvolte **vytvořte novou cloudovou službu** pro první virtuální počítač, vyberte tento stejný název cloudové služby při vytváření další virtuální počítače, který bude hostitelem aplikace.</p><p>Název DNS cloudové služby: Zadejte globálně jedinečného názvu</p><p>Oblasti nebo skupiny vztahů nebo virtuální sítě: Zadejte název virtuální sítě (například WestUSVNet).</p><p>Účet úložiště: Zvolte **použít účet úložiště automaticky generované** pro první virtuální počítač a potom vyberte tento stejný název účtu úložiště při vytváření další virtuální počítače, který bude hostovat aplikaci.</p><p>Skupinu dostupnosti: Zvolit **vytvořit skupinu dostupnosti**.</p><p>Název sady dostupnosti: Zadejte název pro dostupnost nastavit při vytváření první virtuální počítač a potom vyberte stejný název, když vytvoříte další virtuální počítače.</p> |
   |  **Konfigurace virtuálního počítače** |<p>Vyberte <b>nainstalujte agenta virtuálního počítače</b> a další rozšíření, budete potřebovat.</p> |
2. Po zřízení každý virtuální počítač, přihlaste se a připojení k doméně. V **správce serveru**, klikněte na tlačítko **místní Server** > **pracovní skupiny** > **změnu...** a pak vyberte **domény** a zadejte název vaší domény místně. Zadejte přihlašovací údaje uživatele domény a pak restartujte počítač k dokončení připojení k doméně.

Vytvoření virtuálních počítačů místo uživatelského rozhraní pomocí prostředí Windows PowerShell naleznete v tématu [použití Azure PowerShell k vytvoření a nastavení virtuálních počítačích se systémem Windows](../virtual-machines/windows/classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Další informace o používání prostředí Windows PowerShell najdete v tématu [Začínáme s Azure rutiny](/powershell/azure/overview) a [Reference k rutině Azure](/powershell/azure/get-started-azureps).

## <a name="additional-resources"></a>Další zdroje
* [Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
* [Postup nahrání existujících řadičů domény místní technologie Hyper-V do Azure pomocí Azure PowerShell](http://support.microsoft.com/kb/2904015)
* [Instalace nové doménové struktury služby Active Directory na virtuální síť Azure](active-directory-new-forest-virtual-machine.md)
* [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Microsoft Azure IaaS IT specialista: Základy (01) virtuálního počítače](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [Microsoft Azure IaaS IT specialista: (05) vytváření virtuální sítě a připojení mezi různými místy](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
* [Azure PowerShell](/powershell/azure/overview)
* [Rutiny pro správu Azure](/powershell/module/azurerm.compute/#virtual_machines)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
