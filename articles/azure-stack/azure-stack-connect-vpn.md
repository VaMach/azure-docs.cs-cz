---
title: "Připojit zásobník Azure do Azure pomocí sítě VPN"
description: "Postup připojení virtuální sítě v Azure zásobníku virtuálních sítí v Azure pomocí sítě VPN."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: 5d963fe8b1b576768156500af39254f45939f90d
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="connect-azure-stack-to-azure-using-vpn"></a>Připojit zásobník Azure do Azure pomocí sítě VPN

*Platí pro: Azure zásobníku integrované systémy*

Tento článek ukazuje, jak vytvořit síť site-to-site VPN pro připojení virtuální sítě v Azure zásobník virtuální sítě v Azure.

### <a name="connection-diagram"></a>Diagram připojení
Následující diagram znázorňuje, by měla vypadat konfigurace připojení po dokončení:

![Konfigurace připojení Site-to-site VPN](media/azure-stack-connect-vpn/image2.png)

### <a name="before-you-begin"></a>Než začnete
K dokončení konfigurace připojení, zkontrolujte, zda že máte následující položky, než začnete:

* Balíček Azure integrované systémy (více uzly) nasazení, který je přímo připojený k Internetu. To znamená, že vaše externí veřejná IP adresa rozsahu musí být přímo dosažitelný z veřejného Internetu.
* Platné předplatné Azure.  Pokud nemáte předplatné Azure, můžete vytvořit [bezplatný účet Azure zde](https://azure.microsoft.com/free/?b=17.06).

## <a name="network-example-values-table"></a>Tabulka hodnot příklad sítě
V tabulce hodnoty příklad sítě jsou ukázkové hodnoty, které se používají v tomto článku. Můžete použít tyto hodnoty, nebo můžete se podívat do mají lépe pochopit, příklady v tomto článku.

**Tabulka hodnot příklad sítě**
|   |Azure Stack|Azure|
|---------|---------|---------|
|Název virtuální sítě     |Azs-VNet|AzureVNet |
|Adresní prostor virtuální sítě |10.1.0.0/16|10.100.0.0/16|
|Název podsítě     |FrontEnd|FrontEnd|
|Rozsah adres podsítě|10.1.0.0/24 |10.100.0.0/24 |
|Podsíť brány     |10.1.1.0/24|10.100.1.0/24|

## <a name="create-the-network-resources-in-azure"></a>Vytvořte síťové prostředky v Azure

Nejdřív vytvoříte síťovým prostředkům pro Azure. Následující pokyny ukazují, jak vytvořit prostředky pomocí [portál Azure](http://portal.azure.com/).

### <a name="create-the-virtual-network-and-vm-subnet"></a>Vytvoření virtuální sítě a podsítě virtuálních počítačů

1. Přihlaste se k [portál Azure](http://portal.azure.com/) pomocí účtu Azure.
2. V portálu pro uživatele, vyberte **nový**.
3. Přejděte na **Marketplace**a potom vyberte **sítě**.
4. Vyberte **virtuální síť**.
5. Použijte informace z tabulky konfigurace sítě a určete hodnoty pro Azure **název**, **adresní prostor**, **název podsítě**, a **adresa podsítě rozsah**.
6. Pro **skupiny prostředků**, vytvořte novou skupinu prostředků nebo, pokud již účet máte, vyberte **použít existující**.
7. Vyberte **umístění** sítě vnet.  Pokud používáte ukázkové hodnoty, vyberte **východní USA** nebo použijte jiné umístění, pokud dáváte přednost.
8. Zaškrtněte **Připnout na řídicí panel**.
9. Vyberte **Vytvořit**.

### <a name="create-the-gateway-subnet"></a>Vytvoření podsítě brány
1. Otevřete prostředek virtuální sítě, jste vytvořili (**AzureVNet**) na řídicím panelu.
2. Na **nastavení** vyberte **podsítě**.
3. Vyberte **podsíť brány** přidat podsíť brány virtuální sítě.
4. Ve výchozím nastavení je název této podsítě nastavený na **GatewaySubnet**.
   Podsítě brány jsou speciální a musí mít tento konkrétní název, aby fungovaly správně.
5. V **rozsahu adres** pole, ověřte adresu **10.100.1.0/24**.
6. Vyberte **OK** vytvořit podsíť brány.

### <a name="create-the-virtual-network-gateway"></a>Vytvoření brány virtuální sítě
1. Na portálu Azure vyberte **nový**.  
2. Přejděte na **Marketplace**a potom vyberte **sítě**.
3. V seznamu síťových prostředků, vyberte **Brána virtuální sítě**.
4. V **název**, typ **Azure-GW**.
5. Vyberte virtuální síť, vyberte **virtuální síť**. Potom vyberte **AzureVnet** ze seznamu.
6. Vyberte **veřejnou IP adresu**. Když **zvolte veřejnou IP adresu** části otevře, vyberte **vytvořit nový**.
7. V **název**, typ **Azure-GW – PiP**a potom vyberte **OK**.
8. Ve výchozím nastavení pro **typ sítě VPN**, **založené na trasách** je vybrána.
    Zachovat **založené na trasách** typ sítě VPN.
9. Ověřte, že nastavení **Předplatné** a **Umístění** jsou správná. Budete moct připnout prostředků na řídicí panel. Vyberte **Vytvořit**.

### <a name="create-the-local-network-gateway-resource"></a>Vytvořte prostředek brány místní sítě

1. Na portálu Azure vyberte **nový**. 
4. Přejděte na **Marketplace**a potom vyberte **sítě**.
5. V seznamu zdrojů vyberte **brány místní sítě**.
6. V **název**, typ **Azs-GW**.
7. V **IP adresu**, zadejte veřejnou IP adresu pro vaše zásobníku brány virtuální sítě Azure, je uvedena výše v tabulce Konfigurace sítě.
8. V **adresní prostor**, zásobník Azure, zadejte **10.1.0.0/24** a **10.1.1.0/24** adresní prostor pro **AzureVNet**.
9. Ověřte, že vaše **předplatné**, **skupiny prostředků**, a **umístění** jsou správné a potom vyberte **vytvořit**.

## <a name="create-the-connection"></a>Vytvoření připojení
1. V portálu pro uživatele, vyberte **nový**. 
2. Přejděte na **Marketplace**a potom vyberte **sítě**.
3. V seznamu zdrojů vyberte **připojení**.
4. Na **základní** části nastavení pro **typ připojení**, zvolte **Site-to-site (IPSec)**.
5. Vyberte **předplatné**, **skupiny prostředků**, a **umístění**a potom vyberte **OK**.
6. Na **nastavení** vyberte **Brána virtuální sítě**a potom vyberte **Azure-GW**.
7. Vyberte **brány místní sítě**a potom vyberte **Azs-GW**.
8. V **název připojení**, typ **Azure Azs**.
9. V **sdílený klíč (PSK)**, typ **12345**. Pokud zvolíte jinou hodnotu, mějte na paměti, že IT oddělení *musí* odpovídají hodnotě pro sdílený klíč, kterou vytvoříte na druhém konci připojení. Vyberte **OK**.
10. Zkontrolujte **Souhrn** a pak vyberte **OK**.

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
Vytvoření virtuálního počítače v Azure nyní a umístí jej podsíť virtuálních počítačů ve virtuální síti.

1. Na portálu Azure vyberte **nový**.
2. Přejděte na **Marketplace**a potom vyberte **výpočetní**.
3. Vyberte v seznamu bitové kopie virtuálních počítačů, **Windows Server 2016 Datacenter Eval** bitové kopie.
4. Na **Základy** části pro **název**, typ **AzureVM**.
5. Zadejte platné uživatelské jméno a heslo. Tento účet používáte k přihlášení k virtuálnímu počítači po jeho vytvoření.
6. Zadejte **předplatné**, **skupiny prostředků**, a **umístění**a potom vyberte **OK**.
7. Na **velikost** vyberte velikost virtuálního počítače pro tuto instanci a potom vyberte **vyberte**.
8. Na **nastavení** části můžete přijmout výchozí hodnoty. Ujistěte se, že **AzureVnet** virtuální síť je vybrána a ověřte, že podsíť je nastavena na **10.100.0.0/24**. Vyberte **OK**.
9. Zkontrolujte nastavení na **Souhrn** a pak vyberte **OK**.

## <a name="create-the-network-resources-in-azure-stack"></a>Vytvoření síťové prostředky v Azure zásobníku
Dále vytvoříte síťové prostředky v Azure zásobníku.

### <a name="sign-in-as-a-user"></a>Přihlaste se jako uživatel
Správce služeb můžete přihlásit jako uživatel a testovací plány, nabídky a odběry, které uživatelé můžou používat. Pokud jste ještě nemáte, [vytvoření uživatelského účtu](azure-stack-add-new-user-aad.md) před přihlášením.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Vytvoření virtuální sítě a podsítě virtuálních počítačů
1. Použijte uživatelský účet pro přihlášení k portálu user portal.
2. V portálu pro uživatele, vyberte **nový**.

    ![Vytvořit novou virtuální síť](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Přejděte na **Marketplace**a potom vyberte **sítě**.
4. Vyberte **virtuální síť**.
5. Pro **název**, **adresní prostor**, **název podsítě**, a **rozsah adres podsítě**, použijte hodnoty z tabulky konfigurace sítě.
6. V **předplatné**, zobrazí se předplatné, které jste vytvořili dříve.
7. Pro **skupiny prostředků**, můžete vytvořit skupinu prostředků nebo pokud již účet máte, vyberte **použít existující**.
8. Ověřte výchozí umístění.
9. Zaškrtněte **Připnout na řídicí panel**.
10. Vyberte **Vytvořit**.

### <a name="create-the-gateway-subnet"></a>Vytvoření podsítě brány
1. Na řídicím panelu otevřete prostředek Azs-VNet virtuální síť, kterou jste vytvořili.
2. Na **nastavení** vyberte **podsítě**.
3. Chcete-li přidat podsíť brány virtuální sítě, vyberte **podsíť brány**.
   
    ![Přidání podsítě brány](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. Ve výchozím nastavení, je název podsítě hodnotu **GatewaySubnet**.
   Podsítě brány jsou speciální. Chcete-li správně fungoval, musíte použít *GatewaySubnet* název.
5. V **rozsahu adres**, ověřte, že adresa je **10.1.1.0/24**.
6. Vyberte **OK** vytvořit podsíť brány.

### <a name="create-the-virtual-network-gateway"></a>Vytvoření brány virtuální sítě
1. Na portálu Azure zásobníku vyberte **nový**. 
2. Přejděte na **Marketplace**a potom vyberte **sítě**.
3. V seznamu síťových prostředků, vyberte **Brána virtuální sítě**.
4. V **název**, typ **Azs-GW**.
5. Vyberte **virtuální síť** položky vyberte virtuální síť.
   Vyberte **Azs-VNet** ze seznamu.
6. Vyberte **veřejnou IP adresu** položku nabídky. Když **zvolte veřejnou IP adresu** části otevře, vyberte **vytvořit nový**.
7. V **název**, typ **Azs-GW – PiP**a potom vyberte **OK**.
8.  Ve výchozím nastavení pro **typ sítě VPN**, **založené na trasách** je vybrána.
    Zachovat **založené na trasách** typ sítě VPN.
9. Ověřte, že nastavení **Předplatné** a **Umístění** jsou správná. Budete moct připnout prostředků na řídicí panel. Vyberte **Vytvořit**.

### <a name="create-the-local-network-gateway"></a>Vytvoření brány místní sítě
Znalost problematicky *brány místní sítě* v zásobníku Azure je trochu jiná než v nasazení služby Azure.

V nasazení služby Azure představuje bránu místní sítě fyzického zařízení místní (na umístění uživatele), který používáte pro připojení k bráně virtuální sítě v Azure. V zásobníku Azure jsou obou konců připojení brány virtuální sítě!

Způsob, jak více obecně myslíte o této je, že prostředek brány místní sítě vždycky uvádí služby Brána vzdálené na druhém konci připojení. 

### <a name="create-the-local-network-gateway-resource"></a>Vytvořte prostředek brány místní sítě
1. Přihlaste se k portálu Azure zásobníku.
2. V portálu pro uživatele, vyberte **nový**.
3. Přejděte na **Marketplace**a potom vyberte **sítě**.
4. V seznamu zdrojů vyberte **brány místní sítě**.
5. V **název**, typ **Azure-GW**.
6. V **IP adresu**, zadejte veřejné IP adresy pro bránu virtuální sítě v Azure **Azure-GW – PiP**. Tato adresa se zobrazí dříve v tabulce Konfigurace sítě.
7. V **adresní prostor**, adresní prostor sítě vnet Azure, kterou jste vytvořili, zadejte **10.100.0.0/24** a **10.100.1.0/24**.
8. Ověřte, že vaše **předplatné**, **skupiny prostředků**, a **umístění** jsou správné a potom vyberte **vytvořit**.

### <a name="create-the-connection"></a>Vytvoření připojení
1. V portálu pro uživatele, vyberte **nový**.
2. Přejděte na **Marketplace**a potom vyberte **sítě**.
3. V seznamu zdrojů vyberte **připojení**.
4. Na **Základy** části nastavení pro **typ připojení**, vyberte **Site-to-site (IPSec)**.
5. Vyberte **předplatné**, **skupiny prostředků**, a **umístění**a potom vyberte **OK**.
6. Na **nastavení** vyberte **Brána virtuální sítě**a potom vyberte **Azs-GW**.
7. Vyberte **brány místní sítě**a potom vyberte **Azure-GW**.
8. V **název připojení**, typ **Azs Azure**.
9. V **sdílený klíč (PSK)**, typ **12345**a potom vyberte **OK**.
10. Na **Souhrn** vyberte **OK**.

### <a name="create-a-vm"></a>Vytvoření virtuálního počítače
Pro ověření dat, který se přenáší prostřednictvím připojení VPN, musíte vytvořit virtuální počítače na obou koncích odesílat a přijímat data prostřednictvím tunelu VPN. 

1. Na portálu Azure vyberte **nový**.
2. Přejděte na **Marketplace**a potom vyberte **výpočetní**.
3. Vyberte v seznamu bitové kopie virtuálních počítačů, **Windows Server 2016 Datacenter Eval** bitové kopie.
4. Na **Základy** v části **název**, typ **Azs-VM**.
5. Zadejte platné uživatelské jméno a heslo. Tento účet používáte k přihlášení k virtuálnímu počítači po jeho vytvoření.
6. Zadejte **předplatné**, **skupiny prostředků**, a **umístění**a potom vyberte **OK**.
7. Na **velikost** části pro tuto instanci, vyberte velikost virtuálního počítače a potom vyberte **vyberte**.
8. Na **nastavení** část, přijměte výchozí hodnoty. Ujistěte se, že **Azs-VNet** virtuální síť je vybrána. Ověřte, že podsíť je nastavena na **10.1.0.0/24**. Potom vyberte **OK**.
9. Na **Souhrn** část, zkontrolujte nastavení a pak vyberte **OK**.


## <a name="test-the-connection"></a>Otestování připojení
Teď, když připojení site-to-site je třeba ověřit, získáte provoz přes něj. K ověření, přihlaste se do jednoho z virtuálních počítačů, které jste vytvořili v zásobníku Azure. Pak odeslat příkaz ping virtuální počítač, který jste vytvořili v Azure. 

Abyste měli jistotu, že je přenos odesílat přes připojení site-to-site, odeslat příkaz ping na přímé IP (DIP) adresu virtuálního počítače ve vzdálené podsíti, není VIP. K tomuto účelu najít adresu vyhrazené IP adresy na druhém konci připojení. Uložte adresu pro pozdější použití.

### <a name="sign-in-to-the-user-vm-in-azure-stack"></a>Přihlaste se k uživatele virtuálního počítače v Azure zásobníku
1. Přihlaste se k portálu Azure zásobníku.
2. V levém navigačním panelu, vyberte **virtuální počítače**.
3. V seznamu virtuálních počítačů, Najít **Azs-VM** který jste vytvořili dříve a pak ho vyberte.
4. V části pro virtuální počítač, klikněte na tlačítko **Connect**a pak otevřete soubor Azs VM.rdp.
   
     ![Tlačítko pro připojení](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Přihlaste se pomocí účtu, který se konfiguruje při vytváření virtuálního počítače.
6. Otevřít se zvýšenými oprávněními **prostředí Windows PowerShell** okno.
7. Zadejte **ipconfig /all**.
8. Ve výstupu najít **IPv4 adresu**a potom uložte adresu pro pozdější použití. Toto je adresa, která provede testování ping z Azure. V příkladu prostředí, je na adresu **10.1.0.4**, ale ve vašem prostředí mohou být různé. Musí spadat do **10.1.0.0/24** podsíť, kterou jste vytvořili dříve.
9. Pokud chcete vytvořit pravidlo brány firewall, která umožňuje virtuálního počítače, které neodpovídají na ověřování aktivity, spusťte následující příkaz prostředí PowerShell:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-azure"></a>Přihlaste se k klienta virtuálního počítače v Azure
1. Přihlaste se k portálu Azure.
2. V levém navigačním panelu klikněte na **virtuální počítače**.
3. Ze seznamu virtuálních počítačů, Najít **virtuálního počítače Azure** který jste vytvořili dříve a pak ho vyberte.
4. V části pro virtuální počítač, klikněte na tlačítko **Connect**.
5. Přihlaste se pomocí účtu, který se konfiguruje při vytváření virtuálního počítače.
6. Otevřít se zvýšenými oprávněními **prostředí Windows PowerShell** okno.
7. Zadejte **ipconfig /all**.
8. Měli byste vidět IPv4 adresu, která spadá do **10.100.0.0/24**. V příkladu prostředí, je na adresu **10.100.0.4**, ale vaše adresa může být odlišné.
9. Pokud chcete vytvořit pravidlo brány firewall, která umožňuje virtuálního počítače, které neodpovídají na ověřování aktivity, spusťte následující příkaz prostředí PowerShell:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Z virtuálního počítače v Azure zadejte příkaz ping v zásobníku Azure prostřednictvím tunelu virtuálního počítače. K tomuto účelu odeslat příkaz ping vyhrazené IP adresy, které jste si poznamenali ze Azs-VM.
   V příkladu prostředí, je to **10.1.0.4**, ale ujistěte se, na příkaz ping na adresu, které jste si poznamenali ve svém testovacím prostředí. Měli byste vidět výsledek, který vypadá jako na následujícím snímku obrazovky:
   
    ![Úspěšný příkaz ping](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Odpověď od vzdáleného virtuálního počítače označuje testu úspěšné! Můžete zavřít okno virtuálního počítače. Chcete-li otestovat připojení, můžete se pokusit jinými druhy přenosů dat, jako je kopírování souborů.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Zobrazení statistiky přenosu dat prostřednictvím připojení brány
Pokud chcete vědět, kolik dat prochází připojení site-to-site, tyto informace jsou k dispozici na **připojení** části. Tento test je také jiný způsob, jak ověřit, že příkazem ping, které jste poslali ve skutečnosti se prostřednictvím připojení VPN.

1. Když jste přihlášení k virtuálnímu počítači uživatele v zásobníku Azure, pomocí účtu uživatele k přihlášení k portálu user portal.
2. Přejděte na **všechny prostředky**a pak vyberte **Azs Azure** připojení. **Připojení** se zobrazí.
4. Na **připojení** části statistiku **Data v** a **Data** zobrazí. Na následujícím snímku obrazovky jsou velké počty přičítat přenos dalších souborů. Měli byste vidět některé nenulové hodnoty.
   
    ![Vstupní a výstupní data](media/azure-stack-connect-vpn/Connection.png)

## <a name="next-steps"></a>Další postup

[Nasazení aplikací do Azure a Azure zásobníku](azure-stack-solution-pipeline.md)
