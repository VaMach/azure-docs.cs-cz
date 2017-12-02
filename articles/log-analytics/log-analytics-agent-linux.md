---
title: "Připojení počítačů Linux k Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje postup připojení počítače se systémem Linux hostované v Azure, jiné cloudové nebo místní k analýze protokolů s agentem OMS pro Linux."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: magoedte
ms.openlocfilehash: 1f6e0f2a638d9ce9b06f19feba1c9e216812862c
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/01/2017
---
# <a name="connect-your-linux-computers-to-log-analytics"></a>Připojení počítačů Linux k analýzy protokolů 

S Azure Log Analytics můžete shromažďovat a fungovat na informace shromážděné z počítače se systémem Linux a kontejner řešení jako Docker, které se nacházejí ve vašem datovém centru místní jako fyzických serverech nebo virtuálních počítačů, virtuální počítače ve službě hostovaných v cloudu jako Amazon Webové služby (AWS) nebo Microsoft Azure. Můžete použít také k dispozici v řešení pro správu [Azure Automation](../automation/automation-intro.md) například sledování změn, můžete určit změny konfigurace a Správa aktualizací ke správě aktualizací softwaru k proaktivnímu řízení životního cyklu vaší Linux Virtuální počítače. 

Agent OMS pro Linux komunikuje odchozí s analýzy protokolů a služby automatizace Azure přes protokol TCP port 443 a pokud se počítač připojí k serveru brány firewall nebo proxy server komunikovat přes Internet, přečtěte si [konfigurace agenta pro použití s proxy server nebo brány OMS](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) pochopit změny konfigurace, které se musí provést.  Pokud sledujete počítači pomocí System Center 2016 - Operations Manager nebo Operations Manager 2012 R2, může být vícedomé službou analýzy protokolů pro shromažďování dat a předání do služby a bude i nadále monitorovat pomocí nástroje Operations Manager.  Počítače se systémem Linux monitorovány podle skupiny pro správu nástroje Operations Manager, která je integrovaná s analýzy protokolů (momentálně uvedené Operations Management Suite v konzole nástroje Operations Manager Operations) nezadávejte získat konfiguraci zdroje dat a předávání shromážděná data prostřednictvím skupiny pro správu.  K více než jeden pracovní prostor analýzy protokolů sestavy nelze konfigurovat OMS agent.  

Pokud vaše zásady zabezpečení IT neumožňují počítače v síti pro připojení k Internetu, agent může být nakonfigurován pro připojení k bráně OMS shromážděná data v závislosti na řešení, které jste povolili odesílat a přijímat informace o konfiguraci. Další informace a kroky pro konfiguraci agenta OMS Linux komunikaci přes bránu OMS do služeb najdete v tématu [počítače připojit k OMS pomocí brány OMS](log-analytics-oms-gateway.md).  

Následující diagram znázorňuje připojení mezi agentem spravované počítače se systémem Linux a analýzy protokolů, včetně směr a portů.

![agent přímé komunikaci s diagram služby Azure](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Požadavky na systém
Než začnete, zkontrolujte následující podrobnosti k ověření, že splňujete požadavky.

### <a name="supported-linux-operating-systems"></a>Podporované operační systémy Linux
Následující Linuxových distribucích jsou oficiálně podporované.  OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.

* Linux Amazon 2012.09 k 2015.09 (x86/x64)
* CentOS Linux 5, 6 a 7 (x86/x64)
* Oracle Linux 5, 6 a 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 a 7 (x86/x64)
* Debian GNU/Linux 6, 7 a 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 a 12 (x86/x64)

### <a name="network"></a>Síť
Informace o následující seznam konfigurace proxy a firewall informace požadované pro Linux agenta pro komunikaci s analýzy protokolů a automatizace Azure. Přenosy jsou odchozí z vaší sítě do služby. 

|Prostředek agenta| Porty |  
|------|---------|  
|*.ods.opinsights.azure.com | Port 443|   
|*.oms.opinsights.azure.com | Port 443|   
|*.BLOB.Core.Windows.NET/ | Port 443|   
|*.azure-automation.net | Port 443|  

### <a name="package-requirements"></a>Požadavky na balíček

 **Požadovaný balíček**   | **Popis**   | **Minimální verze**
--------------------- | --------------------- | -------------------
Glibc | Knihovny jazyka C GNU   | 2.5-12 
OpenSSL | Knihovny OpenSSL | 0.9.8E nebo 1.0
Curl | cURL webového klienta | 7.15.5
Python ctypes | | 
PAM | PAM moduly | 

> [!NOTE]
>  Rsyslog nebo syslog ng jsou požadované ke shromáždění zprávy syslog. Démon procesu syslog výchozí na verze 5 Red Hat Enterprise Linux a CentOS, Oracle Linux verze (sysklog) není podporována pro shromažďování událostí syslog. Ke shromažďování dat syslog z této verze těchto distribuce, by měl být démon rsyslog nainstalovaná a nakonfigurovaná nahradit sysklog, 

Agent obsahuje více balíčků. Verze souboru obsahuje následující balíčky k dispozici spuštěním sady prostředí s `--extract`:

**Balíček** | **Verze** | **Popis**
----------- | ----------- | --------------
omsagent | 1.4.0 | Agent nástroje Operations Management Suite pro Linux
omsconfig | 1.1.1 | Konfigurace agenta pro agenta OMS
OMI | 1.2.0 | Open Management Infrastructure (OMI) - lightweight CIM Server
scx. | 1.6.3 | Zprostředkovatelé CIM OMI pro metriku výkonu operačního systému
Apache cimprov | 1.0.1 | Zprostředkovatel pro OMI monitorování výkonu serveru Apache HTTP Server. Nainstalovat, pokud je zjištěn serveru Apache HTTP Server.
MySQL cimprov | 1.0.1 | Výkon serveru MySQL monitorování zprostředkovatele pro OMI. Nainstalovat, pokud je zjištěn MySQL nebo MariaDB server.
docker cimprov | 1.0.0 | Zprostředkovatel docker pro OMI. Nainstalovat, pokud je zjištěn Docker.

### <a name="compatibility-with-system-center-operations-manager"></a>Kompatibilita s nástrojem System Center Operations Manager
OMS agenta pro Linux sdílí binárních souborů agenta s agenta System Center Operations Manager. Pokud instalujete agenta OMS pro Linux v systému aktuálně spravován nástrojem Operations Manager, upgraduje OMI a SCX balíčky v počítači na novější verzi. V této verzi OMS a System Center 2016 - Operations Manager, Operations Manager 2012 R2 agenti pro Linux jsou kompatibilní. 

> [!NOTE]
> System Center 2012 SP1 a starší verze nejsou aktuálně kompatibilní nebo není podporován s agentem OMS pro Linux.<br>
> Pokud je Agent OMS pro Linux nainstalován do počítače, který není aktuálně sledovaných nástrojem Operations Manager a potom chcete monitorovat počítač s nástrojem Operations Manager, musíte upravit [OMI konfigurace](#enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager) před zjišťování počítač. **Tento krok je *není* nutný v případě, že je nainstalován agent nástroje Operations Manager před OMS agenta pro Linux.**

### <a name="system-configuration-changes"></a>Změny konfigurace systému
Po instalaci agenta OMS pro balíčky Linux, se použijí následující další konfiguraci systémové změny. Tyto artefakty se odeberou, když omsagent balíček odinstalován.

* Bez oprávnění uživatele s názvem: `omsagent` je vytvořena. Toto je účet, který spouští démon omsagent jako.
* Soubor "zahrnout" sudoers je vytvořena při /etc/sudoers.d/omsagent. To autorizuje omsagent restartovat démoni syslog a omsagent. Pokud direktivy "zahrnout" sudo nejsou podporovány v nainstalované verzi sudo, zapíšou se do /etc/sudoers tyto položky.
* Konfigurace syslog je upravit tak, aby předávat podmnožinu události agenta. Další informace najdete v tématu **konfigurace shromažďování dat** části

### <a name="upgrade-from-a-previous-release"></a>Upgrade z předchozí verze
Upgrade z verze dříve, než je 1.0.0-47 v této verzi podporovány. Instalace se `--upgrade` příkaz upgraduje všechny komponenty agenta na nejnovější verzi.

## <a name="installing-the-agent"></a>Instalace agenta

Tato část popisuje, jak nainstalovat agenta OMS pro Linux ručně pomocí bunndle, který obsahuje Debian a RPM balíčky pro všechny komponenty agenta.  Můžete nainstalovat přímo nebo extrahují pro načtení daných jednotlivých balíčků.  Pokud máte v úmyslu nainstalovat agenta na virtuální počítač Linux Azure, najdete v následujícím tématu [shromažďovat data o virtuálních počítačích Azure](log-analytics-quick-collect-azurevm.md) se dozvíte, jak nainstalovat agenta pomocí rozšíření virtuálního počítače Log Analytics.  Postupujte podle kroků v části *povolit rozšíření virtuálního počítače Log Analytics*.  Pro počítače Linux hostované ve vašem prostředí, můžete zjednodušit proces instalace pomocí skriptované metody popsané v článku, [shromažďovat data z počítače se systémem Linux hostované ve vašem prostředí](log-analytics-quick-collect-linux-computer.md).  

> [!NOTE]
> Při dvou články výše uvedené jsou určené pro někoho, kdo je nový k analýze protokolů a začít používat službu rychle, kroky pro konfiguraci počítače jsou relevantní.  Pokud už máte pracovní prostor a se chtějí připojit počítače se systémem Linux, vyberte existujícímu pracovnímu prostoru, pokud virtuální počítač Azure Linux, nebo pro počítač hostované mimo Azure, kopie ID pracovního prostoru a klíč předat do skriptu.  

Před instalací agenta OMS pro Linux, je nutné ID a klíč vašeho pracovního prostoru analýzy protokolů.  

1. Přihlaste se k portálu Azure v [https://portal.azure.com](https://portal.azure.com). 
2. Na webu Azure Portal klikněte v levém dolním rohu na **Další služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **protokolu analýzy**.
3. V seznamu analýzy protokolů pracovních prostorů vyberte pracovní prostor, kterou má počítač tak, aby odesílaly.
3. Vyberte **upřesňující nastavení**.<br><br> ![Nastavení zálohy analýzy protokolů](media/log-analytics-quick-collect-azurevm/log-analytics-advanced-settings-01.png)<br><br>  
4. Vyberte **připojené zdroje**a potom vyberte **servery se systémem Linux**.   
5. Hodnota napravo od **ID pracovního prostoru** a **primární klíč**. Zkopírujte a vložte do vašeho oblíbeného editoru.  
6. Stáhněte si nejnovější [OMS agenta pro Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x64.sh) nebo [OMS agenta pro Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.0-45/omsagent-1.4.0-45.universal.x86.sh) z Githubu.  
7. Přeneste příslušné sady (x86 nebo x64) do počítače Linux pomocí spojovací bod služby/sftp.
8. Instalaci sady pomocí `--install` nebo `--upgrade` argument. 

    > [!NOTE]
    > Pokud například když je již nainstalován agent nástroje System Center Operations Manager pro Linux jsou nainstalovány všechny existující balíčky, použijte `--upgrade` argument. Při instalaci připojovala k Operations Management Suite, zadejte `-w <WorkspaceID>` a `-s <Shared Key>` parametry.


#### <a name="to-install-and-onboard-directly"></a>Chcete-li nainstalovat a připojit přímo
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key>
```

#### <a name="to-upgrade-the-agent-package"></a>Upgradovat balíček agenta
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade
```

#### <a name="to-install-and-onboard-to-a-workspace-in-us-government-cloud"></a>Chcete-li nainstalovat a zařadit do pracovního prostoru v Cloud vlády USA
```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -w <workspace id> -s <shared key> -d opinsights.azure.us
```

## <a name="configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway"></a>Konfigurace agenta pro použití s proxy serveru nebo brány OMS
OMS agenta pro Linux podporuje komunikaci prostřednictvím serveru proxy nebo brány OMS ke službě Analýza protokolů pomocí protokolu HTTPS.  Anonymní i základní ověřování (uživatelské jméno a heslo) je podporováno.  

### <a name="proxy-configuration"></a>Konfigurace proxy serveru
Hodnota konfigurace proxy serveru má následující syntaxi:

`[protocol://][user:password@]proxyhost[:port]`

Vlastnost|Popis
-|-
Protocol (Protokol)|https
Uživatel|Volitelné uživatelské jméno pro ověření proxy serverem
heslo|Volitelné heslo pro ověření proxy serverem
proxyhost|Adresa nebo plně kvalifikovaný název domény serveru nebo OMS proxy serveru brány
port|Číslo portu volitelné pro server/OMS proxy serveru brány

Příklad: `https://user01:password@proxy01.contoso.com:30443`

Proxy server lze zadat během instalace nebo úpravou konfiguračního souboru proxy.conf po instalaci.   

### <a name="specify-proxy-configuration-during-installation"></a>Zadejte konfiguraci proxy serveru během instalace
`-p` Nebo `--proxy` argument pro instalaci sady omsagent Určuje konfiguraci proxy serveru používat. 

```
sudo sh ./omsagent-<version>.universal.x64.sh --upgrade -p https://<proxy user>:<proxy password>@<proxy address>:<proxy port> -w <workspace id> -s <shared key>
```

### <a name="define-the-proxy-configuration-in-a-file"></a>Zadejte konfiguraci proxy serveru v souboru
Konfiguraci proxy serveru můžete nastavit v souborech `/etc/opt/microsoft/omsagent/proxy.conf` a `/etc/opt/microsoft/omsagent/conf/proxy.conf `. Soubory mohou být přímo vytvořená nebo upravená, ale jejich oprávnění musí být aktualizovány k udělte že oprávnění na souborech čtení omiuser uživatele. Například:
```
proxyconf="https://proxyuser:proxypassword@proxyserver01:30443"
sudo echo $proxyconf >>/etc/opt/microsoft/omsagent/proxy.conf
sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf
sudo chmod 600 /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf  
sudo /opt/microsoft/omsagent/bin/service_control restart [<workspace id>]
```

### <a name="removing-the-proxy-configuration"></a>Odebírá se konfigurace proxy serveru
Chcete-li odebrat konfiguraci dříve definovaném proxy a vrátit se do přímé připojení, odeberte soubor proxy.conf:
```
sudo rm /etc/opt/microsoft/omsagent/proxy.conf /etc/opt/microsoft/omsagent/conf/proxy.conf
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="onboarding-with-log-analytics"></a>Registrace s analýzy protokolů
Pokud ID a klíč nebyla zadána při instalaci sady, musí být agent následně zaregistrován u analýzy protokolů.

### <a name="onboarding-using-the-command-line"></a>Registrace pomocí příkazového řádku
Spusťte příkaz omsadmin.sh se zadaným id pracovního prostoru a klíč vašeho pracovního prostoru. Tento příkaz musí spustit jako kořenového adresáře (s zvýšení oprávnění sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="register-using-a-file"></a>Zaregistrovat pomocí souboru.
1.  Vytvoření souboru `/etc/omsagent-onboard.conf`. Soubor musí být přístupné pro čtení a zápis pro kořenový adresář.
`sudo vi /etc/omsagent-onboard.conf`
2.  V souboru s ID pracovního prostoru a sdílený klíč, vložte následující řádky:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Spusťte následující příkaz pro registraci s analýzy protokolů:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
4.  Soubor je odstraněn na úspěšnou registraci.

## <a name="enable-the-oms-agent-for-linux-to-report-to-system-center-operations-manager"></a>Povolte agenta OMS pro Linux tak, aby odesílaly System Center Operations Manager
Proveďte následující kroky konfigurace agenta OMS pro Linux informuje o skupině pro správu System Center Operations Manager.  

1. Upravte soubor`/etc/opt/omi/conf/omiserver.conf`
2. Ujistěte se, že na začátek řádku s **httpsport =** definuje port 1270. Například:`httpsport=1270`
3. Restartujte OMI server:`sudo /opt/omi/bin/service_control restart`

## <a name="agent-logs"></a>Protokoly agenta
Protokoly OMS agenta pro Linux najdete tady: `/var/opt/microsoft/omsagent/<workspace id>/log/` naleznete v protokolech programu omsconfig (Konfigurace agenta) na: `/var/opt/microsoft/omsconfig/log/` protokoly pro součásti infrastruktury OMI a SCX (které poskytují data metriky výkonu) najdete na:`/var/opt/omi/log/ and /var/opt/microsoft/scx/log`

### <a name="log-rotation-configuration"></a>Protokol otočení konfigurace ##
Konfigurace otáčení protokolu pro omsagent najdete tady:`/etc/logrotate.d/omsagent-<workspace id>`

Výchozí nastavení je: 
```
/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log {
    rotate 5
    missingok
    notifempty
    compress
    size 50k
    copytruncate
}
```

## <a name="uninstalling-the-oms-agent-for-linux"></a>Odinstalování agenta OMS pro Linux
Balíčky agenta odinstalovat spuštěním souboru .sh sady s `--purge` argument, který kompletně odebere agent a jeho konfigurace z počítače.   

```
> sudo rpm -e omsconfig
> sudo rpm -e omsagent
> sudo /opt/microsoft/scx/bin/uninstall
```

## <a name="troubleshooting"></a>Řešení potíží

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
