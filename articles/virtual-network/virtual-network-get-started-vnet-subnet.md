---
title: "Vytvoření první virtuální sítě Azure | Dokumentace Microsoftu"
description: "Zjistěte, jak vytvořit virtuální síť Azure, připojit k ní dva virtuální počítače, a jak se k těmto virtuálním počítačům připojit."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: 071ef9c0784bdb24c7d29f49c36aee2271f4b524
ms.lasthandoff: 03/03/2017


---

# <a name="create-your-first-virtual-network"></a>Vytvoření první virtuální sítě

Zjistěte, jak vytvořit virtuální síť se dvěma podsítěmi a jak vytvořit dva virtuální počítače a oba připojit k jedné z podsítí, jak je znázorněno na následujícím obrázku:

![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/vnet-diagram.png)

Virtuální síť Azure je reprezentace vaší vlastní sítě v cloudu. Můžete ovládat své sítě Azure a určovat bloky adres DHCP, nastavení DNS, zásady zabezpečení a směrování. Další informace o konceptech virtuálních sítí najdete v článku [Přehled virtuálních sítí](virtual-networks-overview.md). Provedením následujících kroků vytvoříte prostředky uvedené na obrázku:

1. [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet)
2. [Vytvoření dvou virtuálních počítačů, každý s jedním síťovým rozhraním](#create-vms), a přidružení skupiny zabezpečení sítě (NSG) ke každému síťovému rozhraní
3. [Připojení k virtuálním počítačům a z virtuálních počítačů](#connect-to-from-vms)
4. [Odstranění všech prostředků](#delete-resources). Za poskytnutí některých prostředků, které v tomto cvičení vytvoříte, se vám budou účtovat poplatky. Abyste tyto poplatky minimalizovali, po dokončení cvičení nezapomeňte dokončit postup v této části, kterým odstraníte vytvořené prostředky.

Po dokončení kroků v tomto článku získáte základní znalosti o používání virtuálních sítí. K dispozici jsou i další kroky, které vám pomůžou k hlubšímu porozumění virtuálním sítím.

## <a name="create-vnet"></a>Vytvoření virtuální sítě se dvěma podsítěmi

Pokud chcete vytvořit virtuální síť se dvěma podsítěmi, dokončete následující kroky. Různé podsítě se obvykle používají k řízení toku přenosů mezi podsítěmi.

1. Přihlaste se k [portálu Azure](<https://portal.azure.com>). Pokud ještě účet nemáte, můžete si zaregistrovat [zkušební verzi na měsíc zdarma](https://azure.microsoft.com/free). 
2. V podokně portálu **Oblíbené** klikněte na **Nový**.
3. V okně **Nový** klikněte na **Sítě**. V okně **Sítě** klikněte na **Virtuální síť**, jak je znázorněno na následujícím obrázku:

    ![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/virtual-network.png)

4.  V okně **Virtuální síť** ponechte vybraný *Resource Manager* jako model nasazení a klikněte na **Vytvořit**.
5.  V okně **Vytvořit virtuální síť**, které se zobrazí, zadejte následující hodnoty a potom klikněte na **Vytvořit**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyVNet*|Název musí být v rámci skupiny prostředků jedinečný.|
    |**Adresní prostor**|*10.0.0.0/16*|Můžete zadat libovolný adresní prostor v notaci CIDR.|
    |**Název podsítě**|*Front-end*|Název podsítě musí být v rámci virtuální sítě jedinečný.|
    |**Rozsah adres podsítě**|*10.0.0.0/24*| Zadaný rozsah musí existovat v rámci adresního prostoru, který jste nadefinovali pro virtuální síť.|
    |**Předplatné**|*[Vaše předplatné]*|Vyberte předplatné, ve kterém se má virtuální síť vytvořit. Virtuální síť existuje v jednom předplatném.|
    |**Skupina prostředků**|**Vytvořit novou:** *MyRG*|Vytvořte skupinu prostředků. Název skupiny prostředků musí být v rámci vybraného předplatného jedinečný. Další informace o skupinách prostředků najdete v článku s přehledem [Resource Manageru](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups).|
    |**Umístění**|*Západní USA*| Obvykle je vybráno umístění co nejblíže vašemu fyzickému umístění.|

    Vytvoření virtuální sítě trvá několik sekund. Jakmile se vytvoří, zobrazí se řídicí panel webu Azure Portal.

6. Když máte vytvořenou virtuální síť, na webu Azure Portal v podokně **Oblíbené** klikněte na **Všechny prostředky**. Klikněte na virtuální síť **MyVNet** v okně **Všechny prostředky**. Pokud předplatné, které jste vybrali, již obsahovalo nějaké prostředky, můžete zadat *MyVNet* do pole **Filtrovat podle názvu...** pro snadný přístup k virtuální síti.
7. Otevře se okno **MyVNet** s informacemi o virtuální síti, jak je znázorněno na následujícím obrázku:

    ![Diagram virtuální sítě](./media/virtual-network-get-started-vnet-subnet/myvnet.png)

8. Jak je vidět na předchozím obrázku, kliknutím na **Podsítě** zobrazíte seznam podsítí v rámci virtuální sítě. Jediná existující podsíť je **Front-end** – podsíť, kterou jste vytvořili v kroku 5.
9. V okně MyVNet – Podsítě kliknutím na **+ Podsíť** vytvořte podsíť s použitím následujících informací, a kliknutím na **OK** podsíť vytvořte:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*Back-end*|Název musí být v rámci virtuální sítě jedinečný.|
    |**Rozsah adres**|*10.0.1.0/24*|Zadaný rozsah musí existovat v rámci adresního prostoru, který jste nadefinovali pro virtuální síť.|
    |**Skupina zabezpečení sítě** a **Tabulka směrování**|*Žádný* (výchozí)|Skupinám zabezpečení sítě se věnujeme dále v tomto článku. Další informace o trasách definovaných uživatelem najdete v článku [Trasy definované uživatelem](virtual-networks-udr-overview.md).|

10. Po přidání nové podsítě do virtuální sítě můžete zavřít okno **MyVNet – Podsítě** a potom zavřete okno **Všechny prostředky**.

## <a name="create-vms"></a>Vytvoření virtuálních počítačů

Když máte vytvořenou virtuální síť a podsítě, můžete vytvořit virtuální počítače. Pro toto cvičení bude na obou virtuálních počítačích operační systém Windows Server, ale může na nich být jakýkoli operační systém podporovaný v Azure, a to včetně několika různých linuxových distribucí.

### <a name="create-web-server-vm"></a>Vytvoření virtuálního počítače s webovým serverem

Pokud chcete vytvořit virtuální počítač s webovým serverem, proveďte následující postup:

1. V podokně webu Azure Portal Oblíbené klikněte na **Nový**, **Výpočetní prostředky** a potom na **Windows Server 2016 Datacenter**.
2. V okně **Windows Server 2016 Datacenter** klikněte na **Vytvořit**.
3. V okně **Základy**, které se zobrazí, zadejte nebo vyberte následující hodnoty a klikněte na **OK**:

    |**Nastavení**| **Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyWebServer*|Tento virtuální počítač slouží jako webový server, ke kterému se připojují internetové prostředky.|
    |**Typ disku virtuálního počítače**|*SSD*|
    |**Uživatelské jméno**|*Nějaké si zvolte*|
    |**Heslo a Potvrzení hesla**|*Nějaké si zvolte*|
    | **Předplatné**|*<Your subscription>*|Předplatné musí být stejné jako předplatné, které jste vybrali v kroku 5 části [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet) tohoto článku. Virtuální počítač a virtuální síť, ke které virtuální počítač připojíte, musí existovat v rámci stejného předplatného.|
    |**Skupina prostředků**|**Použít existující:** Vyberte *MyRG*|I když používáme stejnou skupinu prostředků, jakou jsme použili pro virtuální síť, prostředky nemusí existovat ve stejné skupině prostředků.|
    |**Umístění**|*Západní USA*|Umístění musí být stejné jako umístění, které jste zadali v kroku 5 části [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet) tohoto článku. Virtuální počítače a virtuální sítě, ke kterým se připojují, musí existovat ve stejném umístění.|

4. V okně **Zvolit velikost** klikněte na *DS1_V2 Standard* a pak na **Vybrat**. Seznam všech velikostí virtuálních počítačů s Windows, které Azure podporuje, najdete v článku [Velikosti virtuálních počítačů s Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
5. V okně **Nastavení** zadejte nebo vyberte následující hodnoty a klikněte na **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Úložiště: Použít spravované disky**|*Ano*||
    |**Virtuální síť**| Vyberte *MyVNet*|Můžete vybrat jakoukoli virtuální síť, která existuje ve stejném umístění jako virtuální počítač, který vytváříte. Další informace o virtuálních sítích a podsítích najdete v článku [Virtuální síť](virtual-networks-overview.md).|
    |**Podsíť**|Vyberte *Front-end*|Můžete vybrat jakoukoli podsíť, která existuje v rámci vybrané virtuální sítě.|
    |**Veřejná IP adresa**|Přijměte výchozí hodnotu|Veřejná IP adresa umožňuje připojení k virtuálnímu počítači z internetu. Další informace o veřejných IP adresách najdete v článku [IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).|
    |**Skupina zabezpečení sítě (brána firewall)**|Přijměte výchozí hodnotu|Klikněte na výchozí skupinu zabezpečení sítě **(nové) MyWebServer-nsg**, kterou portál vytvořil, a zobrazte její nastavení. Otevře se okno **Vytvořit skupinu zabezpečení sítě**. V něm si všimněte, že obsahuje jedno příchozí pravidlo, které umožňuje provoz prostřednictvím protokolu TCP/3389 (RDP) z jakékoli zdrojové IP adresy.|
    |**Všechny ostatní hodnoty**|Přijměte výchozí hodnoty|Další informace o zbývajících nastaveních najdete v článku [O virtuálních počítačích](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|

    Skupiny zabezpečení sítě (NSG) umožňují vytvořit příchozí a odchozí pravidla pro typ síťového provozu, který může téct do a z virtuálního počítače. Ve výchozím nastavení je zakázán veškerý příchozí provoz do virtuálního počítače. Pro produkční webový server můžete přidat další příchozí pravidla pro port TCP/80 (HTTP) a TCP/443 (HTTPS). Pro odchozí provoz žádné pravidlo není, protože ve výchozím nastavení je veškerý odchozí provoz povolen. Přidáváním a odebíráním pravidel můžete řídit provoz pro vaše zásady. Další informace o skupinách NSG najdete v tématu [Skupiny zabezpečení sítě](virtual-networks-nsg.md).

6.  V okně **Souhrn** zkontroluje nastavení a klikněte na **OK**. Vytvoří se virtuální počítač. Jakmile se vytvoří virtuální počítač, na řídicím panelu portálu se zobrazí dlaždice stavu. Vytvoření může trvat několik minut. Nemusíte čekat na dokončení. Můžete pokračovat k dalšímu kroku, zatímco se virtuální počítač vytváří.

### <a name="create-database-server-vm"></a>Vytvoření virtuálního počítače s databázovým serverem

Pokud chcete vytvořit virtuální počítač s databázovým serverem, proveďte následující postup:

1.  V podokně Oblíbené klikněte na **Nový**, **Výpočetní prostředky** a potom na **Windows Server 2016 Datacenter**.
2.  V okně **Windows Server 2016 Datacenter** klikněte na **Vytvořit**.
3.  V okně **Základy** zadejte nebo vyberte následující hodnoty a klikněte na **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |---|---|---|
    |**Název**|*MyDBServer*|Tento virtuální počítač slouží jako databázový server, ke kterému se připojuje webový server, ale z internetu k němu není přístup.|
    |**Typ disku virtuálního počítače**|*SSD*||
    |**Uživatelské jméno**|Nějaké si zvolte||
    |**Heslo a Potvrzení hesla**|Nějaké si zvolte||
    |**Předplatné**|<Your subscription>|Předplatné musí být stejné jako předplatné, které jste vybrali v kroku 5 části [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet) tohoto článku.|
    |**Skupina prostředků**|**Použít existující:** Vyberte *MyRG*|I když používáme stejnou skupinu prostředků, jakou jsme použili pro virtuální síť, prostředky nemusí existovat ve stejné skupině prostředků.|
    |**Umístění**|*Západní USA*|Umístění musí být stejné jako umístění, které jste zadali v kroku 5 části [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet) tohoto článku.|

4.  V okně **Zvolit velikost** klikněte na *DS1_V2 Standard* a pak na **Vybrat**.
5.  V okně **Nastavení** zadejte nebo vyberte následující hodnoty a klikněte na **OK**:

    |**Nastavení**|**Hodnota**|**Podrobnosti**|
    |----|----|---|
    |**Úložiště: Použít spravované disky**|*Ano*||
    |**Virtuální síť**|Vyberte *MyVNet*|Můžete vybrat jakoukoli virtuální síť, která existuje ve stejném umístění jako virtuální počítač, který vytváříte.|
    |**Podsíť**|Vyberte *Back-end* tak, že kliknete na **Podsíť** a potom vyberete **Back-end** v okně **Zvolit podsíť**.|Můžete vybrat jakoukoli podsíť, která existuje v rámci vybrané virtuální sítě.|
    |**Veřejná IP adresa**|Žádná – Klikněte na výchozí adresu a potom klikněte na **Žádná** v okně **Zvolit veřejnou IP adresu**.|Bez veřejné IP adresy se k virtuálnímu počítači můžete připojit jenom z jiného virtuálního počítače připojeného ke stejné virtuální síti. Nemůžete se k němu připojit přímo z internetu.|
    |**Skupina zabezpečení sítě (brána firewall)**|Přijměte výchozí hodnotu| Stejně jako výchozí skupina zabezpečení sítě vytvořená pro virtuální počítač MyWebServer, obsahuje i tato skupina zabezpečení sítě ve výchozím nastavení stejné příchozí pravidlo. Pro databázový server můžete přidat další příchozí pravidlo pro protokol TCP/1433 (MS SQL). Pro odchozí provoz žádné pravidlo není, protože ve výchozím nastavení je veškerý odchozí provoz povolen. Přidáváním a odebíráním pravidel můžete řídit provoz pro vaše zásady.|
    |**Všechny ostatní hodnoty**|Přijměte výchozí hodnoty||

6.  V okně **Souhrn** zkontroluje nastavení a klikněte na **OK**. Vytvoří se virtuální počítač. Jakmile se vytvoří virtuální počítač, na řídicím panelu portálu se zobrazí dlaždice stavu. Vytvoření může trvat několik minut. Nemusíte čekat na dokončení. Můžete pokračovat k dalšímu kroku, zatímco se virtuální počítač vytváří.

## <a name="review"></a>Kontrola prostředků

Přestože jste vytvořili jednu virtuální síť a dva virtuální počítače, Azure Portal vytvořil ve skupině prostředků MyRG několik dalších prostředků za vás. Pokud si chcete prohlédnout obsah skupiny prostředků MyRG, proveďte následující kroky:

1. V podokně **Oblíbené** klikněte na **Další služby**.
2. V podokně **Další služby** zadejte *Skupiny prostředků* do pole, které obsahuje slovo *Filtrovat*. Jakmile ve filtrovaném seznamu uvidíte položku **Skupiny prostředků**, klikněte na ni.
3. V podokně **Skupiny prostředků** klikněte na skupinu prostředků *MyRG*. Pokud ve vašem předplatném existuje mnoho skupin prostředků, můžete zadat *MyRG* do pole, které obsahuje text *Filtrovat podle názvu...* pro rychlý přístup ke skupině prostředků MyRG.
4.  V okně **MyRG** uvidíte, že skupina prostředků obsahuje 12 prostředků, jak je znázorněno na následujícím obrázku:

    ![Obsah skupiny prostředků](./media/virtual-network-get-started-vnet-subnet/resource-group-contents.png)

Další informace o virtuálních počítačích, discích a účtech úložiště najdete v článcích, které obsahují přehledy [virtuálních počítačů](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [disků](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [účtů úložiště](../storage/storage-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Vidíte dvě výchozí skupiny zabezpečení sítě, které pro vás portál vytvořil. Vidíte také, že portál vytvořil dva prostředky síťového rozhraní. Síťové rozhraní umožňuje připojení virtuálního počítače k ostatním prostředkům přes virtuální síť. Další informace o síťových rozhraních najdete v článku [Síťová rozhraní](virtual-network-network-interface-overview.md). Portál také vytvořil jeden prostředek veřejné IP adresy. Veřejné IP adresy jsou jedním z nastavení prostředku veřejné IP adresy. Další informace o veřejných IP adresách najdete v článku [IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).

## <a name="connect-to-from-vms"></a>Připojení k virtuálním počítačům

Když máte vytvořenou virtuální síť a dva virtuální počítače, můžete se teď připojit k virtuálním počítačům. K tomu slouží postupy v následujících částech:

### <a name="connect-from-internet"></a>Připojení k virtuálnímu počítači s webovým serverem z internetu

Pokud se chcete připojit k virtuálnímu počítači s webovým serverem přes internet, proveďte následující kroky:

1. Na portálu otevřete skupinu prostředků MyRG podle postupu uvedeného v části [Kontrola prostředků](#review) v tomto článku.
2. V okně **MyRG** klikněte na virtuální počítač **MyWebServer**.
3. V okně **MyWebServer** klikněte na **Připojit**, jak je znázorněno na následujícím obrázku:

    ![Připojení k virtuálnímu počítači s webovým serverem](./media/virtual-network-get-started-vnet-subnet/webserver.png)

4. Povolte v prohlížeči stažení souboru *MyWebServer.rdp* a potom ho otevřete.
5. Pokud se zobrazí dialogové okno s informací o tom, že nelze ověřit vydavatele vzdáleného připojení, klikněte na **Připojit**.
6. Při zadávání přihlašovacích údajů se ujistěte, že se přihlašujete pomocí uživatelského jména a hesla, které jste zadali v kroku 3 části [Vytvoření virtuálního počítače s webovým serverem](#create-web-server-vm) tohoto článku. Pokud v zobrazeném okně **Zabezpečení systému Windows** nejsou uvedeny správné přihlašovací údaje, možná budete muset kliknout na **Další možnosti** a potom na **Použít jiný účet**, abyste mohli zadat správné uživatelské jméno a heslo. Kliknutím na **OK** se připojte k virtuálnímu počítači.
7. Pokud se zobrazí okno **Připojení ke vzdálené ploše** s informací, že nelze ověřit identitu vzdáleného počítače, klikněte na **Ano**.
8. Nyní jste připojeni k virtuálnímu počítači MyWebServer z internetu. Připojení ke vzdálené ploše ponechte otevřené, abyste mohli dokončit kroky v další části.

Toto vzdálené připojení je k veřejné IP adrese přiřazené prostředku veřejné IP adresy, kterou portál vytvořil v kroku 5 části [Vytvoření virtuální sítě se dvěma podsítěmi](#create-vnet) tohoto článku. Připojení je umožněno, protože výchozí pravidlo vytvořené ve skupině zabezpečení sítě **MyWebServer-nsg** povolilo k tomuto virtuálnímu počítači příchozí provoz prostřednictvím protokolu TCP/3389 (RDP) z jakékoli zdrojové IP adresy. Pokud se pokusíte k virtuálnímu počítači připojit přes jakýkoli jiný port, připojení selže, jestliže do skupiny zabezpečení sítě nepřidáte další příchozí pravidla povolující další porty.

>[!NOTE]
>Pokud do skupiny zabezpečení sítě přidáte další příchozí pravidla, ujistěte se, že stejné porty jsou otevřené i v bráně Windows Firewall, jinak připojení selže.
>

### <a name="connect-to-internet"></a>Připojení k internetu z virtuálního počítače s webovým serverem

Pokud se chcete připojit k internetu z virtuálního počítače s webovým serverem, proveďte následující kroky:

1. Pokud ještě nemáte otevřené vzdálené připojení k virtuálnímu počítači MyWebServer, vytvořte ho provedením kroků v části [Připojení k virtuálnímu počítači s webovým serverem z internetu](#connect-from-internet) tohoto článku.
2. Z plochy Windows otevřete aplikaci Internet Explorer. V dialogovém okně **Nastavit aplikaci Internet Explorer 11** klikněte na **Nepoužívat doporučené nastavení** a potom na **OK**. Pro produkční server doporučujeme přijmout doporučené nastavení.
3. Do adresního řádku aplikace Internet Explorer zadejte [bing.com](http:www.bing.com). Pokud se zobrazí dialogové okno aplikace Internet Explorer, klikněte na **Přidat**, potom na **Přidat** v dialogovém okně **Důvěryhodné servery** a klikněte na **Zavřít**. Tento postup opakujte pro všechna další dialogová okna aplikace Internet Explorer.
4. Na stránce vyhledávání Bing zadejte *whatsmyipaddress* a potom klikněte na tlačítko lupy. Bing vrátí veřejnou IP adresu přiřazenou k prostředku veřejné IP adresy, kterou portál vytvořil při vytváření virtuálního počítače. Pokud prozkoumáte nastavení prostředku **MyWebServer-ip**, uvidíte stejnou IP adresu přiřazenou k prostředku veřejné IP adresy, jak je znázorněno na následujícím obrázku. IP adresa přiřazená k vašemu virtuálnímu počítači je však jiná.

    ![Připojení k virtuálnímu počítači s webovým serverem](./media/virtual-network-get-started-vnet-subnet/webserver-pip.png)

5.  Připojení ke vzdálené ploše ponechte otevřené, abyste mohli dokončit kroky v další části.

Z virtuálního počítače se můžete připojit k internetu, protože veškerá odchozí připojení z virtuálního počítače jsou ve výchozím nastavení povolená. Odchozí připojení můžete omezit přidáním dalších pravidel do skupiny zabezpečení sítě, která se používá pro síťové rozhraní, pro podsíť, ke které je síťové rozhraní připojeno nebo pro obojí.

Pokud je virtuální počítač uveden do zastaveného (uvolněného) stavu pomocí portálu, veřejná IP adresa se může změnit. Pokud vyžadujete, aby se veřejná IP adresa nikdy neměnila, můžete pro IP adresu použít metodu statického přidělování namísto metody dynamického přidělování (která je výchozí). Další informace o rozdílech mezi metodami přidělování najdete v článku [Typy IP adres a metody jejich přidělování](virtual-network-ip-addresses-overview-arm.md).

### <a name="webserver-to-dbserver"></a>Připojení k virtuálnímu počítači s databázovým serverem z virtuálního počítače s webovým serverem

Pokud se chcete připojit k virtuálnímu počítači s databázovým serverem z virtuálního počítače s webovým serverem, proveďte následující kroky:

1. Pokud ještě nemáte otevřené vzdálené připojení k virtuálnímu počítači MyWebServer, vytvořte ho provedením kroků v části [Připojení k virtuálnímu počítači s webovým serverem z internetu](#connect-from-internet) tohoto článku.
2. Klikněte na tlačítko Start v levém dolním rohu plochy Windows a začněte zadávat *připojení ke vzdálené*. Jakmile se v seznamu nabídky Start zobrazí **Připojení ke vzdálené ploše**, klikněte na něj.
3. V dialogovém okně **Připojení ke vzdálené ploše** zadejte jako název počítače *MyDBServer* a klikněte na **Připojit**.
4. Zadejte uživatelské jméno a heslo, které jste zadali v kroku 3 části [Vytvoření virtuálního počítače s databázovým serverem](#create-database-server-vm) tohoto článku, a potom klikněte na **OK**.
5. Pokud se zobrazí dialogové okno s informací, že nelze ověřit identitu vzdáleného počítače, klikněte na **Ano**.
6. Připojení ke vzdálené ploše obou serverů ponechte otevřené, abyste mohli dokončit kroky v další části.

Připojení k virtuálnímu počítači s databázovým serverem z virtuálního počítače s webovým serverem můžete navázat z následujících důvodů:

- Příchozí připojení prostřednictvím protokolu TCP/3389 jsou povolena pro jakékoli zdrojové IP adresy ve výchozí skupině zabezpečení sítě vytvořené v kroku 5 části [Vytvoření virtuálního počítače s databázovým serverem](#create-database-server-vm) tohoto článku.
- Připojení jste navázali z virtuálního počítače s webovým serverem, který je připojený ke stejné virtuální síti jako virtuální počítač s databázovým serverem. Abyste se mohli připojit k virtuálnímu počítači, který nemá přiřazenou veřejnou IP adresu, musíte se připojit z jiného virtuálního počítače připojeného ke stejné virtuální síti, dokonce i kdyby byl připojený k jiné podsíti.
- I když jsou virtuální počítače připojené k různým podsítím, Azure vytváří výchozí trasy, které umožňují připojení mezi podsítěmi. Výchozí trasy ale můžete přepsat vytvořením vlastních tras. Další informace o směrování v Azure najdete v článku [Trasy definované uživatelem](virtual-networks-udr-overview.md).

Pokud se pokusíte navázat vzdálené připojení k virtuálnímu počítači s databázovým serverem z internetu, jako jste to udělali v části [Připojení k virtuálnímu počítači s webovým serverem z internetu](#connect-from-internet) tohoto článku, uvidíte, že možnost **Připojit** je zobrazena šedě. Možnost Připojit je zobrazena šedě, protože k tomuto virtuálnímu počítači není přiřazena žádná veřejná IP adresa, takže příchozí připojení z internetu není možné.

### <a name="connect-to-the-internet-from-the-database-server-vm"></a>Připojení k internetu z virtuálního počítače s databázovým serverem

Pokud se chcete připojit k internetu z virtuálního počítače s databázovým serverem, proveďte následující kroky:

1. Pokud ještě nemáte otevřené vzdálené připojení k virtuálnímu počítači MyDBServer z virtuálního počítače MyWebServer, proveďte kroky v části [Připojení k virtuálnímu počítači s databázovým serverem z virtuálního počítače s webovým serverem](#webserver-to-dbserver) tohoto článku.
2. Z plochy Windows na virtuálním počítači MyDBServer otevřete aplikaci Internet Explorer a na dialogová okna reagujte stejně jako v krocích 2 a 3 části [Připojení k internetu z virtuálního počítače s webovým serverem](#connect-to-internet) tohoto článku.
3. Do adresního řádku zadejte [bing.com](http:www.bing.com).
4. V dialogovém okně aplikace Internet Explorer, které se zobrazí, klikněte na **Přidat**, a potom v dialogovém okně **Důvěryhodné servery** klikněte na **Přidat** a **Zavřít**. Stejné kroky proveďte ve všech dalších dialogových oknech, která se případně zobrazí.
5. Na stránce vyhledávání Bing zadejte *whatsmyipaddress* a potom klikněte na tlačítko lupy. Bing vrátí veřejnou IP adresu, která je aktuálně přiřazená k virtuálnímu počítači infrastrukturou Azure. 6. Zavřete připojení ke vzdálené ploše virtuálního počítače MyDBServer z virtuálního počítače MyWebServer a potom zavřete vzdálené připojení k virtuálnímu počítači MyWebServer.

Odchozí připojení k internetu je povoleno, protože ve výchozím nastavení je povolen veškerý odchozí provoz, i když k virtuálnímu počítači MyDBServer není přiřazen prostředek veřejné IP adresy. Všechny virtuální počítače se ve výchozím nastavení můžou připojit k internetu bez ohledu na to, jestli k nim je nebo není přiřazen prostředek veřejné IP adresy. Nemůžete se ale připojit k veřejné IP adrese z internetu, jako jste mohli v případě virtuálního počítače MyWebServer s přiřazeným prostředkem veřejné IP adresy.

## <a name="delete-resources"></a>Odstranění všech prostředků

Pokud chcete odstranit všechny prostředky vytvořené v rámci tohoto článku, proveďte následující kroky:

1. Chcete-li zobrazit skupinu prostředků MyRG vytvořenou v rámci tohoto článku, proveďte kroky 1 až 3 v části [Kontrola prostředků](#review) tohoto článku. Znovu si prohlédněte prostředky ve skupině prostředků. Pokud jste vytvořili skupinu prostředků MyRG podle předchozích postupů, uvidíte 12 prostředků jako na obrázku v kroku 3.
2. V okně MyRG klikněte na tlačítko **Odstranit**.
3. Portál požaduje, abyste zadali název skupiny prostředků pro potvrzení, že ji skutečně chcete odstranit. Pokud uvidíte jiné prostředky než prostředky uvedené v kroku 3 části [Kontrola prostředků](#review) tohoto článku, klikněte na **Zrušit**. Pokud vidíte pouze 12 prostředků vytvořených jako součást tohoto článku, zadejte název skupiny prostředků *MyRG* a potom klikněte na **Odstranit**. Odstraněním skupiny prostředků se odstraní všechny prostředky v rámci dané skupiny prostředků. Proto nikdy nezapomeňte před odstraněním skupiny prostředků zkontrolovat její obsah. Portál odstraní všechny prostředky v rámci skupiny prostředků a potom odstraní samotnou skupinu prostředků. Tento proces trvá několik minut.

## <a name="next-steps"></a>Další kroky

V tomto cvičení jste vytvořili virtuální síť a dva virtuální počítače. Během vytváření virtuálních počítačů jste zadali některá vlastní nastavení a přijali několik výchozích nastavení. Doporučujeme, abyste si před nasazením virtuálních sítí a virtuálních počítačů v produkčním prostředí přečetli následující články a ujistili se, že rozumíte všem dostupným nastavením:

- [Virtuální sítě](virtual-networks-overview.md)
- [Veřejné IP adresy](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Síťová rozhraní](virtual-network-network-interface-overview.md)
- [Skupiny zabezpečení sítě](virtual-networks-nsg.md)
- [Virtual Machines](../virtual-machines/virtual-machines-windows-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

