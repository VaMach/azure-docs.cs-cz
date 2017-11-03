---
title: "Připojení počítačů Linux k Operations Management Suite (OMS) | Microsoft Docs"
description: "Tento článek popisuje postup připojení počítače se systémem Linux hostované v Azure, ostatní cloudu nebo místně do OMS pomocí agenta OMS pro Linux."
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
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: c9902e1b8644c2b0a894f9cde98f2056564775c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="connect-your-linux-computers-to-operations-management-suite-oms"></a>Připojení počítačů Linux k Operations Management Suite (OMS) 

S Microsoft Operations Management Suite (OMS) můžete shromažďovat a provádění akcí s data generovaná z počítače se systémem Linux a kontejner řešení jako Docker, umístěný ve vašem datovém centru místní jako fyzických serverech nebo virtuálních počítačů, virtuálních počítačů v Služba hostovaných v cloudu jako Amazon Web Services (AWS) nebo Microsoft Azure. Také můžete řešení pro správu k dispozici v OMS například sledování změn, k identifikaci změny konfigurace a Správa aktualizací ke správě aktualizací softwaru k proaktivnímu řízení životního cyklu vaší virtuální počítače s Linuxem. 

OMS agenta pro Linux komunikuje přes port 443 protokolu TCP odchozí službou OMS a pokud se počítač připojí k serveru brány firewall nebo proxy server komunikovat přes Internet, přečtěte si [konfigurace agenta pro použití s HTTP proxy server nebo OMS Brána](#configuring-the-agent-for-use-with-an-http-proxy-server-or-oms-gateway) pochopit změny konfigurace, které se musí provést.  Pokud sledujete počítači pomocí System Center 2016 - Operations Manager nebo Operations Manager 2012 R2, může být vícedomé službou OMS pro shromažďování dat a předání do služby a bude i nadále monitorovat pomocí nástroje Operations Manager.  Počítače se systémem Linux monitorovány podle skupiny pro správu nástroje Operations Manager, která je integrovaná s OMS neobdrží konfigurace zdroje dat a předávat shromážděné data prostřednictvím skupiny pro správu.  K více než jeden pracovní prostor sestavy nelze konfigurovat OMS agent.  

Pokud vaše zásady zabezpečení IT neumožňují počítače v síti pro připojení k Internetu, agent může být nakonfigurován pro připojení k bráně OMS shromážděná data v závislosti na řešení, které jste povolili odesílat a přijímat informace o konfiguraci. Další informace a kroky pro konfiguraci agenta OMS Linux komunikaci přes bránu OMS ke službě OMS najdete v tématu [počítače připojit k OMS pomocí brány OMS](log-analytics-oms-gateway.md).  

Následující diagram znázorňuje připojení mezi agentem spravované počítače se systémem Linux a OMS, včetně směr a portů.

![agent přímé komunikaci s OMS diagram](./media/log-analytics-agent-linux/log-analytics-agent-linux-communication.png)

## <a name="system-requirements"></a>Požadavky na systém
Než začnete, zkontrolujte následující podrobnosti k ověření, že splňujete požadavky.

### <a name="supported-linux-operating-systems"></a>Podporované operační systémy Linux
Následující Linuxových distribucích jsou oficiálně podporované.  OMS agenta pro Linux mohou však spustit také na dalších distribuce, které nejsou uvedené.

* Linux Amazon 2012.09 k 2015.09 (x86/x64)
* CentOS Linux 5, 6 a 7 (x86/x64)
* Oracle Linux 5, 6 a 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 a 7 (x86/x64)
* Debian GNU/Linux 6, 7 a 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 15.04, 15.10, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 a 12 (x86/x64)

### <a name="network"></a>Síť
Informace o následující seznam konfigurace proxy a firewall informace požadované pro Linux agenta pro komunikaci s OMS. Přenosy jsou odchozí z vaší sítě do služby OMS. 

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
omsagent | 1.4.1 | Agent nástroje Operations Management Suite pro Linux
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

* Bez oprávnění uživatele s názvem: `omsagent` je vytvořena. Démon omsagent spouští pod tímto účtem.
* Soubor "zahrnout" sudoers je vytvořena při /etc/sudoers.d/omsagent. Tento soubor autorizuje omsagent restartovat démoni syslog a omsagent. Pokud direktivy "zahrnout" sudo nejsou podporovány v nainstalované verzi sudo, zapíšou se do /etc/sudoers tyto položky.
* Konfigurace syslog je upravit tak, aby předávat podmnožinu události agenta. Další informace najdete v tématu **konfigurace shromažďování dat** části níže.

### <a name="upgrade-from-a-previous-release"></a>Upgrade z předchozí verze
Upgrade z verze dříve, než je 1.0.0-47 v této verzi podporovány. Instalace se `--upgrade` příkaz upgraduje všechny komponenty agenta na nejnovější verzi.

## <a name="installing-the-agent"></a>Instalace agenta

Tato část popisuje, jak nainstalovat agenta OMS pro Linux pomocí bunndle, který obsahuje Debian a RPM balíčky pro všechny komponenty agenta.  Můžete nainstalovat přímo nebo extrahují pro načtení daných jednotlivých balíčků.  

Nejprve je třeba vaše OMS ID a klíč, který můžete najít a přepnutí na [klasický portál OMS](https://mms.microsoft.com).  Na **přehled** stránky z hlavní nabídky vyberte možnost **nastavení**a potom přejděte na **připojené servery Sources\Linux**.  Zobrazí hodnotu napravo od **ID pracovního prostoru** a **primární klíč**.  Zkopírujte a vložte do vašeho oblíbeného editoru.    

1. Stáhněte si nejnovější [OMS agenta pro Linux (x64)](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.1-45/omsagent-1.4.1-45.universal.x64.sh) nebo [OMS agenta pro Linux x86](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.1-45/omsagent-1.4.1-45.universal.x86.sh) z Githubu.  
2. Přeneste příslušné sady (x86 nebo x64) do počítače Linux pomocí spojovací bod služby/sftp.
3. Instalaci sady pomocí `--install` nebo `--upgrade` argument. 

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
OMS agenta pro Linux podporuje komunikaci prostřednictvím serveru proxy nebo brány OMS ke službě OMS pomocí protokolu HTTPS.  Anonymní i základní ověřování (uživatelské jméno a heslo) je podporováno.  

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

## <a name="onboarding-with-operations-management-suite"></a>Registrace pomocí služby Operations Management Suite
Pokud ID a klíč nebyla zadána při instalaci sady, musí být agenta následně zaregistrován u služby Operations Management Suite.

### <a name="onboarding-using-the-command-line"></a>Registrace pomocí příkazového řádku
Spusťte příkaz omsadmin.sh se zadaným ID pracovního prostoru a klíč vašeho pracovního prostoru. Tento příkaz musí spustit jako kořenového adresáře (s zvýšení oprávnění sudo):
```
cd /opt/microsoft/omsagent/bin
sudo ./omsadmin.sh -w <WorkspaceID> -s <Shared Key>
```

### <a name="onboarding-using-a-file"></a>Registrace pomocí souboru.
1.  Vytvoření souboru `/etc/omsagent-onboard.conf`. Soubor musí být přístupné pro čtení a zápis pro kořenový adresář.
`sudo vi /etc/omsagent-onboard.conf`
2.  V souboru s ID pracovního prostoru a sdílený klíč, vložte následující řádky:

        WORKSPACE_ID=<WorkspaceID>  
        SHARED_KEY=<Shared Key>  
   
3.  Spusťte následující příkaz k Onboard k OMS:`sudo /opt/microsoft/omsagent/bin/omsadmin.sh`
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

### <a name="issue-unable-to-connect-through-proxy-to-oms"></a>Problém: Nelze se připojit prostřednictvím proxy serveru k OMS

#### <a name="probable-causes"></a>Možných příčin
* Zadaný během registrace proxy serveru byl nesprávný
* Koncové body služby OMS nejsou whitelistested ve vašem datovém centru 

#### <a name="resolutions"></a>Řešení
1. Reonboard ke službě OMS s agentem OMS pro Linux pomocí následujícího příkazu s parametrem `-v` povolena. Tato settubg umožňuje podrobný výstup agenta připojení prostřednictvím proxy serveru pro službu OMS. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <OMS Workspace ID> -s <OMS Workspace Key> -p <Proxy Conf> -v`

2. Projděte si část [konfigurace agenta pro použití s proxy serveru nebo brány OMS](#configuring the-agent-for-use-with-a-proxy-server-or-oms-gateway) ověření jste správně nakonfigurovali agenta pro komunikaci přes proxy server.    
* Překontrolujte, že vytvoření následujících koncových bodů služby OMS jsou seznam povolených adres:

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
Tato chyba je známý problém, proběhne první nahrávání dat Linux do pracovního prostoru OMS. Tato chyba neovlivňuje dat probíhá odeslané nebo služby.

### <a name="issue-you-are-not-seeing-any-data-in-the-oms-portal"></a>Problém: Nevidíte všechna data na portálu OMS

#### <a name="probable-causes"></a>Možných příčin

- Připojování ke službě OMS se nezdařilo
- Připojení ke službě OMS blokováno.
- Vytvoření zálohy OMS agenta pro Linux data

#### <a name="resolutions"></a>Řešení
1. Zkontrolujte, pokud registrace služby OMS po úspěšné kontrole, jestli existuje následující soubor:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Pomocí Reonboard `omsadmin.sh` příkazového řádku pokyny
3. Pokud používáte proxy server, podívejte se na proxy řešení kroků uvedených výše.
4. V některých případech při OMS agenta pro Linux nemůže komunikovat se službou OMS data na agentovi je zařazených do fronty pro velikost vyrovnávací paměti úplná, což je 50 MB. Spuštěním následujícího příkazu by měla být restartována OMS agenta pro Linux: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Tento problém vyřešen v 1.1.0-28 verze agenta nebo novější.

