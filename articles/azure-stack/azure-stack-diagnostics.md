---
title: Diagnostika v Azure Stacku
description: "Jak chcete shromažďovat soubory protokolů pro diagnostiku v Azure zásobníku"
services: azure-stack
author: adshar
manager: byronr
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 10/2/2017
ms.author: adshar
ms.openlocfilehash: 9b1fbbf63ddd8bac2c1a76bbcd5daca69e2513f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stack-diagnostics-tools"></a>Azure zásobníku diagnostické nástroje

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*
 
Zásobník Azure je velké kolekce součástí spolupráci a interakci mezi sebou. Všechny tyto součásti generovat vlastní jedinečné protokoly. Diagnostika problémů může být náročné úlohy, hlavně pro chyby pocházejících z několika součástí vzájemně komunikující zásobník Azure. 

Naše diagnostické nástroje pomoci, ujistěte se, že kolekce mechanismu protokolu snadnou a efektivní. Následující diagram ukazuje jak protokolu nástroje kolekce v pracovním zásobník Azure:

![Kolekce nástrojů protokolu](media/azure-stack-diagnostics/image01.png)
 
 
## <a name="trace-collector"></a>Kolekce trasování
 
Kolekce trasování je ve výchozím nastavení povolena. Ho nepřetržitě běží na pozadí a shromažďuje všechny protokoly trasování událostí pro Windows (ETW) ze služby komponent v zásobníku Azure a ukládá je v běžné místní sdílenou složkou. 

Tady jsou důležité věci vědět o trasování kolekce:
 
* Kolekce trasování běží nepřetržitě se výchozí omezení velikosti. Je výchozí maximální velikost povolená pro každý soubor (200 MB) **není** konečného velikost. Zkontrolujte velikost dochází pravidelně (aktuálně každé dvě minuty) a pokud je aktuální soubor > = 200 MB, bude uložený a se generuje nový soubor. Je také 8 GB (Konfigurovat) omezení na celkové velikosti generovaný podle relace události. Po dosažení tohoto limitu jsou nejstarší soubory odstranit, protože se vytvářejí nové.
* Je na protokoly maximální stáří 5 dní. Tento limit je také možné konfigurovat. 
* Jednotlivé komponenty definuje vlastnosti konfigurace trasování pomocí souboru JSON. Soubory JSON jsou uloženy v `C:\TraceCollector\Configuration`. V případě potřeby tyto soubory lze upravit změnit limity stáří a velikost shromažďovat protokoly. Změny na tyto soubory vyžadovat restartování *kolekce trasování zásobníku Microsoft Azure* služby, aby se změny projevily.
* V následujícím příkladu je soubor JSON Konfigurace trasování pro operace FabricRingServices z XRP virtuálního počítače: 

```
{
    "LogFile": 
    {
        "SessionName": "FabricRingServicesOperationsLogSession",
        "FileName": "\\\\SU1FileServer\\SU1_ManagementLibrary_1\\Diagnostics\\FabricRingServices\\Operations\\AzureStack.Common.Infrastructure.Operations.etl",
        "RollTimeStamp": "00:00:00",
        "MaxDaysOfFiles": "5",
        "MaxSizeInMB": "200",
        "TotalSizeInMB": "5120"
    },
    "EventSources":
    [
        {"Name": "Microsoft-AzureStack-Common-Infrastructure-ResourceManager" },
        {"Name": "Microsoft-OperationManager-EventSource" },
        {"Name": "Microsoft-Operation-EventSource" }
    ]
}
```

* **MaxDaysOfFiles**

    Tento parametr řídí stáří souborů, které chcete zachovat. Starší soubory protokolu se odstraní.
* **Hodnota parametru MaxSizeInMB**

    Tento parametr řídí velikost prahovou hodnotu pro jeden soubor. Když je dosaženo velikost, se vytvoří nový soubor ETL.
* **TotalSizeInMB**

    Tento parametr řídí celková velikost souborů .etl vygenerovat z relace události. Pokud celkové velikosti je větší než hodnota tohoto parametru, starší soubory se odstraní.
  
## <a name="log-collection-tool"></a>Nástroj pro shromažďování protokolů
 
Příkaz prostředí PowerShell `Get-AzureStackLog` slouží k shromažďování protokolů ze všech komponent v prostředí Azure zásobníku. Ukládá je do zip soubory v umístění definovaný uživatelem. Pokud náš tým technické podpory potřebuje vaše protokoly, které pomůžou vyřešit nějaký problém, se může požádat o spuštění tohoto nástroje.

> [!CAUTION]
> Tyto soubory protokolu může obsahovat identifikovatelné osobní údaje (PII). Vzít v úvahu před veřejně post všechny soubory protokolů.
 
Následuje několik příkladů typů protokolu, které byly shromážděny:
*   **Protokoly nasazení Azure zásobníku**
*   **Protokoly událostí systému Windows**
*   **Protokoly Panther**

   Chcete-li vyřešit problémy vytvoření virtuálního počítače:
*   **Protokoly clusteru**
*   **Diagnostické protokoly úložiště**
*   **Protokoly trasování událostí pro Windows**

Tyto soubory jsou shromážděných trasování kolekce a uložených ve sdílené složce, odkud `Get-AzureStackLog` je obnovuje.
 
**Ke spuštění Get-AzureStackLog v systému Azure zásobníku Development Kit (ASDK)**
1.  Přihlaste se jako AzureStack\AzureStackAdmin na hostiteli.
2.  Otevřete okno prostředí PowerShell jako správce.
3.  Spusťte `Get-AzureStackLog`.  

    **Příklady**

    - Shromážděte všechny protokoly pro všechny role:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs`

    - Shromažďování protokolů z virtuálních počítačů a BareMetal rolí:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal`

    - Shromažďování protokolů z role virtuálních počítačů a BareMetal s datem filtrování za posledních 8 hodin pro soubory protokolů:

        `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8)`

    - Shromažďování protokolů z role virtuálních počítačů a BareMetal filtrování pro soubory protokolu pro toto časové období před 8 hodin až 2 hodiny před datem:

      `Get-AzureStackLog -OutputPath C:\AzureStackLogs -FilterByRole VirtualMachines,BareMetal -FromDate (Get-Date).AddHours(-8) -ToDate (Get-Date).AddHours(-2)`

**Ke spuštění Get-AzureStackLog v zásobníku Azure integrované systému:**

Chcete-li spustit nástroj kolekce protokolu na integrovaný systém, mají přístup do privilegované koncového bodu (období). Tady je ukázkového skriptu můžete spustit pomocí období pro shromažďování protokolů na integrovaný systém:

```
$ip = "<IP OF THE PEP VM>" # You can also use the machine name instead of IP here.
 
$pwd= ConvertTo-SecureString "<CLOUD ADMIN PASSWORD>" -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("<DOMAIN NAME>\CloudAdmin", $pwd)
 
$shareCred = Get-Credential
 
$s = New-PSSession -ComputerName $ip -ConfigurationName PrivilegedEndpoint -Credential $cred

$fromDate = (Get-Date).AddHours(-8)
$toDate = (Get-Date).AddHours(-2)  #provide the time that includes the period for your issue
 
Invoke-Command -Session $s {    Get-AzureStackLog -OutputPath "\\<HLH MACHINE ADDREESS>\c$\logs" -OutputSharePath "<EXTERNAL SHARE ADDRESS>" -OutputShareCredential $using:shareCred  -FilterByRole Storage -FromDate $using:fromDate -ToDate $using:toDate}

if($s)
{
    Remove-PSSession $s
}
```

- Pokud shromažďujete protokoly období, zadejte `OutputPath` parametr na umístění na počítači HLH. Ujistěte se také, že umístění je zašifrovaná.
- Parametry `OutputSharePath` a `OutputShareCredential` jsou volitelné a používají se při odeslání protokolů k externí sdílené složce. Použít tyto parametry *kromě* k `OutputPath`. Pokud `OutputPath` není zadán, nástroje kolekce protokol používá systémové jednotce virtuálního počítače období pro úložiště. To může způsobit skript se nezdařila, protože je omezená místo na disku.
- Jak je uvedeno v předchozím příkladu `FromDate` a `ToDate` parametry lze shromažďovat protokoly pro konkrétní časové období. To může se hodit pro scénáře, jako je shromažďování protokolů po použití balíčku aktualizace na integrovaný systém.

**Parametr aspekty ASDK a integrované systémy:**

- Pokud `FromDate` a `ToDate` nebyly zadány parametry, se shromáždí protokoly pro poslední čtyři hodiny ve výchozím nastavení.
- Můžete použít `TimeOutInMinutes` parametr nastavit časový limit pro shromažďování protokolů. Ve výchozím nastavení, jinak je nastavená na 150 (2,5 hodiny).

- V současné době můžete použít `FilterByRole` parametr do kolekce filtrů protokolu podle následujících rolí:

   |   |   |   |
   | - | - | - |
   | `ACSMigrationService`     | `ACSMonitoringService`   | `ACSSettingsService` |
   | `ACS`                     | `ACSFabric`              | `ACSFrontEnd`        |
   | `ACSTableMaster`          | `ACSTableServer`         | `ACSWac`             |
   | `ADFS`                    | `ASAppGateway`           | `BareMetal`          |
   | `BRP`                     | `CA`                     | `CPI`                |
   | `CRP`                     | `DeploymentMachine`      | `DHCP`               |
   |`Domain`                   | `ECE`                    | `ECESeedRing`        |        
   | `FabricRing`              | `FabricRingServices`     | `FRP`                |
   |` Gateway`                 | `HealthMonitoring`       | `HRP`                |               
   | `IBC`                     | `InfraServiceController` | `KeyVaultAdminResourceProvider`|
   | `KeyVaultControlPlane`    | `KeyVaultDataPlane`      | `NC`                 |            
   | `NonPrivilegedAppGateway` | `NRP`                    | `SeedRing`           |
   | `SeedRingServices`        | `SLB`                    | `SQL`                |     
   | `SRP`                     | `Storage`                | `StorageController`  |
   | `URP`                     | `UsageBridge`            | `VirtualMachines`    |  
   | `WAS`                     | `WASPUBLIC`              | `WDS`                |


Pár dalších věcí si:

* Příkaz přijímá chvíli ke spuštění na základě na rolí, které shromažďují v protokolech. Přispívajících faktorů také zahrnovat doby trvání, zadaný pro shromažďování protokolů a počtu uzlů v prostředí Azure zásobníku.
* Po dokončení shromažďování protokolů zkontrolujte do nové složky vytvořené v `-OutputPath` zadaný v příkazu parametr.
* Každá role má protokoly uvnitř zip jednotlivé soubory. V závislosti na velikosti shromažďovat protokoly může mít roli protokoly rozdělit do několika souborů zip. Pro roli Pokud chcete mít všechny soubory protokolu v rozbalené do jediné složky, použijte nástroj, který můžete rozbalte hromadně (například 7zip). Vyberte všechny komprimované soubory pro roli a vyberte **extrahovat zde**. To unzips všechny soubory protokolu pro tuto roli v jedné sloučené složce.
* Soubor nazývá `Get-AzureStackLog_Output.log` se také vytvoří ve složce, která obsahuje soubory komprimované protokolu. Tento soubor je protokolu výstupu příkazu, který můžete použít při řešení problémů během shromáždění protokolů.
* Prozkoumat konkrétní chyby, může být potřeba protokolů z více než jedna součást.
    -   Systém a protokoly událostí pro všechny virtuální počítače infrastruktury se shromažďují v *VirtualMachines* role.
    -   Systém a protokoly událostí pro všechny hostitele se shromažďují v *BareMetal* role.
    -   Protokoly událostí clusteru převzetí služeb při selhání a technologie Hyper-V se shromažďují v *úložiště* role.
    -   Protokoly služby ACS se shromažďují v *úložiště* a *ACS* role.

> [!NOTE]
> Velikost a stáří limity prosazují na protokoly shromážděny, protože to je nezbytné k zajištění efektivní využití prostoru úložiště zajistit, že není získají přenášeny s protokoly. Při diagnostice problém někdy však protokoly, které už možná neexistuje z důvodu těchto mezních hodnot. Z toho důvodu je **důrazně doporučujeme** snižování zátěže protokolů k prostorem externího úložiště (účet úložiště v Azure, zařízení s další místní úložiště atd.) každých 8 až 12 hodin a zůstanou existuje pro 1 – 3 měsíce, v závislosti na vaší požadavky. Také zkontrolujte, zda že toto umístění úložiště je zašifrovaná.

## <a name="next-steps"></a>Další kroky
[Řešení potíží s Microsoft Azure zásobníku](azure-stack-troubleshooting.md)
