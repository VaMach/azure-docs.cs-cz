---
title: "Požadavky nasazení Azure zásobníku Development Kit | Microsoft Docs"
description: "Zobrazte prostředí a hardwarové požadavky pro Azure zásobníku Development Kit (operátor cloudu)."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 32a21d9b-ee42-417d-8e54-98a7f90f7311
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/14/2017
ms.author: jeffgilb
ms.openlocfilehash: 8a0d23e14ef50034d5f9595cf154c3513a09c464
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="azure-stack-deployment-prerequisites"></a>Požadavky nasazení Azure Stack

*Platí pro: Azure zásobníku Development Kit*

Před nasazením [Azure zásobníku Development Kit](azure-stack-poc.md), zajistěte, aby váš počítač splňuje následující požadavky:


## <a name="hardware"></a>Hardware
| Komponenta | Minimální | Doporučené |
| --- | --- | --- |
| Diskové jednotky: Operační systém |Jeden disk s operačním systémem a alespoň 200 GB místa pro systémový oddíl (SSD nebo pevný disk) |Jeden disk s operačním systémem a alespoň 200 GB místa pro systémový oddíl (SSD nebo pevný disk) |
| Diskové jednotky: Obecné development kit dat * |Čtyři disky. Každý disk nabízí minimálně 140 GB místa (SSD nebo pevný disk). Použijí se všechny dostupné disky. |Čtyři disky. Každý disk nabízí minimálně 250 GB místa (SSD nebo pevný disk). Použijí se všechny dostupné disky. |
| Výpočetní služby: Procesor |Duální soket: 12 fyzických jader (celkem) |Duální soket: 16 fyzických jader (celkem) |
| Výpočetní služby: Paměť |96 GB RAM |128 GB paměti RAM (to je minimální pro podporu zprostředkovatele prostředků PaaS.)|
| Výpočetní služby: BIOS |Povolená technologie Hyper-V (s podporou SLAT) |Povolená technologie Hyper-V (s podporou SLAT) |
| Síť: NIC |V případě NIC se vyžaduje certifikace Windows Serveru 2012 R2. Specializované funkce se nepožadují |V případě NIC se vyžaduje certifikace Windows Serveru 2012 R2. Specializované funkce se nepožadují |
| Hardwarová certifikace loga |[Certifikované pro systém Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certifikované pro systém Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |

\*Budete potřebovat větší, než to doporučeno kapacity, pokud chcete použít k přidání řadu [položky marketplace](azure-stack-download-azure-marketplace-item.md) z Azure.

**Konfigurace disku dat:** všechny datové jednotky musí být stejného typu (všechny SAS, všechny SATA nebo všechny NVMe) a výkonu. Pokud použijete disky SAS, musí být diskové jednotky připojené pomocí jedné cesty (žádné funkce MPIO, podpora více cest je zajištěna).

**Možnosti konfigurace hostitelského adaptéru**

* (Upřednostňované) Jednoduchý adaptér HBA
* RAID HBA – adaptér musí být konfigurovaný v režimu průchodu
* RAID HBA – disky musí být konfigurované jako jeden disk, RAID-0

**Podporované sběrnice a média zadejte kombinace**

* Pevný disk SATA
* Pevný disk SAS
* Pevný disk RAID
* RAID SSD (pokud je typ média neurčeno/neznámé\*)
* SATA SSD + pevný disk SATA
* SAS SSD + pevný disk SAS
* NVMe

\*Řadičů RAID bez průchozí schopnosti nemůže rozpoznat typ média. Tyto řadiče označí pevný disk i SSD jako Neurčeno. V takovém případě se místo mezipaměťových zařízení použije SSD jako trvalé úložiště. Proto můžete nasadit development kit na těchto jednotkách SSD.

**Příklad HBA**: LSI 9207-8i, LSI-9300-8i nebo LSI-9265-8i v režimu průchodu

Dostupné jsou ukázkové OEM konfigurace.

## <a name="operating-system"></a>Operační systém
|  | **Požadavky** |
| --- | --- |
| **Verze operačního systému** |Windows Server 2012 R2 nebo novější. Verze operačního systému není důležité před zahájením nasazení, jak budete restartovat hostitelský počítač do virtuální pevný disk, který je součástí instalace Azure zásobníku. Všechny požadované opravy a operačního systému jsou již integrované do bitové kopie. Nepoužívejte žádné klíče k aktivaci žádné instance systému Windows Server používá v sadě pro vývoj. |

## <a name="deployment-requirements-check-tool"></a>Zkontrolujte požadavky na nasazení nástroje
Po instalaci operačního systému, můžete použít [kontrolu nasazení pro Azure zásobníku](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) potvrďte, že váš hardware splňuje všechny požadavky.

## <a name="account-requirements"></a>Požadavky na účet
Zpravidla nasazujete, sadě pro vývoj s připojením k Internetu, kde se můžete připojit k Microsoft Azure. V takovém případě musíte nakonfigurovat účet služby Azure Active Directory (Azure AD) k nasazení development kit.

Pokud vaše prostředí není připojený k Internetu nebo nechcete používat Azure AD, můžete nasadit zásobník Azure pomocí služby Active Directory Federation Services (AD FS). Vývoj sada obsahuje vlastní instancí služby AD FS a Active Directory Domain Services. Pokud nasadíte pomocí této možnosti, nemáte vytváření účtů předem.

>[!NOTE]
Pokud nasadíte pomocí možnosti služby AD FS, je nutné znovu nasadit zásobník Azure přejděte do služby Azure AD.

### <a name="azure-active-directory-accounts"></a>Účty Azure Active Directory
Nasazení zásobník Azure pomocí účtu Azure AD, je nutné připravit účet Azure AD, před spuštěním nasazení skript prostředí PowerShell. Tento účet se změní na globálního správce pro tenanta Azure AD. Použije se pro zajišťování a delegovat aplikací a objekty služby pro všechny služby zásobník Azure, které se s Azure Active Directory a rozhraní Graph API. Používá se také jako vlastník předplatného výchozí poskytovatele (který můžete později změnit). Na portálu správce vašeho systému Azure zásobníku se může přihlásit pomocí tohoto účtu.

1. Vytvořte účet Azure AD, který je správcem adresáře pro alespoň jeden Azure AD. Pokud už účet máte, můžete ho použít. Jinak, můžete jej vytvořit zdarma v [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (v Číně, navštivte <http://go.microsoft.com/fwlink/?LinkID=717821> místo). Pokud budete chtít později [zaregistrovat zásobník Azure s Azure](azure-stack-register.md), je také nutné mít předplatné v tomto nově vytvořený účet.
   
    Uložit tyto přihlašovací údaje pro použití v kroku 6 v [nasazení development kit](azure-stack-run-powershell-script.md#deploy-the-development-kit). Tento účet *správce služby* může konfigurovat a spravovat cloudy prostředků, uživatelské účty, plány tenantů, kvóty a ceny. Na portálu může vytvářet webové cloudy, soukromé cloudy pro virtuální počítače a plány a spravovat předplatné uživatelů.
2. [Vytvoření](azure-stack-add-new-user-aad.md) alespoň jeden účet tak, aby se můžete přihlásit development Kit jako klient.
   
   | **Účet Azure Active Directory** | **Podporovány?** |
   | --- | --- |
   | Pracovní nebo školní účet s platným předplatným veřejný Azure |Ano |
   | Účet Microsoft s platným veřejným předplatným Azure |Ano |
   | Pracovní nebo školní účet s platným předplatným Azure Čína |Ano |
   | Pracovní nebo školní účet s platným US Government Azure předplatným |Ano |

## <a name="network"></a>Síť
### <a name="switch"></a>Přepínač
Jeden dostupný port na přepínači pro počítač development kit.  

Vývojovém počítači kit podporuje připojení k přístupu k portu přepínače nebo portu trunk. Přepínač nevyžaduje žádné specializované funkce. Pokud používáte kmenový port nebo pokud potřebujete nakonfigurovat ID sítě VLAN, musíte ID sítě VLAN zadat jako parametr nasazení. Příklady v uvidíte [seznam parametrů nasazení](azure-stack-run-powershell-script.md).

### <a name="subnet"></a>Podsíť
Nespojujte vývojovém počítači kit následující podsítě:

* 192.168.200.0/24
* 192.168.100.0/27
* 192.168.101.0/26
* 192.168.102.0/24
* 192.168.103.0/25
* 192.168.104.0/25

Tyto podsítě jsou vyhrazené pro interní sítě ve vývojovém prostředí sady.

### <a name="ipv4ipv6"></a>IPv4/IPv6
Podporovaný je jenom protokol IPv4. Nemůžete vytvořit sítě IPv6.

### <a name="dhcp"></a>DHCP
Ověřte si dostupnost serveru DHCP v síti, do které se síťová karta připojuje. Pokud server DHCP není dostupný, připravte další statickou síť IPv4 (kromě té, kterou používá hostitel). Tuto IP adresu a bránu zadejte jako parametr nasazení. Příklady v uvidíte [seznam parametrů nasazení](azure-stack-run-powershell-script.md).

### <a name="internet-access"></a>Přístup k internetu
Zásobník Azure vyžaduje přístup k Internetu, buď přímo nebo prostřednictvím proxy serveru transparentní. Azure zásobník nepodporuje konfiguraci webového proxy serveru pro povolení přístupu k Internetu. IP adresa hostitele a nových IP přiřazená MAS-BGPNAT01 (pomocí protokolu DHCP nebo statickou IP adresu) musí být mít přístup k Internetu. V rámci domény graph.windows.net a login.microsoftonline.com používají porty 80 a 443.

## <a name="telemetry"></a>Telemetrická data

Telemetrie pomáhá nám utvářejí budoucích verzích zásobník Azure. Umožňuje nám rychle reagovat na připomínky, zadejte nové funkce a zlepšení kvality. Microsoft Azure Stack zahrnuje Windows Server 2016 a SQL Server 2014. Ani jeden z těchto produktů se změní oproti výchozímu nastavení a jak jsou popsány v prohlášení o ochraně osobních údajů Microsoft Enterprise. Azure zásobníku také obsahuje software s otevřeným zdrojem, který byl změněn na odeslání telemetrie do společnosti Microsoft. Tady jsou některé příklady zásobník Azure telemetrická data:

- informace o registraci nasazení
- Když je výstraha otevřít a zavřít
- počet síťových prostředků

Pro podporu tok dat telemetrie, musí být otevřen ve vaší síti port 443 (HTTPS). Koncovým bodem klienta je https://vortex-win.data.microsoft.com.

Pokud nechcete, aby zajistit telemetrie pro zásobník Azure, můžete ho vypnout v hostiteli kit vývoj a infrastrukturu virtuálních počítačů, jak je popsáno níže.

### <a name="turn-off-telemetry-on-the-development-kit-host-optional"></a>Vypnutí telemetrie na hostiteli development kit (volitelné)

>[!NOTE]
Pokud chcete k vypnutí telemetrie pro vývoj kit hostitele, musíte tak učinit před spuštěním skriptu nasazení.

Před [skriptu asdk installer.ps1]() k nasazení hostitele development kit, přihlaste se do CloudBuilder.vhdx a v okně Powershellu se zvýšenými oprávněními spusťte následující skript:
```powershell
### Get current AllowTelmetry value on DVM Host
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
### Set & Get updated AllowTelemetry value for ASDK-Host 
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name "AllowTelemetry" -Value '0'  
(Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" `
-Name AllowTelemetry).AllowTelemetry
```

Nastavení **AllowTelemetry** na 0 vypne telemetrii pro nasazení systému Windows a protokolů Azure. Jsou odesílány pouze kritické zabezpečení události z operačního systému. Nastavení řídí telemetrie Windows ve všech hostitelích a infrastrukturu virtuálních počítačů a se znovu použije na nové uzly nebo virtuální počítače při výskytu operace škálování.


### <a name="turn-off-telemetry-on-the-infrastructure-virtual-machines-optional"></a>Vypnutí telemetrie na infrastrukturu virtuálních počítačů (volitelné)

Po úspěšné nasazení, spusťte následující skript v okně prostředí PowerShell se zvýšenými oprávněními (jako uživatel AzureStack\AzureStackAdmin) na hostiteli development kit:

```powershell
$AzSVMs= get-vm |  where {$_.Name -like "AzS-*"}
### Show current AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
### Set & Get updated AllowTelemetry value for all AzS-VMs
invoke-command -computername $AzSVMs.name {Set-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name "AllowTelemetry" -Value '0'}
invoke-command -computername $AzSVMs.name {(Get-ItemProperty -Path `
"HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\DataCollection" -Name AllowTelemetry).AllowTelemetry}
```

Konfigurace systému SQL Server telemetrie najdete v tématu [jak nakonfigurovat SQL Server 2016](https://support.microsoft.com/en-us/help/3153756/how-to-configure-sql-server-2016-to-send-feedback-to-microsoft).

### <a name="usage-reporting"></a>Vytváření sestav využití

Prostřednictvím registrace zásobník Azure nastaven také na informace o využití dopředného do Azure. Vytváření sestav využití je řízena nezávisle z telemetrie. Můžete vypnout využití sestav při [registrace](azure-stack-register.md) pomocí skriptu na Githubu. Stačí nastavit **$reportUsage** parametru **$false**.

Data o využití je naformátován jako podrobné v [zásobník Azure sestavy využití dat do Azure](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-usage-reporting). Ve skutečnosti není účtován Azure uživatelé Development Kit zásobníku. Tato funkce je zahrnutá v sadě pro vývoj, mohli otestovat a zkontrolujte, jak funguje generování sestav o využívání. 


## <a name="next-steps"></a>Další kroky
[Stažení balíčku pro nasazení Azure zásobníku development kit](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Nasazení zásobník Azure development kit](azure-stack-run-powershell-script.md)

