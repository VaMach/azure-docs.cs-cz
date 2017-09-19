---
title: "Analýza hrozeb v Azure Security Center | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže s používáním možnosti analýzy hrozeb v Azure Security Center k identifikaci potenciálních hrozeb na virtuálních počítačích a počítačích."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: a771a3a1-2925-46ca-8e27-6f6a0746f58b
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: c492662aa3ee444347c55d9771790573ad969150
ms.contentlocale: cs-cz
ms.lasthandoff: 09/14/2017

---
# <a name="threat-intelligence-in-azure-security-center"></a>Analýza hrozeb v Azure Security Center
Tento dokument vám pomůže s používáním Analýzy hrozeb v Azure Security Center k vyřešení problémů souvisejících se zabezpečením.

## <a name="what-is-threat-intelligence"></a>Co je analýza hrozeb?
Pomocí možnosti Analýza hrozeb, která je dostupná ve službě Security Center, můžou správci IT identifikovat bezpečnostní hrozby pro prostředí – například můžou určit, jestli je konkrétní počítač součástí botnetu. Z počítačů se můžou stát uzly v botnetu, když útočníci neoprávněně nainstalují malware, který tajně připojí počítač k řídicímu serveru. Může také identifikovat potenciální hrozby přicházející z alternativních komunikačních kanálů, jako je například darknet.

Aby služba Security Center mohla tuto analýzu hrozeb sestavit, používá data přicházející z několika zdrojů v rámci Microsoftu. Služba Security Center tato data využívá k identifikaci potenciálních hrozeb pro vaše prostředí. Podokno Analýza hrozeb se skládá ze tří hlavních možností:

- Zjištěné typy hrozeb
- Původ hrozeb
- Mapa analýzy hrozeb


## <a name="when-should-i-use-threat-intelligence"></a>Kdy mám použít analýzu hrozeb?
Jedním z kroků [procesu reakce na incidenty zabezpečení](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide#incident-response) je identifikace závažnosti napadených systémů. V této fázi byste měli provést následující úlohy:

- Určit povahu útoku.
- Určit, odkud útok přišel.
- Určit záměr útoku. Směřoval útok konkrétně na vaši organizaci za účelem získání určitých informací, nebo se jednalo o náhodný útok?
- Identifikovat napadené systémy.
- Identifikujte soubory, ke kterým byl získán přístup, a určete citlivost takových souborů. S těmito úlohami vám můžou pomoct informace Analýzy hrozeb ve službě Security Center. 

## <a name="how-to-access-the-threat-intelligence"></a>Jak získat přístup k analýze hrozeb?
Pokud chcete vizualizovat aktuální analýzu hrozeb pro vaše prostředí, musíte nejprve vybrat pracovní prostor, kde se nacházejí vaše informace. Pokud nemáte více pracovních prostorů, nezobrazí se selektor pracovního prostoru a budete přesměrováni přímo na řídicí panel **Analýza hrozeb**. Pokud chcete získat přístup k řídicímu panelu analýzy hrozeb, postupujte následovně:

1.  Otevřete řídicí panel **Security Center**.
2.  V levém podokně v části **Detekce** klikněte na **Analýza hrozeb**. Zobrazí se řídicí panel **Analýza hrozeb**.

    ![Analýza hrozeb](./media/security-center-threat-intel/security-center-threat-intel-fig1.png)

    > [!NOTE]
    > Pokud se v posledním sloupci zobrazí **UPGRADUJTE PLÁN**, je to proto, že pracovní prostor používá bezplatné předplatné a pro použití této funkce je potřeba upgradovat na úroveň Standard. Pokud se v něm zobrazí VYŽADUJE UPGRADE, je to proto, že pro použití této funkce je potřeba upgradovat [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview). Další informace o cenovém plánu najdete v článku o cenách Azure Security Center. 
    > 
3. Pokud máte k prozkoumání více než jeden pracovní prostor, můžete určit prioritu prozkoumání podle sloupce **ŠKODLIVÁ IP ADRESA**, ve kterém se zobrazuje aktuální počet škodlivých IP adres v tomto pracovním prostoru. Vyberte pracovní prostor, který chcete použít, a pak se zobrazí řídicí panel **Analýza hrozeb**.

    ![Analýza hrozeb](./media/security-center-threat-intel/security-center-threat-intel-fig5.png)

4. Tento řídicí panel je rozdělen na čtyři dlaždice:
    * **Typy hrozeb:** Obsahuje souhrn typů hrozeb detekovaných ve vybraném pracovním prostoru.
    * **Země původu:** Agreguje objem přenosů podle zdrojového místa.
    * **Poloha hrozeb:** Pomáhá identifikovat aktuální místa po celém světě, ze kterých probíhá komunikace s vaším prostředím. Na této mapě jsou oranžové (příchozí) a červené (odchozí) šipky, které určují směr přenosu. Pokud na některou z těchto šipek kliknete, zobrazí se typ hrozby a směr přenosu.
    * **Podrobnosti o hrozbě:** Zobrazuje více podrobností o hrozbě, kterou jste vybrali na mapě.

Řídicí panel, který se zobrazí po výběru jakékoli dlaždice možností, vychází z dotazu [prohledávání protokolu](https://docs.microsoft.com/azure/security-center/security-center-search), jediným rozdílem je typ dotazu a výsledek.

### <a name="threat-types"></a>Typy hrozeb
Po kliknutí na dlaždici **Typy hrozeb** se zobrazí řídicí panel **Prohledávání protokolu**, na kterém jsou vlevo možnosti filtru a vpravo výsledky dotazu.

![Prohledávání analýzy hrozeb](./media/security-center-threat-intel/security-center-threat-intel-fig3.png)

Výsledek dotazu zobrazuje agregované hrozby podle názvu. V levém podokně můžete vybrat atribut, podle kterého chcete filtrovat – pokud například chcete zobrazit pouze hrozby, které jsou aktuálně připojené k počítačům, vyberete v části **STAV RELACE** možnost **Připojeno** a kliknete na tlačítko **Použít**.

![stav relace](./media/security-center-threat-intel/security-center-threat-intel-fig4.png)

Pro virtuální počítače Azure se na řídicím panelu analýzy hrozeb zobrazí pouze data sítě, která prochází přes agenta. Analýza hrozeb používá také následující typy dat:

- Data CEF (Type=CommonSecurityLog)
- WireData (Type=WireData)
- Protokoly služby IIS (Type=W3CIISLog)
- Brána Windows Firewall (Type=WindowsFirewall)
- Události DNS (Type=DnsEvents)


## <a name="see-also"></a>Viz také
V tomto dokumentu jste zjistili, jak používat analýzu hrozeb ve službě Security Center jako pomoc při identifikování podezřelých aktivit. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Správa a zpracování výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Zjistěte, jak spravovat výstrahy a reagovat na incidenty zabezpečení ve službě Security Center.
* [Monitorování stavu zabezpečení ve službě Azure Security Center](security-center-monitoring.md). Zjistěte, jak monitorovat stav svých prostředků Azure.
* [Principy výstrah zabezpečení ve službě Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Seznamte se s dalšími typy výstrah zabezpečení.
* [Průvodce odstraňováním potíží pro službu Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Zjistěte, jak řešit běžné problémy ve službě Security Center. 
* [Azure Security Center – nejčastější dotazy](security-center-faq.md). Přečtěte si nejčastější dotazy o použití této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/). Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.


