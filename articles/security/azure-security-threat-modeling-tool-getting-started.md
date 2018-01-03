---
title: "Začínáme se službou Azure - modelování nástroj Microsoft Threat - | Microsoft Docs"
description: "Toto je hlubší přehled zvýraznění nástroj modelování hrozeb v akci."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 73d6b42e7a97d6041f6213a1f7d060806734d763
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="getting-started-with-the-threat-modeling-tool"></a>Začínáme s nástrojem modelování hrozeb

Cloudové a podnikové zabezpečení nástroje team vydala Threat modelování nástroj Preview na začátku roku jako bezplatný  **[klikněte na tlačítko Stáhnout](https://aka.ms/tmtpreview)**. Změna v mechanismus doručení umožňuje push nejnovější vylepšení a opravy chyb pro zákazníky pokaždé, když se otevřít nástroj, což usnadňuje údržbě a používání.
Tento článek vás provede procesem Začínáme s Microsoft SDL riziko, že modelování přístup a ukazuje, jak používat nástroj k vývoji kvalitních threat modely jako páteřní zabezpečení procesu.

Tento článek je založený na stávajících znalostí threat SDL modelování přístup. Rychlý přehled najdete v části  **[modelování ohrožení webových aplikací](https://msdn.microsoft.com/library/ms978516.aspx)**  a archivované verze  **[odkrýt zabezpečení nedostatky využitím metody STRIDE](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  Článku na webu MSDN publikován v 2006.

Rychle Shrneme, přístupu vyžaduje vytváření diagram, identifikace hrozeb, jejich zmírnění a ověření každého zmírnění dopadů. Zde je diagram, který označuje tento proces:

![Proces SDL](./media/azure-security-threat-modeling-tool-feature-overview/sdlapproach.png)

## <a name="starting-the-threat-modeling-process"></a>Spouštění riziko, že proces modelování

Když spustíte nástroj modelování hrozeb, můžete si všimnout pár věcí, jak je vidět na obrázku:

![Prázdné úvodní stránky](./media/azure-security-threat-modeling-tool-feature-overview/tmtstart.png)

### <a name="threat-model-section"></a>Část model hrozeb

| Komponenta                                   | Podrobnosti                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Zpětná vazba, návrhy a tlačítko problémy** | Přejdete  **[fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  pro všechny věci SDL. Nabízí možnost si přečíst další činnosti uživatelů, spolu s alternativní řešení a doporučení. Pokud stále nemůžete najít co hledáte, e-mailu tmtextsupport@microsoft.com pro náš tým podpory vám pomohou                                                                                                                            |
| **Vytvoření modelu**                          | Otevře prázdného plátna pro vás k vykreslení diagramu. Nezapomeňte vybrat šablonu, kterou chcete použít pro model                                                                                                                                                                                                                                                                                                                                                                       |
| **Šablonu pro nové modely**                 | Je třeba vybrat, kterou šablonu použít před vytvořením modelu. Naše hlavní šablona je Model šablony Threat Azure, která obsahuje vzorníky, hrozby a jejich zmírnění specifické pro Azure. Pro obecné modely vyberte z rozevírací nabídky znalostní báze TM SDL. Chcete vytvořit vlastní šablonu nebo odesílat novou pro všechny uživatele? Podívejte se na naše  **[šablony úložiště](https://github.com/Microsoft/threat-modeling-templates)**  GitHub stránce Další informace                              |
| **Otevřete Model**                            | <p>Otevře se okno uložili dřív modely hrozeb. Funkce nedávno otevřít modely je skvělé, pokud je třeba otevřít nejnovější soubory. Po přesunutí ukazatele myši na výběr, zjistíte 2 způsoby spuštění modely:</p><p><ul><li>Otevřete z tohoto počítače – classic způsob otevření souboru, který používá místní úložiště</li><li>Otevřete z Onedrivu – týmy můžete použít složky na OneDrive pro ukládání a sdílení jejich modely threat na jednom místě za účelem zvýšení produktivity a spolupráce</li></ul></p> |
| **Příručka Začínáme**                   | Otevře se  **[Microsoft Threat modelování nástroj](./azure-security-threat-modeling-tool.md)**  hlavní stránce                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Část šablony

| Komponenta               | Podrobnosti                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vytvořte novou šablonu** | Otevře prázdné šablony můžete vytvořit. Pokud nemáte rozsáhlé znalosti při vytváření šablony od začátku, doporučujeme vytvořit z existující |
| **Otevřít šablonu**       | Otevře existující šablony můžete provést změny                                                                                                              |

Týmem nástroj modelování hrozeb neustále pracuje na zlepšení nástroj funkce a zkušenosti. Několik malých změn může provést v průběhu roku, ale všechny hlavní změny vyžadují přepisů v průvodci. Najdete ho často se ujistěte se, že dostanete nejnovější oznámení.

## <a name="building-a-model"></a>Vytvoření modelu

V této části jsme podle:

- Cristina (Vývojář)
- Richard (program manager) a
- Ashish (tester)

Se budou prostřednictvím procesu vývoje jejich první model hrozeb.

> Richard: Dobrý den Cristina, I pracovali diagram model hrozeb, a chtěli zajistěte, aby My podrobnosti správné. Můžete mi ji projít pomoci?
> Cristina: absolutně. Podívejme se.
> Richard otevře nástroj a sdílí s Cristina jeho obrazovka.

![Model základní hrozeb](./media/azure-security-threat-modeling-tool-feature-overview/basictmt.png)

> Cristina: Ok, vypadá jasné, ale můžete je provede mi ji?
> Richard: zda! Tady je rozdělení:
> - Naše lidského uživatele vykreslením jako entitu mimo – čtverce
> - Příkazy se byla odeslána na našem webový server – kruhu
> - Webový server je konzultace ohledně databáze (dva řádky paralelní)

Co Richard právě vám ukázal, Cristina je diagramu toku dat, zkratka pro  **[Diagram toku dat](https://en.wikipedia.org/wiki/Data_flow_diagram)**. Nástroj modelování hrozeb mohou uživatelé zadat hranice vztahů důvěryhodnosti, indikován red tečkovaná řádky, kde jsou různých entit v ovládacím prvku. Například správci IT vyžadovat systému Active Directory pro účely ověření, tak služby Active Directory je mimo jejich řízení.

> Cristina: Vypadá správné mně. Co o hrozbách?
> Richard: Chci můžete zobrazit.

## <a name="analyzing-threats"></a>Analýza hrozeb

Jakmile klikne na pohledu analýzy z výběru nabídky ikona (soubor s ikonou lupy), byl přijat na seznam generovaného hrozeb nalezen nástroj modelování hrozeb na základě výchozí šablony, která využívá přístup SDL názvem  **[ STRIDE (falšování identity, úmyslné poškozování, zpřístupnění informací, odepření služby a zvýšení úrovně oprávnění)](https://en.wikipedia.org/wiki/STRIDE_(security))**. Cílem je, že software pochází pod předvídatelný sadu hrozeb, které lze nalézt pomocí těchto 6 kategorií.

Tento přístup je jako zabezpečení domu tím zajistí každé dveře a okna Blokovací mechanizmus má zavedené před přidáním poplašný systém nebo následné dotazy po zloděj.

![Základní hrozeb](./media/azure-security-threat-modeling-tool-feature-overview/basicthreats.png)

Richard začne výběrem první položku v seznamu. Stane se toto:

Nejprve je vylepšená interakce mezi dvěma vzorníky

![Interakce](./media/azure-security-threat-modeling-tool-feature-overview/interaction.png)

Druhý, další informace o riziko, že se zobrazí v okně vlastností hrozeb

![Informace o interakci](./media/azure-security-threat-modeling-tool-feature-overview/interactioninfo.png)

Riziko, že generovaný pomůže mu pochopit potenciální chyby v návrhu. STRIDE kategorizaci mu poskytuje představu na potenciální Vektorům útoku, zatímco další popis mu říkají, přesně co je nesprávný, společně s možné způsoby, jak ji zmírnit. Upravitelné pole kterou může použít k zápisu poznámky v podrobnostech zarovnání do bloku nebo změnit prioritu hodnocení v závislosti na jeho organizace chyb panelu.

Šablony Azure mají další informace, které pomohou uživatelům pomoct pochopit pouze co je nesprávný, ale také informace o vyřešení přidáním popisy, příklady a hypertextové odkazy na dokumentaci specifické pro Azure.

Popis provedené mu mějte na paměti význam přidání mechanismus ověřování, které uživatelům zabrání se maskování odhalil první ohrožení pracovat. Několik minut do diskuse s Cristina, se rozumí význam implementace řízení přístupu a rolí. Richard vyplněno rychlé poznámky a ujistěte se, že tyto byly implementovány.

Jako Richard přešel do hrozeb v části zpřístupnění informací, mohl realizován plán řízení přístupu pro generování auditování a sestava vyžaduje některé účty jen pro čtení. Zadá zajímalo, zda by měl proběhnout nové hrozby, ale jejich zmírnění byly stejné, proto mu si poznamenali riziko, že odpovídajícím způsobem.
Mu taky představit další o zpřístupnění informací a uvědomili si, že záložní pásky se chystáte potřebují šifrování, úlohy pro provozní tým.

Nevztahuje se na návrhu z důvodu zabezpečení nebo existující jejich zmírnění hrozeb zaručuje z rozevíracího seznamu Stav lze změnit na "Není k dispozici". Existují tři další možnosti: není spuštěna – výchozí výběr, potřebuje šetření – používá ke zpracování v položky a Mitigated – po plně pracovali.

## <a name="reports--sharing"></a>Sestavy a sdílení

Jakmile Richard prochází seznam s Cristina a přidá důležité poznámky, jejich zmírnění nebo důkazy, prioritu a změny stavu, he vybere sestavy -> vytvořit úplná sestava -> Uložit sestavu, která vytiskne sestavu dobrý pro mu projít s kolegy aby fungoval správně zabezpečeny je implementováno.

![Informace o interakci](./media/azure-security-threat-modeling-tool-feature-overview/report.png)

Pokud Richard chce sdílet souboru místo toho, mohl snadno to provedete lze uložit v účtu OneDrive jeho organizace. Jakmile se provede, který, mohl zkopírujte odkaz dokumentu a sdílet s jeho kolegové. 

## <a name="threat-modeling-meetings"></a>Schůzek modelování hrozeb

Odeslání Richard jeho model hrozeb do jeho kolegu pomocí OneDrive, Ashish, zkušebního zařízení, byla underwhelmed. Mailech vypadalo jako Richard a Cristina provedena mnoha případech důležité rohu, které by mohly být snadno ohrozit. Jeho pochybnostmi doplňuje modely hrozeb.

V tomto scénáři po Ashish převzal model hrozeb volal pro dvě schůzky modelování hrozeb: schůzek k synchronizaci na proces a provede diagramy, pak druhý schůzky na hrozby zkontrolujte a podpisu.

V první schůzku stráví Ashish proti everyone prostřednictvím SDL riziko, že proces modelování 10 minut. Potom vyžádat si diagram model hrozeb a spuštění vysvětlením podrobně. Do pěti minut byla určena důležité chybí komponenta.

Několik minut později Ashish a Richard tu do rozšířené diskuzi o tom, jak byl sestaven webového serveru. Nebyla nejvhodnější způsob pro schůzky, chcete-li pokračovat, ale všichni nakonec souhlas, že již v rané fázi zjišťování nesoulad mezi databází, bude je uložíte čas v budoucnosti.

V druhé schůzku týmem projít hrozby, popsané některé způsoby, jak je řešit a přihlášeného model hrozeb. Jejich zaškrtnutí dokumentu do správy zdrojového kódu a pokračovat, s vývoj.

## <a name="thinking-about-assets"></a>Přemýšlení o prostředcích

Některé nástroje pro čtení, kteří mají threat modelován všimnout, že už jsme nebyly mluvili o prostředcích ve všech. Zjistili jsme mnoho softwarových vývojářů lépe než porozumí koncept prostředky a jaké prostředky útočníka mohly zajímat pochopení jejich softwaru.

Pokud se chystáte model hrozeb úklidové, můžete začít tím přemýšlení o vaší rodiny, nenahraditelných fotografií nebo cenné kresby. Případně můžete začít tím přemýšlení o kdo může přerušení a v aktuálním zabezpečení systému. Nebo můžete začít tím, že vzhledem k tomu fyzické funkcí, jako například fondu nebo front porch. Toto jsou obdobou přemýšlení o prostředky, útočníkům nebo návrh softwaru. Některé z těchto tří přístupů fungovat.

Přístup k hrozby modelování, které jsme si okomentovat je podstatně jednodušší než co Microsoft provedla v minulosti. Zjistili jsme, že přístup návrh softwaru funguje dobře u mnoha týmy. Věříme, že, které obsahují vaše.

## <a name="next-steps"></a>Další kroky

Odesílat dotazy, komentáře a otázky k tmtextsupport@microsoft.com. **[Stáhněte si](https://aka.ms/tmtpreview)**  nástroj modelování hrozeb začít pracovat.