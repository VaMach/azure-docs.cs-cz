---
title: "Řešení potíží s Application Insights ve webovém projektu Java"
description: "Průvodce odstraňováním potíží s – monitorování provozu aplikace v jazyce Java pomocí Application Insights."
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: ef602767-18f2-44d2-b7ef-42b404edd0e9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: mbullwin
ms.openlocfilehash: 5a729139e122693b4199607919c876bda45fd4b5
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/01/2017
---
# <a name="troubleshooting-and-q-and-a-for-application-insights-for-java"></a>Řešení potíží a otázky a odpovědi v nástroji Application Insights
Dotazy nebo problémy s [Azure Application Insights v jazyce Java][java]? Zde jsou některé tipy.

## <a name="build-errors"></a>Chyby sestavení
**V prostředí Eclipse, při přidání Application Insights SDK prostřednictvím Maven nebo Gradle zobrazuje se chyby ověření sestavení nebo kontrolního součtu.**

* Pokud závislost <version> element je pomocí vzoru se zástupnými znaky (například (Maven) `<version>[1.0,)</version>` nebo (Gradle) `version:'1.0.+'`), zkuste místo toho zadat konkrétní verzi jako `1.0.2`. Najdete v článku [poznámky k verzi](https://github.com/Microsoft/ApplicationInsights-Java#release-notes) na nejnovější verzi.

## <a name="no-data"></a>Žádná data
**I přidat Application Insights úspěšně a spustil mé aplikace, ale nikdy, uloží se data na portálu.**

* Počkejte několik minut a klikněte na tlačítko Aktualizovat. Pravidelně grafy sami obnovit, ale můžete taky aktualizovat ručně. Interval aktualizace závisí na časové rozmezí grafu.
* Zkontrolujte, zda máte klíč instrumentace definované v souboru ApplicationInsights.xml (ve složce prostředky ve vašem projektu)
* Ověřte, zda je žádné `<DisableTelemetry>true</DisableTelemetry>` uzlu v souboru xml.
* V bráně firewall můžete chtít otevřít porty TCP 80 a 443 pro odchozí přenosy na adresu dc.services.visualstudio.com. Najdete v článku [úplný seznam výjimky brány firewall](app-insights-ip-addresses.md)
* Ve službě Microsoft Azure spustit Tabule, podívejte se na mapě služby stavu. Pokud jsou některé výstrahy indikace, počkejte, dokud se změnil na OK a pak zavřete a znovu otevřete okně vaší aplikace Application Insights.
* Povolení protokolování v okně konzoly IDE přidáním `<SDKLogger />` prvek v rámci kořenového uzlu souboru ApplicationInsights.xml (ve složce prostředky ve vašem projektu) a zkontrolujte položky, kterými [Chyba].
* Ujistěte se, že správný soubor ApplicationInsights.xml byl úspěšně načten Java SDK prohlížením výstup zprávy v konzole pro příkaz "konfigurační soubor byl úspěšně nalezl".
* Pokud není nalezen konfigurační soubor, zkontrolujte zprávy výstup zobrazíte, kde má být vyhledán do konfiguračního souboru pro a ujistěte se, že soubor ApplicationInsights.xml nachází v jednom z těchto umístění vyhledávání. Jako existuje pravidlo můžete umístit do konfiguračního souboru téměř Application Insights SDK JARs. Příklad: v Tomcat, to znamená složce webové-INF/lib.

#### <a name="i-used-to-see-data-but-it-has-stopped"></a>Mohu použít chcete zobrazit data, ale byla zastavena
* Zkontrolujte [stav blog](http://blogs.msdn.com/b/applicationinsights-status/).
* Jste nedosáhli vaše měsíční kvóta datových bodů? Otevřete nastavení nebo kvóty a cena chcete zjistit. Pokud ano, můžete upgradovat plán nebo platit dodatečnou kapacitu. Najdete v článku [ceny schéma](https://azure.microsoft.com/pricing/details/application-insights/).

#### <a name="i-dont-see-all-the-data-im-expecting"></a>Všechna data, která se byla očekávána se nezobrazí
* Otevřete kvóty a ceny okno a zkontrolujte, zda [vzorkování](app-insights-sampling.md) je v provozu. (přenos 100 % znamená, že vzorkování není v provozu.) Službu Application Insights můžete nastavit tak, aby přijímal pouze část telemetrická data přenášená z vaší aplikace. To pomáhá při synchronizaci v rámci vaší měsíční kvóta telemetrie. 

## <a name="no-usage-data"></a>Žádná data o využití
**Zobrazuje, data o žádosti a doby odezvy, ale žádné zobrazení stránky, prohlížeč nebo uživatelská data.**

Jste úspěšně nastavili aplikace k odesílání telemetrie ze serveru. Teď je dalším krokem je [nastavení webové stránky k odesílání telemetrie z webového prohlížeče][usage].

Případně pokud se váš klient je aplikace v [telefonu nebo jiné zařízení][platforms], mohla odesílat telemetrii z ní. 

Použijte stejný klíč instrumentace nastavit telemetrie klient i server. Data se zobrazí v rámci stejné prostředku Application Insights a budete moct korelovat události z klienta a serveru.


## <a name="disabling-telemetry"></a>Vypnutí telemetrie
**Jak můžete zakázat telemetrii kolekce?**

V kódu:

```Java

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);
```

**Nebo** 

Aktualizujte soubor ApplicationInsights.xml (ve složce prostředky ve vašem projektu). Přidejte následující do kořenového uzlu:

```XML

    <DisableTelemetry>true</DisableTelemetry>
```

Pomocí metody XML, musíte aplikaci restartovat při změně hodnota.

## <a name="changing-the-target"></a>Změna cíle
**Jak můžete změnit který Azure prostředek projektu odešle data?**

* [Získejte klíč instrumentace nového prostředku.][java]
* Pokud jste přidali Application Insights do projektu pomocí sady nástrojů pro Azure pro prostředí Eclipse, klikněte pravým tlačítkem na webový projekt, vyberte **Azure**, **konfigurovat Application Insights**a změňte klíč.
* V opačném aktualizujte klíč v ApplicationInsights.xml ve složce prostředky ve vašem projektu.

## <a name="debug-data-from-the-sdk"></a>Ladění data ze sady SDK

**Jak můžete zjistit, co je to sada SDK?**

Chcete-li získat další informace o co se děje v rozhraní API, přidejte `<SDKLogger/>` do kořenového uzlu souboru ApplicationInsights.xml konfigurace.

Můžete také určit, aby protokoly pro výstup do souboru:

```XML

    <SDKLogger type="FILE">
      <enabled>True</enabled>
      <UniquePrefix>JavaSDKLog</UniquePrefix>
    </SDKLogger>
```

Soubory lze nalézt v `%temp%\javasdklogs` nebo `java.io.tmpdir` v případě Tomcat server.


## <a name="the-azure-start-screen"></a>Na obrazovce Azure start
**Dívám se na [portálu Azure](https://portal.azure.com). Mapy chci se dozvědět něco o mé aplikaci?**

Ne, zobrazuje stav servery Azure po celém světě.

*Z Azure počáteční Tabule (domovskou obrazovku) jak lze najít data o mé aplikaci?*

Za předpokladu, že jste [nastavit aplikaci pro službu Application Insights][java], klikněte na tlačítko Procházet, vyberte Application Insights a vyberte prostředek aplikace, který jste vytvořili pro vaši aplikaci. Chcete-li získat existuje rychlejší v budoucnosti budete moct připnout aplikaci Tabule start.

## <a name="intranet-servers"></a>Intranetové servery
**Můžete sledovat server v mém intranetu?**

Ano, pokud že váš server mohla odesílat telemetrii do portálu služby Application Insights prostřednictvím veřejného Internetu. 

V bráně firewall můžete chtít otevřít porty TCP 80 a 443 pro odchozí přenosy na adresu dc.services.visualstudio.com a f5.services.visualstudio.com.

## <a name="data-retention"></a>Uchovávání dat
**Jak dlouho se data uchovávají v portálu? Je bezpečné?**

V tématu [uchovávání dat a ochrana osobních údajů][data].

## <a name="next-steps"></a>Další kroky
**Mám nastavit Application Insights pro aplikace my server Java. Kde můžou dělat?**

* [Monitorování dostupnosti webových stránek][availability]
* [Sledování využití webové stránky][usage]
* [Sledování využití a diagnostikovat problémy ve svých aplikacích zařízení][platforms]
* [Zápis kódu pro sledování využití vaší aplikace][track]
* [Zaznamenat diagnostických protokolů][javalogs]

## <a name="get-help"></a>Podpora
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md

