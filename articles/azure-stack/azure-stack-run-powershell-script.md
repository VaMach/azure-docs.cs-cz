<properties
    pageTitle="Nasazení Azure Stack POC | Microsoft Azure"
    description="Zjistěte, jak připravit Azure Stack POC, a spusťte skript prostředí PowerShell, který nasadí Azure Stack POC."
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


# Nasazení Azure Stack POC
Chcete-li nasadit Azure Stack POC, nejprve musíte [připravit počítač nasazení](#prepare-the-deployment-machine) a poté [spustit skript prostředí PowerShell pro nasazení](#run-the-powershell-deployment-script).

## Příprava počítače nasazení

1. Ujistěte se, že se můžete k počítači nasazení fyzicky připojit nebo že máte přístup k fyzické konzole (například KVM). Takový přístup budete potřebovat po restartování počítače nasazení v kroku 9.
 
2. Ujistěte se, že počítač nasazení splňuje [minimální požadavky](azure-stack-deploy.md). K potvrzení požadavků můžete použít [Kontrolu nasazení Azure Stack Technical Preview 1](https://gallery.technet.microsoft.com/Deployment-Checker-for-76d824e1).

3.  [Stáhněte](http://aka.ms/ReqOSforAzureStack) si a nainstalujte Windows Server 2016 Datacenter Edition Technical Preview 4 EM-US (Full Edition).

4.  [Stáhněte](https://azure.microsoft.com/overview/azure-stack/try/?v=try) si balíček pro nasazení Azure Stack POC do složky na jednotce C (například c:\\AzureStack).

5.  Spusťte soubor **Microsoft Azure Stack POC.exe**.

    Tím se vytvoří složka \\Microsoft Azure Stack POC\\, která obsahuje tyto soubory:

    -   DeployAzureStack.ps1: skript prostředí PowerShell, který slouží k instalaci Azure Stack POC

    -   MicrosoftAzureStackPOC.vhdx: balíček dat služby Azure Stack

    -   SQLServer2014.vhdx: SQL Server VHD

    -   WindowsServer2012R2DatacenterEval.vhd

    -   WindowsServer2016Datacenter.vhdx: Windows Server 2016 Datacenter VHD (zahrnuje KB 3124262)

    > [AZURE.IMPORTANT] Musíte mít minimálně 128 GB volného místa na fyzickém spouštěcím svazku.

6. Zkopírujte soubor WindowsServer2016Datacenter.vhdx do jednotky C:\ a přejmenujte jej na MicrosoftAzureStackPOCBoot.vhdx.

7. V Průzkumníku souborů klikněte pravým tlačítkem na soubor MicrosoftAzureStackPOCBoot.vhdx a klikněte na **Připojit**.

8. Otevřete okno příkazového řádku jako správce a spusťte následující příkaz bcdboot. Tento příkaz vytvoří prostředí se současným používáním více operačních systémů. Od této chvíle by se měl systém spouštět použitím horní možnosti spuštění.

        bcdboot <mounted drive letter>:\windows

9. Restartujte počítač. Během přípravy systému VHD se automaticky spustí instalační program systému Windows. Od této chvíle se budete muset fyzicky připojit k počítači nasazení nebo musíte mít přístup k fyzické konzole, abyste mohli dokončit následující kroky.

10. Pokud váš systém BIOS takovou možnost obsahuje, měli byste jej nakonfigurovat, aby používal místní čas namísto času UTC.

11. Jakmile budete vyzváni, zadejte svou zemi, jazyk, klávesnici a další předvolby. Pokud budete vyzváni k zadání kódu Product Key, najdete jej v článku [Systémové požadavky a instalace](https://technet.microsoft.com/library/mt126134.aspx).

12. Po zobrazení výzvy nastavte heslo pro účet správce a následně se přihlaste pomocí názvu a hesla tohoto účtu.

13. Pokud po restartování a přihlášení nemáte protokol DHCP, nastavte statickou konfiguraci síťové karty.

14. Povolte připojení ke vzdálené ploše tak, že přejdete do **Vlastností systému** a vyberete možnost **Povolit vzdálená připojení k tomuto počítači**.

15. Přihlaste se pomocí místního účtu s oprávněními správce.

16. Ověřte, že máte pro data Azure Stack POC **přesně** čtyři jednotky:
  - Jsou viditelné ve správě disků
  - Nejsou používané
  - Zobrazují se jako Online, volné

17. Ověřte, že hostitel není připojený k doméně.

18. Pomocí aplikace Internet Explorer, ověřte síťové připojení k webu Azure.com.

> [AZURE.IMPORTANT] Nasazení TP1 POC podporuje přesně čtyři jednotky pro funkce úložiště a pouze jednu síťovou kartu pro sítě.
>
> - **Pro úložiště** – použijte správce zařízení nebo rozhraní WMI k zakázání všech ostatních jednotek (prosté odstavení disků prostřednictvím správce disků nestačí).
>
> - **Pro sítě** – pokud máte více síťových karet, pak se před spuštěním skriptu nasazení uvedeného níže ujistěte, že je povolená pouze jedna karta (a všechny ostatní jsou zakázané).
>
> Pokud jste postupovali podle výše uvedených kroků pro spuštění z VHD, budete muset provést tyto aktualizace po spuštění VHD, ještě před spuštěním skriptu nasazení.

## Spuštění skriptu prostředí PowerShell pro nasazení

1. Přesuňte následující soubory nasazení ze složky D:\Microsoft Azure Stack POC\ do C:\Microsoft Azure Stack POC.\.
    - DeployAzureStack.ps1
    - MicrosoftAzureStackPOC.vhdx
    - SQLServer14.vhdx
    - WindowsServer2012R2DatacenterEval.vhd
    - WindowsServer2016Datacenter.vhdx

2.  Spusťte PowerShell jako správce.

3.  V prostředí PowerShell přejděte do umístění složky služby Azure Stack (\\Microsoft Azure Stack POC\\, pokud jste použili výchozí).

4.  Spusťte příkaz nasazení:

        .\DeployAzureStack.ps1 –Verbose

    V Číně použijte následující příkaz:

        .\DeployAzureStack.ps1 –Verbose -UseAADChina $true

    Spustí se nasazení a název domény Azure Stack POC je pevně zakódovaný jako azurestack.local.

5.  Na výzvu **Zadejte heslo předdefinovaného správce** zadejte heslo a potvrďte jej. Jedná se o heslo ke všem virtuálním počítačům. Nezapomeňte si toto heslo správce služby poznamenat.

6.  Na výzvu **Přihlaste se ke svému účtu Azure na místní stránce ověřování Azure** stisknutím libovolné klávesy otevřete dialogové okno přihlášení k Microsoft Azure.

7.  Zadejte přihlašovací údaje ke svému účtu Azure Active Directory. Tento uživatel musí mít v tenantu Active Directory roli globálního správce.

8.  Po návratu do prostředí PowerShell na výzvu k potvrzení výběru účtu zadejte *y*. Tím se v daném tenantu Active Directory vytvoří dva uživatelé a tři aplikace služby Azure Stack: uživatel s rolí správce pro službu Azure Stack, uživatel tenanta pro testy TiP a po jedné aplikaci pro Portal, rozhraní API a monitorování poskytovatelů prostředků. Kromě toho instalační program do tohoto tenanta Active Directory přidá souhlasy pro Azure PowerShell, rozhraní příkazového řádku XPlat a sadu Visual Studio.

9.  Na výzvu **Microsoft Azure Stack POC je připraven k nasazení. Chcete pokračovat?** zadejte *y*.

10.  Proces nasazení potrvá několik hodin, během kterých dojde k několika automatickým restartováním systému. Přihlášení v průběhu nasazování automaticky otevře okno prostředí PowerShell s průběhem nasazení. Okno prostředí PowerShell se samo zavře po dokončení nasazení.

11. Na počítači Azure Stack POC se přihlaste jako AzureStack\administrator, otevřete **Správce serveru** a vypněte **Konfiguraci rozšířeného zabezpečení aplikace Internet Explorer** pro správce i uživatele.

Pokud se nasazení nezdaří s chybou času nebo data, nakonfigurujte systém BIOS, aby požíval místní čas namísto času UTC. Poté znovu proveďte nasazení.

Pokud skript selže, restartujte jej. Pokud bude nadále selhávat, proveďte vymazání a restartujte jej.

Protokoly skriptu jsou umístěné na hostiteli POC ve složce `C:\ProgramData\microsoft\azurestack`.

### Volitelné parametry skriptu DeployAzureStack.ps1

**AADCredential** (PSCredential) – Nastaví uživatelské jméno a heslo k Azure Active Directory. Není-li tento parametr zadán, skript vás vyzve k zadání uživatelského jména a hesla.

**AADTenant** (String) – Nastaví adresář tenanta. Tento parametr použijte k určení konkrétního adresáře v případě, že má účet AAD oprávnění ke správě více adresářů. Není-li tento parametr zadán, skript vás vyzve k zadání adresáře.

**AdminPassword** (SecureString) – Nastaví výchozí heslo správce. Není-li tento parametr zadán, skript vás vyzve k zadání hesla.

**Force** (Switch) – Nastaví rutinu, aby se mohla spustit bez potvrzení.

**NATVMStaticGateway** (String) – Nastaví výchozí bránu, která se používá ve statických IP adresách pro NATVM. Tento parametr použijte jenom v případě, že protokol DHCP nemůže přiřadit platnou IP adresu pro přístup k internetu. Použijete-li tento parametr, pak je nutné použít i parametr NATVMStaticIP.
Například: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24 – NATVMStaticGateway 10.10.10.1`

**NATVMStaticIP** (String) – Nastaví další statickou IP adresu pro NATVM. Tento parametr použijte jenom v případě, že protokol DHCP nemůže přiřadit platnou IP adresu pro přístup k internetu.
Například: `.\DeployAzureStack.ps1 –Verbose -NATVMStaticIP 10.10.10.10/24`

**NoAutoReboot** (Switch) – Nastaví skript, aby běžel bez automatického restartování počítače.

**ProxyServer** (String) – Nastaví informace o proxy serveru. Tento parametr použijte pouze v případě, že vaše prostředí musí použít proxy server pro přístup k Internetu. Proxy servery, které vyžadují přihlašovací údaje, nejsou podporovány.
Například: `.\DeployAzureStack.ps1 -Verbose -ProxyServer 172.11.1.1:8080`

**PublicVLan** (String) – Nastaví ID sítě VLAN. Tento parametr použijte pouze v případě, že hostitel a NATVM musí konfigurovat ID sítě VLAN pro přístup k fyzické síti (a k internetu).
Například: `.\DeployAzureStack.ps1 –Verbose –PublicVLan 305`

**TIPServiceAdminCredential** (PSCredential) – Nastaví přihlašovací údaje stávajícího správce účtu Azure Active Directory. Tento účet je používán pro účely TiP (Testování ve výrobě). Není-li tento parametr zadán, účet se vytvoří automaticky.

**TIPTenantAdminCredential** (PSCredential) – Nastaví přihlašovací údaje stávajícího účtu Azure Active Directory správce tenanta. Tento účet je používán pro účely TiP (Testování ve výrobě). Není-li tento parametr zadán, účet se vytvoří automaticky.

**UseAADChina**(Boolean) – Nastavte tento logický parametr na hodnotu $true, pokud chcete nasadit Microsoft Azure Stack POC s Azure Čína (Mooncake).

## Vypnout automatické testy TiP (volitelné)

Microsoft Azure Stack Technical Preview 1 obsahuje sadu ověřovacích testů, které se používají během procesu nasazení a v rámci opakovaného denního plánu. Tyto testy simulují akce přijímané tenantem služby Azure Stack a za účelem spouštění testů vytváří v Azure Active Directory uživatelské účty TiP (Test-in-POC). Po úspěšném nasazení můžete tyto testy TiP vypnout. 

**Vypnutí automatických testů TiP**

  - Na klientském virtuálním počítači spusťte následující rutinu:

  `Disable-ScheduledTask -TaskName AzureStackSystemvalidationTask`

**Zobrazení výsledků testů**

  - Na klientském virtuálním počítači spusťte následující rutinu:

  `Get-AzureStackTiPTestsResult`



## Vypnout telemetrii pro Microsoft Azure Stack POC (volitelné)


Před nasazením Microsoft Azure Stack POC můžete vypnout telemetrii pro službu Microsoft Azure Stack na počítači, ze kterého se nasazení provádí. Informace o vypnutí této funkce na jednom počítači najdete v článku na adrese [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq), a změňte nastavení **Diagnostická data a data o využití** na **Základní**.



Po nasazení Microsoft Azure Stack POC můžete vypnout telemetrii na všech virtuálních počítačích, které se připojily k doméně služby Azure Stack. Informace o vytvoření zásady skupiny a správě nastavení telemetrie na těchto virtuálních počítačích najdete v článku na adrese [https://technet.microsoft.com/library/mt577208(v=vs.85).aspx\#BKMK\_UTC](https://technet.microsoft.com/library/mt577208%28v=vs.85%29.aspx#BKMK_UTC), a pro zásadu skupiny **Povolit telemetrii** vyberte hodnotu **0** nebo **1**. Existují dva virtuální počítače (bgpvm a natvm), které se nepřipojují k doméně služby Azure Stack. Informace o změně nastavení Zpětné vazby a Diagnostiky zvlášť na těchto virtuálních počítačích najdete v článku na adrese [http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq](http://windows.microsoft.com/en-us/windows-10/feedback-diagnostics-privacy-faq).

## Další kroky

[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)



<!--HONumber=Sep16_HO3-->


