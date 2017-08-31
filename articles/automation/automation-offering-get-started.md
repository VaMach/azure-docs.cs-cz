--- 
title: "Začínáme s Azure Automation | Dokumentace Microsoftu"
description: "Tento článek obsahuje přehled služby Azure Automation. Najdete tu popis návrhu a podrobnosti implementace v rámci přípravy na zprovoznění nabídky z Azure Marketplace."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/18/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 54f137b26bf1c8f966e8ef110dcf3d25abf7ac5b
ms.contentlocale: cs-cz
ms.lasthandoff: 08/19/2017

---

# <a name="getting-started-with-azure-automation"></a>Začínáme s Azure Automation

Tato příručka Začínáme představuje základní koncepty související s nasazením služby Azure Automation. Ať už se službou Automation v Azure teprve začínáte nebo máte zkušenostmi se softwarem pro pracovní postupy automatizace, jako je System Center Orchestrator, tento průvodce vám pomůže porozumět přípravě a zprovoznění služby Automation.  Potom budete připraveni začít vyvíjet runbooky na podporu vašich potřeb automatizace procesů. 


## <a name="automation-architecture-overview"></a>Přehled architektury služby Automation

![Přehled Azure Automation](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

Azure Automation je aplikace SaaS (software jako služba), která poskytuje škálovatelné a spolehlivé víceklientské prostředí pro automatizaci procesů pomocí runbooků a správu změn konfigurace v systémech Windows a Linux s využitím konfigurace požadovaného stavu (DSC) v Azure, ostatních cloudových službách nebo místním prostředí. Entity obsažené ve vašem účtu Automation, jako jsou runbooky, prostředky a účty Spustit jako, jsou izolované od ostatních účtů Automation v rámci vašeho předplatného a ostatních předplatných.  

Runbooky, které spouštíte v Azure, běží v sandboxech Automation, které jsou hostované ve virtuálních počítačích Azure typu platforma jako služba (PaaS).  Sandboxy Automation poskytují izolaci tenantů pro všechny aspekty spuštění runbooků – moduly, úložiště, paměť, síťové komunikace, datové proudy úlohy atd. Tato role je spravovaná službou, není dostupná z vašeho účtu Azure nebo Azure Automation a nemůžete ji řídit.         

K automatizaci nasazení a správy prostředků v místním datovém centru nebo jiných cloudových službách můžete po vytvoření účtu Automation určit jeden nebo několik počítačů, na kterých poběží role [Hybrid Runbook Worker (HRW)](automation-hybrid-runbook-worker.md).  Každý proces HRW vyžaduje agenta Microsoft Management Agent s připojením k pracovnímu prostoru Log Analytics a účet Automation.  Log Analytics se používá ke spuštění instalace, údržbě agenta Microsoft Management Agent a monitorování procesu HRW.  Doručování runbooků a instrukce k jejich spuštění provádí Azure Automation.

Můžete nasadit několik HRW k zajištění vysoké dostupnosti pro runbooky, vyrovnávání zatížení runboiokových úloh a v některých případech je můžete vyhradit pro konkrétní úlohy nebo prostředí.  Microsoft Monitoring Agent v procesu HRW navazuje komunikaci se službou Automation přes port TCP 443 a nemá žádné požadavky na bránu firewall pro příchozí provoz.  Když je runbook spuštěný v procesu HRW v rámci prostředí a chcete ho využít k provádění úloh správy pro jiné počítače nebo služby v tomto prostředí, je možné, že runbook bude potřebovat přístup i k dalším portům.  Pokud zásady zabezpečení IT neumožňují, aby se počítače ve vaší síti připojovaly k internetu, přečtěte si článek o [bráně OMS](../log-analytics/log-analytics-oms-gateway.md), která pro HRW funguje jako proxy pro shromažďování stavu úloh a příjem konfiguračních informací z vašeho účtu Automation.

Runbooky, které běží v HRW, se spouští v kontextu místního systémového účtu v počítači, který je doporučeným kontextem zabezpečení při provádění akcí správy v místním počítači s Windows. Pokud chcete, aby runbook spouštěl úlohy s využitím prostředků mimo místní počítač, je možné, že budete muset definovat zabezpečené assety přihlašovacích údajů v účtu Automation, ke kterému máte z runbooku přístup, a používat je k ověřování pro externí zdroj. Assety [Přihlašovací údaje](automation-credentials.md), [Certifikát](automation-certificates.md) a [Propojení](automation-connections.md) ve vašem runbooku můžete používat s rutinami, které umožňují specifikovat přihlašovací údaje, abyste je mohli ověřit.

Konfigurace DSC uložené ve službě Azure Automation je možné použít přímo na virtuálních počítačích Azure. Ostatní fyzické a virtuální počítače mohou žádat o konfigurace ze serveru vyžádané replikace Azure Automation DSC.  Pro správu konfigurací vašich místních fyzických nebo virtuálních systémů Windows a Linux nemusíte nasazovat žádnou infrastrukturu pro podporu serveru vyžádané replikace Automatizace DSC. Stačí, aby byl odchozí internetový přístup z každého systému spravovaný Automatizací DSC a aby se ke komunikaci se službou OMS využíval port TCP 443.   

## <a name="prerequisites"></a>Požadavky

### <a name="automation-dsc"></a>Automatizace DSC
Azure Automation DSC se dá využít ke správě různých počítačů:

* Virtuální počítače Azure (klasické) s Windows nebo Linuxem
* Virtuální počítače Azure s Windows nebo Linuxem
* Virtuální počítače AWS (Amazon Web Services) s Windows nebo Linuxem
* Fyzické nebo virtuální počítače s Windows v místním prostředí nebo v jiném cloudu než Azure nebo AWS
* Fyzické nebo virtuální počítače s Linuxem v místním prostředí nebo v jiném cloudu než Azure nebo AWS

K zajištění komunikace Windows s Azure Automation musí být nainstalovaná nejnovější verze WMF 5 pro agenta PowerShell DSC. K zajištění komunikace Linuxu s Azure Automation musí být nainstalovaná nejnovější verze [agenta PowerShell DSC pro Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150).

### <a name="hybrid-runbook-worker"></a>Hybrid Runbook Worker  
Počítač pro spuštění hybridních runbookových úloh musí splňovat tyto podmínky:

* Windows Server 2012 nebo novější
* Windows PowerShell 4.0 nebo novější  Pro zvýšení spolehlivosti doporučujeme na tento počítač nainstalovat Windows PowerShell 5.0. Nejnovější verzi si můžete stáhnout z webu [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=50395).
* Minimálně dvě jádra
* Minimálně 4 GB paměti RAM

### <a name="permissions-required-to-create-automation-account"></a>Oprávnění požadovaná k vytvoření účtu Automation
Pokud chcete vytvořit nebo aktualizovat účet Automation, musíte mít následující specifická oprávnění vyžadovaná k dokončení tohoto tématu.   
 
* Abyste mohli vytvořit účet Automation, váš uživatelský účet AD musí být přidán do role se stejnými oprávněními jako role vlastníka pro prostředky Microsoft.Automation, jak je uvedeno v článku [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md).  
* Pokud je nastavení Registrace aplikací nastaveno na **Ano**, uživatelé ve vašem tenantovi Azure AD, kteří nejsou správci, můžou [registrovat aplikace služby AD](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions).  Pokud je nastavení Registrace aplikací nastaveno na **Ne**, uživatel provádějící tuto akci musí být globálním správcem služby Azure AD. 

Pokud před přidáním do role globálního správce nebo spolusprávce nejste členem instance Active Directory příslušného předplatného, budete do služby Active Directory přidaní jako host. V takové situaci se zobrazí upozornění Nemáte oprávnění k vytvoření... v okně **Přidání účtu Automation**. Uživatele, kteří byli nejdřív přidaní do role globálního správce nebo spolusprávce, je možné z instance Active Directory předplatného odebrat a potom je znovu přidat – tak se z nich ve službě Active Directory stanou úplní uživatelé. Takovou situaci můžete ověřit v podokně **Azure Active Directory** na webu Azure Portal. Vyberte **Uživatelé a skupiny**, potom **Všichni uživatelé** a po výběru konkrétního uživatele vyberte **Profil**. Hodnota atributu **Typ uživatele** v profilu uživatele by neměla být **Host**.

## <a name="authentication-planning"></a>Plánování ověřování
Azure Automation umožňuje automatizovat úlohy s prostředky v Azure, v místním prostředí a u jiných poskytovatelů cloudu.  Aby mohl runbook provádět požadované akce, musí mít oprávnění pro bezpečný přístup k prostředkům s minimálními požadovanými právy v rámci předplatného.  

### <a name="what-is-an-automation-account"></a>Co je účet Automation 
Všechny úlohy automatizace, které s prostředky provádíte pomocí rutin Azure ve službě Azure Automation, se ověřují pro Azure pomocí ověřování na základě přihlašovacích údajů organizační identity v Azure Active Directory.  Účet Automation nesouvisí s účtem, který používáte k přihlášení na portál a konfiguraci a použití prostředků Azure.  Součástí účtu Automation jsou následující prostředky služby Automation:

* **Certifikáty** – obsahuje certifikát používaný k ověřování z runbooku nebo konfigurace DSC nebo je možné ho přidat.
* **Připojení** – obsahuje ověřovací a konfigurační údaje vyžadované pro připojení k externí službě nebo aplikaci z runbooku nebo konfigurace DSC.
* **Přihlašovací údaje** – jedná se o objekt PSCredential obsahující přihlašovací údaje zabezpečení, jako je uživatelské jméno a heslo, vyžadované pro ověření z runbooku nebo konfigurace DSC.
* **Integrační moduly** – jedná se o moduly PowerShellu zahrnuté v účtu Azure Automation, které umožňují používání rutin v rámci runbooků a konfigurací DSC.
* **Plány** – obsahuje plány, které v zadaném čase spouští nebo zastavují runbooky, včetně frekvencí opakování.
* **Proměnné** – obsahuje hodnoty, které jsou dostupné z runbooku nebo konfigurace DSC.
* **Konfigurace DSC** – jsou skripty PowerShellu popisující, jak nakonfigurovat funkci nebo nastavení operačního systému nebo jak nainstalovat aplikaci na počítači s Windows nebo Linuxem.  
* **Runbooky** – jsou sady úloh, které provádějí určité automatizované procesy v Azure Automation na základě Windows PowerShellu.    

Prostředky Automation jednotlivých účtů Automation jsou přidružené k jedné oblasti Azure, ale účty Automation mohou spravovat veškeré prostředky v rámci předplatného. Účty Automation vytvořte v různých oblastech, pokud máte zásady, které vyžadují izolaci dat a prostředků v určité oblasti.

> [!NOTE]
> Účty Automation a v nich obsažené prostředky, které jsou vytvořené pomocí portálu Azure, nemůžete otevírat pomocí portálu Azure Classic. Pokud chcete tyto účty nebo jejich prostředky spravovat pomocí rozhraní Windows PowerShell, použijte moduly Azure Resource Manageru.
> 

Když na webu Azure Portal vytvoříte účet Automation, automaticky se vytvoří dvě ověřovací entity:

* Účet Spustit jako. Tento účet vytvoří instanční objekt ve službě Azure Active Directory (Azure AD) a certifikát. Přiřadí také řízení přístupu na základě role Přispěvatel (RBAC), které spravuje prostředky Resource Manageru pomocí runbooků.
* Účet Spustit jako pro Azure Classic. Tento účet nahraje certifikát pro správu, který se používá ke správě klasických prostředků pomocí runbooků.

Řízení přístupu na základě role je dostupné v aplikaci Azure Resource Manager pro udělování povolených akcí na uživatelském účtu služby Azure AD a účtu Spustit jako a ověřování takového objektu služby.  Přečtěte si článek [Řízení přístupu na základě role ve službě Azure Automation](automation-role-based-access-control.md), kde najdete další informace, které vám pomůžou s vývojem vašeho modelu pro správu oprávnění ve službě Automation.  

#### <a name="authentication-methods"></a>Metody ověřování
Následující tabulka shrnuje různé metody ověřování pro jednotlivá prostředí podporovaná službou Azure Automation.

| Metoda | Prostředí 
| --- | --- | 
| Účet Spustit jako pro Azure a Spustit jako pro Azure Classic |Azure Resource Manager a nasazení Azure Classic |  
| Uživatelský účet Azure AD |Azure Resource Manager a nasazení Azure Classic |  
| Ověřování systému Windows |Místní datové centrum nebo jiný poskytovatele cloudu s využitím procesu Hybrid Runbook Worker |  
| Přihlašovací údaje služby Amazon Web Services |Amazon Web Services |  

V části **Postupy\Ověřování a zabezpečení** najdete doprovodné články s přehledem a postupem implementace pro konfiguraci ověřování v těchto prostředích, a to buď pomocí stávajícího účtu, nebo nového účtu, který vyhradíte pro příslušné prostředí.  Pro účty Spustit jako pro Azure a Spustit jako pro Azure Classic téma [Aktualizace účtu Automation Spustit jako](automation-create-runas-account.md) popisuje, jak aktualizovat stávající účet Automation s účty Spustit jako z portálu nebo pomocí PowerShellu, pokud původně nebyl s účtem Spustit jako pro Azure nebo Spustit jako pro Azure Classic nakonfigurovaný. Pokud chcete vytvořit účet Spustit jako a účet Spustit jako pro Azure Classic s využitím certifikátu vydaného certifikační autoritou (CA) vaší organizace, přečtěte si tento článek, ve kterém zjistíte, jak vytvořit účty s touto konfigurací.     
 
## <a name="network-planning"></a>Plánování sítě
Aby se Hybrid Runbook Worker připojil k sadě Microsoft Operations Management Suite (OMS) a registroval, musí mít přístup k portu a adresám URL, které jsou uvedené dál.  To je navíc k [portům a adresám URL požadovaným pro agenta MMA (Microsoft Monitoring Agent)](../log-analytics/log-analytics-windows-agents.md#network) pro připojení k OMS. Pokud ke komunikaci mezi agentem a službou OMS používáte proxy server, budete se muset ujistit, že jsou dostupné příslušné prostředky. Pokud používáte k omezení přístupu k internetu bránu firewall, je nutné ji nakonfigurovat tak, aby povolovala přístup.

Dál jsou uvedené informace o portu a adresách URL, které jsou potřeba k tomu, aby Hybrid Runbook Worker mohl komunikovat se službou Automation.

* Port: Vyžaduje se jenom TCP 443 pro odchozí internetový přístup.
* Globální adresa URL: *.azure-automation.net

Pokud máte účet Automation definovaný pro konkrétní oblast a chcete komunikaci s tímto datovým centrem omezit, následující tabulka obsahuje záznam DNS pro každou oblast.

| **Oblast** | **Záznam DNS** |
| --- | --- |
| Střed USA – jih |scus-jobruntimedata-prod-su1.azure-automation.net |
| Východní USA 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Západní střed USA | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Západní Evropa |we-jobruntimedata-prod-su1.azure-automation.net |
| Severní Evropa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Střední Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Jihovýchodní Asie |sea-jobruntimedata-prod-su1.azure-automation.net |
| Střed Indie |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonsko – východ |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Austrálie – jihovýchod |ase-jobruntimedata-prod-su1.azure-automation.net |
| Spojené království – jih | uks-jobruntimedata-prod-su1.azure-automation.net |
| USA (Gov) – Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Pokud chcete seznam IP adres místo názvů, z Microsoft Download Center si stáhněte a prohlédněte soubor XML [IP adresy datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

> [!NOTE]
> Tento soubor obsahuje rozsahy IP adres (včetně rozsahů pro Compute, SQL a službu Storage), které se používají v datacentrech Microsoft Azure. Každý týden se zveřejňuje aktualizovaný soubor odrážející aktuálně nasazené rozsahy a všechny nadcházející změny rozsahů IP adres. Rozsahy, které jsou v souboru uvedeny nově, se v datacentrech nebudou používat ještě minimálně týden. Každý týden si stáhněte nový soubor XML a proveďte na svém webu potřebné změny pro zajištění správné identifikace služeb spuštěných v Azure. Uživatelé ExpressRoute si můžou všimnout, že se tento soubor používá k aktualizaci inzerování prostoru Azure pomocí protokolu BGP, která probíhá první týden každého měsíce. 
> 

## <a name="creating-an-automation-account"></a>Vytvoření účtu Automation

Účet Automation můžete na webu Azure Portal vytvořit několika různými způsoby.  Následující tabulka uvádí jednotlivé typy nasazení a rozdíly mezi nimi.  

|Metoda | Popis |
|-------|-------------|
| Výběr Automation and Control z Marketplace | Nabídka, která vytvoří vzájemně propojený účet Automation a pracovní prostor OMS ve stejné skupině prostředků a oblasti.  Jednou z výhod integrace s OMS je také možnost použít službu Log Analytics k monitorování a analýze stavů úloh runbooků a datových proudů úloh v průběhu času a využít pokročilé funkce k eskalaci nebo vyšetření problémů. Tato nabídka také nasadí řešení Change Tracking a Update Management, které jsou ve výchozím nastavení povolené. |
| Výběr Automation z Marketplace | Vytvoří účet Automation v nové nebo stávající skupině prostředků, který není propojený s pracovním prostorem OMS a neobsahuje žádné dostupné řešení z nabídky Automation and Control. Toto je základní konfigurace, která vás seznámí se službou Automation. Můžete se jejím prostřednictvím naučit, jak psát runbooky, konfigurovat DSC a využívat funkce této služby. |
| Vybraná řešení pro správu | Pokud vyberete řešení – **[Správa aktualizací](../operations-management-suite/oms-solution-update-management.md)**, **[Spustit nebo zastavit virtuální počítače mimo pracovní dobu](automation-solution-vm-management.md)** nebo **[Sledování změn](../log-analytics/log-analytics-change-tracking.md)**, vyzve vás k výběru existujícího účtu Automation a pracovního prostoru OMS nebo vám nabídne možnost vytvořit tento účet i prostor tak, jak to řešení k nasazení ve vašem předplatném potřebuje. |

Toto téma vás provede vytvořením účtu Automation a pracovního prostoru OMS připojením nabídky Automation and Control.  Pokud chcete vytvořit samostatný účet Automation pro testování nebo zobrazení náhledu služby, přečtěte si článek [Vytvoření samostatného účtu Automation](automation-create-standalone-account.md).  

### <a name="create-automation-account-integrated-with-oms"></a>Vytvoření účtu Automation integrovaného s OMS
Doporučenou metodou připojení služby Automation je výběr nabídky Automation and Control z Marketplace.  Tím se vytvoří účet Automation a naváže integrace s pracovním prostorem OMS, včetně možnosti instalovat řešení pro správu, které jsou pro tuto nabídku dostupné.  

1. Přihlaste se k webu Azure Portal pomocí účtu, který je členem role správců předplatného a spolusprávcem předplatného.

2. Klikněte na možnost **Nové**.<br><br> ![Výběr možnosti Nové na webu Azure Portal](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  

3. Vyhledejte **Automation** a potom ve výsledcích hledání vyberte **Automation and Control***.<br><br> ![Hledání a výběr Automation & Control na Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png)<br>   

4. Přečtěte si popis této nabídky a klikněte na **Vytvořit**.  

5. V okně nastavení **Automation and Control** vyberte **Pracovní prostor OMS**.  V okně **Pracovní prostory OMS** vyberte pracovní prostor OMS propojený s předplatným Azure, které obsahuje příslušný účet Automation, nebo vytvořte nový pracovní prostor OMS.  Pokud nemáte pracovní prostor OMS, můžete vybrat **Vytvořit nový pracovní prostor** a v okně **Pracovní prostor OMS** provést následující kroky: 
   - Zadejte název pro nový **pracovní prostor OMS**.
   - Vyberte **předplatné**, které má být cílem propojení, výběrem z rozevíracího seznamu, pokud výchozí vybrané předplatné není vhodné.
   - U položky **Skupina prostředků** můžete vytvořit skupinu prostředků nebo vybrat už existující skupinu prostředků.  
   - Vyberte **Umístění**.  V současnosti jsou pro výběr dostupná jenom umístění **Austrálie – jihovýchod**, **Východní USA**, **Jihovýchodní Asie**, **Střed USA – západ** a **Západní Evropa**.
   - Vyberte možnost u položky **Cenová úroveň**.  Řešení se nabízí ve dvou úrovních: úroveň Free a úroveň Per Node (OMS).  V úrovni Free je omezen objem dat shromážděných za den, doba uchovávání a počet minut běhu úloh runbooku.  V úrovni Per Node (OMS) není objem dat shromážděných za den nijak omezený.  
   - Vyberte **Účet Automation**.  Pokud vytváříte nový pracovní prostor OMS, bude potřeba, abyste vytvořili i nový účet služby Automation, který je přidružený k novému pracovnímu prostoru OMS určenému dříve, a to včetně vašeho předplatného Azure, skupiny prostředků a oblasti.  Můžete vybrat **Vytvořit účet Automation** a v okně **čet Automation** zadat následující údaje: 
  - Do pole **Název** zadejte název účtu služby Automation.

    Všechny ostatní možnosti se vyplní automaticky na základě vybraného pracovního prostoru OMS. Tyto možnosti nelze upravovat.  Účet Spustit v Azure jako představuje výchozí metodu ověřování pro tuto nabídku.  Po kliknutí na **OK** se ověří možnosti konfigurace a vytvoří se účet služby Automation.  Průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

    Další možností je vybrat existující účet služby Automation Spustit jako.  Účet, který vyberete, nemůže být propojený s jiným pracovním prostorem OMS. V opačném případě se v okně zobrazí příslušné oznámení.  Pokud je toto propojení už vytvořené, je potřeba vybrat jiný účet služby Automation Spustit jako nebo vytvořit nový.

    Po dokončení požadovaných informací klikněte na **Vytvořit**.  Informace se ověří a vytvoří se účet Automation a účet Spustit jako.  Automaticky se vrátíte do okna **Pracovní prostor OMS**.  

6. Po zadání požadovaných informací v okně **Pracovní prostor OMS** klikněte na **Vytvořit**.  Během ověřování informací a vytváření pracovního prostoru můžete průběh zpracování sledovat prostřednictvím položky nabídky **Oznámení**.  Vrátíte se zpátky do okna **Přidat řešení**.  

7. V okně nastavení **Automation and Control** potvrďte, že chcete nainstalovat doporučená předem vybraná řešení. Pokud výběr některého z nich zrušíte, můžete ho nainstalovat později.  

8. Klikněte na **Vytvořit** a pokračujte ve zprovozňování služby Automation a pracovního prostoru OMS. Všechna nastavení se ověří a potom se provede pokus o nasazení nabídky v rámci vašeho předplatného.  Dokončení tohoto procesu může trvat několik sekund a průběh zpracování můžete sledovat prostřednictvím možnosti nabídky **Oznámení**. 

Po zprovoznění nabídky můžete začít vytvářet runbooky, pracovat s řešeními pro správu, která jste povolili, nasadit roli [Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) nebo začít využívat službu [Log Analytics](https://docs.microsoft.com/azure/log-analytics) ke shromažďování dat vygenerovaných vašimi prostředky v cloudových nebo místních prostředích.   

## <a name="next-steps"></a>Další kroky
* Pokud chcete ověřit, že nový účet Automation umožňuje ověřování prostřednictvím prostředků Azure, prohlédněte si [test ověřování účtu Azure Automation Spustit jako](automation-verify-runas-authentication.md).
* Pokud chcete začít s vytvářením runbooků, před samotným vytváření nejprve zkontrolujte podporované [Typy runbooků ve službě Automation](automation-runbook-types.md) a související důležité informace.



