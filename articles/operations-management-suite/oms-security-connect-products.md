---
title: "Připojení produktů zabezpečení k Operations Management Suite (OMS) | Dokumentace Microsoftu"
description: "Tento dokument vám pomůže připojit vaše produkty zabezpečení k řešení Zabezpečení a audit pro Operations Management Suite pomocí formátu CEF (Common Event Format)."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 46eee484-e078-4bad-8c89-c88a3508f6aa
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: yurid
translationtype: Human Translation
ms.sourcegitcommit: f0a512fb0684b1db25353bf4bbd35bdf2a07e1b2
ms.openlocfilehash: 5cb5c6b6cc047974013df59d615d4f094d8d60a4


---
# <a name="connecting-your-security-products-to-the-operations-management-suite-oms-security-and-audit-solution"></a>Připojení produktů zabezpečení k Operations Management Suite (OMS) 
Tento dokument vám pomůže připojit vaše produkty zabezpečení k řešení Zabezpečení a audit pro OMS. Jsou podporovány následující zdroje:

- Události CEF (Common Event Format)
- Události Cisco ASA


## <a name="what-is-cef"></a>Co je CEF?
Formát CEF (Common Event Format) je standardní formát nad zprávami Syslog, který používá mnoho dodavatelů zabezpečení, aby byla možná vzájemná spolupráce událostí mezi různými platformami. Řešení Zabezpečení a audit pro OMS podporuje ingestování pomocí formátu CEF, což umožňuje připojit se k produktům zabezpečení pomocí Zabezpečení OMS. 

Připojením zdroje dat k OMS budete moct využívat výhody následujících funkcí, které jsou součástí této platformy:

- Hledání a korelace
- Auditování
- Výstrahy
- Analýza hrozeb
- Významné problémy

## <a name="collection-of-security-solution-logs"></a>Shromažďování protokolů řešení zabezpečení

Zabezpečení OMS podporuje shromažďování protokolů pomocí formátu CEF přes Syslogy a protokoly [Cisco ASA](https://blogs.technet.microsoft.com/msoms/2016/08/25/add-your-cisco-asa-logs-to-oms-security/). V tomto příkladu zdroj (počítač generující protokoly) je počítač s Linuxem a spuštěným démonem syslog-ng a cíl je Zabezpečení OMS. Příprava počítače s Linuxe vyžaduje následující úlohy:

- Stáhněte Agenta OMS pro Linux verze 1.2.0-25 nebo vyšší.
- Podle pokynů v části **Stručná instalační příručka** z [tohoto článku](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#steps-to-install-the-oms-agent-for-linux) nainstalujte a zařaďte agenta do pracovního prostoru.

Obvykle je agent nainstalovaný na jiný počítač než ten, na kterém se generují protokoly. Předávání protokolů na počítač agenta bude obvykle vyžadovat následující kroky:

- Nakonfigurujte produktu nebo počítač protokolování, aby předával požadované události démonu syslog (rsyslog nebo syslog-ng) na počítači agenta.
- Povolte démona syslog na počítači agenta pro příjem zpráv ze vzdáleného systému.

Na počítači agenta se události musí odesílat z démona syslog na místní port UDP 25226. Agent na tomto portu naslouchá příchozím událostem. Následuje příklad konfigurace pro odesílání všech událostí z místního systému agentovi (konfiguraci můžete upravit, aby vyhovovala vašemu místnímu nastavení):

1. Otevřete okno terminálu a přejděte do adresáře */etc/syslog-ng/*. 
2. Vytvořte nový soubor *security-config-omsagent.conf* a přidejte následující obsah:  OMS_facility = local4
    
    filter f_local4_oms { facility(local4); };

    destination security_oms { tcp("127.0.0.1" port(25226)); };

    log { source(src); filter(f_local4_oms); destination(security_oms); };
    
3. Stáhněte soubor *security_events.conf* a umístěte ho do adresáře */etc/opt/microsoft/omsagent/conf/omsagent.d/* v počítači agenta OMS.
4. Zadejte následující příkaz k restartování démona syslog:  *Pro syslog-ng spusťte:*
    
    ```
    sudo service rsyslog restart
    ```

    *Pro rsyslog spusťte:*
    
    ```
    /etc/init.d/syslog-ng restart
    ```
5. Zadejte následující příkaz k restartování agenta OMS:

    *Pro syslog-ng spusťte:*
    
    ```
    sudo service omsagent restart
    ```

    *Pro rsyslog spusťte:*
    
    ```
    systemctl restart omsagent
    ```
6. Zadejte následující příkaz a zkontrolujte výsledek, aby bylo jasné, že v protokolu agenta OMS nejsou žádné chyby:

    ``` 
    tail /var/opt/microsoft/omsagent/log/omsagent.log
    ```

## <a name="reviewing-collected-security-events"></a>Kontrola shromážděných událostí zabezpečení

Po dokončení konfigurace se události zabezpečení začnou ingestovat řešením Zabezpečení OMS. Pokud chcete tyto události vizualizovat, otevřete Hledání v protokolu, do vyhledávacího pole zadejte příkaz *Type=CommonSecurityLog* a stiskněte klávesu ENTER. Následující příklad ukazuje výsledek tohoto příkazu. Všimněte si, že v tomto případě Zabezpečení OMS už ingestovalo protokoly zabezpečení od více dodavatelů:
   
![Vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS](./media/oms-security-connect-products/oms-security-connect-products-fig1.png)

Toto vyhledávání můžete upřesnit pro jednoho dodavatele, například k online zobrazení protokolů Cisco zadejte: *Type=CommonSecurityLog DeviceVendor=Cisco*. CommonSecurityLog má předdefinovaná pole pro libovolnou hlavičku formátu CEF včetně základních rozšíření, zatímco jakékoli jiné rozšíření, bez ohledu na to, zda je to vlastní rozšíření, se vloží do pole AdditionalExtensions. K získání vyhrazených polí můžete použít funkci vlastních polí. 

### <a name="accessing-computers-missing-baseline-assessment"></a>Přístup k počítačům, u nichž nedošlo k vyhodnocení standardních hodnot
OMS podporuje profil standardních hodnot člena domény v systému Windows Server 2008 R2 až po Windows Server 2012 R2. Standardní hodnoty systému Windows Server 2016 ještě nejsou dokončené, ale budou přidány okamžitě po publikování. Všechny ostatní operační systémy kontrolované přes vyhodnocování standardních hodnot v řešení Zabezpečení a audit v OMS jsou uvedené v části **Počítače, u kterých nedošlo k vyhodnocení standardních hodnot**.

## <a name="see-also"></a>Viz také
V tomto dokumentu jste se naučili připojit řešení CEF k OMS. Další informace o zabezpečení v OMS najdete v následujících článcích:

* [Přehled Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Monitorování a reagování na výstrahy zabezpečení v řešení Zabezpečení a audit v Operations Management Suite](oms-security-responding-alerts.md)
* [Monitorování prostředků v řešení Zabezpečení a audit v Operations Management Suite](oms-security-monitoring-resources.md)




<!--HONumber=Dec16_HO1-->


