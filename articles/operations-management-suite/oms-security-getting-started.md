---
title: "Začínáme s řešením Zabezpečení a audit v Operations Management Suite | Dokumentace Microsoftu"
description: "Tento dokument vám poskytne úvodní přehled řešení Zabezpečení a audit v Operations Management Suite, abyste mohli začít monitorovat svůj hybridní cloud."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 754796ef-a43e-468a-86c9-04a2eda55b5b
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: get-started-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: c6944fd4f93d2daa9071bb27f76ea0f772bdb743
ms.lasthandoff: 04/19/2017


---
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Začínáme s řešením Zabezpečení a audit v Operations Management Suite
Tento dokument vám umožní rychle začít používat řešení Zabezpečení a audit v Operations Management Suite (OMS) a provede vás jednotlivými možnostmi.

## <a name="what-is-oms"></a>Co je OMS?
Microsoft Operations Management Suite (OMS) je cloudové řešení společnosti Microsoft pro správu IT, které pomáhá se správou a ochranou místních a cloudových infrastruktur. Další informace o OMS najdete v článku [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>Řídicí panel Zabezpečení a audit v OMS
Řešení Zabezpečení a audit v OMS poskytuje ucelený přehled o stavu zabezpečení IT vaší organizace pomocí předdefinovaných vyhledávacích dotazů na důležité problémy, které vyžadují vaši pozornost. Řídicí panel **Zabezpečení a audit** je domovskou obrazovkou pro všechno, co v OMS souvisí se zabezpečením. Poskytuje celkový přehled o stavu zabezpečení vašich počítačů. Umožňuje také zobrazit všechny události za posledních 24 hodin, 7 dní nebo za jakýkoli zadaný interval. Řídicí panel **Zabezpečení a audit** je přístupný podle tohoto postupu:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Nastavení** nalevo.
2. V okně **Nastavení** v části **Řešení** klikněte na možnost **Zabezpečení a audit**.
3. Zobrazí se řídicí panel **Zabezpečení a audit**:
   
    ![Řídicí panel Zabezpečení a audit v OMS](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Pokud tento řídicí panel otvíráte poprvé a nemáte ve službě OMS žádná sledovaná zařízení, dlaždice nebudou obsahovat žádná data z agentů. Po instalaci agenta může načtení dat ještě nějakou dobu trvat, proto mohou zpočátku chybět některá data, která se ještě odesílají do cloudu.  V takovém případě je normální, že některé dlaždice neobsahují smysluplné informace. Další informace o instalaci agenta OMS v systému Windows obsahuje článek [Přímé připojení počítačů s Windows do OMS](https://technet.microsoft.com/library/mt484108.aspx) a článek [Připojení počítačů s Linuxem do OMS](https://technet.microsoft.com/library/mt622052.aspx) obsahuje stejné informace pro systém Linux.

> [!NOTE]
> Agent shromažďuje informace na základě aktuálních povolených událostí, například název počítače, IP adresu a uživatelské jméno. Nejsou ale shromažďovány žádné soubory, dokumenty, databáze nebo soukromá data.   
> 
> 

Řešení jsou kolekce pravidel pro logiku, vizualizaci a získávání dat, které řeší klíčové problémy zákazníků. Zabezpečení a audit je jedním z nich, další mohou být přidána samostatně. Další informace o přidání nových řešení najdete v článku [Přidání řešení](https://technet.microsoft.com/library/mt674635.aspx).

Řídicí panel Zabezpečení a audit v OMS je rozdělen do čtyř hlavních kategorií:

* **Domény zabezpečení**: v této oblasti můžete podrobněji zkoumat záznamy o zabezpečení v různých časech, zobrazovat informace o posouzení malwaru, o zabezpečení sítě, o posouzení aktualizací, o identitě a přístupu a o počítačích s událostmi zabezpečení, stejně jako rychle přecházet do řídicího panelu Azure Security Center.
* **Významné problémy**: tato možnost vám umožní rychle identifikovat celou řadu aktivních problémů a jejich závažnost.
* **Zjištění (Preview)**: umožňuje identifikovat vzory útoků na vaše prostředky pomocí vizualizací výstrah zabezpečení.
* **Analýza hrozeb**: umožňuje identifikovat vzory útoků vizualizací celkového počtu serverů s odchozími škodlivými přenosy, typu škodlivých hrozeb a mapy, která ukazuje, odkud příslušné adresy IP pocházejí. 
* **Běžné dotazy na zabezpečení**: tato možnost poskytuje seznam nejběžnějších bezpečnostních dotazů které můžete použít k monitorování vašeho prostředí. Když kliknete na některý z těchto dotazů, otevře se okno **Vyhledávání** s výsledky pro daný dotaz.

> [!NOTE]
> Další informace o tom, jak OMS zajišťuje ochranu vašich dat, obsahuje článek Jak OMS chrání vaše data.
> 
> 

## <a name="security-domains"></a>Domény zabezpečení
Při monitorování prostředků je důležité, abyste byli schopni rychle se dostat k aktuálnímu stavu prostředí. Je ale také důležité mít možnost zpětně vysledovat události, ke kterým došlo v minulosti a které vám pomůžou pochopit, co se v daném okamžiku v prostředí děje. 

> [!NOTE]
> Uchovávání dat probíhá podle cenového tarifu OMS. Další informace najdete v článku o cenách služby [Microsoft Operations Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx).
> 
> 

Reakce na události a scénáře forenzního vyšetřování mohou informace dostupné na dlaždici **Záznamy zabezpečení v průběhu času** s výhodou využívat.

![Záznamy zabezpečení v průběhu času](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Když kliknete na tuto dlaždici, otevře se okno **Vyhledávání** s výsledkem dotazu pro **Události zabezpečení** (typ = SecurityEvents) na základě dat z posledních sedmi dnů, například:

![Záznamy zabezpečení v průběhu času](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

Výsledek vyhledávání je rozdělen do dvou podoken: v levém podokně je rozpis počtu nalezených událostí zabezpečení, počítačů, ve kterých k nim došlo, počet účtů, které byly v těchto počítačích zjištěny a typy aktivit. V pravém podokně jsou celkové výsledky a chronologické zobrazení událostí zabezpečení s názvem počítače a aktivitou události. Kliknutím na **Zobrazit další** můžete také zobrazit podrobnosti o této události, například data, ID a zdroj události.

> [!NOTE]
> Další informace o vyhledávacích dotazech OMS najdete v [Referenční příručce k vyhledávání pro službu OMS](https://technet.microsoft.com/library/mt450427.aspx).
> 
> 

### <a name="antimalware-assessment"></a>Posouzení antimalwaru
Tato možnost umožňuje rychle identifikovat počítače s nedostatečnou ochranou a počítače, které jsou zasažené malwarem. Načte se posouzení stavu malwaru a zjištěná ohrožení na monitorovaných serverech a výsledky se odešlou službě OMS v cloudu pro zpracování. Servery, na kterých byla nalezena ohrožení a servery s nedostatečnou ochranou se zobrazí v řídicím panelu posouzení malwaru, která je přístupný kliknutím na dlaždici **Posouzení antimalwaru**. 

![Posouzení malwaru](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Stejně jako u kterékoli jiné živé dlaždice na řídicím panelu OMS můžete kliknutím na ni zobrazit okno **Vyhledávání** s výsledkem dotazu. Pokud u této možnosti kliknete na možnost **Negenerují se sestavy** v části **Stav ochrany**, získáte výsledky dotazu, které odpovídají této jedné položce s názvem počítače a pořadím, jak je uvedeno níže:

![Výsledky vyhledávání](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [!NOTE]
> *Pořadí* odráží stav ochrany (zapnuto, vypnuto, aktualizováno atd.) a nalezených hrozeb. Číselné vyjádření umožňuje využití této hodnoty v agregacích.
> 
> 

Pokud kliknete na název počítače, získáte chronologický přehled o stavu jeho zabezpečení. To je velmi užitečné v případech, kdy potřebujete zjistit, zda byl nainstalován antimalwarový software, případně kdy byl odebrán.   

### <a name="update-assessment"></a>Posouzení aktualizací
Tato možnost umožňuje rychle určit celkovou míru vystavení potenciálním problémům zabezpečení a zda a jak kritické jsou dostupné aktualizace pro vaše prostředí. Řešení Zabezpečení a audit v OMS pouze poskytuje vizualizaci těchto aktualizací, skutečná data pocházejí z [řešení Správa aktualizací](oms-solution-update-management.md), což je jiný modul v rámci OMS. Následuje příklad aktualizací:

![Aktualizace systému](./media/oms-security-getting-started/oms-getting-started-fig6-new.png)

> [!NOTE]
> Další informace o řešení Správa aktualizací najdete v tématu [Řešení Správa aktualizací v OMS](oms-solution-update-management.md).
> 
> 

### <a name="identity-and-access"></a>Identita a přístup
Identita by měla být základní „řídicí plochou“ vaší organizace a její ochrana je tou nejvyšší prioritou. V minulosti byly kolem organizací vytyčeny perimetry, které byly hlavní obrannou linií. V současné době se ale čím dál víc dat a aplikací přesouvá do cloudu a novým perimetrem se proto stává identita. 

> [!NOTE]
> Aktuálně jsou data založena jen na základě přihlašovacích dat Security Events (událost s ID 4624); v budoucnu budou doplněna i přihlášení Office365 a Azure AD.
> 
> 

Monitorování aktivit kolem vaší identity budete moci proaktivně jednat ještě než k problematické události dojde nebo provést reaktivní akci k zastavení pokusu o útok. Řídicí panel **Identita a přístup** poskytuje přehled stavu vaší identity, včetně počtu neúspěšných pokusů o přihlášení, uživatelského účtu použitého při těchto pokusech, uzamčených účtů, účtů se změněným nebo resetovaným heslem nebo počtu účtů, jejichž uživatelé jsou právě přihlášení. 

Když kliknete na dlaždici **Identita a přístupu**, zobrazí se následující řídicí panel:

![Identita a přístup](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

Informace, které jsou k dispozici na tomto řídicím panelu, vám pomůžou okamžitě identifikovat potenciálně podezřelé aktivity. Například je tu 338 pokusů o přihlášení jako **Administrator** a 100 % těchto pokusů o přihlášení se nezdařilo. Může to znamenat útok hrubou silou na tento účet. Když kliknete na tento účet, získáte další informace, které vám pomůžou určit cílový prostředek tohoto potenciálního rizika:

![Výsledky vyhledávání](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

Podrobná sestava obsahuje důležité informace o této události, včetně cílového počítače, typu přihlášení (v tomto případě přihlášení k síti), aktivity (v tomto případě událost 4625) a podrobného časování každého pokusu. 

### <a name="computers"></a>Počítače
Tuto dlaždici můžete použít pro přístup ke všem počítačům, na kterých právě dochází k událostem zabezpečení. Po kliknutí na tuto dlaždici se zobrazí seznam počítačů s událostmi zabezpečení a počet událostí na každém z nich:

![Počítače](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Můžete pokračovat v šetření kliknutím na každý počítač a kontrolou hlášených událostí zabezpečení.

### <a name="threat-intelligence"></a>Analýza hrozeb

Pomocí možnosti Analýza hrozeb, která je dostupná v řešení Zabezpečení a audit v OMS, můžou správci IT identifikovat ohrožení zabezpečení prostředí – například můžou určit, jestli je konkrétní počítač součástí botnetu. Z počítačů se můžou stát uzly v botnetu, když útočníci neoprávněně nainstalují malware, který tajně připojí počítač k řídicímu serveru. Může také identifikovat potenciální hrozby přicházející z alternativních komunikačních kanálů, jako je například darknet. Další informace o analýze hrozeb najdete v článku [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md).

### <a name="baseline-assessment"></a>Vyhodnocení standardních hodnot

Společnost Microsoft, spolu s organizacemi z oboru a vládními organizacemi po celém světě, definuje konfiguraci systému Windows, která představuje vysoce zabezpečená nastavení serverů. Tuto konfiguraci tvoří sada klíčů registru, nastavení zásad auditu, nastavení zásad zabezpečení a společností Microsoft doporučené hodnoty pro tato nastavení. Tato sada pravidel se označuje jako standardní hodnoty zabezpečení. Další informace o této možnosti najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v Operations Management Suite](oms-security-baseline.md).

### <a name="azure-security-center"></a>Azure Security Center
Tato dlaždice je v podstatě zkratkou k řídicímu panelu Azure Security Center. Další informace o tomto řešení najdete v článku [Začínáme s Azure Security Center](../security-center/security-center-get-started.md).

## <a name="notable-issues"></a>Významné problémy
Hlavním záměrem této skupiny možností je poskytnout rychlý přehled o problémech, které máte ve svém prostředí roztříděním na kritické události, na upozornění a na informativní sdělení. Dlaždice typu Aktivní problém obsahuje vizualizaci těchto problémů, ale neumožňuje zkoumání jejich podrobností. K tomu budete muset použít dolní část této dlaždice, ve které je název problému, počet zasažených objektů a závažnost.

![Významné problémy](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Můžete si všimnout, že tyto problémy již jsou pokryté ostatními oblastmi skupiny **Domény zabezpečení**, což posiluje záměr tohoto zobrazení: vizualizovat nejdůležitější problémy ve vašem prostředí na jednom místě.

## <a name="detections-preview"></a>Zjištění (Preview)
Hlavním záměrem této možnosti je umožnit správcům IT rychle identifikovat potenciální ohrožení jejich prostředí a závažnost těchto hrozeb.

![Analýza hrozeb](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Tuto možnost můžete použít také při [šetření incidentů](https://blogs.msdn.microsoft.com/azuresecurity/2016/11/30/investigating-suspicious-activity-in-a-hybrid-cloud-with-oms-security/) k vyhodnocení a získání dalších informací o útoku.

> [!NOTE]
> Ve videu [How to Leverage the Azure Security Center & Microsoft Operations Management Suite for an Incident Response](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) (Jak reagovat na incidenty pomocí služeb Azure Security Center a Microsoft Operations Management Suite) najdete informace, jak používat OMS při reakci na incidenty.
> 
> 

## <a name="threat-intelligence"></a>Analýza hrozeb
Nový oddíl analýzy hrozeb v řešení Zabezpečení a audit vizualizuje vzory možných útoků různými způsoby: celkový počet serverů s odchozími škodlivými přenosy, typy škodlivých hrozeb a mapa, která ukazuje, odkud příslušné IP adresy pocházejí. S mapou můžete pracovat a zjišťovat další informace klikáním na IP adresy.

Žluté špendlíky na mapě označují příchozí provoz ze škodlivých IP adres. U serverů dostupných z internetu je určitý podíl škodlivého provozu normální, doporučujeme ale tyto pokusy kontrolovat a ujistit se, že nejsou úspěšné. Tyto indikátory jsou založené na protokolech služby IIS, WireData a brány Windows Firewall.  

![Analýza hrozeb](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Běžné dotazy zabezpečení
Seznam běžných dotazů zabezpečení může být užitečný pro rychlé získání informací z prostředků a jejich přizpůsobení podle potřeb vašeho prostředí. Mezi běžné dotazy patří:

* Všechny aktivity zabezpečení
* Aktivity zabezpečení v počítači "computer01.contoso.com" (nahraďte název počítače podle potřeby)
* Aktivity zabezpečení v počítači "computer01.contoso.com" pro účet „Administrator“ (nahraďte název počítače a účet podle potřeby)
* Aktivity přihlášení k počítači
* Účty, pod kterými byl na počítači ukončen antimalware Microsoftu
* Počítače, na kterých byl ukončen proces antimalwaru Microsoftu
* Počítače, na kterých spuštěn program "hash.exe" (nahraďte název procesu podle potřeby)
* Všechny názvy procesů, které byly spuštěny
* Aktivity přihlášení podle účtu
* Účty, které se vzdáleně přihlásily k počítači "computer01.contoso.com" (nahraďte název počítače podle potřeby)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se seznámili s řešením Zabezpečení a audit v OMS. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)


