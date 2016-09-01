<properties
    pageTitle="Používám Mobile Services, jak mi pomůže App Service?"
    description="Zjistěte, jaké výhody přináší služba App Service pro existující projekty Mobile Services."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/30/2016"
    ms.author="krisragh"/>

# <a name="getting-started"> </a>Používám Mobile Services, jak mi pomůže App Service?

##Přehled
Vaše stávající služba Mobile Service je v bezpečí a bude i nadále podporována. Je však několik výhod, které vaší mobilní aplikaci poskytuje platforma *Azure App Service* a které dnes v Mobile Services nejsou k dispozici:

- Jednodušší, snazší a nákladově efektivní nabídka pro aplikace, které obsahují jak webové, tak mobilní klienty
- Nové hostitelské funkce včetně webových úloh, vlastní záznamy CNAME, lepší monitorování
- Předpřipravená integrace s nástrojem Traffic Manager
- Připojení k lokálním prostředkům a sítím VPN pomocí virtuální sítě a hybridních připojení
- Monitorování, výstrahy a řešení potíží pro aplikace pomocí nástrojů NewRelic a AppInsights
- Širší spektrum využívaných výpočetních prostředků a cenových úrovní
- Integrované automatické škálování, vyrovnávání zatížení a monitorování výkonu
- Integrované funkce pro fázování, zálohování, vrácení zpět a testování za provozu

## Nové hostitelské funkce
V *Azure App Service* běží back-end kód *mobilní aplikace* ve stejném kontejneru jako webová aplikace a aplikace API. Díky tomu můžete využít všechny funkce tohoto kontejneru, včetně některých funkcí, které nejsou v tuto chvíli dostupné v Mobile Services:

- Přidání neustále běžící back-end logiky přes webové úlohy
- Zajištění, že back-end kód bude vždy spuštěn
- Použití vlastních záznamů CNAME, aby byly k dispozici popisné a stabilní názvy koncových bodů mobilního back-endu
- Geografické škálování aplikace pomocí nástroje Traffic Manager
- Zahrnutí jakýchkoli knihoven nebo balíčků, které chcete použít
- (Pro .NET) Využití jakékoli funkce technologie ASP.NET, včetně MVC
- (Pro Node.js) Využití jakékoli knihovny čistého JavaScriptu ekosystému Node, včetně běžných knihoven MVC

##Přístup k lokálním datům přes virtuální síť
Již dnes můžete pomocí Mobile Services používat hybridní připojení a připojovat se s nimi k lokálním prostředkům. Existují však situace, kdy je lepší použít řešení se sítí VPN. S *Azure App Services* můžete pro back-end mobilní aplikace použít Azure VNet.

##Využití oblíbeného jazyka pro back-end
Služba *Azure App Service* nabízí širší a bohatší podporu platforem ASP.NET a Node.js, včetně přístupu k nejnovějším modulům runtime.

##Nastavení automatického škálování
S Mobile Services běžely všechny instance back-end kódu na malých virtuálních počítačích. Služba *Azure App Services* umožňuje vybrat velikost virtuálního počítače z mnohem širší nabídky. Kromě toho je možné podle různých metrik výkonu rychle změnit škálování (vertikálně nebo horizontálně navýšit kapacitu), abyste zvládali zátěž vyplývající z příchozích zákaznických požadavků.

##Neustálý přehled
Na problémy dokážete reagovat v reálném čase díky monitorování a výstrahám, které vám a vašemu týmu automaticky odešlou oznámení. Integrací pokročilé analýzy aplikací a monitorování funkcí z nástrojů New Relic a AppInsights získáte ještě podrobnější informace o výkonu mobilní aplikace. S *Azure App Service* nyní můžete nastavit upozornění na základě různých metrik výkonu, a to buď programově, nebo přes Portál Azure.

##Zabezpečení prostředků
Back-end a databáze je možné automaticky zálohovat. Kód a data jsou zabezpečeny proti havárii a lze je snadno obnovit. Díky tomu můžete řídit svou firmu s jistotou.

##Připravit, naplánovat, start!
S *Azure App Service* je nyní možné pro své mobilní aplikace vytvořit několik privátních testovacích a přípravných prostředí. Můžete je využít k otestování aplikací předtím, než je nasadíte. Do ostrého provozu je možné přejít zcela bez výpadku. Webové aplikace se načítají předem – díky tomu mají zákazníci maximální pohodlí.

Výhody *App Service* pro existující Mobile Service můžete začít využívat tak, že si projdete tento [kurz](app-service-mobile-migrating-from-mobile-services.md).




<!---HONumber=Aug16_HO4-->


