<properties
    pageTitle="Monitorujte místní infrastrukturu identity v cloudu."
    description="Toto je stránka o službě Azure AD Connect Health. Najdete tu popis této služby a důvody, proč ji používat."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="femila"
    editor="karavar"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="vakarand"/>


# Monitorujte místní infrastrukturu identity a synchronizačních služeb v cloudu.

Služba Azure AD Connect Health pomáhá monitorovat místní infrastrukturu identity a synchronizační služby a lépe proniknout do jejich funkce.  Umožňuje udržovat spolehlivé propojení s Office 365 a službami Microsoft Online Services tím, že zajišťuje monitorování klíčových komponent identity, jako jsou servery služby AD FS, Azure AD Connect (neboli synchronizační stroj), řadiče domény služby Active Directory atd. Také udržuje klíčové datové body těchto komponent dobře přístupné, takže lze snadno získávat přehled o jejich používání a dalších důležitých statistikách.

Další informace najdete v článku [Portál služby Azure AD Connect Health](https://aka.ms/aadconnecthealth). Na portálu služby Azure AD Connect Health můžete zobrazovat upozornění, monitorovat výkon, analýzu využívání a spoustu dalších věcí. Azure AD Connect Health umožňuje mít na jednom místě a v jediném přehledu informace o stavu klíčových komponent identity.

![Co je Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

Budoucí aktualizace služby Azure AD Connect Health budou obsahovat další funkce monitorování a přehledy o dalších komponentách identity. Díky tomu pomocí jediného panelu přehledu identity získáte ještě rozsáhlejší, kvalitnější a propojenější prostředí, které vaši uživatelé budou moci využít ke zvýšení efektivity své práce.

<!-- <center>![What is Azure AD Connect Health](./media/active-directory-aadconnect-health/logo1.png)</center> -->

## Proč používat službu Azure AD Connect Health

Integrace místních adresářů se službou Azure AD zvyšuje produktivitu uživatelů tím, že jim poskytuje společnou identitu pro přístup ke cloudovým i místním prostředkům. Spolu s touto integrací ale přicházejí i další výzvy. Je třeba zajistit, aby toto prostředí bylo v pořádku a uživatelé měli spolehlivý přístup k místním i cloudovým prostředkům z jakéhokoli zařízení. Azure AD Connect Health zajišťuje snadný, na cloudu založený přístup k monitorování a získávání přehledu o místní infrastruktuře identity, která se používá k přístupu k Office 365 nebo k jiným aplikacím služby Azure AD. Stačí jednoduše nainstalovat agenta na každý z vašich místních serverů identity.

## [Azure AD Connect Health pro službu AD FS](active-directory-aadconnect-health-adfs.md)

Azure AD Connect Health pro službu AD FS podporuje služby AD FS 2.0 v systému Windows Server 2008 R2 a AD FS v systému Windows Server 2012 a Windows Server 2012 R2. To zahrnuje také proxy servery služby AD FS nebo proxy servery služby Web Apps, které podporují ověřování pro přístup z extranetu. Spolu se snadnou a ekonomickou instalací agenta stavu poskytuje Azure AD Connect Health pro službu AD FS následující sadu klíčových funkcí:

- monitorování s výstrahami, které oznamují, pokud některé servery služby AD FS nebo proxy servery služby AD FS nejsou v pořádku
- e-mailová oznámení pro kritické výstrahy
- zobrazení trendů v datech o výkonu – vhodné pro plánování kapacity služby AD FS
- analytické funkce pro analýzy využití, které monitorují různá přihlášení do služby AD FS (aplikace, uživatelé, umístění v síti atd.), umožňující snadno pochopit, jak je služba AD FS využívána
- sestavy pro službu AD FS, například 50 uživatelů s největším počtem chybně zadaných kombinací uživatelského jména a hesla

Následující video přináší přehled služby Azure AD Connect Health pro službu AD FS

>[AZURE.VIDEO azure-ad-connect-health--monitor-you-identity-bridge]

## [Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)

Služba Azure AD Connect Health pro synchronizaci monitoruje a poskytuje informace o synchronizacích, ke kterým dochází mezi místní službou Active Directory a službou Azure Active Directory. Služba Azure AD Connect Health pro synchronizaci poskytuje následující sadu klíčových funkcí:

- monitorování s výstražnými upozorněními v případě, že servery Azure AD Connect neboli synchronizační stroj nejsou v pořádku
- e-mailová oznámení pro kritické výstrahy
- synchronizace statistik provozu včetně přehledů latencí pro synchronizační operace a trendy v synchronizačních operacích, jako jsou přidání, aktualizace nebo odstranění
- rychlý přehled informací o vlastnostech synchronizace a posledním úspěšném exportu do služby Azure AD

Následující video přináší přehled služby Azure AD Connect Health pro synchronizaci.

>[AZURE.VIDEO azure-active-directory-connect-health-monitoring-the-sync-engine]

## [Azure AD Connect Health pro službu AD DS (Preview)](active-directory-aadconnect-health-adds.md)

Azure AD Connect Health pro službu AD FS podporuje monitorování řadičů domény instalovaných v systémech Windows Server 2008 R2, Windows Server 2012 a Windows Server 2012 R2. Snadná a nenákladná instalace agenta stavu vám umožní monitorovat místní prostředí AD DS přímo z cloudu. Služba Azure AD Connect Health pro AD DS poskytuje následující sadu klíčových funkcí:

- Monitorování výstrah, aby se zjistilo, kdy řadiče domény nejsou v pořádku, společně s e-mailovými oznámeními pro kritické výstrahy.
- Řídicí panel Řadiče domény, který poskytuje rychlý přehled kondice a provozního stavu řadičů domény.
- Řídicí panel Stav replikace s nejnovějšími informacemi o replikaci, spolu s odkazy na průvodce odstraňováním potíží při zjištění chyb.
- Rychlý přístup odkudkoli ke grafům s daty o výkonu oblíbených čítačů výkonu, které jsou nezbytné pro účely monitorování a řešení potíží.

Následující video přináší přehled služby Azure AD Connect Health pro službu AD DS.

>[AZURE.VIDEO azure-ad-connect-health-monitors-on-premises-ad-domain-services]

## Začínáme se službou Azure AD Connect Health
Se službou Azure AD Connect Health je velmi snadné začít. Postupujte následovně:

1. [Získejte předplatné Azure AD Premium](active-directory-get-started-premium.md) nebo [začněte se zkušební verzí](https://azure.microsoft.com/trial/get-started-active-directory/).

2. [Stáhněte a nainstalujte agenty služby Azure AD Connect Health](#download-and-install-azure-ad-connect-health-agent) na servery identity.

3. Prohlédněte si řídicí panel služby Azure AD Connect Health na adrese [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth)

>[AZURE.NOTE]Nezapomeňte, že než na řídicím panelu služby Azure AD Connect Health uvidíte nějaká data, bude třeba na cílové servery nainstalovat agenty služby Azure AD Connect Health.

## Stažení a instalace agenta služby Azure AD Connect Health

- Přečtěte si [Požadavky](active-directory-aadconnect-health-agent-install.md#Requirements) pro službu Azure AD Connect Health

- Pokud chcete začít používat službu Azure AD Connect Health pro službu AD FS, můžete si nejnovější verzi agenta stáhnout pomocí následujícího odkazu: [Stáhnout agenta služby Azure AD Connect Health pro službu AD FS.](http://go.microsoft.com/fwlink/?LinkID=518973)
[](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs)

- Pokud chcete začít používat službu Azure AD Connect Health pro synchronizaci, stáhněte a nainstalujte si [nejnovější verzi Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771).  Agent stavu bude nainstalován jako součást instalace služby Azure AD Connect (verze 1.0.9125.0 nebo vyšší).  Azure AD Connect podporuje místní upgrade z předchozích verzí.

- Pokud chcete začít používat službu Azure AD Connect Health pro službu AD DS, můžete si nejnovější verzi agenta stáhnout pomocí následujícího odkazu: [Stáhnout agenta služby Azure AD Connect Health pro službu AD DS.](http://go.microsoft.com/fwlink/?LinkID=820540)
[](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).

## Portál služby Azure AD Connect Health
Portál služby Azure AD Connect Health umožňuje zobrazovat upozornění, monitorovat výkon a analýzy využívání. Hlavního okno služby Azure AD Connect Health najdete na následující adrese: https://aka.MS/aadconnecthealth.  Je to v podstatě hlavní obrazovka této služby. V hlavním okně uvidíte možnosti rychlého startu, služeb v rámci Azure AD Connect Health a další možnosti konfigurace. Pod snímkem obrazovky následuje stručné vysvětlení každé z nich.  Jakmile nasadíte agenty, služba stavu se automaticky identifikuje pro služby, které Azure AD Connect Health monitoruje.

![Portál služby Azure AD Connect Health](./media/active-directory-aadconnect-health/portal4.png)

- **Rychlý start** – Pokud vyberete tuto možnost, otevře se okno rychlého startu. Tady budete moci výběrem možnosti Získat nástroje stáhnout agenta služby Azure AD Connect Health. Dále tu budete mít přístup k dokumentaci a budete moci poskytnout zpětnou vazbu.

- **Služba AD FS (Active Directory Federation Services)** – reprezentuje všechny služby AD FS, které aktuálně monitoruje služba Azure AD Connect Health. Pokud vyberete jednu z instancí, otevře se okno s informacemi o instanci dané služby.  Tyto informace zahrnují přehled, vlastnosti, výstrahy, monitorování a analýzu využití. Další informace o možnostech najdete [tady.](active-directory-aadconnect-health-adfs.md)

- **Azure Active Directory Connect (Sync)** – reprezentuje vaše servery Azure AD Connect, které aktuálně monitoruje služba Azure AD Connect Health. Jakmile vyberete položku, otevře se okno s informacemi o vašich serverech Azure AD Connect. Další informace o možnostech najdete [tady.](active-directory-aadconnect-health-sync.md)
 
- **Active Directory Domain Services** – reprezentuje všechny doménové struktury AD DS, které aktuálně monitoruje služba Azure AD Connect Health. Pokud vyberete jednu z doménových struktur, otevře se okno s informacemi o instanci dané doménové struktuře.  Tyto informace zahrnují přehled základních informací, řídicí panel Řadiče domény, řídicí panel Stav replikace, výstrahy a monitorování. Další informace o možnostech najdete [tady](active-directory-aadconnect-health-adds.md).

- **Konfigurovat** – umožňuje vám zapnout nebo vypnout následující možnosti:

    1. Funkce pro automatické aktualizace automaticky aktualizuje agenta služby Azure AD Connect Health na nejnovější verzi. To znamená, že aktualizace na nejnovější verzi automaticky proběhne pokaždé, když bude taková verze agenta služby Azure AD Connect Health k dispozici. Tato možnost je ve výchozím nastavení zapnutá.

    2. Umožňuje Microsoftu, aby výhradně pro případ řešení problémů získával přístup k datům o stavu adresáře Azure AD. To znamená, že pokud je tato možnost povolena, Microsoft bude moci zobrazit stejná data, která vidíte vy. To může pomoci při řešení potíží a problémů. Tato možnost je ve výchozím nastavení zakázána.


## Související odkazy

* [Instalace agenta služby Azure AD Connect Health](active-directory-aadconnect-health-agent-install.md)
* [Operace služby Azure AD Connect Health](active-directory-aadconnect-health-operations.md)
* [Používání služby Azure AD Connect Health se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Používání služby Azure AD Connect Health pro synchronizaci](active-directory-aadconnect-health-sync.md)
* [Používání služby Azure AD Connect Health se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect Health – nejčastější dotazy](active-directory-aadconnect-health-faq.md)
* [Historie verzí služby Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)



<!--HONumber=Sep16_HO4-->


