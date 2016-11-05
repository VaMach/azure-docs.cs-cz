---
title: Zabezpečení dat ve službě Azure Security Center | Microsoft Docs
description: Tento dokument popisuje způsob správy a ochrany dat ve službě Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2016
ms.author: yurid

---
# Zabezpečení dat ve službě Azure Security Center
Služba Azure Security Center pomáhá zákazníkům předcházet hrozbám, detekovat je a reagovat na ně tím, že shromažďuje a zpracovává data o prostředcích Azure, včetně informací o konfiguraci, metadat, protokolů událostí, souborů se stavem systému a dalších dat. Zavázali jsme se, že soukromí a bezpečnost těchto dat budeme chránit. Společnost Microsoft dodržuje přísné pokyny pro dodržování předpisů a zabezpečení – od psaní kódu po provoz služeb. 

Tento článek popisuje způsob správy a ochrany dat ve službě Azure Security Center.

## Zdroje dat
Azure Security Center analyzuje data z následujících zdrojů:

* Služby Azure: Čte informace o konfiguraci služeb Azure, které máte nasazené, tím, že komunikuje s poskytovatelem prostředků pro danou službu.
* Síťový provoz: Čte vzorkovaná metadata síťového provozu z infrastruktury společnosti Microsoft, například zdrojovou a cílovou adresu IP/port, velikost paketu nebo síťový protokol.
* Partnerská řešení: Shromažďuje výstrahy zabezpečení ze všech integrovaných partnerských řešení, jako jsou například brány firewall a antimalwarová řešení. Tato data jsou ukládána do úložiště služby Azure Security Center, které se aktuálně nachází ve Spojených státech.
* Virtuální počítače: Služba Azure Security Center může shromažďovat informace o konfiguraci a informace o událostech zabezpečení, jako jsou například protokoly událostí a auditů systému Windows, protokoly IIS, zprávy syslog a soubory se stavem systému, z vašich virtuálních počítačů pomocí agentů pro shromažďování dat. Další podrobnosti naleznete v části „Správa shromažďování dat“.  

Kromě toho jsou v úložišti Azure Security Center, které se aktuálně nachází ve Spojených státech, uloženy informace o výstrahách zabezpečení, doporučeních a stavu zabezpečení. Tyto informace mohou podle potřeby zahrnovat související údaje o konfiguraci a událostech zabezpečení shromažďované z virtuálních počítačů, a to za účelem poskytování výstrah zabezpečení, doporučení nebo informací o stavu zabezpečení.

## Ochrana dat
**Oddělení dat**: Data se v rámci služby ukládají logicky oddělená pro jednotlivé komponenty. Všechna data jsou označená podle organizace. Toto značení přetrvává v průběhu celého životního cyklu dat a je vyžadováno na každé úrovni služby. Data shromážděná z virtuálních počítačů se navíc ukládají ve vašich účtech úložiště.

**Přístup k datům**: Aby bylo možné poskytovat doporučení týkající se zabezpečení a prošetřovat potenciální ohrožení zabezpečení, mají k informacím shromažďovaným nebo analyzovaným službami Azure, včetně souborů se stavem systému, přístup pracovníci společnosti Microsoft. Soubory se stavem systému a události vytváření procesů mohou náhodně zahrnovat informace o zákaznících nebo osobní údaje z virtuálních počítačů. Dodržujeme [Podmínky online služeb společnosti Microsoft](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), ve kterých je uvedeno, že společnost Microsoft nebude informace o zákaznících používat ani z nich odvozovat další informace pro reklamní nebo podobné obchodní účely. Informace o zákaznících podle potřeby používáme pouze k poskytování služeb Azure a k účelům slučitelným s poskytováním těchto služeb. Všechna práva na informace o zákaznících zůstávají ve vašem vlastnictví.

**Použití dat**: Společnost Microsoft vylepšuje své schopnosti prevence a detekce pomocí schémat a analýzy hrozeb napříč několika klienty. Činíme tak v souladu se závazky k ochraně osobních údajů popsanými v našem [Prohlášení o zásadách ochrany osobních údajů](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Umístění dat**: Pro každou oblast, ve které jsou spuštěny virtuální počítače, je určen účet úložiště. To umožňuje ukládání dat v oblasti, kde se nachází virtuální počítač, ze kterého data jsou shromažďována. Tato data, včetně souborů se stavem systému, budou trvale uložena v účtu úložiště. V úložišti služby Azure Security Center, které se aktuálně nachází ve Spojených státech, služba ukládá také informace o výstrahách zabezpečení (včetně výstrah z integrovaných partnerských řešení), doporučeních a stavu zabezpečení.

## Správa shromažďování dat z virtuálních počítačů
Pokud budete chtít povolit službu Azure Security Center, zapne se u každého vašeho předplatného funkce shromažďování dat. Shromažďování dat můžete vypnout v části „Zásady zabezpečení“ na řídicím panelu služby Azure Security Center. Když je funkce shromažďování dat zapnutá, služba Azure Security Center zřídí ve všech stávajících i nově vytvořených, podporovaných virtuálních počítačích agenta monitorování služby Azure. Rozšíření Azure Security Monitoring prohledává různé konfigurace týkající se zabezpečení a zapisuje události do složek [Trasování událostí pro systém Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). Operační systém bude kromě toho během chodu počítače shromažďovat události protokolu událostí. Mezi příklady těchto údajů patří: typ a verze operačního systému, protokoly operačního systému (protokoly událostí systému Windows), spuštěné procesy, název počítače, IP adresy, přihlášený uživatel a ID klienta. Agent monitorování služby Azure načte položky protokolu událostí a trasování ETW a zkopíruje je do vašeho účtu úložiště za účelem analýzy. 

Pro každou oblast, ve které máte spuštěné virtuální počítače, je určen účet úložiště, na který se ukládají data shromážděná z virtuálních počítačů v dané oblasti. To usnadňuje uchování dat ve stejné geografické oblasti za účelem ochrany osobních údajů a suverenity dat. Účty úložiště pro jednotlivé regiony můžete nakonfigurovat v části „Zásady zabezpečení“ na řídicím panelu služby Azure Security Center.

Agent monitorování služby Azure do účtu úložiště také zkopíruje soubory se stavem systému.  Azure Security Center shromažďuje dočasné kopie souborů se stavem systému a analyzuje je za účelem detekce stop pokusů o napadení zabezpečení, neúspěšných i úspěšných.  Azure Security Center provádí tuto analýzu v rámci zeměpisné oblasti , ve které se nachází účet úložiště, a po dokončení analýzy dočasné kopie odstraní.

Funkci shromažďování dat z virtuálních počítačů můžete kdykoliv vypnout, čímž dojde i k odebrání agentů monitorování, které služba Azure Security Center dříve nainstalovala.

## Další kroky
V tomto dokumentu jste se dozvěděli informace o způsobu správy a ochrany ve službě Azure Security Center. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md) – Zjistěte, jak naplánovat a pochopit aspekty návrhu, abyste mohli přejít na Azure Security Center.
* [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se sledovat stav svých prostředků Azure
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
* [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů

<!--HONumber=Sep16_HO3-->


