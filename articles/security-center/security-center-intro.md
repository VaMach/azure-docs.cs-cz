<properties
   pageTitle="Úvod do Azure Security Center | Microsoft Azure"
   description="Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/22/2016"
   ms.author="terrylan"/>

# Úvod do Azure Security Center

Můžete se dozvědět o službě Azure Security Center, jejích klíčových funkcích a způsobu práce.

> [AZURE.NOTE] Informace v tomto dokumentu se týkají verze Preview služby Azure Security Center. Tento dokument vám tuto službu představí formou ukázkového nasazení.

## Co je Azure Security Center?
 Security Center pomáhá předcházet hrozbám, zjišťovat je a reagovat na ně a nabízí lepší přehled o zabezpečení prostředků Azure a kontrolu nad nimi. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure, pomáhá zjišťovat hrozby, kterých byste si jinak nevšimli, a spolupracuje s řadou řešení zabezpečení.

##  Klíčové funkce
 Security Center nabízí snadno použitelné a efektivní funkce prevence, zjišťování a reakce na ohrožení, které jsou integrované v Azure. Klíčové funkce:

| | |
|----- |-----|
| Prevence | Sleduje stav zabezpečení vašich prostředků Azure. |
| | Definuje zásady pro vaše předplatná Azure a skupiny zdrojů na základě požadavků zabezpečení vaší společnosti, typů aplikací, které používáte, a citlivosti vašich data. |
| | Používá doporučení zabezpečení řízená zásadami, aby vedla vlastníky služby procesem implementace potřebných kontrol. |
| | Rychle nasazuje bezpečnostní služby a zařízení Microsoftu a partnerů. |
| Zjišťování |Automaticky shromažďuje a analyzuje data zabezpečení z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. |
| | Využívá globální analýzu hrozeb z produktů a služeb Microsoftu, od týmu DCU (Microsoft Digital Crimes Unit), ze střediska MSRC (Microsoft Security Response Center) a z externích informačních kanálů. |
| | Používá pokročilou analýzu, včetně machine learningu a analýzy chování. |
| Reakce | Poskytuje incidenty a výstrahy zabezpečení seřazené podle priority. |
| | Nabízí podrobný náhled na zdroj útoku a zasažené prostředky. |
| | Navrhuje způsoby, jak aktuální útok zastavit, a pomáhá zabránit budoucím útokům. |

## Úvodní prohlídka
 Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). [Přihlaste se k portálu](https://portal.azure.com), vyberte **Procházet** a pak přejděte k možnosti **Security Center** nebo vyberte dlaždici **Security Center**, kterou jste dříve připnuli k řídicímu panelu portálu.

![Dlaždice zabezpečení na portálu Azure][1]

Ze služby Security Center můžete nastavovat zásady zabezpečení, sledovat konfigurace zabezpečení a zobrazovat výstrahy zabezpečení.

### Zásady zabezpečení

Můžete definovat zásady pro skupiny prostředků a předplatná Azure podle požadavků vaší společnosti na zabezpečení. Můžete je také přizpůsobit typům aplikací, které používáte, nebo citlivosti dat v každém předplatném. Například prostředky používané pro vývoj nebo testování můžou mít jiné požadavky na zabezpečení než ty, které se používají v aplikacích v produkčním prostředí. Aplikace pracující s regulovanými daty, třeba s osobními údaji, zase můžou vyžadovat jinou úroveň zabezpečení.

> [AZURE.NOTE] Chcete-li upravit zásady zabezpečení na úrovni předplatného nebo na úrovni skupiny prostředků, musíte být vlastníkem předplatného nebo jeho přispěvatelem.

V okně **Security Center** vyberte dlaždici **Zásady**, kde zobrazíte seznam předplatných a skupin prostředků.   

![Okno Security Center][2]

V okně **Zásady zabezpečení** vyberte předplatné, pro které chcete zobrazit podrobnosti zásady.

![Předplatné okna Zásady zabezpečení][3]

**Data collection** (Shromažďování dat) (viz výše) povoluje pro zásadu zabezpečení shromažďování dat. Povolením získáte následující:

- Denní kontrolu všech podporovaných virtuálních počítačů z důvodu monitorování zabezpečení a poskytování doporučení.
- Shromažďování událostí zabezpečení pro analýzu a zjišťování hrozeb.

**Choose a storage account per region** (Zvolit účet úložiště podle oblasti) (viz výše) umožňuje pro každou oblast, ve které máte spuštěné virtuální počítače, zvolit si účet úložiště, na který se mají ukládat data shromážděná z daných virtuálních počítačů. Pokud pro každou oblast nevyberete účet úložiště, vytvoří se vám automaticky. Shromážděná data budou z důvodů zabezpečení logicky oddělená od ostatních zákaznických dat.

> [AZURE.NOTE] Shromažďování dat a volba účtu úložiště podle oblasti se konfigurují na úrovni předplatného.

**Show recommendations for** (Zobrazit doporučení pro) (viz výše) umožňuje vybrat kontroly zabezpečení, které chcete monitorovat a ke kterým chcete nabízet doporučení, podle potřeb zabezpečení prostředků v rámci předplatného.

Dále vyberte skupinu prostředků, abyste zobrazili podrobnosti zásady.

![Skupina prostředků okna Zásady zabezpečení][4]

**Inheritance** (Dědičnost) (viz výše) umožňuje definovat skupiny prostředků jako:

- Zděděné (výchozí), což znamená, že se všechny zásady zabezpečení pro tuto skupinu prostředků dědí z úrovně předplatného.
- Jedinečné, což znamená, že skupina prostředků bude mít vlastní zásady zabezpečení. Budete muset provést změny v části **Show recommendations for** (Zobrazit doporučení pro).

> [AZURE.NOTE] V případě konfliktu mezi zásadou na úrovni předplatného a zásadou na úrovni skupiny prostředků má přednost zásada na úrovni skupiny prostředků.

### Doporučení zabezpečení

 Služba Security Center analyzuje stav zabezpečení vašich prostředků Azure, aby identifikovala potenciální ohrožení zabezpečení. Seznam doporučení vás provede procesem konfigurace potřebných kontrol. Příklady obsahují:

- Zřizování antimalwaru, aby se pomohl identifikovat a odebrat škodlivý software
- Konfigurace skupin zabezpečení sítě a pravidel pro řízení přenosu do virtuálních počítačů
- Zřizování firewallů webových aplikací, které pomáhají bránit útokům zaměřeným na vaše webové aplikace
- Nasazení chybějících aktualizací systému
- Adresování konfigurací operačního systému, které neodpovídají doporučeným standardním hodnotám

Seznam doporučení získáte kliknutím na dlaždici **Doporučení**. Kliknutím na jednotlivá doporučení zobrazíte další informace nebo provedete akce vedoucí k vyřešení daného problému.

![Doporučení zabezpečení v Azure Security Center][5]

### Stav prostředků

Dlaždice **Resource Security Health** (Stav zabezpečení prostředků) ukazuje celkové postavení zabezpečení prostředí podle typů prostředků, včetně virtuálních počítačů, webových aplikací a dalších prostředků.   

Výběrem typu prostředku na dlaždici **Resource Security Health** (Stav zabezpečení prostředků) zobrazíte další informace, včetně seznamu možných ohrožení zabezpečení, které byla identifikována. (V příkladu dále jsou vybrané **Virtuální počítače**.)

![Dlaždice Stav prostředků][6]

### Výstrahy zabezpečení

 Security Center automaticky shromažďuje, analyzuje a integruje data protokolu z vašich prostředků Azure, sítě a řešení partnerů, jako jsou antimalwarové programy a brány firewall. Při zjištění ohrožení zabezpečení se vytvoří výstraha zabezpečení. Příklady zahrnují zjišťování následujících situací:

- Ohrožené virtuální počítače, které komunikují se známými IP adresami se zlými úmysly
- Pokročilý malware zjištěný pomocí zasílání zpráv o chybách systému Windows
- Útoky hrubou silou na virtuální počítače
- Výstrahy zabezpečení z integrovaných antimalwarových programů a bran firewall

Kliknutím na dlaždici **Výstrahy zabezpečení** zobrazíte seznam výstrah seřazený podle priority.

![Výstrahy zabezpečení][7]

Výběrem výstrahy zobrazíte další informace o útoku a návrhy na jeho řešení.

![Podrobnosti výstrahy zabezpečení][8]

### Partnerská řešení

Dlaždice **Partner solutions** (Partnerská řešení) umožňuje přehledně sledovat stav partnerských řešení integrovaných ve vašem předplatném Azure. Security Center zobrazuje výstrahy, které pocházejí z těchto řešení.

Vyberte dlaždici **Partner solutions** (Partnerská řešení). Otevře se okno se seznamem všech připojených partnerských řešení.

![Partnerská řešení][9]

## Začínáme
Pokud chcete začít využívat Security Center, musíte mít předplatné pro Microsoft Azure. Služba Security Center je povolená s vaším předplatným Azure. Pokud nemáte předplatné, můžete si zaregistrovat [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

 Služba Security Center je přístupná prostřednictvím [portálu Azure](https://azure.microsoft.com/features/azure-portal/). Další informace najdete v [dokumentaci k portálu](https://azure.microsoft.com/documentation/services/azure-portal/).

Téma [Začínáme s Azure Security Center](security-center-get-started.md) vás rychle provede komponentami pro monitorování zabezpečení a správu zásad služby Security Center.

## Další kroky
V tomto dokumentu jste se seznámili se službou Security Center, jejími klíčovými funkcemi a způsobu zahájení práce. Další informace najdete v těchto článcích:

- [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se konfigurují zásady zabezpečení pro vaše předplatné Azure a skupiny prostředků.
- [Správa doporučení zabezpečení v Azure Security Center](security-center-recommendations.md) – Zjistěte, jak vám doporučení pomáhají chránit prostředky v Azure.
- [Sledování stavu zabezpečení v Azure Security Center](security-center-monitoring.md) – Naučte se monitorovat stav svých prostředků Azure.
- [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
- [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog Azure Security](http://blogs.msdn.com/b/azuresecurity/) – Získejte nejnovější informace o zabezpečení Azure.

<!--Image references-->
[1]: ./media/security-center-intro/security-tile.png
[2]: ./media/security-center-intro/security-center.png
[3]: ./media/security-center-intro/security-policy.png
[4]: ./media/security-center-intro/security-policy-blade.png
[5]: ./media/security-center-intro/recommendations.png
[6]: ./media/security-center-intro/resources-health.png
[7]: ./media/security-center-intro/security-alert.png
[8]: ./media/security-center-intro/security-alert-detail.png
[9]: ./media/security-center-intro/partner-solutions.png



<!--HONumber=Jun16_HO2--->


