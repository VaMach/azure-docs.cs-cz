<properties
   pageTitle="Správa a zpracování výstrah zabezpečení v Azure Security Center | Microsoft Azure"
   description="Tento dokument vám pomůže používat funkce služby Azure Security Center ke správě výstrah zabezpečení a reagování na ně."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/22/2016"
   ms.author="yurid"/>

# Správa a zpracování výstrah zabezpečení v Azure Security Center
Tento dokument vám pomůže používat funkce služby Azure Security Center ke správě výstrah zabezpečení a reagování na ně.

> [AZURE.NOTE] Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center.

## Co je Azure Security Center?
 Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

## Co jsou výstrahy zabezpečení?
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě, integrované antimalwarové ochrany a bran firewall, aby se zjistily skutečné hrozby a snížil počet falešných poplachů. Seznam upřednostňovaných výstrah zabezpečení, včetně výstrahy z integrovaných partnerských řešení, se zobrazí ve službě Security Center spolu s informacemi, které potřebujete k rychlému prozkoumání, a doporučeními týkajícími se řešení útoku.

Výzkumní pracovníci Microsoftu, kteří se věnují zabezpečení, neustále analyzují vznikající hrozby po celém světě, včetně nových vzorů útoků a trendů, které se vyskytují v jejich spotřebních a podnikových produktech a online službách. V důsledku toho může služba Security Center aktualizovat své detekční algoritmy, jakmile jsou zjištěna nová ohrožení zabezpečení a možnosti zneužití, což zákazníkům umožňuje držet krok se vznikajícími hrozbami. Mezi příklady typů hrozeb, které služba Security Center může rozpoznat, patří:

- **Detekce útoků hrubou silou přes síťová data**: Používá modely machine learningu, které znají vzorky typického síťového provozu pro vaše aplikace a umožňují efektivnější zjišťování pokusů o přístup provedených nesprávnými účastníky namísto oprávněných uživatelů.
- **Detekce útoků hrubou silou přes data koncového bodu**: Na základě analýzy počítačových protokolů umožňuje rozlišovat úspěšné a neúspěšné pokusy.
- **Virtuální počítače komunikující s IP adresami se zlými úmysly**: Porovnává síťové přenosy s globální analýzou hrozeb Microsoftu, vyhledává počítače, které jsou ohrožené a komunikují se servery C&C (Command and Control), a naopak.
- **Ohrožené virtuální počítače**: Na základě analýzy chování počítačových protokolů a korelace s jinými signály identifikuje neobvyklé události, které jsou pravděpodobně výsledkem ohrožení zabezpečení počítače a jeho zneužití.

## Správa výstrah zabezpečení

Aktuální výstrahy můžete zkontrolovat jejich prohlížením na dlaždici **Výstrahy zabezpečení**. Pomocí uvedených pokynů níže zobrazíte o každé výstraze další podrobnosti:

1. Na řídicím panelu Security Center uvidíte dlaždici **Výstrahy zabezpečení**.

    ![Dlaždice Výstrahy zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-new.png)

2.  Kliknutím na dlaždici **Výstrahy zabezpečení** otevřete okno, které obsahuje podrobnosti o výstrahách, jak je uvedeno dále.

    ![Okno Výstrahy zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-new.png)

V dolní části tohoto okna jsou uvedené podrobnosti pro každou výstrahu. Chcete-li je seřadit, klikněte na sloupec, podle kterého chcete řadit. Definice pro každý sloupec je uvedená dále:

- **Alert** (Výstraha): Stručné vysvětlení výstrahy.
- **Count** (Počet): Seznam všech výstrah tohoto konkrétního typu, které byly zjištěny v určitý den.
- **Detected by** (Zjistil): Služba, která je zodpovědná za aktivaci výstrahy.
- **Date** (Datum): Datum, kdy došlo k události.
- **State** (Stav): Aktuální stav výstrahy. Existují tři typy stavů:
    - **Active** (Aktivní): Výstraha zabezpečení byla zjištěna.
    - **Dismissed** (Zamítnuto): Výstraha zabezpečení byla uživatelem zamítnuta. Tento stav se obvykle používá pro výstrahy, které byly prozkoumány, ale buď zmírněny, nebo vyhodnoceny, že nedošlo ke skutečnému útoku.

- **Severity** (Závažnost): Úroveň závažnosti, tj. vysoká, střední nebo nízká.

Výstrahy můžete filtrovat podle data, stavu nebo závažnosti. Filtrování výstrah může být užitečné v případech, kdy potřebujete zúžit obor zobrazených výstrah zabezpečení. Například můžete chtít zabývat se výstrahami zabezpečení, k nimž došlo v posledních 24 hodinách, protože zjišťujete případný průnik do systému.

1. Klikněte v okně **Výstrahy zabezpečení** na **Filtr**. Otevře se okno **Filtr** a vy vyberte hodnoty pro datum, stav a závažnost výstrah, které chcete vidět.

    ![Filtrování výstrah v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-new.png)

2.  Po prozkoumání výstrahy zabezpečení můžete zjistit, že se jedná ve vašem prostředí o falešný poplach nebo o očekávané chování určitého prostředku. Pokud bez ohledu na důvod zjistíte, že výstraha zabezpečení není použitelná, můžete výstrahu zamítnout a pak ji odfiltrovat ze zobrazení. Existují dva způsoby zamítnutí výstrahy zabezpečení. Klikněte pravým tlačítkem na výstrahu a vyberte **Dismiss** (Zamítnout) nebo podržte ukazatel myši nad položku, klikněte na tři tečky, které se zobrazí napravo, a vyberte **Dismiss** (Zamítnout). Zamítnuté výstrahy zabezpečení můžete zobrazit kliknutím na **Filtr** a výběrem **Dismissed** (Zamítnuté).

    ![Filtrování výstrah v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-new.png)

### Reakce na výstrahy zabezpečení
Vyberte výstrahu zabezpečení, abyste se dozvěděli další informace o událostech, které výstrahu vygenerovaly a kroky, pokud existují, které je třeba provést k nápravě útoku. Výstrahy zabezpečení jsou seskupené podle typu a data. Kliknutím na výstrahu zabezpečení se otevře okno obsahující seznam seskupených výstrah.

![Reakce na výstrahy zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig7.png)

V tomto případě odkazují vygenerované výstrahy na podezřelé aktivity protokolu RDP (Remote Desktop Protocol). První sloupec zobrazuje, které prostředky byly napadeny, druhý zobrazuje čas zjištění útoku, třetí zobrazuje stav výstrahy a čtvrtý zobrazuje závažnost útoku. Po zkontrolování těchto informací klikněte na prostředek, který byl napaden. Otevře se nové okno s dalšími návrhy postupu, jak je znázorněno v následujícím příkladu.

![Návrhy postupu při výstrahách zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig8-1.png)

V poli **Výstraha** tohoto okna najdete další podrobnosti o této události. Tyto další podrobnosti nabízejí získání náhledu na příčinu výstrahy zabezpečení, cílový prostředek, případnou zdrojovou IP adresu a doporučení, jak provést nápravu.  V některých případech bude zdrojová IP adresa prázdná (nedostupná), protože ne všechny protokoly událostí zabezpečení Windows obsahující IP adresu.

> [AZURE.NOTE] Náprava navrhovaná službou Security Center se bude lišit podle výstrahy zabezpečení. V některých případech budete muset k implementaci doporučené nápravy použít další možnosti Azure. Například nápravou pro tento útok je uvedení IP adresy, která generuje tento útok, v seznamu zakázaných serverů, a to pomocí [seznamu ACL sítě](../virtual-network/virtual-networks-acl.md) nebo pravidla [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).


## Další kroky
V tomto dokumentu jste zjistili, jak ve službě Security Center konfigurovat zásady zabezpečení. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
- [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Jun16_HO2-->


