---
title: "Nasazení Azure zásobníku Development Kit | Microsoft Docs"
description: "Zjistěte, jak můžete připravit Azure zásobníku Development Kit a spustit skript prostředí PowerShell, který chcete nasadit."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 0ad02941-ed14-4888-8695-b82ad6e43c66
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/17/2017
ms.author: erikje
ms.openlocfilehash: b67cabf0ecdb48f137bfcfbce95eee568a1c298d
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="deploy-the-azure-stack-development-kit"></a>Nasazení Azure zásobníku Development Kit

*Platí pro: Azure zásobníku Development Kit*

K nasazení [Azure zásobníku Development Kit](azure-stack-poc.md), musíte provést následující kroky:

1. [Stažení balíčku pro nasazení](https://azure.microsoft.com/overview/azure-stack/try/?v=try) získat Cloudbuilder.vhdx.
2. [Příprava cloudbuilder.vhdx](#prepare-the-development-kit-host) spuštěním skriptu asdk installer.ps1 konfigurace počítače (hostitel development kit), na kterém chcete nainstalovat development kit. Po provedení tohoto kroku se spustí Cloudbuilder.vhdx development kit hostitele.
3. [Nasazení development kit](#deploy-the-development-kit) na hostiteli development kit.

> [!NOTE]
> Nejlepších výsledků dosáhnete i v případě, že chcete použít odpojené prostředí zásobníku Azure, je nejvhodnější pro nasazení během připojení k Internetu. Tímto způsobem, zkušební verze systému Windows Server 2016 je možné aktivovat v době nasazení.
> 
> 

## <a name="download-and-extract-the-development-kit"></a>Stažení a extrakci development kit
1. Než začnete stahování, ujistěte se, zda počítač splňuje následující požadavky:

   * Počítač musí mít minimálně 60 GB volného místa na disku.
   * [Rozhraní .NET framework 4.6 (nebo novější verze)](https://aka.ms/r6mkiy) musí být nainstalován.

2. [Přejděte na stránku Začínáme](https://azure.microsoft.com/overview/azure-stack/try/?v=try), zadejte svoje údaje a klikněte na tlačítko **odeslání**.
3. V části **stáhnout software**, klikněte na tlačítko **Azure zásobníku Development Kit**.
4. Spusťte stáhnutý soubor AzureStackDownloader.exe.
5. V **Azure zásobníku Development Kit stahovací** okno, postupujte podle kroků 1 až 5.
6. Po dokončení stahování, klikněte na tlačítko **spustit** ke spuštění MicrosoftAzureStackPOC.exe.
7. Přečtěte si licenční smlouvu obrazovky a informace o Průvodci Self-Extractor a pak klikněte na **Další**.
8. Přečtěte si prohlášení o ochraně osobních údajů obrazovky a informace o Průvodci Self-Extractor a pak klikněte na **Další**.
9. Vyberte cíl pro soubory extrahovány, klikněte na **Další**.
   * Výchozí hodnota je: <drive letter>:\<aktuální složce > \Microsoft Azure zásobníku
10. Zkontrolujte cílové umístění obrazovky a informace o Průvodci Self-Extractor a pak klikněte na tlačítko **extrahovat** extrahovat CloudBuilder.vhdx (~ 25 GB) a ThirdPartyLicenses.rtf soubory. Dokončení tohoto procesu bude chvíli trvat.

> [!NOTE]
> Po extrahování souborů, můžete odstranit soubory exe a Koš chcete zotavit prostor na počítači. Nebo můžete přesunout, že tyto soubory do jiného umístění, že pokud potřebujete znovu nasadit je nebudete muset znovu stáhnout soubory.
> 
> 

## <a name="prepare-the-development-kit-host"></a>Příprava hostitele development kit
1. Ujistěte se, že fyzicky připojit k hostiteli development kit, nebo mají přístup ke konzole fyzické (například KVM). Po restartování hostitele development kit v kroku 13 níže, musí mít takový přístup.
2. Zajistěte, aby hostitel development kit splňuje [minimální požadavky](azure-stack-deploy.md). Můžete použít [kontrolu nasazení pro Azure zásobníku](https://gallery.technet.microsoft.com/Deployment-Checker-for-50e0f51b) potvrďte vašim požadavkům.
3. Přihlaste se jako místní správce na hostiteli development kit.
4. Zkopírovat nebo přesunout soubor CloudBuilder.vhdx kořenové jednotce C:\ (C:\CloudBuilder.vhdx).
5. Spusťte následující skript pro stažení instalačního souboru development kit (asdk-installer.ps1) do složky c:\AzureStack_Installer na hostiteli development kit.
    ```powershell
    # Variables
    $Uri = 'https://raw.githubusercontent.com/Azure/AzureStack-Tools/master/Deployment/asdk-installer.ps1'
    $LocalPath = 'c:\AzureStack_Installer'

    # Create folder
    New-Item $LocalPath -Type directory

    # Download file
    Invoke-WebRequest $uri -OutFile ($LocalPath + '\' + 'asdk-installer.ps1')
    ```
6. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními > spuštění skriptu C:\AzureStack_Installer\asdk-installer.ps1 > klikněte na tlačítko **Příprava prostředí**.
7. Na **vyberte Cloudbuilder vhdx** stránka instalační program, vyhledejte a vyberte soubor cloudbuilder.vhdx, který jste stáhli v předchozích krocích.
8. Volitelné: Zkontrolujte **přidejte ovladače** pole pro zadání složky, který obsahuje další ovladače, které chcete, aby na hostiteli.
9. Na **volitelné nastavení** zadejte účet místního správce pro hostitele development kit. Pokud nezadáte tyto přihlašovací údaje, musíte během procesu instalace níže KVM přístup k hostiteli.
10. Na **volitelné nastavení** stránky, máte možnost nastavit následující:
    - **ComputerName**: Tento parametr nastaví název hostitele development kit. Název musí být v souladu s požadavky na plně kvalifikovaný název domény a musí být maximálně 15 znaků nebo méně. Výchozí hodnota je náhodný název generované systémem Windows.
    - **Časové pásmo**: Nastaví časové pásmo pro hostitele development kit. Výchozí hodnota je (UTC-8:00) Tichomoří (USA a Kanada).
    - **Konfiguraci statické IP adresy**: Nastaví nasazení tak, aby používal statickou adresu IP. Jinak když instalační program restartuje do cloudbuilder.vhx, rozhraní sítě jsou nakonfigurované s DHCP.
11. Klikněte na **Další**.
12. Pokud jste v předchozím kroku vybrali konfiguraci statické IP adresy, musíte se teď:
    - Vyberte síťový adaptér. Ujistěte se, zda se můžete připojit k adaptéru před kliknutím na **Další**.
    - Ujistěte se, že **IP adresu**, **brány**, a **DNS** hodnoty jsou správné a potom klikněte na **Další**.
13. Klikněte na tlačítko **Další** ke spuštění procesu přípravy.
14. Určuje, kdy přípravy **dokončeno**, klikněte na tlačítko **Další**.
15. Klikněte na tlačítko **restartovat nyní** spouštění do cloudbuilder.vhdx a pokračujte v procesu nasazení.

## <a name="deploy-the-development-kit"></a>Nasazení development kit
1. Přihlaste se jako místní správce na hostiteli development kit. Pomocí přihlašovacích údajů zadaných v předchozích krocích.

    > [!IMPORTANT]
    > Pro nasazení služby Azure Active Directory zásobník Azure vyžaduje přístup k Internetu, buď přímo nebo prostřednictvím proxy serveru transparentní. Nasazení podporuje přesně jeden síťový adaptér pro síť. Pokud máte několik síťových adaptérů, ujistěte se, že je povolena pouze jedna (a všechny ostatní jsou zakázány) před spuštěním skriptu nasazení v další části.
    
2. Otevřete konzolu prostředí PowerShell se zvýšenými oprávněními > spuštění skriptu \AzureStack_Installer\asdk-installer.ps1 (který může být na jiné jednotce Cloudbuilder.vhdx) > klikněte na tlačítko **nainstalovat**.
3. V **typ** vyberte **cloudu Azure** nebo **služby AD FS**.
    - **Azure Cloud**: Azure Active Directory je zprostředkovatele identity. Tento parametr použijte k určení konkrétního adresáře kde AAD účet má oprávnění globálního správce. Úplný název klienta služby AAD Directory. Například. onmicrosoft.com. 
    - **Služba AD FS**: výchozí razítko adresářová služba je zprostředkovatele identity, je výchozí účet pro přihlášení s azurestackadmin@azurestack.local, a k použití hesla je zadaný jako součást instalace.
4. V části **heslo místního správce**v **heslo** zadejte heslo místního správce (který musí odpovídat aktuální heslo místního správce nakonfigurované) a pak klikněte na tlačítko **Další**.
5. Vyberte síťový adaptér používat pro development kit a potom klikněte na **Další**.
6. Vyberte DHCP nebo konfigurace statických sítě pro virtuální počítač BGPNAT01.
    - **DHCP** (výchozí): virtuální počítač získá konfigurace sítě IP ze serveru DHCP.
    - **Statické**: tuto možnost použijte pouze v případě DHCP nelze přiřadit platnou IP adresu pro zásobník Azure pro přístup k Internetu. S délkou subnetmask (například 10.0.0.5/24) musí zadat statickou IP adresu.
7. Volitelně můžete nastavte následující hodnoty:
    - **ID sítě VLAN**: Nastaví ID sítě VLAN. Tuto možnost použijte pouze v případě hostitele a AzS BGPNAT01 musíte nakonfigurovat ID sítě VLAN pro přístup k fyzické síti (a Internetu). 
    - **Server DNS pro předávání**: server A DNS je vytvořen jako součást nasazení Azure zásobníku. Pokud chcete umožnit uvnitř řešení překládat názvy mimo razítka, zadejte existující infrastrukturu DNS server. Server DNS v razítko předává Neznámý název k tomuto serveru.
    - **Čas serveru**: to vyžadováno pole nastaví čas serveru a musí být IP adresa. Čas serveru najít IP adresu, navštivte [pool.ntp.org](http:\\pool.ntp.org) nebo příkaz ping time.windows.com. 
8. Klikněte na **Další**. 
9. Na **ověření vlastnostech karty síťového rozhraní** stránky, se zobrazí indikátor průběhu. 
    - Pokud se říká **nelze stáhnout aktualizace**, postupujte podle pokynů na stránce.
    - Když uvádí **dokončeno**, klikněte na tlačítko **Další**.
10. Na **Souhrn** klikněte na tlačítko **nasadit**.
11. Pokud používáte nasazení služby Azure Active Directory, budete vyzváni k zadání přihlašovacích údajů účtu globálního správce Azure Active Directory.
12. Proces nasazení může trvat několik hodin, během které automaticky restartuje se jednou.
   
   > [!IMPORTANT]
   > Pokud chcete sledovat průběh nasazení, přihlaste se jako azurestack\AzureStackAdmin. Pokud se přihlásíte jako místní správce poté, co je počítač připojený k doméně, zobrazí se průběh nasazení. Není znovu spustit nasazení, místo toho se přihlaste jako azurestack\AzureStackAdmin k ověření, zda je spuštěna.
   > 
   > 
   
    Pokud je nasazení úspěšná, zobrazí konzole PowerShell: **COMPLETE: akce, nasazení,**.
   
Pokud se nasazení nezdaří, můžete použít následující skript prostředí PowerShell, spusťte znovu z téhož okna prostředí PowerShell zvýšenými oprávněními:

```powershell
cd c:\CloudDeployment\Setup
.\InstallAzureStackPOC.ps1 -Rerun
```

Tento skript se restartuje nasazení z poslední krok, který bylo úspěšné.

Nebo můžete [znovu nasaďte](azure-stack-redeploy.md) od začátku.


## <a name="reset-the-password-expiration-to-180-days"></a>Obnoví na 180 dnů vypršení platnosti hesla

Poté, co nasadíte, abyste měli jistotu, že heslo pro hostitele development kit není příliš brzy vyprší, postupujte takto:

Změna zásad vypršení platnosti hesla z prostředí Powershell:
1. V okně Powershell, spusťte příkaz; Set-ADDefaultDomainPasswordPolicy - MaxPasswordAge 180.00:00:00-Identity azurestack.local

Změna zásad vypršení platnosti hesla ručně:
1. Na hostiteli development kit, otevřete **Správa zásad skupiny** a přejděte do **Správa zásad skupiny** – **doménové struktury: azurestack.local** – **domény** – **azurestack.local**.
2. Klikněte pravým tlačítkem na **výchozí zásady domény** a klikněte na tlačítko **upravit**.
3. V editoru zásad skupiny správy, přejděte na **konfigurace počítače** – **zásady** – **nastavení systému Windows** – **nastavení zabezpečení**– **Zásady účtů** – **zásady hesel**.
4. V pravém podokně klikněte dvakrát na **maximální stáří hesla**.
5. V **maximální stáří hesla vlastnosti** dialogové okno, změny **heslo vyprší za** hodnoty na 180 a pak klikněte na **OK**.


## <a name="next-steps"></a>Další kroky

[Instalace PowerShellu](azure-stack-powershell-configure-quickstart.md)

[Registrace Azure zásobníku u vašeho předplatného Azure](azure-stack-register.md)

[Připojení ke službě Azure Stack](azure-stack-connect-azure-stack.md)

