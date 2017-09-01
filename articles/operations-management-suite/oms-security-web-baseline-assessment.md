---
title: "Vyhodnocování standardních hodnot webu v základních hodnotách řešení Operations Management Suite Security and Audit | Dokumentace Microsoftu"
description: "Tento dokument popisuje, jak pomocí vyhodnocování standardních hodnot webu v řešení OMS Security and Audit provádět vyhodnocování standardních hodnot u všech monitorovaných webových serverů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: 40b0c6ca933ea02ac9f5fe3bfaaf87a310542a8d
ms.contentlocale: cs-cz
ms.lasthandoff: 08/19/2017

---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro Operations Management Suite
Tento dokument vám pomůže s použitím schopností vyhodnocování standardních hodnot webu v řešení OMS Security and Audit pro přístup ke stavu zabezpečení monitorovaných prostředků.

## <a name="what-is-web-baseline-assessment"></a>Co je vyhodnocování standardních hodnot webu?
Zabezpečení v OMS v současné době umožňuje vyhodnocování standardních hodnot zabezpečení pro operační systémy. Každých 24 hodin prohledá nastavení operačního systému vašich serverů a zobrazí potenciálně ohrožená nastavení. Další informace najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline).

Cílem vyhodnocování standardních hodnot webu je najít potenciálně ohrožená nastavení webového serveru. Tři hlavní zdroje konfigurace standardních hodnot webu jsou: .NET, ASP.NET a konfigurace služby IIS.  Stejně jako v případě vyhodnocování standardních hodnot operačního systému bude Zabezpečení v OMS každých 24 hodin prohledávat vaše webové servery a zobrazí stav jejich zabezpečení.  Konfigurace v Internetové informační službě (IIS) jsou vysoce přizpůsobitelné, což umožňuje přepsání úrovní různých webů a aplikací. Skener kromě výchozí kořenové úrovně kontroluje i nastavení na jednotlivých úrovních aplikace nebo webu. Díky tomu můžete identifikovat potenciálně ohrožená nastavení a s našimi doporučeními k těmto nastavením je rychle opravit.

>[!NOTE] 
>Identifikátory obecných konfigurací a základní pravidla používaná zabezpečením OMS si můžete stáhnout na této [stránce](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0).


## <a name="web-security-baseline-assessment"></a>Vyhodnocování standardních hodnot zabezpečení webu

Ve verzi Preview se k této funkci přistupuje prostřednictvím možnosti Vyhledávání v OMS a řídicího panelu řešení OMS Security and Audit. Postupujte podle následujících kroků a proveďte odpovídající dotaz:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
2. Na řídicím panelu řešení **Security and Audit** se vedle perspektivy standardních hodnot operačního systému zobrazí perspektiva standardních hodnot webu.
   
    ![Vyhodnocování standardních hodnot zabezpečení webu v řešení OMS Security and Audit](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. V levém podokně se zobrazí počet webových serverů v porovnání se standardními hodnotami, průměrné procento pravidel, která prošla na všech vyhodnocovaných serverech, a seznam hodnocených serverů.
4. V pravém podokně je seznam jedinečných pravidel, která selhala, podle *závažnosti* a *typu pravidla*. Po kliknutí na libovolné pravidlo v pravém podokně se zobrazí podrobnosti o tomto pravidle. Příklad je uveden na obrázku níže. Vyhodnocované pravidlo je uvedeno v části *Nastavení pravidla*. Pole *AzId* je jedinečný identifikátor každého pravidla, který Microsoft používá ke sledování základních pravidel. Kromě toho můžou uživatelé zobrazit *Očekávaný výsledek* (doporučená hodnota od Microsoftu) a další podrobnosti související s dopadem pravidla na zabezpečení.
    
    ![Dotaz](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. Pro kontrolu výsledků můžete vytvářet vlastní dotazy. 

Jako první dotaz můžete použít **Souhrn vyhodnocení standardních hodnot webu**. Do pole **Zahájit hledání** zadejte tento dotaz: *Type=SecurityBaselineSummary BaselineType=Web*. Následuje ukázka výstupu:

![Výsledek dotazu](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>V tomto dotazu každý záznam označuje souhrn vyhodnocení na jednom serveru.

Když jste v **Prohledávání protokolu**, můžete zadáním různých dotazů získat další informace o vyhodnocení standardních hodnot webu. Kromě předchozího dotazu můžete ve verzi Preview použít také následující dotazy:

**Vyhodnocení pravidel standardních hodnot webu:** Každý záznam představuje jedno vyhodnocení pravidla standardních hodnot webu na jednom serveru. Obsahuje všechna data pro pravidlo, které selhalo, *Cestu k webu*, na které se pravidlo vyhodnocovalo, *Očekávaný výsledek* a *Skutečný výsledek*.

Dotaz: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Výsledek dotazu 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Zobrazení všech výsledků pro konkrétní server:** Tento dotaz předvádí, jak zobrazit výsledky konkrétního serveru: Dotaz: *Type=SecurityBaseline BaselineType=Web Computer=BaselineTestVM1*

![Výsledek dotazu 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Tyto záznamy a dotazy můžete použít k vytvoření vlastních řídicích panelů, sestav nebo upozornění. Tady je ukázka ovládacího prvku uživatelského rozhraní, který můžete přidat na svůj řídicí panel. [Tady](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) můžete zjistit, jak vizualizovat data pomocí Návrháře zobrazení v OMS. Na následující obrazovce je příklad toho, jak bude dlaždice vypadat po přizpůsobení.

![řídicí panel](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot webu v řešení OMS Security and Audit. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)


