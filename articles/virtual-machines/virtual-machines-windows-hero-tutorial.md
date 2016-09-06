<properties
    pageTitle="Vytvoření vašeho prvního virtuálního počítače s Windows | Microsoft Azure"
    description="Zjistěte, jak si vytvořit svůj první virtuální počítač s Windows pomocí Portálu Azure."
    keywords="virtuální počítač s Windows,vytvoření virtuálního počítače,virtuální počítač,nastavení virtuálního počítače"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/29/2016"
    ms.author="cynthn"/>

# Vytvoření vašeho prvního virtuálního počítače s Windows na Portálu Azure

Tento kurz vám ukáže, jak si snadno během několika málo minut vytvořit virtuální počítač s Windows pomocí Portálu Azure.  

Pokud nemáte předplatné Azure, můžete si [bezplatně vytvořit účet](https://azure.microsoft.com/free/) během několika minut.

Tady je [video s návodem](https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Create-A-Virtual-Machine-Running-Windows-In-The-Azure-Preview-Portal) pro tento kurz. 


## Volba image virtuálního počítače z Marketplace

Jako příklad používáme image systému Windows Server 2012 R2 Datacenter, je to ale pouze jedna z mnoha imagí, které Azure nabízí. Volba image závisí na tom, jaké máte předplatné. [Předplatitelé MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) můžou mít například k dispozici image desktopových verzí operačního systému.

1. Přihlaste se k [portálu Azure](https://portal.azure.com).

2. V nabídce centra klikněte na **Nový** > **Virtuální počítače** > **Windows Server 2012 R2 Datacenter**.

    ![Snímek obrazovky zobrazující image virtuálního počítače Azure dostupné na Portálu](./media/virtual-machines-windows-hero-tutorial/marketplace-new.png)


3. V okně **Windows Server 2012 R2 Datacenter** v části **Vybrat model nasazení** ověřte, že je vybraný **Resource Manager**. Klikněte na možnost **Vytvořit**.

    ![Snímek obrazovky zobrazující model nasazení, který se má vybrat pro virtuální počítač](./media/virtual-machines-windows-hero-tutorial/deployment-model.png)

## Vytvoření virtuálního počítače s Windows

Po výběru image můžete pro většinu položek konfigurace použít výchozí nastavení Azure a rychle vytvořit virtuální počítač.

1. V okně **Základy** zadejte **název** pro virtuální počítač. Název musí mít 1 až 15 znaků a nesmí obsahovat speciální znaky.

2. Zadejte **uživatelské jméno** a silné **heslo**, které se použijí k vytvoření místního účtu ve virtuálním počítači. Místní účet se používá k přihlášení k virtuálnímu počítači a jeho správě. 

    Heslo musí mít 8 až 123 znaků a musí splňovat tři ze čtyř bezpečnostních požadavků: jedno malé písmeno, jedno velké písmeno, jedna číslice a jeden speciální znak. Přečtěte si další informace o [požadavcích na uživatelské jméno a heslo](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm).


3. Vyberte existující [skupinu prostředků](../resource-group-overview.md#resource-groups) nebo zadejte název nové skupiny prostředků. Zadejte **umístění** datového centra Azure, například **Západní USA**. 

4. Až to budete mít, přejděte kliknutím na **OK** k další části. 

    ![Snímek obrazovky, který zobrazuje nastavení v okně **Základy** pro konfiguraci virtuálního počítače Azure](./media/virtual-machines-windows-hero-tutorial/basics-blade.png)

    
5. Zvolte [velikost](virtual-machines-windows-sizes.md) virtuálního počítače a potom pokračujte kliknutím na **Vybrat**. 

    ![Snímek obrazovky okna Velikost zobrazující velikosti virtuálního počítače Azure, ze kterých můžete vybírat](./media/virtual-machines-windows-hero-tutorial/size-blade.png)

6. V okně **Nastavení** můžete změnit možnosti úložiště a sítí. V tomto kurzu přijměte výchozí nastavení. Pokud jste vybrali velikost virtuálního počítače, který to podporuje, můžete výběrem možnosti **Premium (SSD)** v části **Typ disku** vyzkoušet Storage úrovně Premium. Po dokončení změn klikněte na **OK**.

    ![Snímek obrazovky okna Nastavení, kde můžete nakonfigurovat volitelné funkce pro virtuální počítač Azure](./media/virtual-machines-windows-hero-tutorial/settings-blade.png)

7. Klikněte na **Souhrn** a zkontrolujte zvolené volby. Jakmile se zobrazí zpráva **Ověření proběhlo úspěšně**, klikněte na **OK**.

    ![Snímek obrazovky stránky Souhrn, který ukazuje možnosti konfigurace zvolené pro virtuální počítač Azure](./media/virtual-machines-windows-hero-tutorial/summary-blade.png)

8. Zatímco bude Azure vytvářet virtuální počítač, můžete sledovat průběh v nabídce centra v části **Virtuální počítače**. 


## Připojení k virtuálnímu počítači a přihlášení

1.  V nabídce centra klikněte na **Virtuální počítače**.

2.  Ze seznamu vyberte virtuální počítač.

3. V okně pro virtuální počítač klikněte na **Připojit**. Tím se vytvoří a stáhne soubor .rdp (Remote Desktop Protocol), který představuje zástupce připojení k vašemu počítači. Můžete si ho uložit na plochu, abyste k němu měli snadno přístup. **Otevřením** tohoto souboru se připojíte ke svému virtuálnímu počítači.

    ![Snímek obrazovky Portálu Azure znázorňující způsob připojení k virtuálnímu počítači](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Zobrazí se upozornění, že soubor .rdp je od neznámého vydavatele. To je normální. Pokračujte kliknutím na **Připojit** v okně Připojení ke vzdálené ploše.

    ![Snímek obrazovky upozornění na neznámého vydavatele](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. V okně zabezpečení systému Windows zadejte uživatelské jméno a heslo pro místní účet, který jste vytvořili při vytváření virtuálního počítače. Zadejte uživatelské jméno v této podobě: *název_virtuálního_počítače*&#92;*uživatelské_jméno* a pak klikněte na **OK**.

    ![Snímek obrazovky zadání názvu virtuálního počítače, uživatelského jména a hesla](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Zobrazí se upozornění, že není možné ověřit certifikát. To je normální. Kliknutím na **Ano** ověřte identitu virtuálního počítače a dokončete přihlášení.

    ![Snímek obrazovky zobrazující zprávu o ověření identity virtuálního počítače](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Pokud budete mít s připojením problémy, projděte si téma [Poradce při potížích s připojením k virtuálnímu počítači s Windows v Azure pomocí Vzdálené plochy](virtual-machines-windows-troubleshoot-rdp-connection.md).

Teď můžete s virtuálním počítačem pracovat stejně jako s jakýmkoli jiným serverem.

## Instalace služby IIS na vašem virtuálním počítači

Teď jste přihlášení k virtuálnímu počítači a můžeme nainstalovat role serveru, abyste mohli více experimentovat.

1. Otevřete **Správce serveru**, pokud ještě není otevřený. Klikněte do nabídky **Start** a potom klikněte na **Správce serveru**.
2. V okně **Správce serveru** vyberte v levém podokně **Místní server**. 
3. V nabídce vyberte **Spravovat** > **Přidat role a funkce**.
4. V Průvodci přidáním rolí a funkcí na stránce **Typ instalace** zvolte **Instalace na základě rolí nebo na základě funkcí** a potom klikněte na **Další**.

    ![Snímek obrazovky s kartou Průvodce přidáním rolí a funkcí pro typ instalace](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Vyberte virtuální počítač ve fondu serverů a klikněte na **Další**.
6. Na stránce **Role serveru** vyberte **Webový server (IIS)**.

    ![Snímek obrazovky s kartou Průvodce přidáním rolí a funkcí pro role serveru](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. V místním okně pro přidávání funkcí potřebných pro službu IIS ověřte, že je zaškrtnuté **Zahrnout nástroje pro správu **, a potom klikněte na **Přidat funkce**. Po zavření místního okna klikněte v průvodci na **Další**.

    ![Snímek obrazovky s místním oknem pro potvrzení přidání role IIS](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Na stránce funkcí klikněte na **Další**.
9. Na stránce **Web Server Role (IIS)** klikněte na **Další**. 
10. Na stránce **Služby rolí** klikněte na **Další**. 
11. Na stránce **Potvrzení** klikněte na **Instalovat**. 
12. Po dokončení instalace klikněte v průvodci na **Zavřít**.



## Otevření portu 80 

Aby váš virtuální počítač přijímal příchozí přenos přes port 80, musíte do skupiny zabezpečení sítě přidat příchozí pravidlo. 

1. Otevřete [portál Azure](https://portal.azure.com).
2. V části **Virtuální počítače** vyberte virtuální počítač, který jste vytvořili.
3. V nastavení virtuálního počítače vyberte **Síťová rozhraní** a potom vyberte existující síťové rozhraní.

    ![Snímek obrazovky s nastavením virtuálního počítače pro síťová rozhraní](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. V části **Základní údaje** pro příslušné síťové rozhraní klikněte na **Skupina zabezpečení sítě**.

    ![Snímek obrazovky s částí Základní údaje pro síťové rozhraní](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. V okně **Základní údaje** pro NSG by mělo být jedno výchozí příchozí rule pro **default-allow-rdp**, které umožňuje přihlásit se k virtuálnímu počítači. Teď přidáte další pravidlo, které povolí provoz IIS. Klikněte na **Příchozí pravidla zabezpečení**.

    ![Snímek obrazovky s částí Základní údaje pro NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**.

    ![Snímek obrazovky s tlačítkem pro přidání pravidla zabezpečení](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. V části **Příchozí pravidla zabezpečení** klikněte na **Přidat**. Do pole pro rozsah portů zadejte **80** a zkontrolujte, že **Povolit** je zaškrtnuté. Až to budete mít, klikněte na **OK**.

    ![Snímek obrazovky s tlačítkem pro přidání pravidla zabezpečení](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Další informace o skupinách NSG a příchozích a odchozích pravidlech najdete v tématu [Povolení externího přístupu k virtuálnímu počítači pomocí webu Azure Portal](virtual-machines-windows-nsg-quickstart-portal.md)
 
## Připojení k výchozímu webu IIS

1. Na webu Azure Portal klikněte na **Virtuální počítače** a vyberte svůj virtuální počítač.
2. V okně **Základní údaje** zkopírujte **veřejnou IP adresu**.

    ![Snímek obrazovky, který ukazuje, kde najdete veřejnou IP adresu vašeho virtuálního počítače](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Otevřete prohlížeč, do adresního řádku zadejte svou veřejnou IP adresu, která vypadá takto: http://<publicIPaddress>, a kliknutím na **Enter** přejděte na tuto adresu.
3. Prohlížeč by měl přejít na výchozí webovou stránku IIS, která bude vypadat přibližně takto:

    ![Snímek obrazovky, který ukazuje, jak v prohlížeči vypadá výchozí stránka IIS](./media/virtual-machines-windows-hero-tutorial/iis-default.png)


## Zastavení virtuálního počítače

Pokud virtuální počítač nepoužíváte, je vhodné ho zastavit, aby vám zbytečně nenabíhaly poplatky. Stačí kliknout na tlačítko **Zastavit** a potom kliknout na **Ano**.

![Snímek obrazovky tlačítka pro zastavení virtuálního počítače](./media/virtual-machines-windows-hero-tutorial/stop-vm.png)
    
Až ho budete chtít zase použít, stačí kliknout na tlačítko **Spustit **.


## Další kroky

* Také můžete experimentovat s [připojením datového disku](virtual-machines-windows-attach-disk-portal.md) k vašemu virtuálnímu počítači. Pomocí datových disků si můžete rozšířit úložiště pro virtuální počítač.

* Můžete také [vytvořit virtuální počítač s Windows pomocí PowerShellu](virtual-machines-windows-ps-create.md) nebo [vytvořit virtuální počítač s Linuxem](virtual-machines-linux-quick-create-cli.md) pomocí Azure CLI.

* Pokud vás zajímá automatizace nasazení, projděte si téma [Vytvoření virtuálního počítač s Windows pomocí šablony Resource Manageru](virtual-machines-windows-ps-template.md).



<!--HONumber=ago16_HO5-->


