---
title: "Řešení potíží s agenta systému Linux Azure Log Analytics | Microsoft Docs"
description: "Popisují příznaky, příčiny a řešení nejběžnějších problémů s agentem Log Analytics Linux."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: magoedte
ms.openlocfilehash: 4f9eaa330ba90052080fc18def974d5dec43c1e0
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-troubleshoot-issues-with-the-linux-agent-for-log-analytics"></a>Řešení potíží s agentem systému Linux pro analýzy protokolů

Tento článek obsahuje nápovědu k odstraňování potíží chyby setkat s agentem systému Linux pro analýzy protokolů a navrhne možná řešení jejich řešení.

### <a name="issue-unable-to-connect-through-proxy-to-log-analytics"></a>Problém: Nelze se připojit prostřednictvím proxy serveru k analýze protokolů

#### <a name="probable-causes"></a>Možných příčin
* Zadaný během registrace proxy serveru byl nesprávný
* Analýzy protokolů a koncové body služby Azure Automation nejsou povolené ve vašem datovém centru 

#### <a name="resolutions"></a>Řešení
1. Reonboard ke službě Analýza protokolů s agentem OMS pro Linux pomocí následujícího příkazu s parametrem `-v` povolena. To umožňuje podrobný výstup agenta připojení prostřednictvím proxy serveru pro službu OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Projděte si část [konfigurace agenta pro použití s proxy serveru nebo brány OMS](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) ověření jste správně nakonfigurovali agenta pro komunikaci přes proxy server.    
* Double zkontrolujte, jestli jsou následující koncové body služby analýzy protokolů seznam povolených adres:

    |Prostředek agenta| Porty |  
    |------|---------|  
    |*.ods.opinsights.azure.com | Port 443|   
    |*.oms.opinsights.azure.com | Port 443|   
    |ods.systemcenteradvisor.com | Port 443|   
    |*.BLOB.Core.Windows.NET/ | Port 443|   

### <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Problém: Obdržíte 403 chybu při pokusu o zařadit do provozu

#### <a name="probable-causes"></a>Možných příčin
* Datum a čas je nesprávný na Linux Server 
* ID pracovního prostoru a klíč pracovního prostoru používá nejsou správné

#### <a name="resolution"></a>Řešení

1. Zkontrolujte čas na serveru Linux s datem příkaz. Pokud je doba +/-15 minut od aktuálního času, registrace se nezdaří. Správné to aktualizujte datum a časové pásmo serveru Linux. 
2. Ověřte, jestli že je nainstalovaná nejnovější verze agenta OMS pro Linux.  Nejnovější verze nyní upozorní, že jste Pokud časového posunu způsobuje selhání registrace.
3. Reonboard pomocí správné ID pracovního prostoru a klíč pracovního prostoru pokynů instalace dříve v tomto tématu.

### <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Problém: Zobrazí 500 a 404 Chyba v souboru protokolu hned po registraci
Jde o známý problém, který se nachází na první nahrávání dat Linux do pracovního prostoru analýzy protokolů. To nemá vliv dat probíhá odeslané nebo služby.

### <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Problém: Nevidíte všechna data na portálu Azure

#### <a name="probable-causes"></a>Možných příčin

- Připojování ke službě Analýza protokolů se nezdařilo
- Připojení ke službě Analýza protokolů je blokovaný.
- Vytvoření zálohy OMS agenta pro Linux data

#### <a name="resolutions"></a>Řešení
1. Zkontrolujte, pokud registrace služby analýzy protokolů po úspěšné kontrole, jestli existuje následující soubor:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Pomocí Reonboard `omsadmin.sh` příkazového řádku pokyny
3. Pokud používáte proxy server, podívejte se na proxy řešení kroků uvedených výše.
4. V některých případech pokud Agent OMS pro Linux nemůže komunikovat se službou, data na agenta je zařazených do fronty pro velikost vyrovnávací paměti úplná, což je 50 MB. Spuštěním následujícího příkazu by měla být restartována OMS agenta pro Linux: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.

### <a name="issue-omsagent-creates-excessive-number-of-user-process-on-computer-and-never-terminates-them"></a>Problém: OMSAgent vytvoří nadměrnému počtu procesů uživatele v počítači a nikdy ukončí je
Když povolíte řešení pro správu, které podporují správu virtuálních počítačů Linux, zahájí počet procesů na agenta systému Linux. Ale předtím, než se proces skončí, je kvůli známému problému spuštěn jiný proces. 

#### <a name="resolutions"></a>Řešení
Chcete-li změnit počet procesů uživatele, které může být generována OMSAgent, nakonfigurujte agenta pomocí omsadmin.sh.  Počet procesů, které se generují ve výchozím nastavení je 75 a před změnou limit, byste měli spustit nejdřív následující příkaz, který najdete v části Jak velký počet procesů OMSAgent jsou aktuálně spuštěny: `ps aux | grep -E '^omsagent' | wc -l`.  
Můžete ověřit, co aktuální limit je nastaven na tak, že spustíte následující příkaz:`cat /etc/security/limits.conf | grep -E '^omsagent'`

Pomocí následujících příkazů buď konfigurovat vlastní proces, který limit nebo omezit na proces zpět na výchozí hodnotu.

1. Nastavit limit procesů pro OMSAgent: `sudo /opt/microsoft/omsagent/bin/omsadmin.sh -n <specific number limit>`.<br>Všimněte si, že je minimální limit, může být nastavena na hodnotu 5.  

2. Nastavit limit procesů pro OMSAgent zpět na výchozí hodnota:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh -N`

Ověřte, že nové nastavení použilo spuštěním následujícího příkazu: `cat /etc/security/limits.conf | grep -E '^omsagent'`.  Pokud nevidíte použitá nové konfigurace, může být vzhledem k tomu, že můžete omezit na proces příliš nízká.  