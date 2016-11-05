---
title: Zřízení virtuálního počítače s SQL Serverem | Microsoft Docs
description: Zjistěte, jak v Azure vytvořit virtuální počítač s SQL Serverem pomocí Portálu. V tomto kurzu se používá režim Resource Manageru.
services: virtual-machines-windows
documentationcenter: na
author: rothja
editor: ''
manager: jhubbard
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: jroth

---
# Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal
> [!div class="op_single_selector"]
> * [Portál](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

V tomto uceleném kurzu si ukážeme, jak pomocí webu Azure Portal zřídit virtuální počítač s SQL Serverem.

Galerie virtuálních počítačů Azure obsahuje několik imagí, které obsahují Microsoft SQL Server. Několika málo kliknutími můžete z galerie vybrat jednu z imagí virtuálního počítače s SQL Serverem a zřídit virtuální počítač v prostředí Azure.

V tomto kurzu provedete následující:

* [Výběr image virtuálního počítače s SQL Serverem z galerie](#select-a-sql-vm-image-from-the-gallery)
* [Nakonfigurování a vytvoření virtuálního počítače](#configure-the-vm)
* [Otevření virtuálního počítače pomocí Vzdálené plochy](#open-the-vm-with-remote-desktop)
* [Vzdálené připojení k SQL Serveru](#connect-to-sql-server-remotely)

## Výběr image virtuálního počítače s SQL Serverem z galerie
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí svého účtu.
   
   > [!NOTE]
   > Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).
   > 
   > 
2. Na webu Azure Portal klikněte na **Nový**. Na Portálu se otevře okno **Nový**. Prostředky virtuálních počítačů se SQL Serverem jsou v Marketplace ve skupině **Virtuální počítače**.
3. V okně **Nový** klikněte na **Virtuální počítače**.
4. Pokud chcete zobrazit všechny dostupné image, klikněte v okně **Virtuální počítače** na **Zobrazit všechny**.
   
    ![Okno Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)
5. V části **Databázové servery** klikněte na **SQL Server**. Možná si budete muset posunout zobrazení, protože možnost **Databázové servery** bude až někde dole. Projděte si dostupné šablony SQL Serveru.
   
    ![Image s SQL Serverem v galerii virtuálních počítačů](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)
6. U každé šablony je označena příslušná verze SQL Serveru a operační systém. Ze seznamu vyberte jednu z těchto imagí. Pak si v okně s podrobnostmi přečtěte popis image virtuálního počítače.
   
   > [!NOTE]
   > Bitové kopie virtuálního počítače SQL zahrnují licenční náklady na SQL Server do ceny vytvořených virtuálních počítačů za minutu. Existuje další možnost používání vlastní licence (BYOL) a platby pouze za virtuální počítače. Tyto názvy bitových kopií mají předponu {BYOL}. Další informace o této možnosti najdete v tématu [Začínáme se serverem SQL ve službě Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
   > 
   > 
7. V části **Vybrat model nasazení** ověřte, že je vybraný **Resource Manager**. U nových virtuálních počítačů se doporučuje používat model nasazení Resource Manageru. Klikněte na možnost **Vytvořit**.
   
    ![Vytvoření virtuálního počítače s SQL Serverem pomocí Resource Manageru](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## Nakonfigurování virtuálního počítače
Pro konfiguraci virtuálního počítače s SQL Serverem se používá pět oken.

| Krok | Popis |
| --- | --- |
| **Základy** |[Konfigurace základního nastavení](#1-configure-basic-settings) |
| **Velikost** |[Volba velikosti virtuálního počítače](#2-choose-virtual-machine-size) |
| **Nastavení** |[Konfigurace volitelných funkcí](#3-configure-optional-features) |
| **Nastavení SQL Serveru** |[Konfigurace nastavení SQL Serveru](#4-configure-sql-server-settings) |
| **Souhrn** |[Kontrola souhrnných informací](#5-review-the-summary) |

## 1. Konfigurace základního nastavení
V okně **Základy** zadejte následující informace:

* Zadejte jedinečný **název** virtuálního počítače.
* Zadejte **uživatelské jméno** pro účet místního správce ve virtuálním počítači. Tento účet je také přidán do pevné role serveru na serveru **sysadmin** SQL Serveru.
* Zadejte silné **heslo**.
* Pokud máte více předplatných, ověřte, že je předplatné správné pro nový virtuální počítač.
* Do pole **Skupina prostředků** zadejte název pro novou skupinu prostředků. Pokud chcete použít existující skupinu prostředků, klikněte na **Vybrat existující**. Skupina prostředků je kolekce souvisejících prostředků v Azure (virtuální počítače, účty úložiště, virtuální sítě atd.).
  
  > [!NOTE]
  > Použití nové skupinu prostředků je užitečné, pokud testujete nasazení SQL Serveru v Azure nebo se snažíte o něm dozvědět více. Až s testováním skončíte, odstraňte skupinu prostředků. Automaticky se tím odstraní virtuální počítač se všemi prostředky spojenými s danou skupinu prostředků. Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../resource-group-overview.md).
  > 
  > 
* Vyberte **umístění** pro toto nasazení.
* Kliknutím na **OK** uložte nastavení.
  
    ![Okno Základy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## 2. Volba velikosti virtuálního počítače
V kroku **Velikost** zvolte velikost virtuálního počítače v okně **Zvolte velikost**. V okně se po jeho otevření zobrazí doporučené velikosti počítačů na základě šablony, kterou jste vybrali. Uvidíte tu také odhadované měsíční náklady na provozování virtuálního počítače.

![Možnosti velikosti virtuálního počítače s SQL Serverem](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Pro úlohy v produkčním prostředí doporučujeme vybrat velikost virtuálního počítače, která podporuje [Storage úrovně Premium](../storage/storage-premium-storage.md). Pokud nevyžadujete tuto úroveň výkonu, použijte tlačítko **Zobrazit vše**, kterým si zobrazíte všechny možnosti velikosti počítačů. Může například použít menší velikost počítače pro vývojové nebo testovací prostředí.

> [!NOTE]
> Další informace o velikostech virtuálních počítačů najdete v tématu [Velikosti virtuálních počítačů](virtual-machines-windows-sizes.md). Důležité informace o velikostech virtuálních počítačů s SQL Serverem najdete v tématu [Osvědčené postupy z hlediska výkonu pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-performance.md).
> 
> 

Zvolte velikost počítače a potom klikněte na **Vybrat**.

## 3. Konfigurace volitelných funkcí
V okně **Nastavení** nakonfigurujte úložiště, sítě a monitorování Azure pro virtuální počítač.

* V části **Úložiště** zadejte **typ disku** Standard nebo Premium (SSD). Pro používání v produkčním prostředí se doporučuje Storage úrovně Premium.

> [!NOTE]
> Pokud vyberete disk typu Premium (SSD) pro velikost počítače, která nepodporuje Storage úrovně Premium, velikost počítače se vám automaticky změní.  
> 
> 

* V části **Účet úložiště** můžete použít název automaticky zřízeného účtu úložiště. Můžete také kliknutím na **Účet úložiště** zvolit existující účet a nakonfigurovat typ účtu úložiště. Ve výchozím nastavení vytvoří Azure nový účet úložiště s místně redundantním úložištěm. Další informace o možnostech úložiště najdete v tématu [Replikace Azure Storage](../storage/storage-redundancy.md).
* V části **Síť** může přijmout automaticky zadané hodnoty. Můžete také kliknutím na jednotlivé funkce ručně nakonfigurovat **virtuální síť**, **podsíť**, **veřejnou IP adresu** a **skupinu zabezpečení sítě**. Pro účely tohoto kurzu nechejte nastavené výchozí hodnoty.
* Azure umožňuje **monitorování** ve výchozím nastavení se stejným účtem, jaký je nastavený pro virtuální počítač. Tato nastavení tady můžete změnit.
* V části **Skupina dostupnosti** zadejte nastavení dostupnosti. Pro účely tohoto kurzu můžete vybrat možnost **Žádná**. Pokud budete chtít nastavit Skupiny dostupnosti AlwaysOn SQL, nakonfigurujte dostupnost tak, aby se předešlo opětovnému vytvoření virtuálního počítače.  Další informace najdete v tématu [Správa dostupnosti virtuálních počítačů](virtual-machines-windows-manage-availability.md).

Po dokončení konfigurace těchto nastavení klikněte na **OK**.

## 4. Konfigurace nastavení SQL Serveru
V okně **Nastavení SQL Serveru** nakonfigurujte konkrétní nastavení a optimalizace pro SQL Server. Pro SQL Server můžete například nakonfigurovat následující nastavení.

| Nastavení |
| --- |
| [Připojení](#connectivity) |
| [Authentication](#authentication) |
| [Konfigurace úložiště](#storage-configuration) |
| [Automatizované opravy](#automated-patching) |
| [Automatizované zálohování](#automated-backup) |
| [Integrace se službou Azure Key Vault](#azure-key-vault-integration) |
| [Služby R](#r-services) |

### Připojení
V části **Připojení SQL** zadejte typ přístupu, který chcete mít k instanci SQL Serveru na tomto virtuálním počítači. Pro účely tohoto kurzu vyberte možnost **Veřejné (internet)**, která umožňuje připojovat se SQL Serveru z počítačů nebo služeb na internetu. Když je vybraná tato možnost, Azure automaticky nakonfiguruje bránu firewall a skupinu zabezpečení sítě tak, aby umožňovaly přenosy na portu 1433.  

![Možnosti připojení SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Aby bylo možné se k SQL Serveru připojovat prostřednictvím internetu, musíte také povolit ověřování SQL Serveru, které je popsané v následující části.

> [!NOTE]
> Pro virtuální počítač s SQL Serverem je také možné přidat další omezení z hlediska síťové komunikace. Můžete to provést úpravou skupiny zabezpečení sítě po vytvoření virtuálního počítače. Další informace najdete v článku [Skupina zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).
> 
> 

Pokud raději nechcete povolovat připojení k databázovému stroji prostřednictvím internetu, zvolte jednu z následujících možností:

* **Místní (jen uvnitř virtuálního počítače):** Umožňuje připojení k SQL Serveru pouze v rámci virtuálního počítače.
* **Privátní (uvnitř virtuální sítě):** Umožňuje připojení k SQL Serveru z počítačů nebo služeb ve stejné virtuální síti.

> [!NOTE]
> Image virtuálního počítače pro edici SQL Serveru Express nepovoluje automaticky protokol TCP/IP. To platí i pro veřejné a privátní možnosti připojení. Po vytvoření virtuálního počítače v edici Express je třeba [povolit protokol TCP/IP ručně](#configure-sql-server-to-listen-on-the-tcp-protocol) pomocí SQL Server Configuration Manageru.
> 
> 

Obecně se doporučuje zvýšit zabezpečení výběrem nejvíce omezujícího připojení, které váš scénář umožňuje. Všechny možnosti je ale možné zabezpečit prostřednictvím pravidel skupin zabezpečení sítě a ověřování SQL Serveru a Windows.

Pro **port** se používá výchozí hodnota 1433. Můžete ale zadat i jiné číslo portu.
Další informace najdete v tématu [Připojení virtuálnímu počítači s SQL Serverem (Resource Manager) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### Ověřování
Pokud budete chtít vyžadovat ověřování SQL Serveru, klikněte v části **Ověřování SQL** na **Povolit**.

![Ověřování SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Pokud máte v plánu připojovat se k SQL Serveru prostřednictvím internetu (tj. s nastavením veřejného připojení), musíte tu povolit ověřování SQL Serveru. Veřejný přístup k SQL Serveru vyžaduje použití ověřování SQL Serveru.
> 
> 

Pokud povolíte ověřování SQL Serveru, zadejte **přihlašovací jméno** a **heslo**. Toto uživatelské jméno se nakonfiguruje jako přihlašovací jméno ověřování SQL Serveru a člen pevné role serveru **sysadmin**. Další informace o režimech ověřování najdete v tématu [Volba režimu ověřování](http://msdn.microsoft.com/library/ms144284.aspx).

Pokud ověřování SQL Serveru nepovolíte, můžete pro připojení k instanci SQL Serveru používat účet místního správce ve virtuálním počítači.

### Konfigurace úložiště
Klikněte na **Konfigurace úložiště** a zadejte požadavky na úložiště.

![Konfigurace úložiště SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Pokud vyberete úložiště Standard, není tato možnost k dispozici. Automatická optimalizace úložiště je k dispozici pouze pro Storage úrovně Premium.
> 
> 

Požadavky můžete zadat jako vstupně-výstupní operace za sekundu (IOPs), propustnost v MB/s a celkovou velikost úložiště. Tyto hodnoty nakonfigurujte pomocí posuvníků. Portál na základě těchto požadavků automaticky vypočítá počet disků.

Ve výchozím nastavení Azure optimalizuje úložiště takto: 5000 vstupně-výstupních operací za sekundu, 200 MB a 1TB kapacita úložiště. Tato nastavení úložiště můžete podle náročnosti zpracovávaných úloh změnit. V části **Optimalizace úložiště** vyberte jednu z následujících možností:

* **Obecné:** Výchozí nastavení a podporuje většinu úloh.
* **Transakční:** Toto zpracování optimalizuje úložiště pro standardní úlohy databází OLTP.
* **Datové sklady:** Optimalizuje úložiště pro úlohy analýz a generování sestav.

> [!NOTE]
> Horní limity posuvníků se liší v závislosti na velikosti vámi vybraného virtuálního počítače.
> 
> 

### Automatizované opravy
**Automatizované opravy** jsou ve výchozím nastavení povolené. Automatizované opravy umožňují na platformě Azure automaticky opravovat SQL Server a operační systém. Zadejte den v týdnu, čas a dobu trvání intervalu údržby. V té době pak Azure nainstaluje potřebné opravy. V rámci plánování intervalu údržby se pro čas používá národní prostředí virtuálních počítačů. Pokud nechcete, aby se v rámci Azure automaticky opravoval SQL Server a operační systém, klikněte na **Zakázat**.  

![Automatizované opravy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Další informace najdete v tématu [Automatizované opravy pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### Automatizované zálohování
Automatické zálohování databází můžete pro všechny databáze povolit v části **Automatizované zálohování**. Automatizované zálohování je ve výchozím nastavení zakázané.

Když povolíte automatizované zálohování SQL, můžete nakonfigurovat následující:

* Doba uchování dat (dny) pro zálohování
* Účet úložiště, který se má používat pro zálohování
* Možnost šifrování a heslo pro zálohování

Pokud chcete zálohy šifrovat, klikněte na **Povolit**. Pak zadejte **heslo**. Azure vytvoří certifikát pro šifrování záloh a používá zadané heslo k ochraně tohoto certifikátu.

![Automatizované zálohování SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### Integrace se službou Azure Key Vault
Pokud budete chtít ukládat tajné klíče zabezpečení v Azure pro šifrování, klikněte na **Integrace se službou Azure Key Vault** a klikněte na **Povolit**.

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

V následující tabulce jsou uvedeny parametry, které jsou nezbytné pro konfiguraci Integrace se službou Azure Key Vault.

| PARAMETR | POPIS | PŘÍKLAD |
| --- | --- | --- |
| **Adresa URL služby Key Vault** |Umístění služby Key Vault |https://contosokeyvault.vault.azure.net/ |
| **Název objektu zabezpečení** |Hlavní název služby Azure Active Directory. Tento název se také označuje jako ID klienta. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Tajný klíč objektu zabezpečení** |Tajný klíč objektu zabezpečení služby Azure Active Directory. Tento tajný klíč se také označuje jako Tajný klíč klienta. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Název přihlašovacího údaje** |**Název přihlašovacího údaje:** Integrace se službou Azure Key Vault vytvoří přihlašovací údaje v rámci SQL Serveru, díky čemuž mají virtuální počítače přístup do trezoru klíčů. Zvolte název pro tyto přihlašovací údaje. |moje_přihlaš1 |

Další informace najdete v tématu [Konfigurace Integrace se službou Azure Key Vault pro virtuální počítače Azure](virtual-machines-windows-ps-sql-keyvault.md).

Po dokončení konfigurace nastavení SQL Serveru klikněte na **OK**.

### Služby R
V edici SQL Serveru 2016 Enterprise je dostupná možnost [Služby R na SQL Serveru](https://msdn.microsoft.com/library/mt604845.aspx). Ta umožňuje pomocí SQL Serveru 2016 provádět pokročilou analýzu. V okně **Nastavení SQL Serveru** klikněte na **Povolit**.

![Povolení služeb R na SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

> [!NOTE]
> Image SQL Serveru v jiné edici než 2016 Enterprise možnost povolit služby R nemají.
> 
> 

## 5. Kontrola souhrnných informací
V okně **Souhrn** zkontrolujte souhrn a pak kliknutím na **OK** vytvořte SQL Server, skupinu prostředků a prostředky zadané pro tento virtuální počítač.

Nasazení můžete monitorovat z webu Azure Portal. Tlačítko **Oznámení** v horní části obrazovky zobrazuje základní stav nasazení.

> [!NOTE]
> Abyste si udělali představu o tom, jak dlouho nasazování trvá, nasadil jsem virtuální počítač s SQL Serverem pro oblast Východní USA s výchozím nastavením. Toto testovací nasazení trvalo celkem 26 minut. Na základě vaší oblasti a vybraného nastavení ale můžete zaznamenat kratší nebo delší čas nasazení.
> 
> 

## Otevření virtuálního počítače pomocí Vzdálené plochy
Podle následujícího postupu se připojte k virtuálnímu počítači pomocí Vzdálené plochy.

1. Jakmile bude virtuální počítač Azure vytvořen, zobrazí se ikona virtuálního počítače v řídicím panelu Azure. Můžete ji také najít procházením stávajících virtuálních počítačů. Klikněte na nový virtuální počítač s SQL Serverem. Zobrazí se okno **Virtuální počítač** s podrobnostmi o vašem virtuálním počítači.
2. V horní části okna **Virtuální počítač** klikněte na **Připojit**.
3. Prohlížeč stáhne soubor RDP pro virtuální počítač. Otevřete soubor RDP.
    ![Připojení k virtuálnímu počítači s SQL Serverem pomocí Vzdálené plochy](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
4. Připojení ke vzdálené ploše vás upozorní, že nelze identifikovat vydavatele tohoto vzdáleného připojení. Pokračujte kliknutím na **Připojit**.
5. V dialogovém okně **Zabezpečení systému Windows** klikněte na **Použít jiný účet**.
6. Do pole **Uživatelské jméno** zadejte **\<uživatelské jméno >**, kde <user name> je uživatelské jméno, které jste určili při konfigurování virtuálního počítače. Před název je nutné přidat počáteční zpětné lomítko.
7. Zadejte **heslo**, které jste dříve nakonfigurovali pro tento virtuální počítač, a potom se kliknutím na **OK** připojte.
8. Pokud se vám zobrazí další dialog **Připojení ke vzdálené ploše** a zeptá se vás, jestli se chcete připojit, klikněte na **Ano**.

Po připojení k virtuálnímu počítači s SQL Serverem můžete spustit SQL Server Management Studio a připojit se pomocí ověřování systému Windows se svými přihlašovacími údaji místního správce. Pokud jste povolili ověřování SQL Serveru, můžete se také připojit pomocí ověřování SQL Serveru a použít k tomu přihlašovací jméno a heslo SQL Serveru, které jste nakonfigurovali během zřizování.

Přístup k počítači vám umožňuje podle potřeb přímo měnit nastavení počítače a SQL Serveru. Můžete například nakonfigurovat nastavení brány firewall nebo změnit nastavení konfigurace SQL Serveru.

## Vzdálené připojení k SQL Serveru
V tomto kurzu jsme pro virtuální počítač vybrali **veřejný** přístup a **ověřování SQL Serveru**. Tato nastavení automaticky nakonfigurovala virtuální počítač tak, aby povoloval připojení k SQL Serveru z libovolného klienta přes internet (za předpokladu, že má správné přihlašovací údaje SQL Serveru).

> [!NOTE]
> Pokud jste během zřizování nevybrali veřejný přístup, je nutné provést pár kroků navíc pro přístup k instanci SQL Serveru přes internet. Další informace najdete v tématu [Připojení virtuálnímu počítači s SQL Serverem](virtual-machines-windows-sql-connect.md).
> 
> 

Následující části vysvětlují, jak se připojit k instanci SQL Serveru na vašem virtuálním počítači z jiného počítače přes internet.

> [!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]
> 
> 

## Další kroky
Další informace o používání SQL Serveru v Azure najdete v tématu [SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) a [Nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).

Podívejte se na video s přehledem SQL Serveru v Azure Virtual Machines v tématu [Azure Virtual Machines je nejlepší platformou pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Projděte si mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure.

<!--HONumber=Sep16_HO4-->


