---
title: "Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines (klasické) | Microsoft Docs"
description: "Vytvoření skupiny dostupnosti Always On s virtuálními počítači Azure. Tento kurz používá primárně uživatelské rozhraní a nástroje místo skriptování."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 8d48a3d2-79f8-4ab0-9327-2f30ee0b2ff1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: b360fe9f28eeb9b10c82fce729165b1b572ac3c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="configure-always-on-availability-group-in-azure-virtual-machines-classic"></a>Konfigurace skupiny dostupnosti Always On v Azure Virtual Machines (klasické)
> [!div class="op_single_selector"]
> * [Klasické: uživatelského rozhraní](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klasické: prostředí PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Než začnete, vezměte v úvahu, že je nyní možné dokončit tuto úlohu v modelu Azure Resource Manager. Doporučujeme, abyste modelu Azure Resource Manager pro nová nasazení. V tématu [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT] 
> Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager. Azure má dva různé modely nasazení k vytváření a práci s prostředky: [Resource Manager a klasický](../../../azure-resource-manager/resource-manager-deployment-model.md). Tento článek vysvětluje, jak pomocí modelu nasazení classic. 

Pro dokončení této úlohy s modelem Azure Resource Manager, najdete v části [SQL serveru Always On skupiny dostupnosti na virtuálních počítačích Azure](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

V tomto kurzu začátku do konce ukazuje, jak implementovat skupiny dostupnosti pomocí SQL serveru Always On spuštěná ve virtuálních počítačích Azure.

Na konci tohoto kurzu vaše řešení SQL serveru Always On v Azure budou tvořeny následující prvky:

* Virtuální síť, která obsahuje několik podsítí a zahrnuje front-end a back-end podsítě
* Řadič domény s doménou služby Active Directory (Azure AD)
* Dva virtuální počítače, které se systémem SQL Server a jsou nasazené do podsítě back-end a připojený k doméně služby Azure AD
* Clusteru s podporou převzetí služeb při selhání tři uzly s modelem kvora Většina uzlů
* Skupiny dostupnosti, která má dva replik se synchronním potvrzováním databáze dostupnosti

Na následujícím obrázku je grafické reprezentace řešení.

![Architektura testovací laboratoř pro skupiny dostupnosti v Azure](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC791912.png)

Všimněte si, že toto je možné jednu konfiguraci. Například můžete minimalizovat počet virtuálních počítačů pro skupinu dostupnosti dvě repliky. Tato konfigurace se uloží na výpočetní hodiny v Azure pomocí řadiče domény jako sdílenou složku kvora v clusteru se dvěma uzly. Tato metoda snižuje počet virtuálních počítačů pomocí jedné z ilustrované konfigurace.

Tento kurz předpokládá následující:

* Už máte účet Azure.
* Už víte, jak zřídit klasické virtuální počítač, který spouští server SQL Server pomocí grafického uživatelského rozhraní v galerii virtuálních počítačů.
* Už máte plnou Principy skupin dostupnosti Always On. Další informace najdete v tématu [skupin dostupnosti Always On (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

> [!NOTE]
> Pokud vás zajímá pomocí skupin dostupnosti Always On s SharePoint, také zobrazit [nakonfigurovat SQL Server 2012 skupin dostupnosti Always On pro SharePoint 2013](https://technet.microsoft.com/library/jj715261.aspx).
> 
> 

## <a name="create-the-virtual-network-and-domain-controller-server"></a>Vytvořit virtuální síť a doména serveru controller
Začínat nové zkušební účet Azure. Po nastavení účtu musí být na domovské obrazovce portálu Azure classic.

1. Klikněte **nový** tlačítko v levém horním rohu dolní části stránky, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Kliknutím na tlačítko Nová na portálu](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665511.gif)
2. Klikněte na tlačítko **síťové služby** > **virtuální sítě** > **vytvořit vlastní**, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Vytvoření virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665512.gif)
3. V **vytvořit virtuální síť A** dialogové okno pole, vytvořte nové virtuální sítě tak, že procházení stránek a pomocí nastavení v následující tabulce. 
   
   | Stránka | Nastavení |
   | --- | --- |
   | Podrobnosti virtuální sítě |**Název = ContosoNET**<br/>**OBLAST = západní USA** |
   | Servery DNS a připojení VPN |Žádný |
   | Adresní prostory virtuální sítě |Následující snímek obrazovky ukazuje nastavení: ![Vytvoření virtuální sítě](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784620.png) |
4. Vytvořte virtuální počítač, který budete používat jako řadič domény (DC). Klikněte na tlačítko **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Vytvoření virtuálního počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784621.png)
5. V **vytvořit virtuální počítač A** dialogové okno Nový virtuální počítač nakonfigurujte tak, že procházení stránek a pomocí nastavení v následující tabulce. 
   
   | Stránka | Nastavení |
   | --- | --- |
   | Vyberte operační systém virtuálního počítače |Windows Server 2012 R2 Datacenter |
   | Konfigurace virtuálního počítače |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoDC<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A2 (2 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |
   | Konfigurace virtuálního počítače |**CLOUDOVÉ služby** = vytvořit novou cloudovou službu<br/>**Název CLOUDOVÉ služby DNS** = název jedinečný cloudové služby<br/>**NÁZEV DNS** = jedinečný název (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**Skupina dostupnosti** = (None) |
   | Možnosti virtuálního počítače |Použít výchozí nastavení |

Po dokončení konfigurace nového virtuálního počítače, počkejte, než pro virtuální počítač jako provsioned. Tento proces trvá delší dobu pro dokončení. Pokud kliknete **virtuálního počítače** karta v portálu Azure classic, uvidíte ContosoDC recyklace stavů ze **počáteční (zřizování)** k **Zastaveno**, **počáteční**, **spuštění (zřizování)**a nakonec **systémem**.

Nyní je úspěšně zřízen serveru řadiče domény. Dále nakonfigurujete domény služby Active Directory na tomto serveru řadiče domény.

## <a name="configure-the-domain-controller"></a>Konfigurace řadiče domény
V následujících krocích nakonfigurujete na počítači ContosoDC jako řadič domény pro spolecnost.contoso.com.

1. Na portálu, vyberte **ContosoDC** počítače. Na **řídicí panel** , klikněte na **Connect** otevřete soubor vzdálené plochy (RDP) pro přístup ke vzdálené ploše.
   
    ![Připojte se k počítači Vritual](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784622.png)
2. Přihlaste se pomocí účtu správce nakonfigurované (**\AzureAdmin**) a heslo (**Contoso! 000**).
3. Ve výchozím nastavení **správce serveru** měla by se zobrazit řídicí panel.
4. Klikněte **přidat role a funkce** odkaz na řídicím panelu.
   
    ![V Průzkumníku serveru přidat role](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784623.png)
5. Klikněte na tlačítko **Další** až na **role serveru** části.
6. Vyberte **Active Directory Domain Services** a **DNS Server** role. Po zobrazení výzvy, přidejte další funkce, které vyžadují tyto role.
   
   > [!NOTE]
   > Zobrazí se upozornění, že neexistuje žádná statická IP adresa ověřování. Pokud testujete konfiguraci, klikněte na tlačítko **pokračovat**. Pro produkčních scénářích [nastavit statickou IP adresu počítače řadiče domény pomocí prostředí PowerShell](../../../virtual-network/virtual-networks-reserved-private-ip.md).
   > 
   > 
   
    ![Role dialogové okno Přidání](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784624.png)
7. Klikněte na tlačítko **Další** dokud nepřejdete **potvrzení** části. Vyberte **cílový server automaticky restartovat, pokud je to nutné** zaškrtávací políčko.
8. Klikněte na **Nainstalovat**.
9. Po tyto funkce jsou instalovány, vraťte k **správce serveru** řídicího panelu.
10. Vyberte novou **služby AD DS** možnost v levém podokně.
11. Klikněte **Další** odkaz na žlutý pruh upozornění.
    
     ![AD DS dialogové ve virtuálním počítači serveru DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784625.png)
12. V **akce** sloupec **všechny podrobnosti úlohy serveru** dialogové okno, klikněte na tlačítko **zvýšit úroveň tohoto serveru na řadič domény**.
13. V **Průvodce konfigurací služby domény služby Active Directory**, použijte následující hodnoty:
    
    | Stránka | Nastavení |
    | --- | --- |
    | Konfigurace nasazení |**Přidat novou doménovou strukturu** = vybrané<br/>**Název kořenové domény** = corp.contoso.com |
    | Možnosti řadiče domény |**Heslo** = Contoso! 000<br/>**Potvrzení hesla** = Contoso! 000 |
14. Klikněte na tlačítko **Další** projít dalších stránek v průvodci. Na **Kontrola předpokladů** ověřte, že, zobrazí se následující zpráva: **všech požadovaných součástí byly úspěšně zkontrolovány**. Všimněte si, že byste měli zkontrolovat všechny příslušné zprávy upozornění, ale je možné pokračovat v instalaci.
15. Klikněte na **Nainstalovat**. **ContosoDC** virtuální počítač se automaticky restartuje.

## <a name="configure-domain-accounts"></a>Konfigurace účtů domény
Další kroky konfigurace účtů služby Active Directory pro pozdější použití.

1. Přihlaste se znovu na **ContosoDC** počítače.
2. V **správce serveru**, klikněte na tlačítko **nástroje** > **Centrum správy služby Active Directory**.
   
    ![Centrum správy služby Active Directory](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784626.png)
3. V **Centrum správy služby Active Directory**, vyberte **corp (místní)** v levém podokně.
4. Na pravé straně **úlohy** podokně klikněte na tlačítko **nový** > **uživatele**. Použijte následující nastavení:
   
   | Nastavení | Hodnota |
   | --- | --- |
   | **Křestní jméno** |Instalace |
   | **SamAccountName uživatele** |Instalace |
   | **Heslo** |Contoso! 000 |
   | **Potvrdit heslo** |Contoso! 000 |
   | **Další možnosti hesla** |Vybráno |
   | **Heslo je platné stále** |Zaškrtnuté |
5. Klikněte na tlačítko **OK** vytvořit **nainstalovat** uživatele. Tento účet se použije ke konfiguraci clusteru převzetí služeb při selhání a skupiny dostupnosti.
6. Vytvořte dva další uživatele, **CORP\SQLSvc1** a **CORP\SQLSvc2**, pomocí stejných kroků. Tyto účty se použije pro instance systému SQL Server. Dále musíte poskytnout **CORP\Install** potřebná oprávnění ke konfiguraci služby Windows failover clustering.
7. V **Centrum správy služby Active Directory**, klikněte na tlačítko **corp (místní)** v levém podokně. V **úlohy** podokně klikněte na tlačítko **vlastnosti**.
   
    ![Vlastnosti uživatele CORP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784627.png)
8. Vyberte **rozšíření**a klikněte **Upřesnit** na tlačítko **zabezpečení** kartě.
9. V **Upřesnit nastavení zabezpečení pro corp** dialogové okno, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **vybrat objekt zabezpečení**, vyhledejte **CORP\Install**a potom klikněte na **OK**.
11. Vyberte **číst vlastnosti všech** a **vytvářet objekty počítačů** oprávnění.
    
     ![Oprávnění uživatele Corp](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784628.png)
12. Klikněte na tlačítko **OK**a potom klikněte na **OK** znovu. Zavřete okno vlastností corp.

Teď, když jste nakonfigurovali služby Active Directory a uživatelských objektů, vytvoříte tři virtuální počítače systému SQL Server a připojovat je k této doméně.

## <a name="create-the-sql-server-virtual-machines"></a>Vytvoření virtuálního počítače systému SQL Server
Vytvořte tři virtuální počítače. Jeden je pro uzel clusteru a dva jsou pro systém SQL Server. Chcete-li vytvořit všechny virtuální počítače, přejděte zpět na portálu Azure classic, klikněte na **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie**. Pak pomocí šablony v následující tabulce vám pomohou vytvořit virtuální počítače.

| Stránka | VM1 | VIRTUÁLNÍHO POČÍTAČE 2 | VM3 |
| --- | --- | --- | --- |
| Vyberte operační systém virtuálního počítače |**Windows Server 2012 R2 Datacenter** |**SQL Server 2014 RTM Enterprise** |**SQL Server 2014 RTM Enterprise** |
| Konfigurace virtuálního počítače |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoWSFCNode<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A2 (2 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoSQL1<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A3 (4 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |**Datum vydání verze** = (nejnovější)<br/>**Název VIRTUÁLNÍHO počítače** = ContosoSQL2<br/>**ÚROVEŇ** = STANDARD<br/>**VELIKOST** = A3 (4 jádra)<br/>**NOVÉ uživatelské jméno** = AzureAdmin<br/>**NOVÉ heslo** = Contoso! 000<br/>**POTVRĎTE** = Contoso! 000 |
| Konfigurace virtuálního počítače |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = vytvořit dostupnosti nastavit<br/>**NÁZEV SADY DOSTUPNOSTI** = SQLHADR |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = SQLHADR (můžete také nakonfigurovat skupinu dostupnosti po vytvoření na počítač. Všechny tři počítače musí být přiřazen do skupiny dostupnosti SQLHADR.) |**CLOUDOVÉ služby** = název DNS dříve vytvořenou jedinečný cloudové služby (např: ContosoDC123)<br/>**OBLASTI nebo skupiny vztahů nebo VIRTUÁLNÍCH SÍŤOVÝCH** = ContosoNET<br/>**PODSÍTĚ virtuální sítě** = Back(10.10.2.0/24)<br/>**ÚČET úložiště** = použít účet úložiště automaticky generované<br/>**SKUPINY dostupnosti** = SQLHADR (můžete také nakonfigurovat skupinu dostupnosti po vytvoření na počítač. Všechny tři počítače musí být přiřazen do skupiny dostupnosti SQLHADR.) |
| Možnosti virtuálního počítače |Použít výchozí nastavení |Použít výchozí nastavení |Použít výchozí nastavení |

<br/>

> [!NOTE]
> Předchozí konfiguraci doporučuje úroveň STANDARD virtuálních počítačů, protože základní vrstvě počítače nepodporují koncové body s vyrovnáváním zatížení. Musíte Vyrovnávání zatížení sítě koncové body později k vytvořit naslouchací proces skupiny dostupnosti. Navíc velikosti počítačů, které jsou zde navrhovaná jsou určené pro testování v Azure Virtual Machines skupiny dostupnosti. Nejlepšího výkonu dosáhnete v produkčním prostředí, najdete v části doporučení pro velikosti počítačů systému SQL Server a konfigurace v [osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](../sql/virtual-machines-windows-sql-performance.md).
> 
> 

Po tři virtuální počítače jsou plně zřízený, je nutné připojit, aby **corp.contoso.com** domény a přidělit CORP\Install práva správce na počítače. K tomu použijte následující kroky pro všechny tři virtuální počítače.

1. Nejprve změňte upřednostňovanou adresu serveru DNS. Stáhnout soubor RDP každého virtuálního počítače do svého místního adresáře vyberte virtuální počítač v seznamu a klikněte na **Connect** tlačítko. Vyberte virtuální počítač, klikněte kamkoli ale první buňky v řádku, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Stáhnout soubor RDP](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664953.jpg)
2. Otevřete soubor RDP, který jste stáhli a přihlaste se k virtuálnímu počítači pomocí účtu správce nakonfigurované (**BUILTIN\AzureAdmin**) a heslo (**Contoso! 000**).
3. Po přihlášení, měli byste vidět **správce serveru** řídicího panelu. Klikněte na tlačítko **místní Server** v levém podokně.
4. Klikněte **IPv4 adresy přiřazené serverem DHCP, pro protokol IPv6** odkaz.
5. V **připojení k síti** dialogovém okně klikněte na ikonu sítě.
   
    ![Změňte virtuální počítač upřednostňovaný server DNS](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784629.png)
6. Na panelu příkazů klikněte na tlačítko **změnit nastavení pro toto připojení**. (V závislosti na velikost okna aplikace, budete možná muset klikněte na dvojitou šipku vpravo zobrazíte tento příkaz).
7. Vyberte **Internet Protocol verze 4 (TCP/IPv4)**a potom klikněte na **vlastnosti**.
8. Vyberte **použít následující adresy serverů DNS** a pak zadejte **10.10.2.4** v **upřednostňovaný server DNS**.
9. **10.10.2.4** adresa je adresa, která je přiřazena k virtuálnímu počítači v podsíti 10.10.2.0/24 v virtuální sítě Azure. Tento virtuální počítač je **ContosoDC**. Chcete-li ověřit **ContosoDC**na IP adresu, použijte **nslookup contosodc** v okně příkazového řádku, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Použití nástroje NSLOOKUP najít IP adresu pro řadič domény](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC664954.jpg)
10. Klikněte na tlačítko **OK** > **Zavřít** k provedení změn. Nyní můžete připojit virtuální počítač **corp.contoso.com**.
11. Zpět v **místní Server** okně klikněte na tlačítko **pracovní skupiny** odkaz.
12. V **název počítače** klikněte na tlačítko **změnu**.
13. Vyberte **domény** zaškrtávací políčko, typ **corp.contoso.com** textového pole a pak klikněte na **OK**.
14. V **zabezpečení systému Windows** dialogovém okně zadejte přihlašovací údaje pro výchozí účet správce domény (**CORP\AzureAdmin**) a heslo (**Contoso! 000**).
15. Když se zobrazí zpráva "Vítejte v doméně corp.contoso.com", klikněte na tlačítko **OK**.
16. Klikněte na tlačítko **Zavřít** > **restartovat nyní** v dialogovém okně.

### <a name="add-the-corpinstall-user-as-an-administrator-on-each-virtual-machine"></a>Přidejte uživatele Corp\Install jako správce na každém virtuálním počítači
1. Počkejte, dokud se virtuální počítač restartuje a pak otevřete protokol RDP soubor znovu se přihlásit k virtuálnímu počítači pomocí **BUILTIN\AzureAdmin** účtu.
2. V **správce serveru** klikněte na tlačítko **nástroje** > **Správa počítače**.
   
    ![Správa počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784630.png)
3. V **Správa počítače** dialogové okno, rozbalte seznam **místní uživatelé a skupiny**a potom klikněte na **skupiny**.
4. Dvakrát klikněte **správci** skupiny.
5. V **správci vlastnosti** dialogové okno, klikněte **přidat** tlačítko.
6. Zadejte uživatele **CORP\Install**a potom klikněte na **OK**. Pokud budete vyzváni k zadání pověření, použijte **AzureAdmin** účet, který **Contoso! 000** heslo.
7. Klikněte na tlačítko **OK** zavřete **vlastností správce** dialogové okno.

### <a name="add-the-failover-clustering-feature-to-each-virtual-machine"></a>Přidejte funkci Clustering převzetí služeb při selhání na každém virtuálním počítači
1. V **správce serveru** řídicí panel, klikněte na tlačítko **přidat role a funkce**.
2. V **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další** až na **funkce** stránky.
3. Vyberte **Clustering převzetí služeb při selhání**. Po zobrazení výzvy, přidejte další závislé součásti.
   
    ![Přidejte funkci Clustering převzetí služeb při selhání virtuálního počítače](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784631.png)
4. Klikněte na tlačítko **Další**a potom klikněte na **nainstalovat** na **potvrzení** stránky.
5. Když **Clustering převzetí služeb při selhání** po dokončení instalace funkce, klikněte na tlačítko **Zavřít**.
6. Odhlásit se z virtuálního počítače.
7. Opakujte kroky v této části pro všechny tři servery: **ContosoWSFCNode**, **ContosoSQL1**, a **ContosoSQL2**.

SQL Server, které teď zřizování virtuálních počítačů a spuštěná, ale každý má výchozí možnosti pro SQL Server.

## <a name="create-the-failover-cluster"></a>Vytvoření clusteru převzetí služeb při selhání
V této části vytvoříte clusteru převzetí služeb při selhání, který bude hostovat skupině dostupnosti, který chcete vytvořit později. Nyní by měli mít provést následující všechny tři virtuální počítače, které budete používat v clusteru převzetí služeb při selhání:

* Kompletní zřízení virtuálního počítače v Azure
* Virtuální počítač připojený k doméně
* Přidat **CORP\Install** do místní skupiny Administrators
* Přidat převzetí služeb při selhání funkce clustering

Jsou všechny tyto požadavky na každém virtuálním počítači, než bude možné připojit ke clusteru převzetí služeb při selhání.

Všimněte si také, že se virtuální síť Azure není chovají stejným způsobem jako místní sítě. Budete muset vytvořit cluster v uvedeném pořadí:

1. Vytvoření clusteru s jedním uzlem na jednom uzlu (**ContosoSQL1**).
2. Upravit IP adresu clusteru nepoužívané IP adresu (**10.10.2.101**).
3. Uveďte název clusteru online.
4. Přidání dalších uzlů (**ContosoSQL2** a **ContosoWSFCNode**).

Použijte následující postup k dokončení úlohy, které plně konfigurace clusteru.

1. Otevřete soubor RDP pro **ContosoSQL1**a přihlaste se pomocí účtu domény **CORP\Install**.
2. V **správce serveru** řídicí panel, klikněte na tlačítko **nástroje** > **Správce clusteru převzetí služeb při selhání**.
3. V levém podokně klikněte pravým tlačítkem na **Správce clusteru převzetí služeb při selhání**a potom klikněte na **vytvoření clusteru s podporou**, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Vytvoření clusteru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784632.png)
4. V Průvodci vytvořením clusteru vytvořte jednouzlového clusteru procházení stránek a pomocí nastavení v následující tabulce:
   
   | Stránka | Nastavení |
   | --- | --- |
   | Než začnete |Použít výchozí nastavení |
   | Vyberte servery |Typ **ContosoSQL1** v **zadejte název serveru** a klikněte na tlačítko **přidat** |
   | Upozornění ověření |Vyberte **ne. I nevyžadují podporu společnosti Microsoft pro tento cluster a proto nechcete, aby ke spuštění ověřovacích testů. Po klepnutí na tlačítko Další, pokračovat ve vytváření clusteru**. |
   | Přístupový bod pro správu clusteru |Typ **Cluster1** v **název clusteru** |
   | Potvrzení |Použít výchozí hodnoty, pokud nepoužíváte prostory úložiště. Přečtěte si upozornění za touto tabulkou. |
   
   > [!WARNING]
   > Pokud používáte [prostory úložiště](https://technet.microsoft.com/library/hh831739), které skupiny více disků do fondů úložiště, musíte vymazat **přidat do clusteru veškeré oprávněné úložiště** v zaškrtávací políčko **potvrzení** stránky. Pokud není zaškrtnutí tohoto políčka, odpojit virtuální disky, během procesu clusteringu. V důsledku toho se také neobjeví v Správce disků nebo v Průzkumníku dokud prostory úložiště se odebral z clusteru a znovu připojit pomocí prostředí PowerShell.
   > 
   > 
5. V levém podokně rozbalte **Správce clusteru převzetí služeb při selhání**a potom klikněte na **Cluster1.corp.contoso.com**.
6. V prostředním podokně přejděte dolů k položce **základní prostředky clusteru** části a rozbalte **název: Clutser1** podrobnosti. Měli byste vidět, jak **název** a **IP adresu** prostředky v **se nezdařilo** stavu. Prostředek IP adresy nelze uvést do režimu online, protože clusteru je přiřazen stejnou IP adresu jako počítač, na sebe, což je duplicitní adresu.
7. Klikněte pravým tlačítkem na chybných **IP adresu** prostředek a pak klikněte na tlačítko **vlastnosti**.
   
    ![Vlastnosti clusteru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784633.png)
8. Vyberte **statickou IP adresu**, zadejte **10.10.2.101** v **adresu** textového pole a pak klikněte na tlačítko **OK**.
9. V **základní prostředky clusteru** části, klikněte pravým tlačítkem na **název: Cluster1**a potom klikněte na **přepnout do režimu Online**. Počkejte, dokud jsou obě prostředky online. Při přechodu prostředek názvu clusteru do režimu online, serveru řadiče domény se aktualizuje pomocí nového účtu počítače služby Active Directory. Tento účet služby Active Directory se použije ke spuštění služby clusteru skupiny dostupnosti později.
10. Přidáte zbývající uzly do clusteru. Ve stromu prohlížeče klikněte pravým tlačítkem na **Cluster1.corp.contoso.com**a potom klikněte na **přidat uzel**, jak je znázorněno na následujícím snímku obrazovky.
    
     ![Přidání uzlu do clusteru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC784634.png)
11. V **Průvodce přidáním uzlu**, klikněte na tlačítko **Další** na **zvolit servery** přidejte **ContosoSQL2** a **ContosoWSFCNode** do seznamu tak, že zadáte název serveru v **zadejte název serveru** a pak levým na **přidat**. Až budete hotovi, klikněte na tlačítko **Další**.
12. Na **upozornění ověření** klikněte na tlačítko **ne**, i když v produkční scénář, měli byste provést testy pro ověření. Pak klikněte na **Další**.
13. Na **potvrzení** klikněte na tlačítko **Další** přidat uzly.
    
    > [!WARNING]
    > Pokud používáte [prostory úložiště](https://technet.microsoft.com/library/hh831739), které skupiny více disků do fondů úložiště, musíte vymazat **přidat do clusteru veškeré oprávněné úložiště** zaškrtávací políčko. Pokud není zaškrtnutí tohoto políčka, odpojit virtuální disky, během procesu clusteringu. V důsledku toho také nezobrazí v Správce disků nebo v Průzkumníku dokud prostory úložiště jsou odebrány z clusteru a znovu připojit pomocí prostředí PowerShell.
    > 
    > 
14. Po přidání uzlu do clusteru, klikněte na tlačítko **Dokončit**. Správce clusteru převzetí služeb při selhání by měl nyní zobrazit, že váš cluster má tři uzly a je v seznamu **uzly** kontejneru.
15. Odhlásit z této relaci vzdálené plochy.

## <a name="prepare-the-sql-server-instances-for-availability-groups"></a>Příprava instance systému SQL Server pro skupiny dostupnosti
V této části provedete následující na obou **ContosoSQL1** a **contosoSQL2**:

* Přidat přihlašovací údaje pro **NT AUTHORITY\System** potřebná oprávnění, nastavena na výchozí instanci SQL serveru.
* Přidat **CORP\Install** jako role sysadmin na výchozí instanci SQL serveru.
* Otevření brány firewall pro vzdálený přístup systému SQL Server.
* Povolte funkci dostupnosti skupin Always On.
* Změna účtu služby SQL Server na **CORP\SQLSvc1** a **CORP\SQLSvc2**, v uvedeném pořadí.

Tyto akce lze provést v libovolném pořadí. Nicméně následující kroky provede je v pořadí. Postupujte podle kroků pro oba **ContosoSQL1** a **ContosoSQL2**:

1. Pokud nebyly odhlásili z relace vzdálené plochy pro virtuální počítač, udělejte to teď.
2. Otevřít soubory protokolu RDP pro **ContosoSQL1** a **ContosoSQL2**a přihlaste se jako **BUILTIN\AzureAdmin**.
3. Přidat **NT AUTHORITY\System** do přihlášení serveru SQL s potřebnými oprávněními. Otevřete **SQL Server Management Studio**.
4. Klikněte na tlačítko **Connect** pro připojení k výchozí instanci SQL serveru.
5. V **Průzkumník objektů**, rozbalte položku **zabezpečení**a potom rozbalte **přihlášení**.
6. Klikněte pravým tlačítkem myši **NT AUTHORITY\System** přihlašovací údaje a pak klikněte na tlačítko **vlastnosti**.
7. Na **zabezpečitelné prostředky** stránky pro místní server, vyberte **Grant** pro následující oprávnění a pak klikněte na tlačítko **OK**.
   
   * Příkaz ALTER žádnou skupinu dostupnosti
   * Připojení SQL
   * Zobrazení stavu serveru
8. Přidat **CORP\Install** jako **sysadmin** role na výchozí instanci SQL serveru. V **Průzkumník objektů**, klikněte pravým tlačítkem na **přihlášení**a potom klikněte na **nového přihlašovacího jména**.
9. Typ **CORP\Install** v **přihlašovací jméno**.
10. Na **role serveru** vyberte **sysadmin**a potom klikněte na **OK**. Po vytvoření přihlášení, zobrazí se rozšířením **přihlášení** v **Průzkumník objektů**.
11. K vytvoření pravidla brány firewall pro SQL Server, na **spustit** obrazovce otevřete **brány Windows Firewall s pokročilým zabezpečením**.
12. V levém podokně vyberte **příchozí pravidla**. V pravém podokně klikněte na **nové pravidlo**.
13. Na **typ pravidla** klikněte na tlačítko **programu** > **Další**.
14. Na **programu** vyberte **tato cesta k programu**, typ **%ProgramFiles%\Microsoft SQL Server\MSSQL12. MSSQLSERVER\MSSQL\Binn\sqlservr.exe** textového pole a pak klikněte na **Další**. Pokud jsou následující těchto pokynů, ale pomocí SQL Server 2012, adresář systému SQL Server je **MSSQL11. MSSQLSERVER**.
15. Na **akce** ponechte **povolit připojení** vybrané a potom klikněte na **Další**.
16. Na **profil** stránky, přijměte výchozí nastavení a pak klikněte na tlačítko **Další**.
17. Na **název** stránky, zadejte název pravidla, například **systému SQL Server (pravidlo programu)**v **název** textové pole a pak klikněte na **Dokončit**.
18. Chcete-li povolit **skupin dostupnosti Always On** funkce na **spustit** obrazovce otevřete **SQL Server Configuration Manager**.
19. Ve stromu prohlížeče klikněte na tlačítko **služby SQL Server**, klikněte pravým tlačítkem myši **serveru SQL (MSSQLSERVER)** služby a potom klikněte na **vlastnosti**.
20. Klikněte **vždy na vysokou dostupnost** vyberte **povolte skupiny dostupnosti Always On**, jak ukazuje následující snímek obrazovky a pak klikněte na **použít**. Klikněte na tlačítko **OK** v dialogovém okně a proveďte nezavírejte **vlastnosti** zatím dialogové. Služba SQL Server se restartuje po provedení změny účtu služby.
    
     ![Vždy zapnout na skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665520.gif)
21. Chcete-li změnit účet služby SQL Server, klikněte na tlačítko **přihlášení** , zadejte **CORP\SQLSvc1** (pro **ContosoSQL1**) nebo **CORP\SQLSvc2** (pro **ContosoSQL2**) v **název účtu**, zadejte a potvrďte heslo a pak klikněte na tlačítko **OK**.
22. V dialogovém okně, které se otevře, klikněte na tlačítko **Ano** restartovat službu systému SQL Server. Po restartování služby SQL Server, změny, které provedených **vlastnosti** dialogové okno, které jsou platné.
23. Odhlásit z virtuálních počítačů.

## <a name="create-the-availability-group"></a>Vytvoření skupiny dostupnosti
Nyní jste připraveni ke konfiguraci skupiny dostupnosti. Dole je přehled co provedete:

* Vytvořit novou databázi (**MyDB1**) na **ContosoSQL1**.
* Proveďte úplné zálohování a zálohování protokolu transakcí databáze.
* Obnovit celý a záloh protokolu **ContosoSQL2** s **NORECOVERY** možnost.
* Vytvoření skupiny dostupnosti (**AG1**) se synchronním potvrzováním, automatické převzetí služeb při selhání a čitelných místních replikách.

### <a name="create-the-mydb1-database-on-contososql1"></a>Vytvořit databázi MyDB1 na ContosoSQL1
1. Pokud jste již nezaregistrovali mimo relací vzdálené plochy pro **ContosoSQL1** a **ContosoSQL2**, udělejte to teď.
2. Otevřete soubor RDP pro **ContosoSQL1**a přihlaste se jako **CORP\Install**.
3. V **Průzkumníka souborů**v části **C:\\**, vytvořte adresář s názvem **zálohování**. Tento adresář bude používat k zálohování a obnovení databáze.
4. Klikněte pravým tlačítkem na nový adresář, přejděte na **sdílet s**a potom klikněte na **Určití lidé**, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Vytvořte složku s zálohování](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665521.gif)
5. Přidat **CORP\SQLSvc1**a pak poskytněte **pro čtení a zápis** oprávnění. Přidat **CORP\SQLSvc2**a pak poskytněte **čtení** oprávnění, jak je uvedené v následující snímek obrazovky a pak klikněte na tlačítko **sdílené složky**. Po dokončení procesu sdílení souborů, klikněte na tlačítko **provádí**.
   
    ![Udělení oprávnění pro složku zálohování](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665522.gif)
6. K vytvoření databáze, z **spustit** nabídce otevřete **SQL Server Management Studio**a potom klikněte na **Connect** pro připojení k výchozí instanci SQL serveru.
7. V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze**a potom klikněte na **novou databázi**.
8. V **název databáze**, typ **MyDB1**a potom klikněte na **OK**.

### <a name="make-a-full-backup-of-mydb1-and-restore-it-on-contososql2"></a>Proveďte úplné zálohování MyDB1 a obnovte ji na ContosoSQL2
1. Aby úplnou zálohu databáze, v **Průzkumník objektů**, rozbalte položku **databáze**, klikněte pravým tlačítkem na **MyDB1**, přejděte na příkaz **úlohy**a potom klikněte na **zálohování**.
2. V **zdroj** část, zachovat **typ zálohování** nastavena na **úplné**. V **cílové** klikněte na tlačítko **odebrat** odebrat výchozí cestu k souboru pro soubor zálohy.
3. V **cílové** klikněte na tlačítko **přidat**.
4. V **název souboru** textového pole, typ  **\\ContosoSQL1\backup\MyDB1.bak**, klikněte na tlačítko **OK**a potom klikněte na **OK** znovu k zálohování databáze. Po dokončení operace zálohování, klikněte na tlačítko **OK** zavřete dialogové okno.
5. Chcete-li zálohu transakčního protokolu databáze, v **Průzkumník objektů**, rozbalte položku **databáze**, klikněte pravým tlačítkem na **MyDB1**, přejděte na **úlohy**a potom klikněte na **zálohování**.
6. V **typ zálohování**, vyberte **transakčního protokolu**. Zachovat **cílové** souboru cesta nastaven na jednu jste dřív zadali a potom klikněte na **OK**. Po dokončení operace zálohování, klikněte na tlačítko **OK** znovu.
7. K obnovení úplné a transakce protokolu zálohování **ContosoSQL2**, otevřete soubor RDP pro **ContosoSQL2**a přihlaste se jako **CORP\Install**. Nechte relace vzdálené plochy pro **ContosoSQL1** otevřete.
8. Z **spustit** nabídce otevřete **SQL Server Management Studio**a potom klikněte na **Connect** pro připojení k výchozí instanci SQL serveru.
9. V **Průzkumník objektů**, klikněte pravým tlačítkem na **databáze**a potom klikněte na **obnovit databázi**.
10. V **zdroj** vyberte **zařízení**a klikněte na tlačítko se třemi tečkami **...** .
11. V **vyberte zálohovací zařízení**, klikněte na tlačítko **přidat**.
12. V **umístění souboru zálohy**, typ  **\\ContosoSQL1\backup**, klikněte na tlačítko **aktualizovat**, vyberte **MyDB1.bak**, klikněte na tlačítko **OK**a potom klikněte na **OK** znovu. Teď byste měli vidět úplné zálohování a zálohování protokolu **zálohovací sklady k obnovení** podokně.
13. Přejděte na **možnosti** vyberte **RESTORE WITH NORECOVERY** v **stav obnovení**a potom klikněte na **OK** obnovit databázi. Po dokončení operace obnovení, klikněte na tlačítko **OK**.

### <a name="create-the-availability-group"></a>Vytvoření skupiny dostupnosti
1. Přejděte zpět na relaci vzdálené plochy pro **ContosoSQL1**. V **Průzkumník objektů** v systému SQL Server Management Studio klikněte pravým tlačítkem na **vždy na vysokou dostupnost**a potom klikněte na **Průvodce novou skupinou dostupnosti**, jak je znázorněno na následujícím snímku obrazovky.
   
    ![Spuštění Průvodce vytvořením skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665523.gif)
2. Na **ÚVOD** klikněte na tlačítko **Další**. Na **zadejte název skupiny dostupnosti** zadejte **AG1** v **název skupiny dostupnosti**, klikněte **Další** znovu.
   
    ![Průvodce pro nové skupiny dostupnosti, zadejte název skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665524.gif)
3. Na **vyberte databáze** vyberte **MyDB1**a potom klikněte na **Další**. Databáze splňuje předpoklady pro skupinu dostupnosti, protože jste provedli aspoň jednu úplnou zálohu na určený primární replice.
   
    ![Průvodce pro nové skupiny Availabilty, vyberte možnost databáze](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665525.gif)
4. na **zadejte repliky** klikněte na tlačítko **přidat repliky**.
   
    ![Průvodce pro nové skupiny Availabilty, zadejte repliky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665526.gif)
5. V **připojit k serveru** dialogové okno, typ **ContosoSQL2** v **název serveru**a potom klikněte na **Connect**.
   
    ![Průvodce pro nové skupiny Availabilty, připojit k serveru](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665527.gif)
6. Zpět na **zadejte repliky** stránky, měli byste vidět **ContosoSQL2** uvedené v **dostupné repliky**. Konfigurace repliky, jak je znázorněno na následujícím snímku obrazovky. Až budete hotovi, klikněte na tlačítko **Další**.
   
    ![Průvodce pro nové skupiny Availabilty, zadejte repliky (Dokončit)](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665528.gif)
7. Na **vybrat počáteční synchronizaci dat** vyberte **připojení pouze**a potom klikněte na **Další**. Jste již provedli synchronizace dat ručně při probíhají zálohování úplné a transakce **ContosoSQL1** a obnoví je na **ContosoSQL2**. Můžete není k provedení zálohování a obnovení operací v databázi a místo toho vyberte **úplné** umožníte Průvodce novou skupinou dostupnosti proveďte synchronizaci dat pro vás. Nedoporučujeme ale tuto možnost pro velké databáze, které se nacházejí v některé podniky.
   
    ![Průvodce novou skupinou Availabilty, vyberte data počáteční synchronizace](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665529.gif)
8. Na **ověření** klikněte na tlačítko **Další**. Tato stránka by měla vypadat podobně jako na následujícím snímku obrazovky. Protože jste nenakonfigurovali naslouchací proces skupiny dostupnosti je upozornění pro konfiguraci naslouchacího procesu. Toto upozornění můžete ignorovat, protože v tomto kurzu nekonfiguruje naslouchací proces. Naslouchací proces konfigurace, po dokončení tohoto kurzu, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).
   
    ![Průvodce vytvořením skupiny Availabilty, ověření](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665530.gif)
9. Na **Souhrn** klikněte na tlačítko **Dokončit**a potom chvíli počkejte, než Průvodce nakonfiguruje do nové skupiny dostupnosti. Na **průběh** stránky, můžete kliknout na **další podrobnosti** zobrazíte podrobné průběh. Po ukončení průvodce, zkontrolujte **výsledky** ověření úspěšně vytváření skupiny dostupnosti, jak je znázorněno na následujícím snímku obrazovky a pak klikněte na tlačítko **Zavřít** ukončíte průvodce.
   
    ![Průvodce vytvořením skupiny Availabilty, výsledky](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665531.gif)
10. V **Průzkumník objektů**, rozbalte položku **vždy na vysokou dostupnost**a potom rozbalte **skupiny dostupnosti**. Teď byste měli vidět nové skupiny dostupnosti v tomto kontejneru. Klikněte pravým tlačítkem na **AG1 (primární)**a potom klikněte na **zobrazit řídicí panel**.
    
     ![Řídicí panel zobrazit skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665532.gif)
11. Vaše **vždy na řídicí panel** by měl vypadat podobně jako v následujícím snímku obrazovky. Můžete zobrazit repliky, režim převzetí služeb při selhání každou repliku a stav synchronizace.
    
     ![Řídicí panel skupiny dostupnosti](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665533.gif)
12. Vraťte se do **správce serveru**, klikněte na tlačítko **nástroje**a pak otevřete **Správce clusteru převzetí služeb při selhání**.
13. Rozbalte položku **Cluster1.corp.contoso.com**a potom rozbalte **služeb a aplikací**. Vyberte **role** a Všimněte si, že **AG1** byla vytvořená role skupiny dostupnosti. Všimněte si, že AG1 nemá IP adresu podle databázi, kterou klienti mohou připojit ke skupině dostupnosti, protože jste nenakonfigurovali naslouchací proces. Můžete připojit přímo na primárním uzlu, který pro operace čtení a zápis a sekundární uzel pro dotazy jen pro čtení.
    
     ![AG ve Správci clusteru převzetí služeb při selhání](./media/virtual-machines-windows-classic-portal-sql-alwayson-availability-groups/IC665534.gif)

> [!WARNING]
> Nepokoušejte se převzít služby skupiny dostupnosti ze Správce clusteru převzetí služeb při selhání. Všechny operace převzetí služeb při selhání by měla provést uvnitř **vždy na řídicí panel** v systému SQL Server Management Studio. Další informace najdete v tématu [omezení na používání nástroje převzetí služeb při selhání clusteru Manager se skupinami dostupnosti](https://msdn.microsoft.com/library/ff929171.aspx).
> 
> 

## <a name="next-steps"></a>Další kroky
Jste teď úspěšně implementovali SQL serveru Always On tak, že vytvoříte skupinu dostupnosti v Azure. Ke konfiguraci naslouchacího procesu pro tuto skupinu dostupnosti, najdete v části [nakonfigurovat modul pro naslouchání ILB pro skupiny dostupnosti Always On v Azure](../classic/ps-sql-int-listener.md).

Další informace o používání systému SQL Server v Azure najdete v tématu [systému SQL Server na virtuálních počítačích Azure](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

