---
title: "Funkce zabezpečení Azure používat s virtuálními počítači Azure | Microsoft Docs"
description: " Přehled funkcí zabezpečení Azure jádra, které lze použít s virtuálními počítači Azure. Virtuální počítače Azure nabízejí flexibilitu virtualizace bez nutnosti zakoupení a údržby fyzického hardwaru, na kterém běží virtuální počítač. "
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: 9d10b9fde99867a218c4368f0be6bee00bc44dc1
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/22/2017
---
# <a name="azure-virtual-machines-security-overview"></a>Přehled zabezpečení služby Azure virtuální počítače
Azure Virtual Machines vám umožní pružně nasadit řadu různých výpočetních řešení. Díky podpoře systémů Windows, Linux, SQL Server, Oracle, IBM, SAP a Azure BizTalk Services můžete nasadit jakoukoli úlohu a jakýkoli jazyk skoro v každém operačním systému.

Virtuální počítač Azure vám nabídne flexibilitu virtualizace bez nutnosti zakoupení a údržby fyzického hardwaru, na kterém virtuální počítač běží.  Můžete vytvořit a nasadit aplikace s záruku, že vaše data jsou chráněné a bezpečného v našich datacentrech vysoce zabezpečených.

S Azure můžete vytvořit rozšířené zabezpečení a kompatibilní řešení který:

* Ochrana virtuálních počítačů před viry a malwarem
* Šifrování citlivých dat
* Zabezpečení provozu sítě
* Identifikace a detekce hrozeb
* Splnění požadavků na dodržování předpisů

Cílem tohoto článku je poskytovat přehled o základní funkce zabezpečení Azure, které můžete používat s virtuálními počítači. Poskytujeme také odkazy na články, které poskytují podrobnosti o každé funkce tak další informace.  

Základní funkce zabezpečení virtuálního počítače Azure mají být zahrnuty v tomto článku:

* Antimalware
* Modul hardwarového zabezpečení
* Šifrování disku virtuálního počítače
* Záloha virtuálního počítače
* Azure Site Recovery
* Virtuální síť
* Správa zásad zabezpečení a vytváření sestav
* Dodržování předpisů

## <a name="antimalware"></a>Antimalware
S Azure můžete použít antimalwarový software od dodavatelů zabezpečení, jako je Microsoft, Symantec, Trend Micro a Kaspersky k ochraně vašich virtuálních počítačů ze škodlivých souborů, adwaru a dalšími hrozbami. Projděte si další část níže najít články v partnerských řešení.

Antimalware od Microsoftu pro Azure Cloud Services a virtuální počítače je funkce ochrany v reálném čase, který pomáhá identifikovat a odstraňovat viry, spyware a další škodlivý software.  Antimalware od Microsoftu poskytuje konfigurovat výstrahy, když označuje, že škodlivý nebo nežádoucí software pokusí nainstalovat nebo spustit v Azure systémech.

Antimalware od Microsoftu je řešení jednoho agenta pro aplikace a prostředí klientovi, určený ke spouštění na pozadí bez lidského zásahu. Můžete nasadit ochranu na základě potřeb vašich zatížení aplikací, s buď základní zabezpečení výchozím nebo advanced vlastní konfigurace, včetně monitorování proti malwaru.

Při nasazení a povolit Antimalware od Microsoftu, jsou k dispozici následující klíčových funkcí:

* Ochrana v reálném čase – monitorování aktivity v cloudové služby a na virtuálních počítačích, na zjištění a blokování malwaru provádění.
* Naplánované prohledávání - pravidelně provádí cílové kontrolu ke zjištění malwaru, včetně aktivně spouštění programů.
* Malwarové nápravy - automaticky provádět akce se zjištěným malwarem, jako je například odstranit nebo umístění do karantény škodlivé soubory a čištění položky škodlivé registru.
* Aktualizace podpisu - automaticky nainstaluje nejnovější podpisy ochrany (definice virů) pro zajištění ochrany je aktuální na předem určené frekvenci.
* Antimalwarový stroj aktualizuje – automaticky aktualizuje modul Antimalware od Microsoftu.
* Platforma antimalwarových – se automaticky aktualizuje, aktualizuje platformou Antimalware od Microsoftu.
* Aktivní ochranu - sestav Azure telemetrie metadata o zjištěných hrozeb a podezřelé prostředky zajistit rychlou reakci a umožňuje v reálném čase synchronní podpis doručení prostřednictvím sítě Microsoft Active Protection System (MAPS).
* Ukázky vytváření sestav – poskytuje a sestavy ukázky pro službu Microsoft Antimalware pomohou Upřesnit, služby a povolte řešení potíží.
* Vyloučení – umožňuje aplikace a správců služeb ke konfiguraci určitých souborů a procesy a jednotky je vyloučit z ochrany a kontrolu pro výkon a z jiných důvodů.
* Shromažďování událostí antimalwarových - zaznamenává stav antimalwarové služby, podezřelé aktivity a nápravné akce prováděné v protokolu událostí operačního systému a shromažďuje je do účtu úložiště Azure zákazníka.

Další informace: Další informace o antimalwarový software k ochraně vašich virtuálních počítačů najdete v tématu:

* [Antimalware od Microsoftu pro cloudové služby Azure a virtuální počítače](azure-security-antimalware.md)
* [Nasazování antimalwarových řešení na virtuálních počítačích Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Postup instalace a konfigurace Trend Micro hluboké Security jako služba na virtuální počítač s Windows](../virtual-machines/windows/classic/install-trend.md)
* [Postup instalace a konfigurace Symantec Endpoint Protection na virtuální počítač s Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Řešení zabezpečení v Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Zabezpečení hardwaru modulu
Šifrování a ověření ochrany lze zvýšit pomocí zvýšení zabezpečení klíčů. Díky ukládání v Azure Key Vault můžete zjednodušit správu a zabezpečení vašich důležitých tajných klíčů a klíče. Key Vault umožňuje ukládat klíče v modulech zabezpečení hardwaru (HSM) s certifikací podle standardů FIPS 140-2 úrovně 2. Klíčů pro šifrování SQL Server pro zálohování nebo [transparentní šifrování dat](https://msdn.microsoft.com/library/bb934049.aspx) všechny se uloží Key Vault se klíčů nebo tajných údajů z vašich aplikací. Oprávnění a přístup k tyto chráněné položky se spravují prostřednictvím [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Další informace:

* [Co je Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Blog Azure Key Vault](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Šifrování disku virtuálního počítače
Azure Disk Encryption je novou funkci, která umožňuje šifrování disků systému Windows a Linux Azure virtuálního počítače. Azure Disk Encryption používá oborový standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce systému Linux zajistit šifrování svazku operačního systému a datové disky.

Řešení jsou integrované s Azure Key Vault můžete řídit a spravovat disku šifrovacích klíčů a tajných klíčů v trezoru klíčů předplatného, a zajistit, že všechna data v disky virtuálního počítače jsou zašifrovaná přinejmenším ve službě Azure storage.

Další informace:

* [Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
* [Azure Disk Encryption pro systémy Linux a virtuální počítače s Windows](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
* [Šifrování virtuálního počítače](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Záloha virtuálního počítače
Azure Backup je škálovatelné řešení, které chrání aplikační data. Nevyžaduje žádnou počáteční investici a má minimální provozní náklady. Chyby aplikací můžou poškodit vaše data a lidské omyly zase můžou způsobit chyby v aplikacích. S Azure Backup jsou chráněné virtuální počítače s Windows a Linux.

Další informace:

* [Co je Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup studijní](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Služby zálohování Azure – nejčastější dotazy](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Důležitou součástí strategie BCDR organizace je přijít na to, jak udržovat firemní úlohy a aplikace v provozu, když dojde k plánovanému i neplánovanému výpadku. Azure Site Recovery pomáhá orchestraci replikace, převzetí služeb při selhání a obnovení úloh a aplikací, aby byly k dispozici ze sekundární lokality Pokud primární lokalita ocitne mimo provoz.

Site Recovery:

* **Zjednodušuje strategie BCDR** – Site Recovery usnadňuje zpracování replikace, převzetí služeb při selhání a obnovení několika podnikových úloh a aplikací z jednoho umístění. Site Recovery orchestruje replikaci a převzetí služeb při selhání, ale nezachycuje data aplikací ani o nich nemá žádné informace.
* **Nabízí flexibilní replikace** – pomocí Site Recovery můžete replikovat úlohy běžící na virtuálních počítačích Hyper-V, virtuálních počítačů VMware a fyzické servery Windows nebo Linuxem.
* **Podporuje převzetí služeb při selhání a obnovení** – Site Recovery poskytuje testovací převzetí služeb při selhání na Nácvik zotavení po havárii aniž by to ovlivňovalo produkční prostředí. Pro očekávané výpadky je možné spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak mohou proběhnout neplánovaná převzetí služeb s minimálními ztrátami dat (podle četnosti replikací). Po obnovení můžete služby, které byly převzaty při selhání, navrátit na primární lokality. Site Recovery poskytuje plány obnovení, které mohou obsahovat skripty a sešity automatizace Azure, což vám umožní přizpůsobit si přebírání služeb při selhání a obnovování vícevrstvých aplikací.
* **Eliminuje sekundárního datacentra** – můžete replikovat do sekundárního místního lokality nebo do Azure. Pomocí Azure jako cíl pro zotavení po havárii se eliminují náklady a složitost udržováním sekundární lokality. Replikovaná data jsou uložena ve službě Azure Storage.
* **Umožňuje integraci s existujícími technologiemi BCDR** – Site Recovery spolupracuje s funkcemi BCDR jiných aplikací. Například můžete pomocí Site Recovery k ochraně SQL Server back-end podnikové úloh. To zahrnuje nativní podpora pro SQL Server AlwaysOn pro správu převzetí služeb při selhání skupiny dostupnosti.

Další informace:

* [Co je Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Jak funguje Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Jaké úlohy jsou chráněné službou Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuální síť
Třeba virtuální počítače připojení k síti. Pro podporu tohoto požadavku, Azure vyžaduje virtuální počítače k připojení k virtuální síti Azure. Virtuální síť Azure je logická konstrukce postavená na Azure síťových prostředcích infrastruktury. Každé logické síti Azure Virtual Network je izolovaná od všech ostatních Azure virtuálních sítí. Tato izolace pomáhá zajistit, že síťový provoz v nasazeních není dostupný ostatním zákazníkům Microsoft Azure.

Další informace:

* [Přehled zabezpečení sítě Azure](security-network-overview.md)
* [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md)
* [Funkce sítě a partnerství pro podnikové scénáře](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Správa zásad zabezpečení a vytváření sestav
Azure Security Center pomáhá zabránit, zjistit a reagovat na hrozby a poskytuje že vyšší přehled a kontrolu nad, zabezpečení vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

Azure Security Center pomáhá optimalizovat a monitorování zabezpečení virtuálního počítače pomocí:

* Virtuální počítač poskytuje [doporučení zabezpečení](../security-center/security-center-recommendations.md) jako aktualizace systému konfigurace seznamy ACL koncových bodů, povolení antimalwarového řešení, povolit skupin zabezpečení sítě a použít šifrování disku.
* Monitorování stavu virtuálních počítačů

Další informace:

* [Úvod do Azure Security Center](../security-center/security-center-intro.md)
* [Nejčastější dotazy k Azure Security Center](../security-center/security-center-faq.md)
* [Operace a plánování Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Dodržování předpisů
Virtuální počítače Azure je certifikované pro FISMA, FedRAMP, HIPAA, PCI DSS úrovně 1 a další programy klíče dodržování předpisů. Tato certifikační usnadní pro vaše vlastní aplikace Azure pro splnění požadavků na dodržování předpisů a pro vaši firmu, chcete-li vyřešit širokou škálu příslušné místní i mezinárodní zákonné požadavky.

Další informace:

* [Centrum zabezpečení Microsoft: dodržování předpisů](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
* [Důvěryhodné cloudu: Microsoft Azure zabezpečení, ochrany osobních údajů a dodržování předpisů](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
