<properties
   pageTitle="Sledování stavu zabezpečení v Azure Security Center | Microsoft Azure"
   description="Tento dokument vám pomůže začít pracovat s funkcemi sledováním v Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="yurid"/>

#Sledování stavu zabezpečení v Azure Security Center
Tento dokument vám pomůže používat možnosti sledování v Azure Security Center k monitorování dodržování zásad.

##Co je sledování stavu zabezpečení?
Pod pojmem sledování si často představujeme pozorování a čekání, až se stane nějaká událost, abychom na situaci reagovali. Sledování zabezpečení odkazuje na používání proaktivní strategie, kdy se auditují vaše prostředky a identifikují ty systémy, které nesplňují organizační standardy nebo osvědčené postupy.

##Sledování stavu zabezpečení
Po povolení [zásad zabezpečení](security-center-policies.md) pro prostředky předplatného bude služba Security Center analyzovat zabezpečení vašich prostředků, aby identifikovala potenciální ohrožení zabezpečení.  Informace o konfiguraci sítě jsou k dispozici okamžitě, ale získání informací o konfiguraci virtuálního počítače, jako je stav aktualizace zabezpečení nebo konfigurace operačního systému, může trvat hodinu i víc. V okně **Resource Security Health** (Stav zabezpečení prostředků) můžete zobrazit stav zabezpečení svých prostředků spolu s případnými problémy. Můžete také zobrazit seznam těchto problémů v okně **Doporučení**.

Další informace o tom, jak používat doporučení, najdete v tématu [Implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md).

Na dlaždici **Stav zabezpečení prostředků** můžete sledovat stav zabezpečení svých prostředků. V následujícím příkladu uvidíte řadu problémů vysoké a střední závažnosti, které vyžadují pozornost. Zásady zabezpečení, které jsou povolené, budou mít vliv na typy ovládacích prvků, které jsou monitorovány.

![Stav prostředků](./media/security-center-monitoring/security-center-monitoring-fig1-ga.png)

Pokud Security Center identifikuje ohrožení zabezpečení, které je třeba řešit, jako je například virtuální počítač s chybějící aktualizací zabezpečení nebo podsíť bez [skupiny zabezpečení sítě](../virtual-network/virtual-networks-nsg.md), budou zde uvedené.

###Monitorování virtuálních počítačů
Po kliknutí na **Virtuální počítače** na dlaždici **Stav zabezpečení prostředků** se okno **Virtuální počítače** otevře s dalšími podrobnostmi týkajícími se připojování a preventivních kroků, jakož i seznam všech virtuálních počítačů, které jsou monitorovány pomocí služby Security Center, jak je uvedeno níže.

![Chybějící aktualizace systému podle virtuálních počítačů](./media/security-center-monitoring/security-center-monitoring-fig2-ga.png)

- Kroky připojování
- Doporučení pro virtuální počítače
- Virtual Machines

V každé části můžete vybrat jednotlivé možnosti, abyste zobrazili další podrobnosti týkající se doporučeného kroku k vyřešení tohoto problému. Dále uvedené části budou zahrnovat tyto oblasti podrobněji.

#### Doporučení pro monitorování
Tato část uvádí celkový počet virtuálních počítačů, které byly inicializovány pro shromažďování dat, a jejich aktuální stav. Jakmile je u všech virtuálních počítačů shromažďování dat inicializováno, budou připravené přijímat zásady zabezpečení služby Security Center. Po kliknutí na tuto položku se otevře okno **Stav instalace shromažďování dat** a bude moct zobrazit názvy virtuálních počítačů a ve sloupci **Stav instalace** aktuální stav shromažďování dat, jak je uvedeno dále.

![Stav inicializace](./media/security-center-monitoring/security-center-monitoring-fig3-ga.png)


####Doporučení pro virtuální počítače
Tato část obsahuje sadu doporučení pro každý virtuální počítač monitorovaný pomocí služby Azure Security Center. První sloupec obsahuje doporučení, druhý sloupec celkový počet virtuálních počítačů, které jsou ovlivněné tímto doporučením, a třetí sloupec zobrazuje závažnost problému, jak je uvedeno dále.

![Doporučení pro virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig4-ga.png)

> [AZURE.NOTE] V seznamu Síťová topologie v okně Networking Health (Stav sítě) se zobrazí pouze virtuální počítače s alespoň jedním veřejným koncovým bodem.

Každé doporučení obsahuje sadu akcí, které lze provést, když na ni kliknete. Pokud například kliknete na **Missing system updates** (Chybějící aktualizace systému), otevře se okno **Missing system updates** (Chybějící aktualizace systému). Uvádí seznam virtuálních počítačů, kterým chybí opravy, a závažnost chybějící aktualizace, jak je uvedeno dále.

![Chybějící aktualizace systému](./media/security-center-monitoring/security-center-monitoring-fig5-ga.png)

Okno **Missing system updates** (Chybějící aktualizace systému) zobrazí tabulku s následujícími informacemi:

- **VIRTUAL MACHINE** (Virtuální počítač): Název virtuálního počítače, kterému chybí aktualizace.
- **SYSTEM UPDATES** (Aktualizace systému): Počet aktualizací systému, které chybí.
- **LAST SCAN TIME** (Čas poslední kontroly): Čas, kdy služba Security Center naposledy hledala ve virtuálním počítači aktualizace.
- **STATE** (Stav): Aktuální stav doporučení:
    - **Open** (Otevřené): Doporučení dosud nebylo řešeno.
    - **In Progress** (Probíhá): Doporučení se aktuálně na tyto prostředky používá, není třeba provádět žádnou akci.
    - **Resolved** (Vyřešeno): Doporučení již bylo dokončeno (pokud byl tento problém vyřešen, položka je zobrazena šedě).
- **SEVERITY** (Závažnost): Popisuje závažnost tohoto konkrétního doporučení:
    - **High** (Vysoká): Ohrožení zabezpečení existuje u významného prostředku (aplikace, virtuální počítač, skupina zabezpečení sítě) a vyžaduje pozornost.
    - **Medium** (Střední): Nekritické nebo další kroky potřebné k dokončení procesu nebo odstranění ohrožení.
    - **Low** (Nízká): Ohrožení zabezpečení by se mělo řešit, ale nevyžaduje okamžitou pozornost. (Ve výchozím nastavení nejsou doporučení s nízkou závažností uváděny, ale pokud je chcete zobrazit, je možné je vyfiltrovat.)

Chcete-li zobrazit podrobnosti o doporučení, klikněte na název virtuálního počítače. Otevře se pro tento virtuální počítač nové okno se seznamem aktualizací, jak je uvedeno dále.

![Chybějící aktualizace systému na virtuální počítač](./media/security-center-monitoring/security-center-monitoring-fig6-ga.png)

> [AZURE.NOTE] Bezpečnostní doporučení uvedená tady jsou stejná jako v okně Doporučení. Další informace o řešení doporučení najdete v tématu [Implementace doporučení zabezpečení v Azure Security Center](security-center-recommendations.md). Týká se to nejen virtuálních počítačů, ale všech prostředků, které jsou k dispozici na dlaždici Stav prostředku.

####Část virtuálních počítačů
Část virtuálních počítačů poskytuje přehled všech virtuálních počítačů a doporučení. Každý sloupec představuje jednu sadu doporučení, jak je uvedeno dále:

![Virtuální počítače](./media/security-center-monitoring/security-center-monitoring-fig7-ga.png)

Ikona, která se zobrazí pod každým doporučením, pomáhá rychle zjistit, které virtuální počítače vyžadují pozornost a o jaký typ doporučení se jedná.

V dříve uvedeném příkladu má jeden virtuální počítač kritické doporučení týkající se ochrany koncových bodů. Abyste získali další informace o virtuálním počítači, klikněte na něj. Otevře se nové okno, které představuje tento virtuální počítač, jak je uvedeno dále.

![Podrobnosti zabezpečení virtuálního počítače](./media/security-center-monitoring/security-center-monitoring-fig8-ga.png)

Toto okno obsahuje podrobné informace o zabezpečení pro virtuální počítač. V dolní části tohoto okna se zobrazí doporučená akce a závažnost jednotlivých problémů.

#### Oddíl Cloud Services (Preview)
Stav pro cloudové služby je součástí dlaždice se stavem zabezpečení virtuálních počítačů. Pokud je verze operačního systému zastaralá, vytvoří se doporučení, jak je vidět dál: 

![Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new2.png)

Je třeba postupovat podle kroků v doporučení a aktualizovat verzi operačního systému. Například když kliknete na červenou výstrahu v jedné z webových rolí (spouští Windows Server s vaší webovou aplikací automaticky nasazenou do služby IIS) nebo rolí pracovního procesu (spouští Windows Server s vaší webovou aplikací automaticky nasazenou do služby IIS), otevře se nové okno s dalšími podrobnostmi týkajícími se tohoto doporučení, jak je vidět dál:

![Podrobnosti cloudových služeb](./media/security-center-monitoring/security-center-monitoring-fig8-new3.png) 

Chcete-li zobrazit podrobnější vysvětlení týkající se tohoto doporučení, klikněte na **Aktualizovat verzi operačního systému** ve sloupci **Popis**. Otevře se okno **Aktualizovat verzi operačního systému (Preview)** s dalšími podrobnostmi.

![Doporučení pro Cloud Services](./media/security-center-monitoring/security-center-monitoring-fig8-new4.png)  

### Monitorování virtuálních sítí
Po kliknutí na **Sítě** na dlaždici **Stav zabezpečení prostředků** se otevře okno **Sítě** s dalšími podrobnostmi, jak je uvedeno dále:

![Sítě](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

####Doporučení pro sítě

Podobně jako u informací o stavu prostředků virtuálních počítačů, toto okno obsahuje v horní části okna souhrnný seznam problémů a v dolní části seznam monitorovaných sítí.

Část rozpisu stavu sítí uvádí možné problémy zabezpečení a nabízí doporučení. Možné problémy mohou zahrnovat:

- Není nainstalována brána firewall příští generace (NGFW).
- Nejsou povolené skupiny zabezpečení sítě (NSG) v podsítích.
- Nejsou povolené skupiny zabezpečení sítě (NSG) na virtuálních počítačích.
- Omezený externí přístup prostřednictvím veřejného externího koncového bodu.
- Stav internetových koncových bodů.

Po kliknutí na některé z těchto doporučení se otevře nové okno s dalšími podrobnostmi týkajícími se doporučení, jak je znázorněno v následujícím příkladu.

![Omezení koncového bodu](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

V tomto příkladu obsahuje okno **Configure Missing Network Security Groups for Subnets** (Nakonfigurovat chybějící skupiny zabezpečení sítě pro podsítě) seznam podsítí a virtuálních počítačů, kde chybí ochrana NSG. Pokud kliknete na podsíť, kde chcete použít NSG, otevře se další okno.

V okně **Zvolit skupinu zabezpečení sítě** vyberete nejvhodnější skupinu zabezpečení sítě pro podsíť nebo můžete vytvořit novou skupinu zabezpečení sítě. 

####Část internetových koncových bodů

V části **Internetové koncové body** uvidíte virtuální počítače, které jsou aktuálně nakonfigurované s internetovým koncovým bodem, a jeho aktuální stav.

![Internetový koncový bod](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Tato tabulka obsahuje název koncového bodu, který představuje virtuální počítač, internetovou IP adresu, aktuální stav závažnosti NSG a NGFW. Tabulka je řazená podle závažnosti, jak je popsáno dále:
- Červené (nahoře): vysoká priorita, mělo by se řešit okamžitě 
- Oranžová: střední priorita, mělo by se řešit co nejdříve
- Zelená (poslední): stav

####Část topologie sítě

Část **Topologie sítě** obsahuje hierarchické zobrazení prostředků, jak je uvedeno dále:

![Topologie sítě](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Tabulka je řazená podle závažnosti (virtuální počítače a podsítě), jak je popsáno dále:
- Červené (nahoře): vysoká priorita, mělo by se řešit okamžitě 
- Oranžová: střední priorita, mělo by se řešit co nejdříve
- Zelená (poslední): stav

V tomto zobrazení topologie obsahuje první úroveň položky [Virtuální sítě](../virtual-network/virtual-networks-overview.md), [Brány virtuální sítě](../vpn-gateway/vpn-gateway-site-to-site-create.md) a [Virtuální síť (klasická)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Na druhé úrovni jsou podsítě a na třetí úrovni virtuální počítače, které patří do těchto podsítí. V pravém sloupci je aktuální stav skupiny zabezpečení sítě (NSG) pro tyto prostředky, jak ukazuje následující příklad:

![Strom sítě](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Dolní část toto okna obsahuje doporučení pro tento virtuální počítač, podobně jako dříve. Kliknutím na doporučení získáte další informace nebo použijete potřebný ovládací prvek nebo konfiguraci zabezpečení.

###Monitorování prostředků SQL
Když kliknete na **SQL** na dlaždici **Stav zabezpečení prostředků**, otevře se okno SQL s doporučeními pro problémy, jako je nepovolení auditování nebo transparentního šifrování dat. Také obsahuje doporučení pro obecný stav databáze.

![Stav prostředků SQL](./media/security-center-monitoring/security-center-monitoring-fig13-ga.png)

Můžete kliknout na kterékoli z těchto doporučení a získat podrobnosti o další akci k řešení problému. Následující příklad ukazuje rozbalení doporučení **Auditování databáze není povoleno**.

![Stav prostředků SQL](./media/security-center-monitoring/security-center-monitoring-fig14-ga.png)

Okno **Enable Auditing on SQL databases** (Povolit auditování databází SQL) obsahuje následující informace:

- Seznam databází SQL
- Server, na kterém jsou umístěné
- Informace o tom, zda bylo toto nastavení zděděno ze serveru nebo zda je v této databázi jedinečné
- Aktuální stav
- Závažnosti problému

Když kliknete na databáze, abyste vyřešili toto doporučení, otevře se okno **Auditování a detekce hrozeb**, jak je uvedeno dále.

![Stav prostředků SQL](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Chcete-li povolit auditování, jednoduše vyberte **Zapnuto** pod možností **Auditování**.

### Monitorování aplikací

Pokud má vaše úloha Azure aplikace, které jsou umístěné ve [virtuálních počítačích s Resource Managerem](../resource-manager-deployment-model.md) s odhalenými webovými porty (porty TCP 80 a 443), Security Center je může sledovat, aby se identifikovaly možné problémy zabezpečení a doporučené kroky k nápravě. Když kliknete na dlaždici **Aplikace**, otevře se okno **Aplikace** s řadou doporučení v části preventivních kroků. Také ukazuje rozpis aplikací na hostitele nebo virtuální IP adresu, jak je uvedeno dále.

![Stav zabezpečení aplikací](./media/security-center-monitoring/security-center-monitoring-fig16-ga.png)

Stejně jako u ostatních doporučení můžete kliknutím zobrazit další podrobnosti o problému a způsobu řešení. Příklad uvedený na následujícím obrázku je aplikace, která byla identifikována jako nezabezpečená webová aplikace. Když vyberete aplikaci, která není považovaná za bezpečnou, otevře se další okno, kde je k dispozici následující možnost:

![Aplikace](./media/security-center-monitoring/security-center-monitoring-fig17-ga.png)

Toto okno bude mít seznam všech doporučení pro tuto aplikaci. Pokud kliknete na doporučení **Přidat firewall webových aplikací**, otevře se okno **Přidat firewall webových aplikací**s možnostmi instalace firewallu webových aplikací (WAF) od jiných dodavatelů, jak je uvedeno dál.

![Přidat firewall webových aplikací](./media/security-center-monitoring/security-center-monitoring-fig18-ga.png)

## Viz také
V tomto dokumentu jste zjistili, jak ve službě Azure Security Center používat funkce sledování. Pokud se o službě Azure Security Center chcete dozvědět víc, pročtěte si tato témata:

- [Nastavení zásad zabezpečení v Azure Security Center](security-center-policies.md) – Zjistěte, jak se v Azure Security Center konfigurují zásady zabezpečení.
- [Správa a zpracování výstrah zabezpečení v Azure Security Center](security-center-managing-and-responding-alerts.md) – Zjistěte, jak spravovat výstrahy zabezpečení a reagovat na ně.
- [Sledování partnerských řešení pomocí Azure Security Center](security-center-partner-solutions.md) – Zjistěte, jak pomocí Azure Security Center sledovat stav vašich partnerských řešení.
- [Azure Security Center – nejčastější dotazy](security-center-faq.md) – Přečtěte si nejčastější dotazy o použití této služby.
- [Blog o zabezpečení Azure](http://blogs.msdn.com/b/azuresecurity/) – Přečtěte si příspěvky o zabezpečení Azure a dodržování předpisů.



<!--HONumber=Aug16_HO4-->


