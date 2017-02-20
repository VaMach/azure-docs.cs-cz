---
title: "Vytvoření připojení sítě site-to-site VPN mezi dvěma virtuálními sítěmi v různých prostředích Azure Stack PoC | Dokumentace Microsoftu"
description: "Podrobný postup, který umožňuje správci cloudu vytvořit připojení sítě site-to-site VPN mezi dvěma prostředími POC jedním uzlem v TP2."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: scottnap
translationtype: Human Translation
ms.sourcegitcommit: 5104c7996de9dc0597e65be31c28a9aaa1243a90
ms.openlocfilehash: d324290caf5b5a085a2daf67e541c295dffda732


---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-poc-environments"></a>Vytvoření připojení sítě site-to-site VPN mezi dvěma virtuálními sítěmi v různých prostředích Azure Stack PoC
## <a name="overview"></a>Přehled
V tomto článku najdete postup vytvoření připojení sítě site-to-site VPN mezi dvěma virtuálními sítěmi ve dvou samostatných prostředích Azure Stack pro testování konceptu (PoC). Nakonfigurujete tato připojení a naučíte se, jak fungují brány VPN ve službě Azure Stack.

> [!NOTE]
> Tento dokument se vztahuje konkrétně k Azure Stack TP2 POC.
> 
> 

### <a name="connection-diagram"></a>Diagram připojení
Následující diagram ukazuje, jak by měla konfigurace po dokončení vypadat:

![](media/azure-stack-create-vpn-connection-one-node-tp2/image1.png)

### <a name="before-you-begin"></a>Než začnete
Než začnete, ujistěte se, že máte následující položky, abyste mohli tuto konfiguraci dokončit:

* Dva servery, které splňují požadavky na hardware Azure Stack POC definované v tématu [Požadavky nasazení Azure Stack](azure-stack-deploy.md), a další požadavky definované v tomto dokumentu.
* Balíček pro nasazení Azure Stack Technical Preview 2.

## <a name="deploy-the-poc-environments"></a>Nasazení prostředí POC
K dokončení této konfigurace nasadíte dvě prostředí Azure Stack POC.

* Pro každé prostředí POC, které nasazujete, můžete použít postup nasazení, který je podrobně popsaný v článku [Nasazení Azure Stack POC](azure-stack-run-powershell-script.md).
  Prostředí POC jsou v tomto dokumentu obecně označená jako *POC1* a *POC2*.

## <a name="configure-quotas-for-compute-network-and-storage"></a>Konfigurace kvót pro služby Compute, Network a Storage
Nejdřív musíte nakonfigurovat *kvóty* pro služby Compute, Network a Storage. Tyto služby je možné přidružit k *plánu* a potom k *nabídce*, k jejímuž odběru se tenanti mohou přihlásit.

> [!NOTE]
> Tyto kroky musíte provést pro každé prostředí Azure Stack POC.
> 
> 

Vytváření kvót pro služby se oproti TP1 změnilo. Kroky pro vytvoření kvót v TP2 najdete na adrese <http://aka.ms/mas-create-quotas>. Pro účely tohoto cvičení můžete pro všechna nastavení kvót přijmout výchozí hodnoty.

## <a name="create-a-plan-and-offer"></a>Vytvoření plánu a nabídky
[Plány](azure-stack-key-features.md) představují seskupení jedné nebo více služeb. Jako poskytovatel můžete vytvořit plány, které nabídnete svým tenantům. Vaši tenanti se pak můžou přihlásit k odběru nabídek, aby mohli používat plány a služby, které k nim patří.

> [!NOTE]
> Tyto kroky musíte provést pro každé prostředí Azure Stack POC.
> 
> 

1. Nejdřív vytvořte plán. Můžete k tomu použít kroky v online článku [Vytvoření plánu](azure-stack-create-plan.md).
2. Pomocí postupu popsaného v tématu [Vytvoření nabídky ve službě Azure Stack](azure-stack-create-offer.md) vytvořte nabídku.
3. Přihlaste se k portálu jako správce tenantů a [přihlaste se k odběru nabídky, kterou jste vytvořili](azure-stack-subscribe-plan-provision-vm.md).

## <a name="create-the-network-resources-in-poc-1"></a>Vytvoření síťových prostředků v POC 1
Teď vytvoříte prostředky, které budete potřebovat pro vytvoření požadované konfigurace. Následující kroky popisují, co budete dělat. Tyto pokyny ukazují, jaky toto prostředky vytvořit pomocí portálu, ale stejných výsledků můžete dosáhnout i pomocí PowerShellu.

![](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="log-in-as-a-tenant"></a>Přihlášení jako tenant
Správce služby se může přihlásit jako tenant, aby otestoval plány, nabídky a předplatné, které můžou jeho tenanti používat. Pokud ho ještě nemáte, [vytvořte si účet tenanta](azure-stack-add-new-user-aad.md) se kterým se potom přihlásíte.

### <a name="create-the-virtual-network--vm-subnet"></a>Vytvoření virtuální sítě a podsítě virtuálních počítačů
1. Přihlaste se pomocí účtu tenanta.
2. Na webu Azure Portal klikněte na **Nový**.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)
3. V nabídce Marketplace vyberte **Síť**.
4. V nabídce klikněte na položku **Virtuální síť**.
5. U dolního okraje okna s popisem prostředku klikněte na **Vytvořit**. Do příslušných polí zadejte následující hodnoty podle této tabulky.
   
   | **Pole** | **Hodnota** |
   | --- | --- |
   | Name (Název) |vnet-01 |
   | Adresní prostor |10.0.10.0/23 |
   | Název podsítě |subnet-01 |
   | Rozsah adres podsítě |10.0.10.0/24 |
6. V poli **Předplatné** by se mělo zobrazovat předplatné, které jste vytvořili dřív.
7. Pokud už máte skupinu prostředků, můžete vybrat **Použít stávající**, nebo můžete vytvořit novou skupinu prostředků.
8. Ověřte výchozí umístění.
9. Klikněte na možnost **Vytvořit**.

### <a name="create-the-gateway-subnet"></a>Vytvoření podsítě brány
1. Na řídicím panelu otevřete prostředek virtuální sítě, který jste právě vytvořili (Vnet-01).
2. V okně Nastavení vyberte **Podsítě**.
3. Kliknutím na **Podsíť brány** přidejte k virtuální síti podsíť brány.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)
4. Ve výchozím nastavení je název této podsítě nastavený na **GatewaySubnet**.
   Podsítě brány jsou speciální a musí mít tento konkrétní název, aby fungovaly správně.
5. Do pole **Rozsah adres** zadejte **10.0.11.0/24**.
6. Kliknutím na **Vytvořit** vytvořte podsíť brány.

### <a name="create-the-virtual-network-gateway"></a>Vytvoření brány virtuální sítě
1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Síť**.
3. V seznamu síťových prostředků vyberte **bránu virtuální sítě**.
4. Zkontrolujte popis a klikněte na **Vytvořit**.
5. Do pole **Název** zadejte **GW1**.
6. Pro výběr virtuální sítě klikněte na položku **Virtuální síť**.
   V seznamu vyberte **Vnet-01**.
7. Klikněte na položku nabídky **Veřejná IP adresa**. Po otevření okna **Zvolit veřejnou IP adresu** klikněte na **Vytvořit novou**.
8. Do pole **Název** zadejte **GW1-PiP** a klikněte na **OK**.
9. V poli **Typ brány** by ve výchozím nastavení měla být vybraná možnost **VPN**. Toto nastavení zachovejte.
10. Jako **Typ VPN** by ve výchozím nastavení měla být vybraná možnost **Založená na trasách**.
    Toto nastavení zachovejte.
11. Ověřte, že nastavení **Předplatné** a **Umístění** jsou správná. Pokud chcete, můžete prostředek k řídicímu panelu připnout. Klikněte na možnost **Vytvořit**.

### <a name="create-the-local-network-gateway"></a>Vytvoření brány místní sítě
Implementace *brány místní sítě* v tomto nasazení Azure Stack pro účely vyhodnocení je trochu jiná než ve skutečném nasazení Azure.

Stejně jako v Azure se využívá koncepce brány místní sítě. Ale brána místní sítě v nasazení Azure představuje místní fyzické zařízení (u tenanta), které používáte pro připojení k bráně virtuální sítě v Azure. Ale v tomto nasazení Azure Stack pro účely vyhodnocení jsou na obou koncích připojení brány virtuální sítě!

Můžete se na to obecněji dívat také tak, že prostředek brány místní sítě vždycky představuje vzdálenou bránu na druhém konci připojení. Vzhledem ke způsobu, jakým je navržené prostředí POC, budete muset na virtuálním počítači pro překlad adres druhého prostředí POC jako veřejnou IP adresu brány místní sítě zadat IP adresu externího síťového adaptéru. Potom na tomto virtuálním počítači vytvoříte mapování překladu adres, abyste zajistili, že jsou oba konce správně propojené.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Získání IP adresy externího adaptéru virtuálního počítače pro překlad adres
1. Přihlaste se k fyzickému počítači Azure Stack pro POC2.
2. Stisknutím [klávesa Windows] + R otevřete nabídku **Spustit**, zadejte **mstsc** a stiskněte Enter.
3. Do pole **Počítač** zadejte **MAS-BGPNAT01** a klikněte na  **Připojit**.
4. Klikněte na nabídku Start, potom pravým tlačítkem klikněte na **Windows PowerShell** a vyberte **Spustit jako správce**.
5. Zadejte **ipconfig /all**.
6. Najděte ethernetový adaptér připojený k vaší místní síti a poznamenejte si IPv4 adresu, která je k němu přidružená. V našem ukázkovém prostředí je to **10.16.167.195**, ale ve vašem prostředí bude tato hodnota jiná.
7. Tuto adresu si poznamenejte. Použijete ji jako veřejnou IP adresu prostředku brány místní sítě, který jste vytvořili v POC1.

### <a name="create-the-local-network-gateway-resource"></a>Vytvoření prostředku brány místní sítě
1. Připojte se k fyzickému počítači Azure Stack pro POC1.
2. Do pole **Počítač** zadejte **MAS-CON01** a klikněte na **Připojit**.
3. Na webu Azure Portal klikněte na **Nový**.
   
4. V nabídce Marketplace vyberte **Síť**.
5. V seznamu prostředků vyberte **bránu místní sítě**.
6. Do pole **Název** zadejte **POC2-GW**.
7. IP adresu této druhé brány ještě neznáte, ale to je v pořádku, protože se později můžete vrátit a změnit ji. Teď do pole **IP adresa** zadejte **10.16.167.195**.
8. Do pole **Adresní prostor** zadejte adresní prostor virtuální sítě, kterou vytvoříte v POC2. Bude to **10.0.20.0/23**, proto zadejte tuto hodnotu.
9. Ověřte správnost nastavení **Předplatné**, **Skupina prostředků** a **Umístění** a klikněte na **Vytvořit**.

### <a name="create-the-connection"></a>Vytvoření připojení
1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Síť**.
3. V seznamu prostředků vyberte **Připojení**.
4. V okně nastavení **Basic** jako **Typ připojení** zvolte **Site-to-Site (protokol IPSec)**.
5. Vyberte **předplatné**, **skupinu prostředků** a **umístění** a klikněte na **OK**.
6. V okně **Nastavení** zvolte **bránu virtuální sítě** (**GW1**), kterou jste předtím vytvořili.
7. Zvolte **bránu místní sítě** (**POC2-GW**), kterou jste předtím vytvořili.
8. Do pole **Název připojení** zadejte **POC1-POC2**.
9. Do pole **Sdílený klíč (PSK)** zadejte **12345** a klikněte na **OK**.

### <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Pokud chcete ověřit průchod dat přes připojení VPN, budete v obou prostředích POC potřebovat virtuální počítače pro odesílání a příjem dat. Vytvořte virtuální počítač v POC1 a umístěte ho do podsítě virtuálních počítačů ve vaší virtuální síti.

1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Virtual Machines**.
3. V seznamu imagí virtuálních počítačů vyberte image **Windows Server 2012 R2 Datacenter**.
4. V okně **Základy** do pole **Název** zadejte **VM01**.
5. Zadejte platné uživatelské jméno a heslo. Tento účet použijete pro přihlášení k virtuálnímu počítači, až se vytvoří.
6. Zadejte **předplatné**, **skupinu prostředků** a **umístění** a potom klikněte na **OK**.
7. V okně **Velikost** zvolte velikost virtuálního počítače pro tuto instanci a potom klikněte na **Vybrat**.
8. V okně **Nastavení** můžete použít výchozí hodnoty. Jenom zkontrolujte, že je vybraná virtuální síť **VNET-01** a že podsíť je nastavená na **10.0.10.0/24**. Klikněte na tlačítko **OK**.
9. Zkontrolujte nastavení v okně **Souhrn** a klikněte na **OK**.

## <a name="create-the-network-resources-in-poc-2"></a>Vytvoření síťových prostředků v POC 2
### <a name="log-in-as-a-tenant"></a>Přihlášení jako tenant
Správce služby se může přihlásit jako tenant, aby otestoval plány, nabídky a předplatné, které můžou jeho tenanti používat. Pokud ho ještě nemáte, [vytvořte si účet tenanta](azure-stack-add-new-user-aad.md) se kterým se potom přihlásíte.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Vytvoření virtuální sítě a podsítě virtuálních počítačů
1. Přihlaste se pomocí účtu tenanta.
2. Na webu Azure Portal klikněte na **Nový**.
   
3. V nabídce Marketplace vyberte **Síť**.
4. V nabídce klikněte na **Virtuální síť**.
5. U dolního okraje okna s popisem prostředku klikněte na **Vytvořit**. Do příslušných polí uvedených v tabulce zadejte následující hodnoty.
   
   | **Pole** | **Hodnota** |
   | --- | --- |
   | Name (Název) |vnet-02 |
   | Adresní prostor |10.0.20.0/23 |
   | Název podsítě |subnet-02 |
   | Rozsah adres podsítě |10.0.20.0/24 |
6. V poli **Předplatné** by se mělo zobrazovat předplatné, které jste předtím vytvořili.
7. Pokud už máte skupinu prostředků, můžete vybrat **Použít stávající**, nebo můžete vytvořit novou skupinu prostředků.
8. Ověřte výchozí **umístění**. Pokud chcete, můžete pro usnadnění přístupu připnout virtuální síť k řídicímu panelu.
9. Klikněte na možnost **Vytvořit**.

### <a name="create-the-gateway-subnet"></a>Vytvoření podsítě brány
1. Na řídicím panelu otevřete prostředek virtuální sítě, který jste vytvořili (**Vnet-02**).
2. V okně **Nastavení** vyberte **Podsítě**.
3. Kliknutím na **Podsíť brány** přidejte k virtuální síti podsíť brány.
   
4. Ve výchozím nastavení je název této podsítě nastavený na **GatewaySubnet**.
   Podsítě brány jsou speciální a musí mít tento konkrétní název, aby fungovaly správně.
5. Do pole **Rozsah adres** zadejte **10.0.20.0/24**.
6. Kliknutím na **Vytvořit** vytvořte podsíť brány.

### <a name="create-the-virtual-network-gateway"></a>Vytvoření brány virtuální sítě
1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Síť**.
3. V seznamu síťových prostředků vyberte **bránu virtuální sítě**.
4. Zkontrolujte popis a klikněte na **Vytvořit**.
5. Do pole **Název** zadejte **GW2**.
6. Klikněte na **Virtuální síť** a zvolte požadovanou virtuální síť.
   V seznamu vyberte **Vnet-02**.
7. Klikněte na **Veřejná IP adresa**. Po otevření okna **Zvolit veřejnou IP adresu** klikněte na **Vytvořit novou**.
8. Do pole **Název** zadejte **GW2-PiP** a klikněte na **OK**.
9. V poli **Typ brány** by ve výchozím nastavení měla být vybraná možnost **VPN**. Toto nastavení zachovejte.
10. Jako **Typ VPN** by ve výchozím nastavení měla být vybraná možnost **Založená na trasách**.
    Toto nastavení zachovejte.
11. Ověřte, že nastavení **Předplatné** a **Umístění** jsou správná. Pokud chcete, můžete prostředek k řídicímu panelu připnout. Klikněte na možnost **Vytvořit**.

### <a name="create-the-local-network-gateway"></a>Vytvoření brány místní sítě
#### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Získání IP adresy externího adaptéru virtuálního počítače pro překlad adres
1. Připojte se k fyzickému počítači Azure Stack pro POC1.
2. Stisknutím a podržením kombinace [klávesa Windows] + R otevřete nabídku **Spustit**, zadejte **mstsc** a stiskněte Enter.
3. Do pole **Počítač** zadejte **MAS-BGPNAT01** a klikněte na  **Připojit**.
4. Klikněte na nabídku Start, potom pravým tlačítkem klikněte na **Windows PowerShell** a vyberte **Spustit jako správce**.
5. Zadejte **ipconfig /all**.
6. Najděte ethernetový adaptér připojený k vaší místní síti a poznamenejte si IPv4 adresu, která je k němu přidružená. V našem ukázkovém prostředí je to **10.16.169.131**, ale ve vašem prostředí bude tato hodnota jiná.
7. Tuto adresu si poznamenejte. Použijete ji později jako veřejnou IP adresu prostředku brány místní sítě, který jste vytvořili v POC1.

#### <a name="create-the-local-network-gateway-resource"></a>Vytvoření prostředku brány místní sítě
1. Přihlaste se k fyzickému počítači Azure Stack pro POC2.
2. Do pole **Počítač** zadejte **MAS-CON01** a klikněte na **Připojit**.
3. Na webu Azure Portal klikněte na **Nový**.
   
4. V nabídce Marketplace vyberte **Síť**.
5. V seznamu prostředků vyberte **bránu místní sítě**.
6. Do pole **Název** zadejte **POC1-GW**.
7. Teď budete potřebovat veřejnou IP adresu, kterou jste si poznamenali pro bránu virtuální sítě v POC1. Do pole **IP adresa** zadejte **10.16.169.131**.
8. Do pole **Adresní prostor** zadejte adresní prostor pro **Vnet-01** z POC1 – **10.0.0.0/16**.
9. Ověřte správnost nastavení **Předplatné**, **Skupina prostředků** a **Umístění** a klikněte na **Vytvořit**.

## <a name="create-the-connection"></a>Vytvoření připojení
1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Síť**.
3. V seznamu prostředků vyberte **Připojení**.
4. V okně nastavení **Basic** jako **Typ připojení** zvolte **Site-to-Site (protokol IPSec)**.
5. Vyberte **předplatné**, **skupinu prostředků** a **umístění** a klikněte na **OK**.
6. V okně **Nastavení** zvolte **bránu virtuální sítě** (**GW1**), kterou jste předtím vytvořili.
7. Zvolte **bránu místní sítě** (**POC1-GW**), kterou jste předtím vytvořili.
8. Do pole **Název připojení** zadejte **POC2-POC1**.
9. Do pole **Sdílený klíč (PSK)** zadejte **12345**. Pokud zvolíte jinou hodnotu, nezapomeňte, že MUSÍ odpovídat hodnotě pro sdílený klíč, který jste vytvořili v POC1. Klikněte na tlačítko **OK**.

## <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Vytvořte virtuální počítač v POC1 a umístěte ho do podsítě virtuálních počítačů ve vaší virtuální síti.

1. Na webu Azure Portal klikněte na **Nový**.
   
2. V nabídce Marketplace vyberte **Virtual Machines**.
3. V seznamu imagí virtuálních počítačů vyberte image **Windows Server 2012 R2 Datacenter**.
4. V okně **Základy** do pole **Název** zadejte **VM02**.
5. Zadejte platné uživatelské jméno a heslo. Tento účet použijete pro přihlášení k virtuálnímu počítači, až se vytvoří.
6. Zadejte **předplatné**, **skupinu prostředků** a **umístění** a potom klikněte na **OK**.
7. V okně **Velikost** zvolte velikost virtuálního počítače pro tuto instanci a potom klikněte na **Vybrat**.
8. V okně Nastavení můžete použít výchozí hodnoty. Jenom zkontrolujte, že je vybraná virtuální síť **VNET-02** a že podsíť je nastavená na **20.0.0.0/24**. Klikněte na tlačítko **OK**.
9. Zkontrolujte nastavení v okně **Souhrn** a klikněte na **OK**.

## <a name="configure-the-nat-vm-in-each-poc-for-gateway-traversal"></a>Konfigurace virtuálního počítače pro překlad adres v prostředí POC pro přecházení přes bránu
Vzhledem k tomu, že prostředí POC je navržené jako samostatné a izolované od sítě, ve které je nasazený fyzický hostitel, síť „externí“ virtuální IP adresy, ke které jsou brány připojené, není ve skutečnosti externí, ale je skrytá za směrovačem, který provádí překlad adres (NAT). Tímto směrovačem je virtuální počítač s Windows Serverem (**MAS-BGPNAT01**), na kterém je v infrastruktuře POC spuštěná role Služba Směrování a vzdálený přístup (RRAS). Na virtuálním počítači MAS-BGPNAT01 musíte nakonfigurovat překlad adres (NAT) a umožnit tak připojení Site-to-Site VPN na obou koncích. Je proto potřeba vytvořit statické mapování NAT, které pro porty vyžadované pro připojení VPN mapuje externí rozhraní virtuálního počítače BGPNAT VM na virtuální IP adresu fondu hraniční brány.

> [!NOTE]
> Tato konfigurace je nutná jenom pro prostředí POC.
> 
> 

### <a name="configure-nat"></a>Konfigurace překladu adres
Tento postup musíte použít v OBOU prostředích POC.

1. Připojte se k fyzickému počítači Azure Stack pro POC1.
2. Stisknutím a podržením kombinace [klávesa Windows] + R otevřete nabídku **Spustit**, zadejte **mstsc** a stiskněte **Enter**.
3. Do pole **Počítač** zadejte **MAS-BGPNAT01** a klikněte na **Připojit**.
4. Klikněte na nabídku Start, potom pravým tlačítkem klikněte na **Windows PowerShell** a vyberte **Spustit jako správce**.
5. Zadejte **ipconfig /all**.
6. Najděte ethernetový adaptér připojený k vaší místní síti a poznamenejte si IPv4 adresu, která je k němu přidružená. V našem ukázkovém prostředí je to **10.16.169.131** (červeně zakroužkované na následujícím obrázku), ale ve vašem prostředí bude tato hodnota jiná.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image16.png)
7. Zadejte následující příkaz PowerShellu pro určení externí adresy NAT pro porty s ověřováním IKE. Nezapomeňte změnit IP adresu tak, aby odpovídala vašemu prostředí.
   
       Add-NetNatExternalAddress -NatName BGPNAT -IPAddress 10.16.169.131 -PortStart 499 -PortEnd 501
8. Dál vytvoříte statické mapování NAT, které mapuje externí adresu na veřejnou IP adresu brány pro port ISAKMP 500 pro PHASE 1 tunelu IPSEC.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 500 -InternalPort 500
> [!NOTE] 
> Parametrem `-InternalAddress` je tady veřejná IP adresa brány virtuální sítě, kterou jste předtím vytvořili.  Pokud chcete zjistit tuto IP adresu, podívejte se na vlastnosti v okně Brána virtuální sítě a najděte hodnotu pro veřejnou IP adresu.       

9. Nakonec musíte nakonfigurovat přecházení NAT, které využívá port 4500 k úspěšnému vytvoření kompletního tunelu IPEC přes zařízení NAT.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 4500 -InternalPort 4500
> [!NOTE] 
> Parametrem `-InternalAddress` je tady veřejná IP adresa brány virtuální sítě, kterou jste předtím vytvořili.  Pokud chcete zjistit tuto IP adresu, podívejte se na vlastnosti v okně Brána virtuální sítě a najděte hodnotu pro veřejnou IP adresu.       

10. Krok 1 až 9 opakujte v prostředí POC2.

## <a name="test-the-connection"></a>Otestování připojení
Připojení Site-to-Site je teď navázané a měli byste ověřit, že v něm dokážete zajistit provoz. Tento úkol je jednoduchý, protože stačí přihlásit se k jednomu z virtuálních počítačů, které jste vytvořili v některém z prostředí POC, a odeslat příkaz ping virtuálnímu počítači, který jste vytvořili v druhém prostředí. Abyste zajistili, že provoz skutečně využívá připojení Site-to-Site, pro příkaz ping použijte DIP adresu virtuálního počítače ve vzdálené podsíti (místo virtuální IP adresy). Je potřeba jenom najít adresu na druhém konci připojení a poznamenat si ji.

### <a name="log-in-to-the-tenant-vm-in-poc1"></a>Přihlášení k virtuálnímu počítači tenanta v POC1
1. Připojte se k fyzickému počítači Azure Stack pro POC1 a potom se přihlaste k portálu pomocí účtu tenanta.
2. V levém navigačním panelu klikněte na **Virtual Machines**.
3. V seznamu virtuálních počítačů najděte počítač **VM01**, který jste předtím vytvořili, a klikněte na něj.
4. V okně pro virtuální počítač klikněte na **Připojit**.
   
     ![](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Uvnitř virtuálního počítače otevřete příkazový řádek a zadejte **ipconfig /all**.
6. Ve výstupu najděte **IPv4 adresu** a poznamenejte si ji. Tuto adresu použijete pro příkaz ping z POC2. V našem ukázkovém prostředí je touto adresou **10.0.10.4**, ale ve vašem prostředí tato hodnota může být jiná. Měla by ale spadat do podsítě **10.0.10.0/24**, kterou jste předtím vytvořili.

### <a name="log-in-to-the-tenant-vm-in-poc2"></a>Přihlášení k virtuálnímu počítači tenanta v POC2
1. Připojte se k fyzickému počítači Azure Stack pro POC2 a potom se přihlaste k portálu pomocí účtu tenanta.
2. V levém navigačním panelu klikněte na **Virtual Machines**.
3. V seznamu virtuálních počítačů najděte počítač **VM02**, který jste předtím vytvořili, a klikněte na něj.
4. V okně pro virtuální počítač klikněte na **Připojit**.
   
5. Uvnitř virtuálního počítače otevřete příkazový řádek a zadejte **ipconfig /all**.
6. Měla by se zobrazit IPv4 adresa, která spadá do 10.0.20.0/24. V našem ukázkovém prostředí je touto adresou 10.0.20.4, ale ve vaše adresa může být jiná.
7. Teď chcete z virtuálního počítače v POC2 přes tunel poslat příkaz ping virtuálnímu počítači v POC1. K tomuto účelu použijte adresu DIP, kterou jste si poznamenali z VM01.
   V našem ukázkovém prostředí je touto adresou 10.0.10.4, ale vy pro příkaz ping použijte adresu, kterou jste si poznamenali. Výsledek by měl vypadat přibližně takto:
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
8. Odpověď od vzdáleného virtuálního počítače je indikátorem toho, že test proběhl úspěšně. Teď můžete okno připojení virtuálního počítače zavřít. Nebo si můžete vyzkoušet další přenosy dat, třeba kopírování souborů, abyste připojení dál otestovali.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Zobrazení statistiky přenosu dat prostřednictvím připojení brány
Pokud chcete vědět, kolik dat prochází připojením Site-to-Site, najdete tyto informace v okně Připojení. Tento test je také dalším vhodným způsobem ověření, že příkaz ping, který jste právě odeslali, skutečně prošel přes připojení VPN.

1. Zůstaňte přihlášení k virtuálnímu počítači tenanta v POC2 a přihlaste se k portálu **Microsoft Azure Stack POC** pomocí účtu tenanta.
2. Klikněte na položku nabídky **Procházet** a vyberte **Připojení**.
3. V seznamu připojení klikněte na **POC2-POC1**.
4. V okně Připojení se zobrazují statistiky pro **vstupní ** a **výstupní data**. Na následujícím snímku uvidíte větší čísla, než jaká by odpovídala jenom odeslání příkazu ping. To proto, že došlo i k dalším přenosům souborů. Měly by se tady zobrazovat nenulové hodnoty.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)




<!--HONumber=Feb17_HO2-->


