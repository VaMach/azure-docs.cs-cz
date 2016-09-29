<properties
    pageTitle="Než nasadíte Azure Stack POC | Microsoft Azure"
    description="Seznamte se s požadavky na prostředí a hardware, které má Azure Stack POC (správce služby)."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/29/2016"
    ms.author="erikje"/>


# Požadavky nasazení Azure Stack

Před nasazením Azure Stack POC ([testování konceptu](azure-stack-poc.md)) se ujistěte, že váš počítač splňuje následující požadavky.
Tyto požadavky platí jenom pro Azure Stack POC a můžou se v budoucích verzích změnit.

Doporučujeme zhlédnout tento videokurz nasazení:

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-deployment-tutorial]

## Hardware

| Komponenta | Minimální  | Doporučené |
|---|---|---|
| Diskové jednotky: Operační systém | Jeden disk s operačním systémem a alespoň 200 GB místa pro systémový oddíl (SSD nebo pevný disk) | Jeden disk s operačním systémem a alespoň 200 GB místa pro systémový oddíl (SSD nebo pevný disk) |
| Diskové jednotky: Obecná data služby Azure Stack POC | Čtyři disky. Každý disk nabízí minimálně 140 GB místa (SSD nebo pevný disk). Použijí se všechny dostupné disky. | Čtyři disky. Každý disk nabízí minimálně 250 GB místa (SSD nebo pevný disk). Použijí se všechny dostupné disky.|
| Výpočetní služby: Procesor | Duální soket: 12 fyzických jader (celkem)  | Duální soket: 16 fyzických jader (celkem) |
| Výpočetní služby: Paměť | 96 GB RAM  | 128 GB RAM |
| Výpočetní služby: BIOS | Povolená technologie Hyper-V (s podporou SLAT)  | Povolená technologie Hyper-V (s podporou SLAT) |
| Síť: NIC | V případě NIC se vyžaduje certifikace Windows Serveru 2012 R2. Specializované funkce se nepožadují | V případě NIC se vyžaduje certifikace Windows Serveru 2012 R2. Specializované funkce se nepožadují |
| Hardwarová certifikace loga | [Certifikováno pro Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0) |[Certifikováno pro Windows Server 2012 R2](http://windowsservercatalog.com/results.aspx?&chtext=&cstext=&csttext=&chbtext=&bCatID=1333&cpID=0&avc=79&ava=0&avq=0&OR=1&PGS=25&ready=0)|

K ověření splnění požadavků můžete použít článek [Kontrola nasazení Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1).

**Konfigurace datových disků:** Všechny datové jednotky musí být stejného typu (buď všechny SAS, nebo všechny SATA) a mít stejnou kapacitu. Pokud použijete disky SAS, musí být diskové jednotky připojené pomocí jedné cesty (žádné funkce MPIO, podpora více cest je zajištěna).

**Možnosti konfigurace adaptéru HBA**
 
- (Upřednostňované) Jednoduchý adaptér HBA
- RAID HBA – adaptér musí být konfigurovaný v režimu průchodu
- RAID HBA – disky musí být konfigurované jako jeden disk, RAID-0

**Podporované kombinace sběrnice a typů médií**

-   Pevný disk SATA

-   Pevný disk SAS

-   Pevný disk RAID

-   RAID SSD (pokud je typ média neurčeno/neznámé\*)

-   SATA SSD + pevný disk SATA

-   SAS SSD + pevný disk SAS

\* Řadiče RAID bez možnosti průchodu nerozpoznají typ média. Tyto řadiče označí pevný disk i SSD jako Neurčeno. V takovém případě se místo mezipaměťových zařízení použije SSD jako trvalé úložiště. Proto můžete na tyto disky SSD nasadit službu Microsoft Azure Stack POC.

**Příklad HBA**: LSI 9207-8i, LSI-9300-8i nebo LSI-9265-8i v režimu průchodu

Dostupné jsou ukázkové OEM konfigurace.




## Operační systém

| | **Požadavky**  |
|---|---|
| **Verze operačního systému** | Windows Server 2016 Datacenter Edition **Technical Preview 4** s nejnovějšími důležitými aktualizacemi. Součástí staženého balíčku je WindowsServer2016Datacenter.vhdx. Můžete spustit systém VHDX a potom ho použít jako základní operační systém pro nasazení služby Azure Stack POC.|
| **Metoda instalace** | Čistá instalace. K rychlé instalaci operačního systému do počítače s Azure Stack POC můžete použít soubor WindowsServer2016Datacenter.vhdx, který je obsažený v balíčku pro nasazení. |
| **Připojení k doméně?** | Ne. |


## Účty Microsoft Azure Active Directory

1. Vytvořte účet Azure AD, který je správcem adresáře nejméně jedné služby Azure Active Directory. Pokud už účet máte, můžete ho použít. V opačném případě si můžete účet vytvořit zdarma na adrese [http://azure.microsoft.com/en-us/pricing/free-trial/](http://azure.microsoft.com/pricing/free-trial/) (v Číně použijte web <http://go.microsoft.com/fwlink/?LinkID=717821>.)

    Uložte si tyto přihlašovací údaje, abyste je mohli použít v kroku 6 postupu [Spuštění powershellového skriptu pro nasazení](azure-stack-run-powershell-script.md#run-the-powershell-deployment-script). Tento účet *správce služby* může konfigurovat a spravovat cloudy prostředků, uživatelské účty, plány tenantů, kvóty a ceny. Na portálu může vytvářet webové cloudy, soukromé cloudy pro virtuální počítače a plány a spravovat předplatné uživatelů.

2. [Vytvořte](azure-stack-add-new-user-aad.md) alespoň jeden účet, abyste se mohli přihlásit ke službě Azure Stack POS jako tenant.

  	| **Účet Azure Active Directory**  | **Podporované?** |
  	|---|---| 
  	| ID organizace s platným veřejným předplatným Azure  | Ano |
  	| Účet Microsoft s platným veřejným předplatným Azure  | Ano |
  	| ID organizace s platným čínským předplatným Azure  | Ano |
  	| ID organizace s platným předplatným Azure pro vládu USA  | Ne |

>[AZURE.NOTE] Azure Stack POC podporuje jenom ověřování Azure Active Directory.


## Network (Síť)

### Přepínač

Jeden dostupný port na přepínači počítače POC.  

Počítač Azure Stack POC podporuje připojení k přístupovému nebo kmenovému portu přepínače. Přepínač nevyžaduje žádné specializované funkce. Pokud používáte kmenový port nebo pokud potřebujete nakonfigurovat ID sítě VLAN, musíte ID sítě VLAN zadat jako parametr nasazení. Příklad:

    DeployAzureStack.ps1 –Verbose –PublicVLan 305

Zadání tohoto parametru nastavíte ID sítě VLAN jenom pro hostitele a NATVM.

### Podsíť

Nepřipojujte počítač POC k podsítím 192.168.200.0/24, 192.168.100.0/24 a 192.168.133.0/24. Ty jsou vyhrazené pro interní sítě v rámci prostředí Microsoft Azure Stack POC.

### IPv4/IPv6

Podporovaný je jenom protokol IPv4. Nemůžete vytvořit sítě IPv6.

### DHCP

Ověřte si dostupnost serveru DHCP v síti, do které se síťová karta připojuje. Pokud server DHCP není dostupný, připravte další statickou síť IPv4 (kromě té, kterou používá hostitel). Tuto IP adresu a bránu zadejte jako parametr nasazení. Příklad:

    DeployAzureStack.ps1 -Verbose -NATVMStaticIP 10.10.10.10/24 -NATVMStaticGateway 10.10.10.1

### Přístup k internetu

Ujistěte se, že se síťová karta může připojit k internetu. IP adresa hostitele i nová IP adresa přiřazená k NATVM (pomocí DHCP nebo statické IP adresy) musí mít přístup k internetu. V rámci domén graph.windows.net a login.windows.net se používají porty 80 a 443.

### Proxy server

Pokud vaše prostředí vyžaduje proxy server, zadejte adresu a port proxy serveru jako parametr nasazení. Příklad:

    DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080

Azure Stack POC nepodporuje ověřování proxy serveru. 

### Telemetrická data

V síti musí být otevřený port 443 (HTTPS). Koncovým bodem klienta je https://vortex-win.data.microsoft.com.


## Další kroky

[Stažení balíčku pro nasazení Azure Stack POC](https://azure.microsoft.com/overview/azure-stack/try/?v=try)

[Nasazení Azure Stack POC](azure-stack-run-powershell-script.md)



<!--HONumber=Sep16_HO3-->


