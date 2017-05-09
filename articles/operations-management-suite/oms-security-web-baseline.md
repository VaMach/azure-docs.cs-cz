---
title: "Standardní hodnoty webu v řešení Zabezpečení a audit pro Operations Management Suite | Dokumentace Microsoftu"
description: "Tento dokument popisuje, jak použít řešení Zabezpečení a audit pro OMS k provedení vyhodnocení standardních hodnot webu u všech monitorovaných webových serverů za účelem dodržování předpisů a zabezpečení."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 0d4707dc0c0ffbf40d0d10a6d12b9709a9655258
ms.contentlocale: cs-cz
ms.lasthandoff: 05/03/2017


---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro Operations Management Suite
Tento dokument vám pomůže s použitím schopností vyhodnocování standardních hodnot webu v [řešení Zabezpečení a audit pro Operations Management Suite (OMS)](operations-management-suite-overview.md) pro přístup ke stavu zabezpečení monitorovaných prostředků.

## <a name="what-is-web-baseline-assessment"></a>Co je vyhodnocování standardních hodnot webu?
Zabezpečení v OMS v současné době umožňuje vyhodnocování standardních hodnot zabezpečení pro operační systémy. Každých 24 hodin prohledá nastavení operačního systému vašich serverů a zobrazí potenciálně ohrožená nastavení. Další informace najdete v tématu [Vyhodnocování standardních hodnot v řešení Zabezpečení a audit pro Operations Management Suite](oms-security-baseline.md).

Cílem vyhodnocování standardních hodnot webu je najít potenciálně ohrožená nastavení webového serveru. Tři hlavní zdroje konfigurace standardních hodnot webu jsou: .NET, ASP.NET a konfigurace služby IIS.  Stejně jako v případě vyhodnocování standardních hodnot operačního systému bude Zabezpečení v OMS každých 24 hodin prohledávat vaše webové servery a zobrazí stav jejich zabezpečení.  Konfigurace v Internetové informační službě (IIS) jsou vysoce přizpůsobitelné, což umožňuje přepsání úrovní různých webů a aplikací. Skener kromě výchozí kořenové úrovně kontroluje i nastavení na jednotlivých úrovních aplikace nebo webu. Díky tomu můžete identifikovat umístění potenciální ohrožených nastavení a rychle je opravit.


## <a name="web-security-baseline-assessment"></a>Vyhodnocování standardních hodnot zabezpečení webu
V této verzi Preview se k této funkci bude přistupovat pomocí možnosti Vyhledávání v OMS. Postupujte podle následujících kroků a proveďte odpovídající dotaz:

1. Na hlavním řídicím panelu **Microsoft Operations Management Suite** klikněte na dlaždici **Zabezpečení a audit**.
2. Na řídicím panelu **Zabezpečení a audit** klikněte na tlačítko **Prohledávání protokolu**.
3. Jako první dotaz můžete použít **Souhrn vyhodnocení standardních hodnot webu**. Do pole **Zahájit hledání** zadejte tento dotaz: Type*=SecurityBaselineSummary BaselineType=web*. Na následující obrazovce je ukázka výstupu:

![Souhrn vyhodnocení standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> V tomto dotazu každý záznam označuje souhrn vyhodnocení na jednom serveru.

Když jste v **Prohledávání protokolu**, můžete zadáním různých dotazů získat další informace o vyhodnocení standardních hodnot webu. Kromě předchozího dotazu můžete ve verzi Preview použít také následující dotazy.

**Vyhodnocení pravidel standardních hodnot webu:** Každý záznam představuje jedno vyhodnocení pravidla standardních hodnot webu na jednom serveru. Obsahuje veškerá data o pravidlu, umístění, očekávaném výsledku a skutečném výsledku.

**Dotaz:** Type*=SecurityBaseline BaselineType=web*

![Vyhodnocení pravidel standardních hodnot webu](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Zobrazení všech výsledků pro konkrétní server:** Tento dotaz ukazuje, jak zobrazit výsledky pro konkrétní server.

**Dotaz:** *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Všechny výsledky](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Tyto záznamy a dotazy můžete také použít k vytvoření vlastních řídicích panelů, sestav nebo upozornění. Na následující obrazovce je ukázka ovládacího prvku uživatelského rozhraní, který můžete přidat na svůj řídicí panel. [Tady](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/) můžete zjistit, jak vizualizovat data pomocí Návrháře zobrazení v OMS. Na následující obrazovce je příklad toho, jak bude dlaždice vypadat po přizpůsobení.

![Ukázka uživatelského rozhraní](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Pokud chcete zjistit, která nastavení jsou pro vyhodnocování standardních hodnot zapnutá, můžete si stáhnout [tabulku aplikace Excel](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690), která tato nastavení obsahuje.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se dozvěděli o vyhodnocování standardních hodnot webu v řešení Zabezpečení a audit pro OMS. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)


