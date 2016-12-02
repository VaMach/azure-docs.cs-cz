---
title: "Vytvoření prvního virtuálního počítače s Windows | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit svůj první virtuální počítač s Windows pomocí webu Azure Portal."
keywords: "virtuální počítač s Windows,vytvoření virtuálního počítače,virtuální počítač,nastavení virtuálního počítače"
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 785e17eb-4a13-4f06-b70f-4bd496d0ec5d
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: hero-article
ms.date: 11/16/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 22156a9f3c5a57e10cad96661f50531b309c3a04
ms.openlocfilehash: a25d394e83d7945f0ea50de1fc65a68a96190a34


---
# <a name="create-your-first-windows-virtual-machine-in-the-azure-portal"></a>Vytvoření vašeho prvního virtuálního počítače s Windows na Portálu Azure
Tento kurz vám ukáže, jak si snadno během několika málo minut vytvořit virtuální počítač s Windows pomocí webu Azure Portal.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Volba image virtuálního počítače z Marketplace
Jako příklad používáme image systému Windows Server 2012 R2 Datacenter, je to ale pouze jedna z mnoha imagí, které Azure nabízí. Volba image závisí na tom, jaké máte předplatné. Některé desktopové image jsou například dostupné pro [Předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. V nabídce centra klikněte na **Nový**  > **Compute** >  **Windows Server 2012 R2 Datacenter**.
   
    ![Snímek obrazovky zobrazující image virtuálního počítače Azure dostupné na Portálu](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)
3. V okně **Windows Server 2012 R2 Datacenter** v části **Vybrat model nasazení** ověřte, že je vybrán **Resource Manager**. Klikněte na možnost **Vytvořit**.
   
    ![Snímek obrazovky zobrazující model nasazení, který se má vybrat pro virtuální počítač](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## <a name="create-the-windows-virtual-machine"></a>Vytvoření virtuálního počítače s Windows
Po výběru image můžete použít výchozí nastavení a rychle vytvořit virtuální počítač.

1. V okně **Základy** zadejte **název** pro virtuální počítač. Název musí mít 1 až 15 znaků a nesmí obsahovat speciální znaky.
2. Zadejte **uživatelské jméno** a silné **heslo**, které se použijí k vytvoření místního účtu ve virtuálním počítači. Místní účet slouží k přihlášení k virtuálnímu počítači a jeho správě. 
   
    Heslo musí mít 8 až 123 znaků a musí splňovat tři ze čtyř bezpečnostních požadavků: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).
3. Vyberte existující [skupinu prostředků](../azure-resource-manager/resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků. Zadejte **umístění** datového centra Azure, například **Západní USA**. 
4. Až to budete mít, přejděte kliknutím na **OK** k další části. 
   
    ![Snímek obrazovky, který zobrazuje nastavení v okně **Základy** pro konfiguraci virtuálního počítače Azure](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)
5. Zvolte [velikost](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) virtuálního počítače a poté pokračujte kliknutím na **Vybrat**. 
   
    ![Snímek obrazovky okna Velikost zobrazující velikosti virtuálního počítače Azure, ze kterých můžete vybírat](./media/virtual-machines-windows-hero-tutorial/size-blade.png)
6. V okně **Nastavení** můžete změnit možnosti úložiště a sítě. V tomto kurzu přijměte výchozí nastavení. Pokud jste vybrali velikost virtuálního počítače, který to podporuje, můžete výběrem možnosti **Premium (SSD)** jako **Typ disku** vyzkoušet službu Azure Storage úrovně Premium. Jakmile budete se změnami hotovi, klikněte na **OK**.
   
    ![Snímek obrazovky okna Nastavení, kde můžete nakonfigurovat volitelné funkce pro virtuální počítač Azure](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)
7. Klikněte na **Souhrn** a zkontrolujte zvolené volby. Jakmile se zobrazí zpráva **Ověření proběhlo úspěšně**, klikněte na **OK**.
   
    ![Snímek obrazovky stránky Souhrn, který ukazuje možnosti konfigurace zvolené pro virtuální počítač Azure](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)
8. Zatímco bude Azure vytvářet virtuální počítač, můžete sledovat průběh v nabídce centra v části **Virtual Machines**. 

## <a name="connect-to-the-virtual-machine-and-sign-on"></a>Připojení k virtuálnímu počítači a přihlášení
1. V nabídce centra klikněte na **Virtual Machines**.
2. Ze seznamu vyberte virtuální počítač.
3. V okně pro virtuální počítač klikněte na **Připojit**. Tím se vytvoří a stáhne soubor .rdp (Remote Desktop Protocol), který představuje zástupce připojení k vašemu počítači. Můžete si ho uložit na plochu, abyste k němu měli snadno přístup. **Otevřením** tohoto souboru se připojíte ke svému virtuálnímu počítači.
   
    ![Snímek obrazovky webu Azure Portal znázorňující způsob připojení k virtuálnímu počítači](./media/virtual-machines-windows-hero-tutorial/connect.png)
4. Zobrazí se upozornění, že soubor .rdp je od neznámého vydavatele. To je normální. Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.
   
    ![Snímek obrazovky s upozorněním na neznámého vydavatele](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)
5. V okně zabezpečení systému Windows zadejte uživatelské jméno a heslo pro místní účet, který jste vytvořili při vytváření virtuálního počítače. Zadejte uživatelské jméno v této podobě: *název_virtuálního_počítače*&#92;*uživatelské_jméno* a pak klikněte na **OK**.
   
    ![Snímek obrazovky se zadáním názvu virtuálního počítače, uživatelského jména a hesla](./media/virtual-machines-windows-hero-tutorial/credentials.png)
6. Zobrazí se upozornění, že certifikát nelze ověřit. To je normální. Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)

Pokud budete mít s připojením problémy, projděte si téma [Poradce při potížích s připojením k virtuálnímu počítači s Windows v Azure pomocí Vzdálené plochy](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Nyní můžete s virtuálním počítačem pracovat stejně jako s jakýmkoli jiným serverem.

## <a name="optional-stop-the-vm"></a>Volitelné: Zastavení virtuálního počítače
Pokud virtuální počítač nepoužíváte, je vhodné ho zastavit, aby vám zbytečně nenabíhaly poplatky. Stačí kliknout na **Zastavit** a poté na **Ano**.

![Snímek obrazovky s tlačítkem pro zastavení virtuálního počítače](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)

Když ho budete chtít zase použít, klikněte na tlačítko **Spustit **.

## <a name="next-steps"></a>Další kroky
* Můžete [nainstalovat službu IIS](virtual-machines-windows-hero-role.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a experimentovat se svým novým virtuálním počítačem. Tento kurz také ukazuje, jak otevřít port 80 pro příchozí webový provoz pomocí skupiny zabezpečení sítě (NSG). 
* Můžete také [vytvořit virtuální počítač s Windows pomocí prostředí PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [vytvořit virtuální počítač se systémem Linux](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) pomocí rozhraní příkazového řádku Azure.
* Pokud vás zajímá automatizace nasazení, projděte si téma [Vytvoření virtuálního počítač s Windows pomocí šablony Resource Manageru](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




<!--HONumber=Nov16_HO3-->


