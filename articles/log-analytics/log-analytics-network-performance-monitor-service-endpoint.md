---
title: "Síťová řešení pro sledování výkonu v Azure Log Analytics | Microsoft Docs"
description: "Funkce správce koncový bod služby v nástroji Sledování výkonu sítě umožňuje monitorovat připojení k síti pro libovolný koncový bod, který má otevřený port TCP."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 7eb31b91480b6e57135581cfa2f5503de3189e10
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="service-endpoint-manager"></a>Správce koncový bod služby

Funkce správce koncový bod služby v [sledování výkonu sítě](log-analytics-network-performance-monitor.md) vám umožní monitorovat připojení k síti pro libovolný koncový bod, který má otevřený port TCP. Tyto koncové body můžou být webů, aplikací SaaS, PaaS aplikací a databází SQL. 

Můžete provádět následující funkce s **monitorování koncového bodu služby**: 

- Monitorovat síťové připojení k vašim aplikacím a síťové služby (např. Office 365, Dynamics CRM, interní řádek obchodní aplikace, databáze SQL atd.) z více větve pobočky na umístění 
- Předdefinované testy monitorování síťové připojení k Office 365 a Dynamics365 koncové body 
- Určit dobu odezvy, latence sítě, ztráta paketů při připojení ke koncovému bodu 
- Určit, zda je aplikace nízký výkon z důvodu sítě nebo z důvodu některých problém na konci zprostředkovatele aplikace 
- Identifikujte aktivní body v síti, který může být příčinou aplikace nízký výkon zobrazením latence přispěli každého směrování na mapě topologie. 


![Monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-intro.png)


## <a name="configuration"></a>Konfigurace 
Chcete-li spustit nástroj konfigurace pro sledování výkonu sítě, otevřete [řešení pro sledování výkonu sítě](log-analytics-network-performance-monitor.md) a klikněte na tlačítko **konfigurace** tlačítko.

![Konfigurace programu Sledování výkonu sítě](media/log-analytics-network-performance-monitor/npm-configure-button.png)


### <a name="configure-oms-agents-for-the-monitoring"></a>Konfigurace OMS agentů pro monitorování.  
Povolte následující pravidla brány firewall na uzlech používá pro monitorování, aby řešení můžete zjistit topologii z uzlů pro koncový bod služby: 

```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action 
```

### <a name="create-service-endpoint-monitor-tests"></a>Vytváření testů monitorování koncového bodu služby 

Začít vytvářet testy monitorování síťové připojení ke koncovým bodům služby 

1. Klikněte na **monitorování koncového bodu služby** kartě.
2. Klikněte na tlačítko **přidat Test** a zadejte testovací název a popis. 
3. Vyberte typ testu:<br>Vyberte **webového testu** Pokud sledujete připojení pro službu, která reaguje na požadavky HTTP/S, jako je například outlook.office365.com, vyhledávače bing.com.<br>Vyberte **test sítě** Pokud sledujete připojení pro službu, která reaguje na požadavek TCP, ale nereaguje na požadavek HTTP/S, třeba s SQL serverem, FTP server, SSH port atd. 
4. Pokud nechcete provést měření sítě (latence sítě, ztráta paketů, zjišťování topologie) a potom zrušte zaškrtnutí políčka textové pole. Doporučujeme, aby měl že být zkontrolovány. Tím lze získat maximální výhody z možnosti. 
5. Zadejte cílovou adresu URL nebo plně kvalifikovaný název domény nebo IP adresu, do které chcete monitorovat připojení k síti.  
6. Zadejte číslo portu z cílových služeb. 
7. Zadejte frekvenci chcete spustit test. 
8. Vyberte uzel, z nichž chcete sledovat síťové připojení ke službě. 

    >[!NOTE]
    > Pro uzly na serveru Windows používá funkce založených na protokolu TCP požadavky k provedení měření sítě. Pro uzly na základě klienta Windows používá možnosti založené na protokolu ICMP požadavky k provedení měření sítě. V některých případech blokuje cílová aplikace příchozího požadavku na základě protokolu ICMP z důvodu, který při uzly jsou Windows, na základě klienta, řešení se nepodařilo provést měření sítě. Doporučujeme proto, že používáte uzly na serveru systému Windows v takových případech. 

9. Pokud nechcete, aby k vytvoření události stavu pro položky, které jste vybrali, poté zrušte **povolit sledování stavu na cíle pro tento test předmětem**. 
10. Vyberte sledování podmínek. Můžete nastavit vlastní prahové hodnoty pro generování událostí stavu zadáním prahové hodnoty. Vždy, když je hodnota stavu překročí jeho vybraná prahová hodnota pro dvojici vybrané sítě nebo podsítí, je generována událost stavu. 
11. Klikněte na tlačítko **Uložit** konfiguraci uložíte. 

 ![Konfigurace monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-configuration.png)



## <a name="walkthrough"></a>Názorný postup 

Přesunout do zobrazení řídicího panelu monitorování sítě výkonu a sledovat **monitorování koncového bodu služby** stránky chcete získat shrnutí stavu různých testů, které jste vytvořili.  

![Stránku sledování koncový bod služby](media/log-analytics-network-performance-monitor/service-endpoint-blade.png)

Klikněte na dlaždici k podrobnostem a zobrazit podrobnosti o testy na **testy** stránky. V tabulce LHS můžete zobrazit stav bodu v čase a hodnota Doba odezvy služby, latence sítě a ztráta paketů pro všechny testy. Můžete použít ovládací prvek zapisovač stavu sítě chcete-li zobrazit síťové snímku v jiné čas v minulosti. Klikněte na test v tabulce, které chcete prozkoumat. V podokně RHS můžete zobrazit historických trendů ztrátě, latence a hodnoty času odezvy z grafy. Klikněte na odkaz Test podrobnosti zobrazení výkonu z každého uzlu. 

![Testy monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-tests.png)

Na **testovací uzly** zobrazení, můžete sledovat připojení k síti z jednotlivých uzlů. Klikněte na uzel s snížení výkonu.  Toto je uzel z kde aplikace pozorovanou, aby byl spuštěn pomalu. 

Určit, zda je aplikace nízký výkon z důvodu sítě nebo z důvodu některých problém na konci zprostředkovatele aplikace pomocí sledování korelace mezi doba odezvy aplikace a latence sítě- 

**Problém s aplikací:** je špička doby odezvy, ale je konzistentní latence sítě a pak to naznačuje, že v síti funguje správně, a je problém z důvodu problému na konci aplikace.  

![Problém s aplikací monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-application-issue.png)

**Problém sítě:** Pokud Špička doby odezvy je společně s odpovídající Špička v latenci sítě, pak to naznačuje, že prodloužením doby odezvy je z důvodu zvýšení latence sítě.  

![Problém sítě monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-network-issue.png)

Jakmile jste zjistili, že se z důvodu sítě, můžete kliknutím na **topologie** zobrazení odkaz k identifikaci problémových směrování na mapě topologie. Například na obrázku níže můžete zobrazit, mimo 105 ms nízkou celkovou latenci mezi uzlu a koncový bod aplikace, 96 ms je z důvodu přechodu označen červeně. Jakmile jste zformulovali problémových směrování, může trvat opravné akce.  

![Testy monitorování koncového bodu služby](media/log-analytics-network-performance-monitor/service-endpoint-topology.png)

## <a name="diagnostics"></a>Diagnostika 

Pokud zjistíte anomálií, postupujte podle těchto kroků:

Pokud doba odezvy služby ztráty sítě a prodlevy jsou zobrazeny jako NA, pak může být z jednoho nebo více z následujících důvodů:
- Aplikace je mimo provoz.
- Uzel používá pro kontrolu síťové připojení ke službě je mimo provoz.
- Cíl zadaný v konfiguraci testovací je nesprávný.
- Uzel nemá připojení k síti.

Pokud doba odezvy platný služby se zobrazí, ale ztráty sítě, jakož i latence se zobrazují jako NA, může být z jednoho nebo více z následujících důvodů:
- Pokud se používá pro kontrolu síťové připojení ke službě uzel klientský počítač systému Windows, Cílová služba neblokuje požadavky ICMP nebo síťová brána firewall neblokuje požadavky protokolu ICMP z uzlu.
- Zaškrtávací políčko pro **provést měření sítě** bylo zrušeno v testovací konfigurace. 

Pokud doba odezvy služby NÁ, ale jsou platné ztráty sítě, jakož i latence, pak jej naznačuje, že cílová služba není webovou aplikaci. Upravte konfiguraci testovací a vyberte typ testu jako test sítě místo webového testu. 

Pokud aplikace běží pomalu, byste měli zjistit, zda je aplikace nízký výkon z důvodu sítě nebo z důvodu některých problém na konci poskytovatele aplikace.


## <a name="next-steps"></a>Další postup
* [V protokolech Hledat](log-analytics-log-searches.md) zobrazíte podrobné sítě záznamů dat výkonu.
