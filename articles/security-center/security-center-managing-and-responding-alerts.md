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
   ms.date="08/07/2016"
   ms.author="yurid"/>

# Správa a zpracování výstrah zabezpečení v Azure Security Center
Tento dokument vám pomůže používat Azure Security Center ke správě výstrah zabezpečení a reagování na ně.

## Co jsou výstrahy zabezpečení?
Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a připojených partnerských řešení, jako jsou brány firewall a řešení ochrany koncových bodů, aby se zjistily skutečné hrozby a snížil počet falešných poplachů. Seznam upřednostňovaných výstrah zabezpečení se zobrazí ve službě Security Center spolu s informacemi, které potřebujete k rychlému prozkoumání problému, a doporučeními týkajícími se řešení útoku. Azure Security Center také agreguje výstrahy, které vedou k ukončení řetězových vzorů do podoby [Incidentů](security-center-incident.md). 

> [AZURE.NOTE] Další informace o tom, jak detekce služby Security Center pracuje, najdete v článku [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md).


## Správa výstrah zabezpečení

Aktuální výstrahy můžete zkontrolovat jejich prohlížením na dlaždici **Výstrahy zabezpečení**. Přejděte na Azure Portal a podle následujících pokynů zobrazte podrobnosti o jednotlivých výstrahách:

1. Na řídicím panelu Security Center uvidíte dlaždici **Výstrahy zabezpečení**.

    ![Dlaždice Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2.  Kliknutím na dlaždici **Výstrahy zabezpečení** otevřete okno, které obsahuje podrobnosti o výstrahách, jak je uvedeno dále.

    ![Okno Výstrahy zabezpečení ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

V dolní části tohoto okna jsou uvedené podrobnosti pro každou výstrahu. Chcete-li je seřadit, klikněte na sloupec, podle kterého chcete řadit. Definice pro každý sloupec je uvedená dále:

- **Alert** (Výstraha): Stručné vysvětlení výstrahy.
- **Count** (Počet): Seznam všech výstrah tohoto konkrétního typu, které byly zjištěny v určitý den.
- **Detected by** (Zjistil): Služba, která je zodpovědná za aktivaci výstrahy.
- **Date** (Datum): Datum, kdy došlo k události.
- **State** (Stav): Aktuální stav výstrahy. Existují dva typy stavů:
    - **Active** (Aktivní): Výstraha zabezpečení byla zjištěna.
    - **Dismissed** (Zamítnuto): Výstraha zabezpečení byla uživatelem zamítnuta. Tento stav se obvykle používá pro výstrahy, které byly prozkoumány, ale buď zmírněny, nebo vyhodnoceny, že nedošlo ke skutečnému útoku.

- **Severity** (Závažnost): Úroveň závažnosti, tj. vysoká, střední nebo nízká.

### Filtrování výstrah

Výstrahy můžete filtrovat podle data, stavu nebo závažnosti. Filtrování výstrah může být užitečné v případech, kdy potřebujete zúžit obor zobrazených výstrah zabezpečení. Například můžete chtít zabývat se výstrahami zabezpečení, k nimž došlo v posledních 24 hodinách, protože zjišťujete případný průnik do systému.

1. Klikněte v okně **Výstrahy zabezpečení** na **Filtr**. Otevře se okno **Filtr** a vy vyberte hodnoty pro datum, stav a závažnost výstrah, které chcete vidět.

    ![Filtrování výstrah ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-ga.png)

2.  Po prozkoumání výstrahy zabezpečení můžete zjistit, že se jedná ve vašem prostředí o falešný poplach nebo o očekávané chování určitého prostředku. Pokud bez ohledu na důvod zjistíte, že výstraha zabezpečení není použitelná, můžete výstrahu zamítnout a pak ji odfiltrovat ze zobrazení. Existují dva způsoby zamítnutí výstrahy zabezpečení. Klikněte pravým tlačítkem na výstrahu a vyberte **Dismiss** (Zamítnout) nebo podržte ukazatel myši nad položku, klikněte na tři tečky, které se zobrazí napravo, a vyberte **Dismiss** (Zamítnout). Zamítnuté výstrahy zabezpečení můžete zobrazit kliknutím na **Filtr** a výběrem **Dismissed** (Zamítnuté).

    ![Zavírání výstrah ve službě Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig4-ga.png)

### Reakce na výstrahy zabezpečení

Vyberte výstrahu zabezpečení, abyste se dozvěděli další informace o událostech, které výstrahu vygenerovaly a kroky, pokud existují, které je třeba provést k nápravě útoku. Výstrahy zabezpečení jsou seskupené podle typu a data. Kliknutím na výstrahu zabezpečení se otevře okno obsahující seznam seskupených výstrah.

![Reakce na výstrahy zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

V tomto případě odkazují vygenerované výstrahy na podezřelé aktivity protokolu RDP (Remote Desktop Protocol). První sloupec zobrazuje, které prostředky byly napadeny, druhý zobrazuje počet útoků, třetí čas útoku, čtvrtý stav výstrahy a pátý závažnost útoku. Po zkontrolování těchto informací klikněte na prostředek, který byl napaden. Otevře se nové okno.

![Návrhy postupu při výstrahách zabezpečení v Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

V poli **Popis** tohoto okna najdete další podrobnosti o této události. Tyto další podrobnosti nabízejí získání náhledu na příčinu výstrahy zabezpečení, cílový prostředek, případnou zdrojovou IP adresu a doporučení, jak provést nápravu.  V některých případech bude zdrojová IP adresa prázdná (nedostupná), protože ne všechny protokoly událostí zabezpečení Windows obsahující IP adresu.

> [AZURE.NOTE] Náprava navrhovaná službou Security Center se bude lišit podle výstrahy zabezpečení. V některých případech budete muset k implementaci doporučené nápravy použít další možnosti Azure. Například nápravou pro tento útok je uvedení IP adresy, která generuje tento útok, v seznamu zakázaných serverů, a to pomocí [seznamu ACL sítě](../virtual-network/virtual-networks-acl.md) nebo pravidla [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).

## Výstrahy zabezpečení podle typu
Stejné kroky jako při přístupu k výstrahám na podezřelé aktivity protokolu RDP použijte i pro přístup k ostatním typům výstrah. Tady jsou příklady výstrah, se kterými se ve službě Security Center můžete setkat:

### Potenciální útok SQL Injection
Při útoku SQL Injection se škodlivý kód vkládá do řetězců, které jsou potom předány instanci SQL Serveru k parsování a spuštění. SQL Server spustí všechny syntakticky platné dotazy, které obdrží. Všechny procedury, které sestavují SQL příkazy, by tedy měly být chráněny proti tomuto typu útoku. 

![Výstraha na útok SQL Injection](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9.png) 

Tato výstraha vám pomůže identifikovat napadený zdroj, ukazuje čas detekce a stav útoku a poskytuje odkaz na další kroky šetření.

### Zjištěn podezřelý odchozí provoz

Síťová zařízení lze zjistit a profilovat velmi podobně jako jiné typy systémů. Útočníci obvykle začínají skenováním portů (port scanning) nebo hledáním konkrétního otevřeného portu (port sweeping). Následující příklad ukazuje podezřelý provoz SSH z virtuálního počítače, který může provádět útok na SSH hrubou silou nebo cílit na konkrétní port externího prostředku. 

![Výstraha na podezřelý odchozí provoz](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig-10.png)

Tato výstraha vám pomůže identifikovat prostředek, který byl použitý k zahájení tohoto útoku, ohrožený počítač, čas detekce, protokol a port. V tomto okně se zobrazuje taky seznam nápravných kroků, kterými můžete problém zmírnit.

### Síťová komunikace se škodlivým počítačem
 
Azure Security Center může pomocí kanálů analýzy hrozeb Microsoft rozpoznat ohrožené počítače, které komunikují se škodlivými IP adresami. V mnoha případech jde o adresy řídicího centra. V tomto případě služba Azure Security Center rozpoznala komunikaci provedenou pomocí malwaru Pony Loader (známého taky pod názvem [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF)).

![Výstraha na síťovou komunikaci se škodlivým počítačem](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9-ga.png)

Tato výstraha vám pomůže identifikovat prostředek použitý k provedení útoku, ohrožený prostředek, IP adresu oběti i útočníka a čas detekce.

> [AZURE.NOTE] V zájmu ochrany osobních údajů jsme ze snímku obrazovky odebrali skutečné IP adresy.

### Zjištěn skrytý spustitelný kód 

Skrytý spustitelný kód je datová část, která se spouští potom, co malware zneužije chybu zabezpečení softwaru.  Tato výstraha znamená, že při analýze výpisu stavu systému byl nalezen spustitelný kód, který vykazuje chování typické pro škodlivý software.  Někdy se může takovým způsobem chovat i software bez zlých úmyslů, pro běžný vývoj softwaru ale toto chování není typické. 

Následující pole jsou společná pro všechny výstrahy s výpisem stavu systému:

- DUMPFILE: Název souboru s výpisem stavu systému 
- PROCESSNAME: Název procesu, ve kterém došlo k chybě 
- PROCESSVERSION: Verze procesu, ve kterém došlo k chybě 

Tato výstraha navíc obsahuje následující pole:

- ADDRESS: Umístění skrytého spustitelného kódu v paměti

Příklad tohoto typu výstrahy:

![Výstraha na skrytý spustitelný kód](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig10-ga.png)

### Zjištěna injektáž kódu

Injektáž kódu představuje vložení spustitelných modulů do spuštěných procesů nebo vláken.  Tato technika je používána malwarem pro přístup k datům, ukrytí malwaru nebo zabránění jeho odstranění.  Tato výstraha znamená, že při analýze výpisu stavu systému byl nalezen injektovaný modul.
 
Vývojáři legitimního softwaru někdy provádějí injektáž kódu bez zlých úmyslů, například kvůli úpravě nebo rozšiřování stávající aplikace nebo součásti operačního systému.  Azure Security Center rozlišuje škodlivé injektované moduly a moduly bez zlých úmyslů na základě profilů podezřelého chování. Výsledek kontroly je u výstrahy uveden v poli SIGNATURE a určuje závažnost výstrahy, popis výstrahy a nápravné kroky.  

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

- ADDRESS: Umístění injektovaného modulu v paměti
- IMAGENAME: Název injektovaného modulu. Pokud není v imagi uveden název image, může být toto pole prázdné.
- SIGNATURE: Udává, jestli injektovaný modul odpovídá profilu podezřelého chování. V následující tabulce najdete příklady výsledků kontroly a jejich popis:

| **Hodnota podpisu**                  | **Popis**                                                                                                   |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| Podezřelé zneužití zavaděče Reflective Loader | Toto podezřelé chování často souvisí s načítáním injektovaného kódu nezávisle na zavaděči operačního systému |
| Podezřelé zneužití vkládání kódu          | Podezřelé chování, které často souvisí s injektáží kódu do paměti                                       |
| Podezřelé zneužití vloženého kódu         | Podezřelé chování, které často souvisí s použitím kódu injektovaného do paměti                                   |
| Podezřelé zneužití injektáže ladicího programu | Podezřelé chování, které často souvisí s nalezením nebo obcházením ladicího programu                         |
| Podezřelé zneužití vzdáleného vkládání kódu   | Podezřelé chování, které často souvisí se spuštěním kódu a převzetím řízení počítače (tzv. scénář Command and control, C2)                                 |

Příklad tohoto typu výstrahy:

![Zjištěna injektáž kódu](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig11-ga.png)

### Zjištěno napadení modulu

Systém Windows umožňuje softwaru využívat základní systémové funkce prostřednictvím knihoven DLL (Dynamic Link Libraries).  Při napadení knihoven DLL malware pozmění pořadí načítání knihoven DLL tak, aby umožnil načtení škodlivých datových částí do paměti, ze které je pak bude možné spustit. Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny dva moduly s podobným názvem, ale různými cestami. Jedna z načtených cest přitom odpovídá běžnému umístění binárních souborů systému Windows.

Vývojáři legitimního softwaru někdy mění pořadí načítání knihoven DLL bez zlých úmyslů, například při instrumentaci, rozšiřování systému Windows nebo rozšiřování aplikací.  Azure Security Center rozlišuje škodlivé změny pořadí načítání knihoven DLL od pravděpodobně bezpečných změn na základě profilů podezřelého chování.   Výsledek kontroly je u výstrahy uveden v poli SIGNATURE a určuje závažnost výstrahy, popis výstrahy a nápravné kroky.  Další informace o povaze nestandardního modulu může poskytnout analýza kopie modulu uložené na disku. Při této analýze se provádí například ověření digitálního podpisu nebo antivirová kontrola.

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

- SIGNATURE: Udává, jestli napadený modul odpovídá profilu podezřelého chování
- HIJACKEDMODULE: Název napadeného modulu systému Windows
- HIJACKEDMODULEPATH: Cesta k napadenému modulu systému Windows
- HIJACKINGMODULEPATH: Cesta k napadajícímu modulu 

Příklad tohoto typu výstrahy:

![Výstraha na napadení knihovny DLL](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig12-ga.png)

### Zjištěno maskování za modul systému Windows

Malware může používat běžné názvy spustitelných souborů (např. SVCHOST.EXE) nebo modulů (např. NTDLL.DLL) systému Windows, aby nevyvolával podezření a skryl svou škodlivou povahu před správci systému.  Tato výstraha znamená, že při analýze výpisu stavu systému byly nalezeny moduly, které svým názvem odpovídají systémovým modulům Windows, ale nesplňují další kritéria typická pro systémové moduly.. Další informace o povaze maskovaného modulu může poskytnout analýza kopie modulu na disku. Součástí analýzy může být:

- Potvrzení, že je příslušný soubor součástí balíčku legitimního softwaru
- Ověření digitálního podpisu souboru 
- Antivirová kontrola souboru

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

- DETAILS: Udává, jestli jsou metadata modulu platná a jestli byl modul načten ze systémové cesty.
- NAME: Název maskovaného modulu systému Windows
- PATH: Cesta k maskovanému modulu systému Windows.

Tato výstraha také extrahuje a zobrazuje určitá pole ze záhlaví PE modulu, např. pole CHECKSUM a TIMESTAMP.  Tato pole se zobrazí, pouze pokud se v modulu nacházejí. Podrobnosti o těchto polích naleznete ve [specifikaci formátů Microsoft PE a COFF](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Příklad tohoto typu výstrahy:

![Výstraha na maskování](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig13-ga.png)

### Zjištěna úprava binárního systémového souboru 

Malware může upravit binární systémové soubory, aby mohl nepozorovaně přistupovat k datům nebo skrytě přetrvávat v napadeném systému.  Tato výstraha znamená, že při analýze stavu systému byly v paměti nebo na disku nalezeny upravené binární soubory jádra systému Windows. 

Vývojáři legitimního softwaru někdy upravují systémové soubory v paměti bez zlých úmyslů, například soubory balíčku Detours pro zajištění kompatibility aplikací. Azure Security Center rozlišuje škodlivé moduly od pravděpodobně bezpečných modulů na základě profilů podezřelého chování.   Výsledek kontroly určuje závažnost výstrahy, popis výstrahy a nápravné kroky. 

Kromě společných polí popsaných výše v části „Zjištěn skrytý spustitelný kód“ tato výstraha obsahuje taky následující pole:

- MODULENAME: Název upraveného binárního systémového souboru 
- MODULEVERSION: Verze upraveného binárního systémového souboru

Příklad tohoto typu výstrahy:

![Výstraha na upravený binární soubor](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig14-ga.png)


## Viz také

V tomto dokumentu jste zjistili, jak ve službě Security Center konfigurovat zásady zabezpečení. Pokud se o službě Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Řešení bezpečnostních incidentů v Azure Security Center](security-center-incident.md)
- [Funkce detekce ve službě Azure Security Center](security-center-detection-capabilities.md)
- [Průvodce plánováním a provozem služby Azure Security Center](security-center-planning-and-operations-guide.md)
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Aug16_HO4-->


