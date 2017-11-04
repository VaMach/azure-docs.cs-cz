---
title: "Aktualizace zásobníku Azure 1710 (sestavení 20171020.1) | Microsoft Docs"
description: "Další informace o tom, co je v aktualizaci 1710 pro Azure zásobníku integrované systémy, známé problémy a kde se stáhnout aktualizaci."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: 135314fd-7add-4c8c-b02a-b03de93ee196
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: twooley
ms.openlocfilehash: a66c7dddab1a2246dd0a9fa3c7533063140d2f01
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-stack-1710-update-build-201710201"></a>Aktualizace zásobníku Azure 1710 (sestavení 20171020.1)

*Platí pro: Azure zásobníku integrované systémy*

Tento článek popisuje vylepšení a opravy v tomto balíčku aktualizace – známé problémy pro tuto verzi a toho, kde se stáhnout aktualizaci. Známé problémy jsou rozdělené do známé problémy přímo související s proces aktualizace a známé problémy s sestavení (po instalaci).

> [!IMPORTANT]
> Tento balíček aktualizace je pouze pro Azure zásobníku integrované systémy. Tento balíček aktualizace nevztahují na Azure zásobníku Development Kit.

## <a name="improvements-and-fixes"></a>Vylepšení a opravy

Tato aktualizace zahrnuje následující vylepšení kvality a opravy pro Azure zásobníku.
 
### <a name="windows-server-2016-improvements-and-fixes"></a>Windows Server 2016 vylepšení a opravy

- Aktualizace pro systém Windows Server 2016: 10. října 2017 – KB4041691 (sestavení 14393.1770 operačního systému. V tématu [https://support.microsoft.com/help/4041691](https://support.microsoft.com/help/4041691) Další informace.

### <a name="additional-quality-improvements-and-fixes"></a>Další kvalitou vylepšení a opravy

- Přidat privilegované koncový bod rutiny prostředí PowerShell pro aktualizaci serveru protokolu NTP (Network Time) a odstranění potíží.
- Byla přidána podpora pro aktualizaci modulů koncový bod privilegované koncový bod právě dostatečně správy JEA () a rutiny seznamu povolených IP adres. 
- Chyby opravené jazyce v privilegované koncový bod.
- Přidání možnosti otočit přihlašovací údaje brány.
- Odebrat CBLocalAdmin účet místního správce. 
- Pevné obsahu výstrahy šablony prezenčního signálu chcete Ujistěte se, že prezenčního signálu výstrahy pracovní správně po aktualizaci.
- Pevné zprostředkovatele prostředků infrastruktury řešení překročení časového limitu během operací FRU. 
- Přidání možnosti pro vývojáře v cloudu používat profily rozhraní API Správce prostředků Azure v zásobníku Azure.
- Zakázat službu Windows Update na nasazení virtuálního počítače (DVM). 
- Odebrat uzel power zapnout nebo vypnout akce z uživatelského rozhraní.
- Různé ostatní výkonu a stability opravy. 
 
## <a name="known-issues-with-the-update-process"></a>Známé problémy s proces aktualizace

Tato část obsahuje známé problémy, které mohou nastat při instalaci aktualizace 1710.

> [!IMPORTANT]
> Pokud se aktualizace nezdaří, pokud později pokusíte obnovit aktualizace je nutné použít `Resume-AzureStackUpdate` rutiny z privilegovaných koncový bod. Pokračovat aktualizace pomocí portálu správce. (Jde o známý problém v této verzi.) Další informace najdete v tématu [monitorování aktualizací v zásobníku Azure pomocí privilegované koncový bod](azure-stack-monitor-update.md).

| Příznaky  | Příčina  | Řešení |
|---------|---------|---------|
|Když provedete aktualizaci, může dojít k chybě, která je podobná následující chybě během kroku "Úložiště hostitelů restartovat uzel úložiště" akce plánu aktualizace.<br><br>**{"název": "Restartujte úložiště hostitelů", "Popis": "Restartujte úložiště hostitelů.", "errorMessage": "typ restartování Role 'BareMetal' vyvolána jako výjimka: \n\nThe počítač HostName-05 bylo přeskočeno. Nepodařilo se načíst jeho LastBootUpTime prostřednictvím služby WMI se následující chybová zpráva: RPC server nedostupný. (Výjimka z HRESULT: 0x800706BA). \nat Restart hostitel** | Tento problém je způsoben potenciální vadný ovladač existuje v některých konfiguracích. | 1. Přihlaste se k rozhraní základní desky řadiče (BMC) pro správu webových a restartujte hostitele, který je identifikován v chybové zprávě.<br><br>2. Obnovení aktualizace pomocí privilegované koncový bod. |
| Při provádění aktualizace proces aktualizace stalace a zobrazuje není poskytnutí přehledu o pokroku po kroku "krok: spuštěním kroku 2.4 - nainstalujte aktualizaci" akce plánu aktualizace.<br><br>Tento krok je následována řadu procesy kopírování souborů .nupkg ke sdíleným složkám interní infrastruktury. Například:<br><br>**Kopírování souborů 1 z content\PerfCollector\VirtualMachines do \VirtualMachineName-ERCS03\C$\TraceCollectorUpdate\PerfCounterConfiguration**  | Problém je způsoben soubory protokolu naplňování disky na virtuální počítač infrastruktury a o problém v systému Windows Server Škálováním na více systémů souborovým serverem (SOFS), bude dodána v další aktualizaci. | O pomoc obraťte se na oddělení zákaznických služeb a podpory společnosti Microsoft. | 
| Když provedete aktualizaci, může dojít k chybě, která je podobný následujícímu během kroku "krok: spuštěním kroku 2.13.2 - aktualizace *VM_Name*" akce plánu aktualizace. (Název virtuálního počítače se může lišit.)<br><br>**ActionPlanInstanceWarning ece/MachineName: WarningMessage:Task: volání rozhraní 'LiveUpdate' role Cloud\Fabric\WAS se nezdařilo:<br>typ "LiveUpdate" Role 'WAS' způsobila výjimku:<br>došlo k CHYBĚ během úložiště Inicializace: Při pokusu o vytvoření volání rozhraní API pro službu Microsoft Storage došlo k chybě: {"Zpráva": "při komunikaci se Service Fabric došlo k vypršení časového limitu. Typ výjimky: TimeoutException. Zpráva o výjimce: Vypršel časový limit operace. "}**  | Problém je způsoben vypršení časového limitu vstupně-výstupních operací v systému Windows Server, který bude opraven v další aktualizaci. | Požádejte o pomoc Microsoft CSS.
| Když provedete aktualizaci, může dojít k chybě podobný následujícímu během kroku "krok 21 restartování virtuálních počítačů SQL serveru."<br><br>**Typ 'LiveUpdateRestart' Role 'VirtualMachines' způsobila výjimku:<br>VerboseMessage: [VirtualMachines:LiveUpdateRestart] dotazování pro virtuální počítač MachineName-Sql01. - 13/10/2017 5:23:50: 00 VerboseMessage: [virtuálních počítačů: LiveUpdateRestart] virtuálního počítače je označena jako vysoké dostupnosti. -VerboseMessage 5:23:50: 00 10/13/2017: [VirtualMachines:LiveUpdateRestart] v MS. Internal.ServerClusters.ExceptionHelp.Build v MS. Internal.ServerClusters.ClusterResource.BeginTakeOffline (Boolean force) v Microsoft.FailoverClusters.PowerShell.StopClusterResourceCommand.BeginTimedOperation() v Microsoft.FailoverClusters.PowerShell.TimedCmdlet.WrappedProcessRecord() v Microsoft.FailoverClusters.PowerShell.FCCmdlet.ProcessRecord() - WarningMessage:Task 5:23:50: 00 13/10/2017: volání rozhraní 'LiveUpdateRestart' role. Nepodařilo se Cloud\Fabric\VirtualMachines:** | Tento problém může dojít, pokud virtuální počítač se nepodařilo restartovat. | Požádejte o pomoc Microsoft CSS.
| Když provedete aktualizaci, může dojít k chybě podobný následujícímu:<br><br>**2017-10-22T01:37:37.5369944Z typ "Vypnout" Role 'SQL' způsobila výjimku: pozastavení nástroje uzlu 's45r1004-Sql01' Stop-SQL, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_ došlo k chybě App1\EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Roles\SQL\SQL.psm1: řádek 542at vypnutí, C:\ProgramData\SF\ErcsClusterNode2\Fabric\work\Applications\EnterpriseCloudEngineApplicationType_App1\ EnterpriseCloudEngineServicePkg.Code.1.0.597.18\CloudDeployment\Classes\SQL\SQL.psm1: řádek 50at <ScriptBlock>, <No file>: řádek 18at <ScriptBlock>, <No file>: řádek 16** | Tento problém může dojít, pokud virtuální počítač nelze vložit do pozastaveném stavu na vyprazdňování role. | Požádejte o pomoc Microsoft CSS.
| Při aktualizaci, může dojít k některým z těchto chyb:<br><br>**Typ "Ověřit" Role "ADFS" způsobila výjimku: ověření pro roli služby AD FS nebo grafu se nezdařilo s chybou: Chyba při kontrole koncový bod služby AD FS testu *endpoint_URI*: výjimka volání "GetResponse" s "0" argumenty: "vzdáleného serveru došlo k chybě: (503) serveru není k dispozici. "v Invoke-ADFSGraphValidation**<br><br>**Typ "Ověřit" Role "ADFS" způsobila výjimku: ověření pro roli služby AD FS nebo grafu se nezdařilo s chybou: Chyba při načítání vlastnosti služby AD FS: se nemohl připojit k net.tcp://localhost: 1 500 nebo zásad. Pokus o připojení trval časový rozsah 00:00:02.0498923. Kód chyby TCP 10061: žádné připojení může být vytvořeno, protože cílový počítač aktivně odmítl 127.0.0.1:1500. Při vyvolání ADFSGraphValidation** | Plán akcí aktualizace nelze ověřit stav služby Active Directory Federation Services (AD FS). | Požádejte o pomoc Microsoft CSS.

## <a name="known-issues-post-installation"></a>Známé problémy (po instalaci)

Tato část obsahuje známé problémy s sestavení 20171020.1 po instalaci.

### <a name="portal"></a>Portál

- Nemusí být možné zobrazit úložiště nebo výpočetní prostředky na portálu správce. To znamená, že došlo k chybě při instalaci aktualizace a že aktualizace byla nahlášena nesprávně jako úspěšné. Pokud k tomuto problému dochází, požádejte o pomoc Microsoft CSS.
- Může se zobrazit prázdný řídicí panel portálu. Chcete-li obnovit řídicí panel, vyberte ikonu ozubené kolečko v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.
- Uživatelé mohou procházet na kompletní nabídku marketplace bez předplatného a uvidí položky pro správu jako plány a nabízí. Tyto položky jsou pro uživatele funkční.
- **Přesunout** tlačítko k dispozici, když máte zobrazeny vlastnosti skupiny zdrojů. Toto chování je očekávané. Přesunutí skupin prostředků mezi předplatnými není aktuálně podporován.
- Nemůžete se moci zobrazit oprávnění k vašemu předplatnému používání portálů zásobník Azure. Jako alternativní řešení můžete ověřit oprávnění pomocí prostředí PowerShell.
-  Pro jakýkoli pracovní postup, kde v rozevíracím seznamu vyberte předplatné, skupinu prostředků nebo umístění může zaznamenat jeden nebo více z následujících důvodů:

   - Může se zobrazit prázdný řádek v horní části seznamu. By měl mít pořád povolený výběr položky podle očekávání.
   - Pokud je seznam položek v rozevíracím seznamu krátký, asi nebudete moci zobrazit některé názvy položek.
   - Pokud máte více předplatných uživatele, rozevíracího seznamu skupiny prostředků může být prázdná. 

   Obejít poslední dva problémy, můžete zadat název předplatné nebo skupinu prostředků (pokud ho znáte), nebo můžete místo toho použít PowerShell.
  
### <a name="backup"></a>Zálohování

- Nepovolujte infrastruktura zálohování na **infrastruktura zálohování** okno.

### <a name="health-and-monitoring"></a>Sledování stavu a

- Pokud restartujete instance role infrastruktury, zobrazí se zpráva s oznámením, že restartování se nezdařilo. Ale restartování ve skutečnosti bylo úspěšné.

### <a name="services"></a>Služby

**Marketplace**
- Při pokusu o přidání položky do zásobníku Azure marketplace s použitím **přidat z Azure** možnost, všechny položky můžou být vidět ke stažení.
- Neexistuje žádné zkušenosti marketplace pro vytvoření sady škálování virtuálního počítače. Můžete vytvořit škálování nastavit pomocí šablony.
- Klient musí zaregistrovat zprostředkovatele prostředku úložiště dřív, než vytvoří jejich první funkce Azure v rámci předplatného.
- Odstranění odběrů uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstraňte prostředky uživatele nebo skupinu celý prostředků a potom odstraňte odběry uživatele. 

**SQL nebo MySQL**
- To může trvat až jednu hodinu, než klienti databáze můžete vytvářet v nové SQL nebo MySQL SKU. 
- Vytvoření položky přímo na SQL a MySQL hostitelské servery, které nejsou prováděné poskytovatelem prostředků není podporována a může mít za následek neodpovídající stavu.

**Compute**
- Uživatelé mají možnost vytvoření virtuálního počítače pomocí geograficky redundantní úložiště. Tato konfigurace způsobuje, že vytvoření virtuálního počítače k selhání.
- Můžete nakonfigurovat dostupnost virtuálního počítače nastavit pouze u domény selhání jednoho a doméně aktualizace jednoho.
 
**Síť**
- Nástroj pro vyrovnávání zatížení s veřejnou IP adresu nelze vytvořit pomocí portálu. Jako alternativní řešení můžete použít PowerShell k vytvoření nástroje pro vyrovnávání zatížení.
- Když vytvoříte Vyrovnávání zatížení sítě, musíte vytvořit pravidlo překladu adres sítě. Pokud to neuděláte, obdržíte chybu při pokusu přidat pravidlo NAT po vytvoření nástroje pro vyrovnávání zatížení.
- Veřejnou IP adresu z virtuálního počítače (VM) nelze zrušit přidružení, po virtuálního počítače byla vytvořena a související s touto adresou IP. Zrušení přidružení se zobrazí postup, ale pořád přidruženy původní virtuální počítač dříve přiřazenou veřejnou IP adresu. K tomuto chování dochází i v případě, že je přiřadit IP adresu na nový virtuální počítač (obvykle označuje jako *prohození*). Všechny budoucí pokusy o připojení přes tuto IP adresu povede připojení do původně přidružený virtuální počítač a ne do nového. V současné době je nutné pouze použít nové veřejné IP adresy pro vytvoření nového virtuálního počítače.
 
### <a name="field-replaceable-unit-fru-procedures"></a>Postupy pole replaceable jednotka (FRU)

- Během hromadné postupné aktualizace nejsou opravit offline bitové kopie. Pokud potřebujete nahradit uzlu jednotek škálování, pracujete s OEM dodavatele hardwaru a ujistěte se, že nahrazené uzel má nejnovější úroveň opravy.

## <a name="download-the-update"></a>Stažení aktualizace

Můžete si stáhnout balíček aktualizace 1710 [zde](https://aka.ms/azurestackupdatedownload).

## <a name="next-steps"></a>Další kroky

- Přehled správy aktualizací v zásobníku Azure najdete v tématu [správě aktualizací v přehledu Azure zásobníku](azure-stack-updates.md).
- Informace o tom, jak použít aktualizace najdete v tématu [aktualizace v zásobníku Azure](azure-stack-apply-updates.md).