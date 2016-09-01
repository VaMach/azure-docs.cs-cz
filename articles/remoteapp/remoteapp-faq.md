<properties 
    pageTitle="Časté otázky k Azure RemoteAppu | Microsoft Azure" 
    description="Přečtěte si odpovědi na časté otázky k Azure RemoteAppu." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# Časté otázky k Azure RemoteAppu

> [AZURE.IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).

K Azure RemoteAppu jste nám položili následující otázky. Máte další? Navštivte [fóra RemoteAppu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) a napište nám, co potřebujete vědět, nebo níže zanechte komentář.

## Nemůžete najít, co hledáte? Máte otázku, na kterou jste nenašli odpověď?
Pokud nemůžete najít potřebné informace nebo máte další otázku, kterou jste zde nenašli, navštivte prosím [fórum věnované Azure RemoteApp](http://aka.ms/araforum) a položte svou otázku tam. Do tohoto fóra pořád přidáváme další odpovědi.

## Co je Azure RemoteApp? ##


- **Co je Azure RemoteApp?** RemoteApp je služba Azure, která pomáhá poskytovat zabezpečený vzdálený přístup k aplikacím z mnoha různých uživatelských zařízení. Další informace o [Azure RemoteAppu](remoteapp-whatis.md).
- **Jaké jsou možnosti nasazení?** Existují dva typy kolekcí RemoteAppu: cloudová a hybridní. Který typ budete potřebovat, závisí na několika faktorech, například zda potřebujete připojení k doméně. O všech těchto rozhodujících faktorech mluvíme [zde](remoteapp-collections.md).

## Rychlé tipy k používání Azure RemoteAppu ##
- **Jak dlouho bude trvat, než mě odpojíte? Jak dlouho můžu být neaktivní, než mě odpojíte?** 4 hodiny. Pokud jste vy nebo jeden z vašich uživatelů budete neaktivní 4 hodiny, budete z Azure RemoteAppu automaticky odhlášeni. Podívejte se na další výchozí nastavení v části [Omezení a kvóty předplatného a služeb Azure](../azure-subscription-service-limits.md).
- **Můžu zkusit tuto službu zdarma?** Ano. 30 dní je k dispozici bezplatná zkušební verze. Po skončení zkušební doby můžete přejít na placený účet (který můžete použít v produkci) nebo službu přestat používat. Vyzkoušejte si bezplatnou verzi na stránkách [portal.azure.com](http://portal.azure.com) a vytvořte si novou instanci RemoteAppu. S bezplatnou zkušební verzí je možné vytvořit 2 instance RemoteAppu s 10 uživateli na každou instanci. Mějte na paměti, že tato zkušební verze funguje jen 30 dní.
## Podrobnosti o předplatném Azure RemoteAppu ##

- **Jaká má služba omezení?** Další informace o výchozím nastavení a omezení služby Azure RemoteApp najdete v části [Omezení a kvóty předplatného a služeb Azure](../azure-subscription-service-limits.md). Dejte nám vědět, pokud máte další dotazy.
- **Kolik uživatelů musím mít?** Minimálně 20 uživatelů. Ještě to zopakuji, aby to bylo naprosto jasné - MINIMUM je 20. Poplatky vám budou účtovány za 20 uživatelů. 
- **Kolik stojí RemoteApp?** Podívejte se na [Podrobnosti o cenách Azure RemoteAppu ](https://azure.microsoft.com/pricing/details/remoteapp/).
- **Stojí jeden typ kolekce víc než druhý?** Ano, může to tak být. Záleží na požadavcích vaší kolekce. Hybridní kolekce vyžaduje připojení z Azure RemoteAppu k místní síti. Pokud používáte stávající virtuální síť / síť ExpressRoute, je to bez dalších poplatků. Pokud ale používáte novou virtuální síť Azure a bránu nebo ExpressRoute, budou se vám účtovat poplatky za [VPN Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) nebo [ ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/). Tyto poplatky (podrobně v odkazech) se přičtou k měsíčním nákladům na Azure RemoteApp.

## Kolekce - co je podporováno, které byste měli používat a další
- **Jsou podporovány vlastní obchodní aplikace (LOB)?** Ano. Pokud v Azure RemoteAppu chcete používat vlastní aplikaci, vytvořte si [image vlastní šablony](remoteapp-create-custom-image.md) a nahrajte ho do kolekce RemoteAppu.
- **Budou mé vlastní obchodní aplikace fungovat v Azure RemoteAppu?** Nejlépe to zjistíte tak, že to vyzkoušíte. Podívejte se do [Centra pro kompatibilitu VP](http://www.rdcompatibility.com/compatibility/default.aspx).
- **Jaká metoda nasazení (cloudová nebo hybridní) je nejvhodnější pro mojí organizaci?** Hybridní kolekce poskytují nejúplnější prostředí, pokud chcete plnou integraci s jednotným přihlašováním (SSO) a zabezpečené místní připojení k síti. Cloudové kolekce poskytují agilní a snadný způsob izolace nasazení pomocí několika metod ověřování. Další informace o [možnostech nasazení](remoteapp-whatis.md).
- **Máme SQL nebo jinou databázi, buď místní nebo ve službě Azure. Který typ nasazení bychom měli použít?** To závisí na tom, kde se vaše databáze SQL nebo back-end nachází. Pokud se databáze nachází v privátní síti, použijte hybridní kolekci. Pokud má databáze přístup na internet a umožňuje připojení klientů, můžete použít cloudovou kolekci.
- **A co mapování jednotky USB a sériového portu, sdílení schránky a přesměrování tiskárny?** Všechny tyto funkce jsou v Azure RemoteAppu podporovány. Ve výchozím nastavení je sdílení schránky a přesměrování tiskárny povoleno. Další informace o přesměrování najdete [zde](remoteapp-redirection.md). 


## Image šablony
- **Můžu jako šablonu pro kolekci RemoteAppu použít cloud nebo existující virtuální počítač?** Ano! Můžete vytvořit image na základě virtuálního počítače Azure, použít image obsažený v předplatném nebo si vytvořit vlastní image. Podívejte se na [Možnosti imagů RemoteAppu](remoteapp-imageoptions.md).


## Možnosti sítě
- **Hybridní kolekce vyžaduje virtuální síť. Můžeme použít stávající virtuální síť?** Je to možné, pokud se jedná o virtuální síť Azure. Další informace naleznete v části „Krok 1: nastavení virtuální sítě“ v [pokynech k hybridní kolekci](remoteapp-create-hybrid-deployment.md).
- **Můžu virtuální síť použít s cloudovou kolekcí?** To opravdu můžete. Další informace najdete v části [Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md), zejména v kroku 1.

## Možnosti ověřování



- **A co ověřování? Které metody jsou podporovány?** Cloudová kolekce podporuje účty Microsoft a účty služby Azure Active Directory, což jsou také účty Office 365. Hybridní kolekce podporuje pouze účty Azure Active Directory, které byly synchronizovány (pomocí nástroje, jako například [Azure Active Directory Sync](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) z nasazení systému Windows Server Active Directory, a to s možností synchronizace hesel nebo s nakonfigurovanou službou Active Directory Federation Services (AD FS). Můžete také nakonfigurovat službu [Multi-Factor Authentication (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/).

>[AZURE.NOTE]Uživatelé Azure Active Directory musí pocházet z klienta, který je spojen s vaším předplatným. (Předplatné můžete zobrazit a upravit na kartě **Nastavení** v portálu. Další informace najdete v části [Změna klienta Azure Active Directory používaného RemoteAppem](remoteapp-changetenant.md).)

- **Proč nemůžu poskytnout přístup k účtu Azure Active Directory?** Uživatelé Azure Active Directory musí pocházet z adresáře, který je spojen s vaším předplatným. Tento adresář můžete zobrazit nebo upravit na kartě Nastavení v portálu. Další informace najdete v části [Změna klienta Azure Active Directory používaného RemoteAppem](remoteapp-changetenant.md).

## Klienti – které zařízení můžu použít pro přístup k Azure RemoteAppu?
Informace o funkčních klientech, včetně postupu pro instalaci různých klientů najdete v části [Přístup k aplikacím v Azure RemoteAppu](remoteapp-clients.md).

- **Jaká zařízení a operační systémy klientské aplikace podporují?**
Nejdříve počítače a tablety: 
    - Windows 10 (náhled klienta)
    - Windows 8.1 a Windows 8
    - Windows 7 Service Pack 1
    - Mac OS X
    - Windows RT
    - tablety Android
    - iPady a telefony:
    - iPhone
    - telefon se systémem Android
    - telefon se systémem Windows
 
    [Stáhněte](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) si klienta RemoteApp.
- **Podporuje Azure RemoteApp tenké klienty?** Ano, jsou podporovány následující tenké klienty systému Windows Embedded:
    - Windows Embedded Standard 7
    - Windows Embedded 8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **Která verze Windows Serveru je podporována u hostitele relací vzdálené plochy (RDSH)?** Windows Server 2012 R2.

## Podpora a zpětná vazba


- **Co je plán podpory pro RemoteApp?** Podpora pro správu fakturace a předplatného je k dispozici zdarma. Odborná pomoc je k dispozici prostřednictvím [plánů služby Azure](https://azure.microsoft.com/support/plans/). Bezplatnou podporu komunity můžete získat také na našem [diskusním fóru Azure](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp). 
- **Jak se odešlu svůj názor?** Navštivte [fórum názorů](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Kdo mi může sdělit další informace o Azure RemoteAppu?** Kromě [diskusního fóra](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp), které je skvělým místem pro zveřejňování dotazů, se můžete připojit k našemu týdennímu semináři [zeptejte se odborníka](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html), ve kterém probíráme všechny věci týkající se RemoteAppu.
- **A co dokumentace k RemoteAppu?** Jsme rádi, že jste se zeptali. Kromě nápovědy na portálu (stačí kliknout na **?** na libovolné stránce na portálu) jsou k dispozici následující články, ve kterých se o RemoteAppu dozvíte vše potřebné:
    - **Začínáme:**
        - [Co je RemoteApp?](remoteapp-whatis.md)
        - [Co jsou image šablony Remote Appu?](remoteapp-images.md)
        - [Jak funguje správa licencí?](remoteapp-licensing.md)
        - [Jak spolupracují aplikace RemoteApp a Office?](remoteapp-o365.md)
        - [Jak v RemoteAppu funguje přesměrování](remoteapp-redirection.md)?
    - **Nasazení:**
        - [Vytvoření vlastního image šablony](remoteapp-create-custom-image.md)
        - [Vytvoření hybridní kolekce](remoteapp-create-hybrid-deployment.md)
        - [Vytvoření cloudové kolekce](remoteapp-create-cloud-deployment.md)
        - [Konfigurace Azure Active Directory pro  RemoteApp](remoteapp-ad.md)
        - [Publikování aplikace v RemoteAppu](remoteapp-publish.md)
    - **Správa:**
        - [Přidání uživatelů](remoteapp-user.md)
        - [Doporučené postupy pro konfiguraci a použití RemoteAppu](remoteapp-bestpractices.md)  

    Videa! O  RemoteAppu máme také řadu videí. V některých naleznete úvod ([Úvod do Azure RemoteAppu](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), ostatní vás provedou nasazením ([cloudové nasazení](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) a [hybridní nasazení](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Podívejte se na ně!

 
### Umožněte nám, abychom vám mohli pomoct 
Věděli jste, že kromě hodnocení tohoto článku a přidání komentářů pod článkem také můžete měnit samotný článek? Něco chybí? Něco není v pořádku? Něco je matoucí? Vraťte se na začátek článku, klikněte na **Upravit na GitHubu** a proveďte změny – změny se nám odešlou ke kontrole a po jejich schválení je doplníme do článku.



<!--HONumber=Aug16_HO4-->


