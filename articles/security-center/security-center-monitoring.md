---
title: "Sledování zabezpečení v Azure Security Center | Dokumentace Microsoftu"
description: "Tento článek vám pomůže začít pracovat s funkcemi sledování v Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: f41fbee742daf2107b57caa528e53537018c88c6
ms.openlocfilehash: d796566676e13b312832dd543f52d3475d877ef8
ms.lasthandoff: 03/31/2017


---
# <a name="security-health-monitoring-in-azure-security-center"></a>Sledování stavu zabezpečení v Azure Security Center
Tento článek vám pomůže s využíváním možností v Azure Security Center ke sledování dodržování zásad.

## <a name="what-is-security-health-monitoring"></a>Co je sledování stavu zabezpečení?
Pod pojmem sledování si často představujeme pozorování a čekání, až se stane nějaká událost, abychom na situaci reagovali. Sledování zabezpečení odkazuje na používání proaktivní strategie, kdy se auditují vaše prostředky a identifikují ty systémy, které nesplňují organizační standardy nebo osvědčené postupy.

## <a name="monitoring-security-health"></a>Sledování stavu zabezpečení
Po povolení [zásad zabezpečení](security-center-policies.md) pro prostředky předplatného služba Security Center analyzuje zabezpečení vašich prostředků, aby identifikovala potenciální ohrožení zabezpečení. Informace o konfiguraci vaší sítě jsou k dispozici okamžitě. Získání informací o konfiguraci virtuálního počítače, jako je stav aktualizace zabezpečení nebo konfigurace operačního systému, může trvat hodinu i déle. V okně **Stav zabezpečení prostředku** můžete zobrazit stav zabezpečení svých prostředků a případné problémy. Můžete také zobrazit seznam těchto problémů v okně **Doporučení**.

Další informace o tom, jak používat doporučení, najdete v článku [Implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).

Na dlaždici **Stav zabezpečení prostředku** můžete sledovat stav zabezpečení svých prostředků. V následujícím příkladu vidíte, že řada problémů má vysokou a střední závažnost a vyžadují pozornost. Zásady zabezpečení, které jsou povolené, budou mít vliv na typy ovládacích prvků, které jsou monitorovány.

![Dlaždice stavu zabezpečení prostředků](./media/security-center-monitoring/security-center-monitoring-fig1-new001-2017.png)

Pokud Security Center identifikuje ohrožení zabezpečení, které je potřeba řešit, jako je například virtuální počítač s chybějícími aktualizacemi zabezpečení nebo podsíť bez [skupiny zabezpečení sítě](/virtual-network/virtual-networks-nsg.md), budou zde uvedené.

### <a name="monitor-compute"></a>Monitorování služby Compute
Když kliknete na **Compute** na dlaždici **Stav zabezpečení prostředků**, okno **Compute**, které se otevře, zobrazuje tři karty:

- **Přehled:** Doporučení pro monitorování a virtuální počítač
- **Virtuální počítače:** Seznam všech virtuálních počítačů a jejich aktuálního stavu zabezpečení
- **Cloudová služby:** Seznam všech webových a pracovních rolí monitorovaných pomocí služby Security Center.

![Chybějící aktualizace systému podle virtuálních počítačů](./media/security-center-monitoring/security-center-monitoring-fig1-new002-2017.png)

Na každé kartě můžete mít několik částí a v každé části můžete vybrat jednotlivé možnosti, abyste zobrazili další podrobnosti o doporučeném postupu k vyřešení konkrétního problému. 

#### <a name="monitoring-recommendations"></a>Doporučení pro monitorování
Tato část uvádí celkový počet virtuálních počítačů, které byly inicializovány pro shromažďování dat, a jejich aktuální stavy. Jakmile je u všech virtuálních počítačů shromažďování dat inicializováno, budou připravené přijímat zásady zabezpečení služby Security Center. Po kliknutí na tuto položku se otevře okno **Agent virtuálního počítače chybí nebo neodpovídá**. 

![Chybějící aktualizace systému podle virtuálních počítačů](./media/security-center-monitoring/security-center-monitoring-fig1-new003-2017.png)


#### <a name="virtual-machine-recommendations"></a>Doporučení pro virtuální počítače
Tato část obsahuje sadu [doporučení pro každý virtuální počítač](security-center-virtual-machine-recommendations.md) monitorovaný pomocí Azure Security Center. První sloupec uvádí doporučení. Druhý sloupec zobrazuje celkový počet virtuálních počítačů, které jsou ovlivněny tímto doporučením. Třetí sloupec zobrazuje závažnost problému, jak je znázorněno na následujícím snímku obrazovky.

![Doporučení pro virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig1-new004-2017.png)

> [!NOTE]
> V seznamu **Síťová topologie** v okně **Stav sítě** se zobrazí jenom virtuální počítače s aspoň jedním veřejným koncovým bodem.
>
>

Každé doporučení obsahuje sadu akcí, které můžete provést, když na ni kliknete. Pokud například kliknete na **Chybějící aktualizace systému**, otevře se okno **Chybějící aktualizace systému**. Uvádí seznam virtuálních počítačů, kterým chybí opravy, a závažnost chybějící aktualizace, jak je znázorněno na následujícím snímku obrazovky.

![Chybějící aktualizace systému pro virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Okno **Chybějící aktualizace systému** zobrazí tabulku s následujícími informacemi:

* **VIRTUÁLNÍ POČÍTAČ**: Název virtuálního počítače, kterému chybí aktualizace.
* **AKTUALIZACE SYSTÉMU**: Počet aktualizací systému, které chybí.
* **ČAS POSLEDNÍ KONTROLY**: Čas, kdy služba Security Center naposledy hledala ve virtuálním počítači aktualizace.
* **STAV**: Aktuální stav doporučení:
  * **Otevřené**: Doporučení dosud nebylo řešeno.
  * **Probíhá**: Doporučení se aktuálně na tyto prostředky používá a není třeba provádět žádnou akci.
  * **Vyřešeno**: Doporučení už je dokončené. (Pokud byl problém vyřešen, položka je vyšedlá.)
* **ZÁVAŽNOST**: Popisuje závažnost tohoto konkrétního doporučení:
  * **Vysoká**: Ohrožení zabezpečení existuje u významného prostředku (aplikace, virtuální počítač nebo skupina zabezpečení sítě) a vyžaduje pozornost.
  * **Střední**: Nekritické nebo další kroky jsou potřebné k dokončení procesu nebo odstranění ohrožení.
  * **Nízká**: Ohrožení zabezpečení by se mělo řešit, ale nevyžaduje okamžitou pozornost. (Ve výchozím nastavení nejsou doporučení s nízkou závažností uváděny, ale pokud je chcete zobrazit, je možné je vyfiltrovat.)

Pokud chcete zobrazit podrobnosti o doporučení, klikněte na název virtuálního počítače. Otevře se nové okno pro tento virtuální počítač se seznamem aktualizací, jak ukazuje následující snímek obrazovky.

![Chybějící aktualizace systému pro konkrétní virtuální počítač](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [!NOTE]
> Bezpečnostní doporučení uvedená tady jsou stejná jako v okně **Doporučení**. Další informace o tom, jak řešit doporučení, najdete v článku [Implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md). Týká se to nejen virtuálních počítačů, ale taky všech prostředků, které jsou k dispozici na dlaždici **Stav prostředku**.
>
>

#### <a name="virtual-machines-section"></a>Část virtuálních počítačů
Část virtuálních počítačů poskytuje přehled všech virtuálních počítačů a doporučení. Každý sloupec představuje jednu sadu doporučení, jak ukazuje následující snímek obrazovky:

![Přehled všech virtuálních počítačů a doporučení](./media/security-center-monitoring/security-center-monitoring-fig1-new005-2017.png)

Ikona, která se zobrazí pod každým doporučením, pomáhá rychle zjistit, které virtuální počítače vyžadují pozornost a o jaký typ doporučení se jedná.

V předchozím příkladu má jeden virtuální počítač kritické doporučení týkající se ochrany koncových bodů. Pokud chcete získat další informace o tomto virtuálním počítači, klikněte na něj. Nové okno, které se otevře, představuje tento virtuální počítač, jak je znázorněno na následujícím snímku obrazovky.

![Podrobné informace o zabezpečení virtuálního počítače](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Toto okno obsahuje podrobné informace o zabezpečení pro virtuální počítač. V dolní části tohoto okna vidíte doporučenou akci a závažnost jednotlivých problémů.

#### <a name="cloud-services-section"></a>Část cloudových služeb
Pro cloudové služby se doporučení vytvoří, když je verze operačního systému zastaralá, jak je znázorněno na následujícím snímku obrazovky:

![Stav pro cloudové služby](./media/security-center-monitoring/security-center-monitoring-fig1-new006-2017.png)

V případě, kdy máte doporučení (což není případ předchozího příkladu), budete muset postupovat podle kroků v doporučení, abyste aktualizovali verzi operačního systému. Když je k dispozici aktualizace, obdržíte výstrahu (červenou nebo oranžovou – v závislosti na závažnosti problému). Když kliknete na tuto výstrahu v řádku Webová role 1 (spouští Windows Server s vaší webovou aplikací automaticky nasazenou do služby IIS) nebo Role pracovního procesu 1 (spouští Windows Server s vaší webovou aplikací automaticky nasazenou do služby IIS), otevře se nové okno s dalšími podrobnostmi týkajícími se tohoto doporučení, jak je znázorněno na následujícím snímku obrazovky:

![Podrobnosti cloudové služby](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png)

Pokud chcete zobrazit podrobnější vysvětlení tohoto doporučení, klikněte na **Aktualizovat verzi operačního systému** ve sloupci **POPIS**. Otevře se okno **Aktualizovat verzi operačního systému (Preview)** s dalšími podrobnostmi.

![Doporučení pro Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### <a name="monitor-virtual-networks"></a>Monitorování virtuálních sítí
Po kliknutí na **Sítě** na dlaždici **Stav zabezpečení prostředku** se otevře okno **Sítě** s dalšími podrobnostmi, jak je znázorněno na následujícím snímku obrazovky:

![Okno Sítě](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Doporučení pro sítě
Podobně jako u informací o stavu prostředků virtuálních počítačů toto okno obsahuje v horní části souhrnný seznam problémů a v dolní části seznam monitorovaných sítí.

Část rozpisu stavu sítí uvádí možné problémy zabezpečení a nabízí [doporučení](security-center-network-recommendations.md). Možné problémy mohou zahrnovat:

* Není nainstalována brána firewall příští generace (NGFW).
* Nejsou povolené skupiny zabezpečení sítě v podsítích.
* Nejsou povolené skupiny zabezpečení sítě ve virtuálních počítačích.
* Omezený externí přístup prostřednictvím veřejného externího koncového bodu.
* Internetové koncové body, které jsou v pořádku

Po kliknutí na doporučení se otevře nové okno s dalšími podrobnostmi o doporučení, jak je znázorněno na následujícím snímku obrazovky.

![Podrobnosti o doporučení v okně Sítě](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

V tomto příkladu obsahuje okno **Nakonfigurovat chybějící skupiny zabezpečení sítě pro podsítě** seznam podsítí a virtuálních počítačů, kde chybí ochrana skupiny zabezpečení sítě. Pokud kliknete na podsíť, na kterou chcete použít skupinu zabezpečení sítě, otevře se jiné okno.

V okně **Zvolit skupinu zabezpečení sítě** můžete vybrat nejvhodnější skupinu zabezpečení sítě pro podsíť nebo můžete vytvořit novou skupinu zabezpečení sítě.

#### <a name="internet-facing-endpoints-section"></a>Část internetových koncových bodů
V části **Internetové koncové body** uvidíte virtuální počítače, které jsou aktuálně nakonfigurované s internetovým koncovým bodem, a jejich aktuální stav.

![Virtuální počítače nakonfigurované s internetovým koncovým bodem a stav](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Tato tabulka obsahuje název koncového bodu, který představuje virtuální počítač, internetovou IP adresu, aktuální stav závažnosti skupiny zabezpečení sítě a bránu firewall příští generace. Tabulka je řazená podle závažnosti:

* Červená (nahoře): Vysoká priorita, mělo by se řešit okamžitě.
* Oranžová: Střední priorita, mělo by se řešit co nejdříve.
* Zelená (poslední): Stav v pořádku.

#### <a name="networking-topology-section"></a>Část topologie sítě
Část **Topologie sítě** obsahuje hierarchické zobrazení prostředků, jak je znázorněno na následujícím snímku obrazovky:

![Hierarchické zobrazení prostředků v části topologie sítě](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Tato tabulka je řazená podle závažnosti (virtuální počítače a podsítě):

* Červená (nahoře): Vysoká priorita, mělo by se řešit okamžitě.
* Oranžová: Střední priorita, mělo by se řešit co nejdříve.
* Zelená (poslední): Stav v pořádku.

V tomto zobrazení topologie obsahuje první úroveň [virtuální sítě](../virtual-network/virtual-networks-overview.md), [brány virtuální sítě](/vpn-gateway/vpn-gateway-site-to-site-create.md) a [virtuální sítě (klasické)](/virtual-network/virtual-networks-create-vnet-classic-pportal.md). Na druhé úrovni jsou podsítě a na třetí úrovni virtuální počítače, které patří do těchto podsítí. V pravém sloupci je aktuální stav skupiny zabezpečení sítě pro tyto prostředky, jak ukazuje následující příklad:

![Stav skupiny zabezpečení sítě v části topologie sítě](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Dolní část tohoto okna obsahuje doporučení pro tento virtuální počítač, podobně jako jsme už uváděli dříve. Kliknutím na doporučení získáte další informace nebo použijete potřebný ovládací prvek zabezpečení nebo konfiguraci zabezpečení.

### <a name="monitor-data"></a>Monitorování dat

Když kliknete na **SQL & Data** na dlaždici **Stav zabezpečení prostředků**, otevře se okno **Datové prostředky** s doporučeními pro SQL a službu Storage. Také obsahuje [doporučení](security-center-sql-service-recommendations.md) pro obecný stav databáze. Další informace o šifrování úložiště najdete v tématu [Povolení šifrování účtu úložiště Azure v Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Datové prostředky](./media/security-center-monitoring/security-center-monitoring-fig13-ga-new.png)

V části **Doporučení SQL** můžete kliknout na jakékoli doporučení a získáte další podrobnosti o další akci pro řešení problému. Následující příklad ukazuje rozbalení doporučení **Auditování databáze a detekce hrozeb u databází SQL**.

![Podrobnosti o doporučení SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

Okno **Povolit auditování a detekci hrozeb u databází SQL** obsahuje následující informace:

* Seznam databází SQL
* Server, na kterém jsou umístěné
* Informace o tom, zda bylo toto nastavení zděděno ze serveru nebo zda je v této databázi jedinečné
* Aktuální stav
* Závažnosti problému

Když kliknete na databázi, abyste vyřešili toto doporučení, otevře se okno **Auditování a detekce hrozeb**, jak je znázorněno na následující obrazovce.

![Okno Auditování a detekce hrozeb](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Pokud chcete povolit auditování, vyberte **Zapnuto** pod možností **Auditování**.

### <a name="monitor-applications"></a>Monitorování aplikací

Pokud má vaše úloha Azure aplikace, které jsou umístěné ve [virtuálních počítačích (vytvořených přes Azure Resource Managera)](../azure-resource-manager/resource-manager-deployment-model.md) se zpřístupněnými webovými porty (porty TCP 80 a 443), Security Center je může sledovat, aby se zjistily možné problémy zabezpečení a doporučily se kroky k nápravě. Když kliknete na dlaždici **Aplikace**, otevře se okno **Aplikace** s řadou doporučení v části **Doporučení pro aplikace**. Také ukazuje rozpis aplikací na hostitele nebo virtuální IP adresu, jak je znázorněno na následujícím snímku obrazovky.

![Stav zabezpečení aplikací](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Stejně jako u ostatních doporučení můžete kliknutím na doporučení zobrazit další podrobnosti o problému a způsobu řešení. Příklad uvedený na následujícím obrázku je aplikace, která byla identifikována jako nezabezpečená webová aplikace. Když vyberete aplikaci, která není považovaná za bezpečnou, otevře se další okno, kde je k dispozici následující možnost:

![Podrobnosti o aplikaci, která není zabezpečená](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Toto okno bude mít seznam všech doporučení pro tuto aplikaci. Pokud kliknete na doporučení **Přidat firewall webových aplikací**, otevře se okno **Přidat firewall webových aplikací** s možnostmi instalace firewallu webových aplikací (WAF) od partnera, jak je znázorněno na následujícím snímku obrazovky.

![Dialogové okno Přidat firewall webových aplikací](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## <a name="see-also"></a>Viz také
V tomto článku jste zjistili, jak ve službě Azure Security Center používat funkce sledování. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

* [Nastavení zásad zabezpečení ve službě Azure Security Center](security-center-policies.md): Zjistěte, jak se v Azure Security Center konfiguruje nastavení zabezpečení.
* [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md): Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
* [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md): Zjistěte, jak sledovat stav vašich partnerských řešení.
* [Časté otázky k Azure Security Center](security-center-faq.md): Přečtěte si nejčastější dotazy k používání této služby.
* [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/): Přečtěte si příspěvky o zabezpečení a dodržování předpisů Azure.

