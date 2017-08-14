---
title: "Zřízení virtuálního počítače s SQL Serverem | Dokumentace Microsoftu"
description: "Zjistěte, jak v Azure vytvořit virtuální počítač s SQL Serverem a připojit se k němu pomocí portálu. V tomto kurzu se používá režim Resource Manageru."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 08/08/2017
ms.author: jroth
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 98e307bc3b2dd876b74d64fbdeec162827a2e86e
ms.contentlocale: cs-cz
ms.lasthandoff: 08/09/2017

---
# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Zřízení virtuálního počítače s SQL Serverem na webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

V tomto uceleném kurzu se dozvíte, jak pomocí webu Azure Portal zřídit virtuální počítač s SQL Serverem.

Galerie virtuálních počítačů Azure obsahuje několik imagí, které obsahují Microsoft SQL Server. Několika málo kliknutími můžete z galerie vybrat jednu z imagí virtuálního počítače s SQL Serverem a zřídit virtuální počítač v prostředí Azure.

V tomto kurzu provedete následující:

* [Výběr image virtuálního počítače s SQL Serverem z galerie](#select-a-sql-vm-image-from-the-gallery)
* [Nakonfigurování a vytvoření virtuálního počítače](#configure-the-vm)
* [Otevření virtuálního počítače pomocí Vzdálené plochy](#open-the-vm-with-remote-desktop)
* [Vzdálené připojení k SQL Serveru](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Výběr image virtuálního počítače s SQL Serverem z galerie

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com) pomocí svého účtu.

   > [!NOTE]
   > Pokud účet Azure nemáte, můžete začít používat [bezplatnou zkušební verzi Azure](https://azure.microsoft.com/pricing/free-trial/).

2. Na webu Azure Portal klikněte na **Nový**. Na Portálu se otevře okno **Nový**.

3. V okně **Nový** klikněte na **Compute** a pak klikněte na **Zobrazit všechno**.

   ![Okno Nový výpočet v Azure](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

4. Do pole hledání zadejte **SQL Server** a stiskněte ENTER.

5. Pak klikněte na ikonu **Filtr** a jako vydavatele vyberte **Microsoft**. Kliknutím na **Hotovo** v okně filtru vyfiltrujte z výsledků image SQL Serveru publikované Microsoftem.

   ![Okno Azure Virtual Machines](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Projděte si dostupné image SQL Serveru. U každé image je označena příslušná verze SQL Serveru a operační systém.

6. Vyberte image s názvem **Bezplatná licence: SQL Server 2016 SP1 Developer v systému Windows Server 2016**.

   > [!TIP]
   > V tomto kurzu se používá edice Developer, protože je to plná verze SQL Serveru, která je zdarma pro účely testování vývoje. Platíte jenom náklady na provozování virtuálního počítače. Pro použití v tomto kurzu ale můžete zvolit libovolnou z imagí.

   > [!TIP]
   > Image virtuálního počítače SQL zahrnují licenční náklady na SQL Server do minutové ceny vytvořených virtuálních počítačů (s výjimkou edic Developer a Express). SQL Server Developer je zdarma pro vývoj/testování (ale ne pro produkci) a SQL Express je zdarma pro nenáročné úlohy (méně než 1 GB paměti, méně než 10GB úložiště). Existuje další možnost používání vlastní licence (BYOL) a platby pouze za virtuální počítače. Tyto názvy bitových kopií mají předponu {BYOL}. 
   >
   > Další informace o těchto možnostech najdete v tématu [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

7. V části **Vybrat model nasazení** ověřte, že je vybraný **Resource Manager**. U nových virtuálních počítačů se doporučuje používat model nasazení Resource Manageru. 

8. Klikněte na možnost **Vytvořit**.

    ![Vytvoření virtuálního počítače s SQL Serverem pomocí Resource Manageru](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Nakonfigurování virtuálního počítače
Pro konfiguraci virtuálního počítače s SQL Serverem se používá pět oken.

| Krok | Popis |
| --- | --- |
| **Základy** |[Konfigurace základního nastavení](#1-configure-basic-settings) |
| **Velikost** |[Volba velikosti virtuálního počítače](#2-choose-virtual-machine-size) |
| **Nastavení** |[Konfigurace volitelných funkcí](#3-configure-optional-features) |
| **Nastavení SQL Serveru** |[Konfigurace nastavení SQL Serveru](#4-configure-sql-server-settings) |
| **Souhrn** |[Kontrola souhrnných informací](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Konfigurace základního nastavení

V okně **Základy** zadejte následující informace:

* Zadejte jedinečný **název** virtuálního počítače.

* Vyberte **SSD** jako typ disku virtuálního počítače pro zajištění optimálního výkonu.

* Zadejte **uživatelské jméno** pro účet místního správce ve virtuálním počítači. Tento účet je také přidán do pevné role serveru na serveru **sysadmin** SQL Serveru.

* Zadejte silné **heslo**.

* Pokud máte více předplatných, ověřte, že je předplatné správné pro nový virtuální počítač.

* Do pole **Skupina prostředků** zadejte název pro novou skupinu prostředků. Pokud chcete použít existující skupinu prostředků, klikněte na **Použít existující**. Skupina prostředků je kolekce souvisejících prostředků v Azure (virtuální počítače, účty úložiště, virtuální sítě atd.).

  > [!NOTE]
  > Použití nové skupinu prostředků je užitečné, pokud testujete nasazení SQL Serveru v Azure nebo se snažíte o něm dozvědět více. Až s testováním skončíte, odstraňte skupinu prostředků. Automaticky se tím odstraní virtuální počítač se všemi prostředky spojenými s danou skupinu prostředků. Další informace o skupinách prostředků najdete v tématu [Přehled Azure Resource Manageru](../../../azure-resource-manager/resource-group-overview.md).

* Jako **Umístění** vyberte oblast Azure, která bude toto nasazení hostovat.

* Kliknutím na **OK** uložte nastavení.

    ![Okno Základy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Volba velikosti virtuálního počítače

V kroku **Velikost** zvolte velikost virtuálního počítače v okně **Zvolte velikost**. V okně se po jeho otevření zobrazí doporučené velikosti počítačů na základě image, kterou jste vybrali.

> [!IMPORTANT]
> Odhadované měsíční náklady zobrazené v okně **Zvolit velikost** nezahrnují náklady na licencování SQL Serveru. Toto jsou náklady pouze na virtuální počítač. U edice SQL Serveru Express a Developer se jedná o celkové odhadované náklady. Pro ostatní edice se podívejte na [stránku s cenami pro virtuální počítače s Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) a vyberte cílovou edici vašeho SQL Serveru. Podívejte se také na [Doprovodné materiály k cenám pro virtuální počítače Azure s SQL Serverem](virtual-machines-windows-sql-server-pricing-guidance.md).

![Možnosti velikosti virtuálního počítače s SQL Serverem](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Doporučené velikosti a konfiguraci počítačů pro produkční úlohy najdete v tématu [Osvědčené postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-performance.md). Pokud potřebujete velikost počítače, která není uvedena, klikněte na tlačítko **Zobrazit všechno**.

> [!NOTE]
> Další informace o velikostech virtuálních počítačů najdete v tématu [Velikosti virtuálních počítačů](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Zvolte velikost počítače a potom klikněte na **Vybrat**.

## <a name="3-configure-optional-features"></a>3. Konfigurace volitelných funkcí

V okně **Nastavení** nakonfigurujte úložiště, sítě a monitorování Azure pro virtuální počítač.

* V části **Úložiště** vyberte **Ano** u možnosti **Použít Managed Disks**.

   > [!NOTE]
   > Microsoft pro SQL Server doporučuje službu Managed Disks. Služba Managed Disks se stará o úložiště na pozadí. Navíc, pokud jsou virtuální počítače se službou Managed Disks ve stejné skupině dostupnosti, Azure distribuuje prostředky úložiště pro zajištění odpovídající redundance. Další informace najdete v tématu [Přehled služby Azure Managed Disks](../../../storage/storage-managed-disks-overview.md). Podrobnosti o spravovaných discích ve skupině dostupnosti najdete v tématu [Použití spravovaných disků pro virtuální počítače ve skupině dostupnosti](../manage-availability.md).

* V části **Síť** může přijmout automaticky zadané hodnoty. Můžete také kliknutím na jednotlivé funkce ručně nakonfigurovat **virtuální síť**, **podsíť**, **veřejnou IP adresu** a **skupinu zabezpečení sítě**. Pro účely tohoto kurzu nechejte nastavené výchozí hodnoty.

* Azure umožňuje **monitorování** ve výchozím nastavení se stejným účtem, jaký je nastavený pro virtuální počítač. Tato nastavení tady můžete změnit.

* V části **Skupina dostupnosti** můžete pro účely tohoto kurzu ponechat výchozí hodnotu **žádná**. Pokud budete chtít nastavit Skupiny dostupnosti AlwaysOn SQL, nakonfigurujte dostupnost tak, aby se předešlo opětovnému vytvoření virtuálního počítače.  Další informace najdete v tématu [Správa dostupnosti virtuálních počítačů](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Po dokončení konfigurace těchto nastavení klikněte na **OK**.

## <a name="4-configure-sql-server-settings"></a>4. Konfigurace nastavení SQL Serveru
V okně **Nastavení SQL Serveru** nakonfigurujte konkrétní nastavení a optimalizace pro SQL Server. Pro SQL Server můžete například nakonfigurovat následující nastavení.

| Nastavení |
| --- |
| [Připojení](#connectivity) |
| [Ověřování](#authentication) |
| [Konfigurace úložiště](#storage-configuration) |
| [Automatizované opravy](#automated-patching) |
| [Automatizované zálohování](#automated-backup) |
| [Integrace se službou Azure Key Vault](#azure-key-vault-integration) |
| [Služby R](#r-services) |

### <a name="connectivity"></a>Připojení

V části **Připojení SQL** zadejte typ přístupu, který chcete mít k instanci SQL Serveru na tomto virtuálním počítači. Pro účely tohoto kurzu vyberte možnost **Veřejné (internet)**, která umožňuje připojovat se SQL Serveru z počítačů nebo služeb na internetu. Když je vybraná tato možnost, Azure automaticky nakonfiguruje bránu firewall a skupinu zabezpečení sítě tak, aby umožňovaly přenosy na portu 1433.

![Možnosti připojení SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Ve výchozím nastavení SQL Server naslouchá na dobře známém portu **1433**. Pokud chcete zvýšit zabezpečení, změňte port v předchozím dialogovém okně tak, aby SQL Server naslouchal na jiném než výchozím portu, například 1401. Pokud to provedete, ze všech nástrojů klienta, jako je SSMS, bude nutné se přihlašovat pomocí tohoto portu.

Aby bylo možné se k SQL Serveru připojovat prostřednictvím internetu, musíte také povolit ověřování SQL Serveru, které je popsané v následující části.

Pokud raději nechcete povolovat připojení k databázovému stroji prostřednictvím internetu, zvolte jednu z následujících možností:

* **Místní (jen uvnitř virtuálního počítače):** Umožňuje připojení k SQL Serveru pouze v rámci virtuálního počítače.
* **Privátní (uvnitř virtuální sítě):** Umožňuje připojení k SQL Serveru z počítačů nebo služeb ve stejné virtuální síti.

Obecně se doporučuje zvýšit zabezpečení výběrem nejvíce omezujícího připojení, které váš scénář umožňuje. Všechny možnosti je ale možné zabezpečit prostřednictvím pravidel skupin zabezpečení sítě a ověřování SQL Serveru a Windows. Skupinu zabezpečení sítě můžete upravit po vytvoření virtuálního počítače. Další informace najdete v tématu [Informace o zabezpečení pro SQL Server na virtuálních počítačích Azure](virtual-machines-windows-sql-security.md).

> [!NOTE]
> Image virtuálního počítače pro edici SQL Serveru Express nepovoluje automaticky protokol TCP/IP. To platí i pro veřejné a privátní možnosti připojení. Po vytvoření virtuálního počítače v edici Express je třeba [povolit protokol TCP/IP ručně](#configure-sql-server-to-listen-on-the-tcp-protocol) pomocí SQL Server Configuration Manageru.

### <a name="authentication"></a>Authentication

Pokud budete chtít vyžadovat ověřování SQL Serveru, klikněte v části **Ověřování SQL** na **Povolit**.

![Ověřování SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Pokud máte v plánu připojovat se k SQL Serveru prostřednictvím internetu (tj. s nastavením veřejného připojení), musíte tu povolit ověřování SQL Serveru. Veřejný přístup k SQL Serveru vyžaduje použití ověřování SQL Serveru.
> 
> 

Pokud povolíte ověřování SQL Serveru, zadejte **přihlašovací jméno** a **heslo**. Toto uživatelské jméno se nakonfiguruje jako přihlašovací jméno ověřování SQL Serveru a člen pevné role serveru **sysadmin**. Další informace o režimech ověřování najdete v tématu [Volba režimu ověřování](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

Pokud ověřování SQL Serveru nepovolíte, můžete pro připojení k instanci SQL Serveru používat účet místního správce ve virtuálním počítači.

### <a name="storage-configuration"></a>Konfigurace úložiště

Klikněte na **Konfigurace úložiště** a zadejte požadavky na úložiště.

![Konfigurace úložiště SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Pokud jste virtuální počítač ručně nakonfigurovali tak, aby používal Storage úrovně Standard, tato možnost není dostupná. Automatická optimalizace úložiště je k dispozici pouze pro Storage úrovně Premium.

> [!TIP]
> Počet zastavení a horní omezení každého posuvníku závisí na velikosti vybraného virtuálního počítače. Větší a výkonnější virtuální počítač umožňuje větší vertikální navýšení kapacity.

Požadavky můžete zadat jako vstupně-výstupní operace za sekundu (IOPs), propustnost v MB/s a celkovou velikost úložiště. Tyto hodnoty nakonfigurujte pomocí posuvníků. Tato nastavení úložiště můžete podle náročnosti zpracovávaných úloh změnit. Portál na základě těchto požadavků automaticky vypočítá počet disků, které se mají připojit a nakonfigurovat.

V části **Optimalizace úložiště** vyberte jednu z následujících možností:

* **Obecné:** Výchozí nastavení a podporuje většinu úloh.
* **Transakční:** Toto zpracování optimalizuje úložiště pro standardní úlohy databází OLTP.
* **Datové sklady:** Optimalizuje úložiště pro úlohy analýz a generování sestav.

### <a name="automated-patching"></a>Automatizované opravy

**Automatizované opravy** jsou ve výchozím nastavení povolené. Automatizované opravy umožňují na platformě Azure automaticky opravovat SQL Server a operační systém. Zadejte den v týdnu, čas a dobu trvání intervalu údržby. V té době pak Azure nainstaluje potřebné opravy. V rámci plánování intervalu údržby se pro čas používá národní prostředí virtuálních počítačů. Pokud nechcete, aby se v rámci Azure automaticky opravoval SQL Server a operační systém, klikněte na **Zakázat**.  

![Automatizované opravy pro SQL Server](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Další informace najdete v tématu [Automatizované opravy pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatizované zálohování

Automatické zálohování databází můžete pro všechny databáze povolit v části **Automatizované zálohování**. Automatizované zálohování je ve výchozím nastavení zakázané.

Když povolíte automatizované zálohování SQL, můžete nakonfigurovat následující:

* Doba uchování dat (dny) pro zálohování
* Účet úložiště, který se má používat pro zálohování
* Možnost šifrování a heslo pro zálohování
* Zálohování systémových databází
* Konfigurování plánu zálohování

Pokud chcete zálohy šifrovat, klikněte na **Povolit**. Pak zadejte **heslo**. Azure vytvoří certifikát pro šifrování záloh a používá zadané heslo k ochraně tohoto certifikátu.

![Automatizované zálohování SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 Další informace najdete v tématu [Automatizované zálohování pro SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integrace se službou Azure Key Vault

Pokud budete chtít ukládat tajné klíče zabezpečení v Azure pro šifrování, klikněte na **Integrace se službou Azure Key Vault** a klikněte na **Povolit**.

![Integrace se službou Azure Key Vault pro SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

V následující tabulce jsou uvedeny parametry, které jsou nezbytné pro konfiguraci Integrace se službou Azure Key Vault.

| PARAMETR | POPIS | PŘÍKLAD |
| --- | --- | --- |
| **Adresa URL služby Key Vault** |Umístění služby Key Vault |https://contosokeyvault.vault.azure.net/ |
| **Název objektu zabezpečení** |Hlavní název služby Azure Active Directory. Tento název se také označuje jako ID klienta. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Tajný kód objektu zabezpečení** |Tajný klíč objektu zabezpečení služby Azure Active Directory. Tento tajný klíč se také označuje jako Tajný klíč klienta. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Název přihlašovacího údaje** |**Název přihlašovacího údaje:** Integrace se službou Azure Key Vault vytvoří přihlašovací údaje v rámci SQL Serveru, díky čemuž mají virtuální počítače přístup do trezoru klíčů. Zvolte název pro tyto přihlašovací údaje. |moje_přihlaš1 |

Další informace najdete v tématu [Konfigurace Integrace se službou Azure Key Vault pro virtuální počítače Azure](virtual-machines-windows-ps-sql-keyvault.md).

### <a name="r-services"></a>Služby R

Je dostupná možnost pro povolení [služeb R na SQL Serveru](https://msdn.microsoft.com/library/mt604845.aspx). Ta umožňuje pomocí SQL Serveru 2016 provádět pokročilou analýzu. V okně **Nastavení SQL Serveru** klikněte na **Povolit**.

> [!NOTE]
> Pro SQL Server 2016 Developer Edition je tato možnost portálem nesprávně zakázaná. Pro edici Developer je potřeba služby R povolit ručně po vytvoření virtuálního počítače.

![Povolení služeb R na SQL Serveru](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

Po dokončení konfigurace nastavení SQL Serveru klikněte na **OK**.

## <a name="5-review-the-summary"></a>5. Kontrola souhrnných informací

V okně **Souhrn** zkontrolujte souhrn a pak kliknutím na **Koupit** vytvořte SQL Server, skupinu prostředků a prostředky zadané pro tento virtuální počítač.

Nasazení můžete monitorovat z webu Azure Portal. Tlačítko **Oznámení** v horní části obrazovky zobrazuje základní stav nasazení.

> [!NOTE]
> Abyste si udělali představu o tom, jak dlouho nasazování trvá, nasadil jsem virtuální počítač s SQL Serverem pro oblast Východní USA s výchozím nastavením. Toto testovací nasazení trvalo celkem 26 minut. Na základě vaší oblasti a vybraného nastavení ale můžete zaznamenat kratší nebo delší čas nasazení.

## <a name="open-the-vm-with-remote-desktop"></a>Otevření virtuálního počítače pomocí Vzdálené plochy

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

## <a name="connect-to-sql-server-remotely"></a>Vzdálené připojení k SQL Serveru

V tomto kurzu jsme pro virtuální počítač vybrali **veřejný** přístup a **ověřování SQL Serveru**. Tato nastavení automaticky nakonfigurovala virtuální počítač tak, aby povoloval připojení k SQL Serveru z libovolného klienta přes internet (za předpokladu, že má správné přihlašovací údaje SQL Serveru).

> [!NOTE]
> Pokud jste během zřizování nevybrali veřejný přístup, je nutné provést pár kroků navíc pro přístup k instanci SQL Serveru přes internet. Další informace najdete v tématu [Připojení virtuálnímu počítači s SQL Serverem](virtual-machines-windows-sql-connect.md).
> 
> 

Následující části vysvětlují, jak se připojit k instanci SQL Serveru na vašem virtuálním počítači z jiného počítače přes internet.

> [!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]
> 
> 

## <a name="next-steps"></a>Další kroky
Další informace o používání SQL Serveru v Azure najdete v tématu [SQL Server v Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) a [Nejčastější dotazy](virtual-machines-windows-sql-server-iaas-faq.md).

Podívejte se na video s přehledem SQL Serveru v Azure Virtual Machines v tématu [Azure Virtual Machines je nejlepší platformou pro SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Projděte si mapy kurzů](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) pro SQL Server na virtuálních počítačích Azure.
