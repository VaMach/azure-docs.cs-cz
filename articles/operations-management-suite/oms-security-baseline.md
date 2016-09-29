<properties
   pageTitle="Standardní hodnoty řešení Zabezpečení a audit pro Operations Management Suite | Microsoft Azure"
   description="Tento dokument popisuje, jak použít řešení Zabezpečení a audit v OMS k provedení vyhodnocení standardních hodnot u všech monitorovaných počítačů za účelem dodržování předpisů a zabezpečení."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/08/2016"
   ms.author="yurid"/>


# Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite

Tento dokument vám pomůže s použitím schopností vyhodnocování standardních hodnot v [řešení Zabezpečení a audit pro Operations Management Suite (OMS)](operations-management-suite-overview.md) pro přístup ke stavu zabezpečení monitorovaných prostředků.

## Co je vyhodnocování standardních hodnot?

Společnost Microsoft, spolu s organizacemi z oboru a vládními organizacemi po celém světě, definuje konfiguraci systému Windows, která představuje vysoce zabezpečená nastavení serverů. Tuto konfiguraci tvoří sada klíčů registru, nastavení zásad auditu, nastavení zásad zabezpečení a společností Microsoft doporučené hodnoty pro tato nastavení. Tato sada pravidel se označuje jako standardní hodnoty zabezpečení. Schopnost vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS umožňuje bezproblémovou kontrolu dodržování předpisů na všech vašich počítačích. 

Existují tři typy pravidel:

- **Pravidla registru**: kontrolují, jestli jsou správně nastavené klíče registru.
- **Pravidla zásad auditu**: pravidla týkající se vašich zásad auditu.
- **Pravidla zásad zabezpečení**: pravidla týkající se oprávnění uživatele na počítači.

> [AZURE.NOTE] Stručný přehled této funkce najdete v tématu [Použití zabezpečení OMS k vyhodnocení standardních hodnot konfigurace zabezpečení](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/).

## Vyhodnocování standardních hodnot zabezpečení

Můžete zkontrolovat aktuální stav vyhodnocení standardních hodnot zabezpečení pro všechny počítače monitorované řešením Zabezpečení a audit v OMS pomocí řídicího panelu.  Provedením následujících kroků otevřete řídicí panel vyhodnocování standardních hodnot zabezpečení:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
2. Na řídicím panelu **Zabezpečení a audit** klikněte na **Vyhodnocování standardních hodnot** v části **Domény zabezpečení**. Zobrazí se řídicí panel **Vyhodnocování standardních hodnot zabezpečení**, jak je znázorněno na následujícím obrázku:
    
    ![Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Tento řídicí panel je rozdělen na tři hlavní oblasti:

- **Počítače porovnané se standardními hodnotami**: Tato část obsahuje souhrn počtu počítačů, ke kterým se zdařil přístup, a procentuální podíl počítačů, pro které bylo vyhodnocení úspěšné. Obsahuje také seznam 10 nejúspěšnějších počítačů a procentuální výsledek vyhodnocení.
- **Stav požadovaných pravidel**: Účelem této části je zvýšit povědomí o pravidlech, která selhala, podle závažnosti a podle typu. Při pohledu na první graf je na první pohled patrné, jestli byla většina pravidel, která selhala, kritických, nebo ne. Obsahuje také seznam Top 10 pravidel, která selhala, a jejich závažnost. Druhý graf ukazuje typy pravidel, která během vyhodnocování selhala. 
- **Počítače, u kterých nedošlo k vyhodnocení standardních hodnot**: Tato část obsahuje seznam počítačů, ke kterým se nezdařil přístup z důvodu nekompatibility operačního systému nebo selhání. 

### Přístup k počítačům porovnaným se standardními hodnotami

V ideálním případě jsou všechny vaše počítače kompatibilní s vyhodnocováním standardních hodnot zabezpečení. Nicméně se dá očekávat, že v některých případech tomu tak není. Je důležité zahrnout kontrolu počítačů, kterým se nepodařilo projít všemi testy vyhodnocování, jako součást procesu správy zabezpečení. Rychlý způsob, jak to vizualizovat, je výběrem možnosti **Dostupné počítače** umístěné v části **Počítače porovnané se standardními hodnotami**. Měl by se zobrazit výsledek hledání v protokolu se seznamem počítačů, jak je znázorněno na následující obrazovce:

![Výsledky hledání dostupných počítačů](./media/oms-security-baseline/oms-security-baseline-fig2.png)

Výsledek hledání je zobrazen ve formátu tabulky, kde první sloupec obsahuje název počítače a druhý sloupec obsahuje počet pravidel, která selhala. Chcete-li načíst informace týkající se typu pravidla, které selhalo, klikněte na počet pravidel, která selhala, vedle názvu počítače. Zobrazený výsledek by se měl podobat tomu na následujícím obrázku:

![Podrobnosti o výsledku hledání dostupných počítačů](./media/oms-security-baseline/oms-security-baseline-fig3.png)

Tento výsledek hledání obsahuje celkový počet dostupných pravidel, počet kritických pravidel, která selhala, a počet pravidel upozornění a informací, která selhala.

### Přístup ke stavu požadovaných pravidel

Po obdržení informace o procentuálním zastoupení počítačů, které prošly vyhodnocením, možná budete chtít získat další informace o tom, která pravidla selhávají podle jejich závažnosti. Tato vizualizace vám pomůže určit prioritu adresování počítačů k zajištění, aby při dalším vyhodnocení splňovaly požadavky. V části **Stav požadovaných pravidel** na dlaždici **Pravidla, která selhala, podle závažnosti** najeďte myší na část grafu Kritické a klikněte na ni. Zobrazený výsledek by měl vypadat přibližně jako na tomto obrázku:

![Podrobnosti o pravidlech, která selhala, podle závažnosti](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

V tomto výsledku protokolu uvidíte typ základního pravidla, které selhalo, popis pravidla a Common Configuration Enumeration (CCE) ID tohoto pravidla zabezpečení. Tyto atributy by měly být dostačující k provedení nápravné akce, která tento problém na cílovém počítači opraví.

> [AZURE.NOTE] Další informace o CCE získáte na webu [Národní databáze ohrožení zabezpečení](https://nvd.nist.gov/cce/index.cfm).

### Přístup k počítačům, u nichž nedošlo k vyhodnocení standardních hodnot

OMS podporuje profil standardních hodnot člena domény v systému Windows Server 2008 R2 až po Windows Server 2012 R2. Standardní hodnoty systému Windows Server 2016 ještě nejsou dokončené, ale budou přidány okamžitě po publikování. Všechny ostatní operační systémy kontrolované přes vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS jsou uvedené v části **Počítače, u kterých nedošlo k vyhodnocení standardních hodnot**.

## Viz také

V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS. Další informace o zabezpečení v OMS najdete v následujících článcích:

- [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
- [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
- [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Sep16_HO3-->


