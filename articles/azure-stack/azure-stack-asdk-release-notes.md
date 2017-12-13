---
title: "Poznámky k verzi Microsoft Azure zásobníku Development Kit | Microsoft Docs"
description: "Vylepšení, opravy a známé problémy pro Azure zásobníku Development Kit."
services: azure-stack
documentationcenter: 
author: andredm7
manager: femila
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: andredm
ms.openlocfilehash: 49d26defef56e6fc174cda57b24a126cd77227e4
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/12/2017
---
# <a name="azure-stack-development-kit-release-notes"></a>Poznámky k verzi Azure zásobníku Development Kit

*Platí pro: Azure zásobníku Development Kit*

Tyto poznámky k verzi obsahují informace o vylepšení, opravy a známých problémů v Azure zásobníku Development Kit. Pokud si nejste jistí, kterou verzi používáte, můžete [použití portálu ke kontrole](azure-stack-updates.md#determine-the-current-version).

## <a name="build-201711221"></a>Sestavení 20171122.1

### <a name="new-features-and-fixes"></a>Nové funkce a opravy

- Najdete v článku [nové funkce a opravy](azure-stack-update-1711.md#new-features-and-fixes) části k vydání verze update 1711 zásobník Azure pro Azure zásobníku integrované systémy.

    > [!IMPORTANT]
    > Některé z uvedených v položek **nové funkce a opravy** části se vztahují pouze k Azure zásobníku integrované systémy.

### <a name="known-issues"></a>Známé problémy
 
#### <a name="deployment"></a>Nasazení
- Čas serveru podle IP adresy musí zadat během nasazování.

#### <a name="infrastructure-management"></a>Správu infrastruktury
- Nepovolujte infrastruktura zálohování na **infrastruktura zálohování** okno.
- Baseboard management controller, (BMC) IP adresu a model se nezobrazí v uzlu jednotky škálování základní informace. Toto chování se očekává v Azure zásobníku Development Kit.

#### <a name="portal"></a>Portál
- Může se zobrazit prázdný řídicí panel portálu. Chcete-li obnovit řídicí panel, vyberte ikonu ozubené kolečko v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.
- Když máte zobrazeny vlastnosti skupiny prostředků, **přesunout** tlačítko k dispozici. Toto chování je očekávané. Přesunutí skupin prostředků mezi předplatnými není aktuálně podporován.
-  Pro jakýkoli pracovní postup, kde v rozevíracím seznamu vyberte předplatné, skupinu prostředků nebo umístění může zaznamenat jeden nebo více z následujících důvodů:

   - Může se zobrazit prázdný řádek v horní části seznamu. By měl mít pořád povolený výběr položky podle očekávání.
   - Pokud je seznam položek v rozevíracím seznamu krátký, asi nebudete moci zobrazit některé názvy položek.
   - Pokud máte více předplatných uživatele, rozevíracího seznamu skupiny prostředků může být prázdná. 

   Obejít poslední dva problémy, můžete zadat název předplatné nebo skupinu prostředků (pokud ho znáte), nebo můžete místo toho použít PowerShell.

- Zobrazí se **je vyžadována aktivace** upozornění s výzvou k registraci vaší Azure zásobníku Development Kit. Toto chování je očekávané.
- Pokud **součást** po kliknutí na odkaz z libovolného **Role infrastruktury** výstrahy, výsledná **přehled** okno se pokusí načíst a selže. Kromě toho ** přehled ** okno nemá vypršení časového limitu.
- Odstranění odběrů uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstraňte prostředky uživatele nebo skupinu celý prostředků a potom odstraňte odběry uživatele.
- Nemůžete se moci zobrazit oprávnění k předplatnému pomocí portálů zásobník Azure. Jako alternativní řešení můžete ověřit oprávnění pomocí prostředí PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Při pokusu o přidání položky do zásobníku Azure marketplace s použitím **přidat z Azure** možnost, všechny položky můžou být vidět ke stažení.
- Uživatelé mohou procházet na kompletní nabídku marketplace bez předplatného a uvidí položky pro správu jako plány a nabízí. Tyto položky jsou pro uživatele funkční.
 
#### <a name="compute"></a>Compute
- Uživatelé mají možnost vytvoření virtuálního počítače pomocí geograficky redundantní úložiště. Tato konfigurace způsobuje, že vytvoření virtuálního počítače k selhání. 
- Můžete nakonfigurovat dostupnost virtuálního počítače nastavit pouze u domény selhání jednoho a doméně aktualizace jednoho.
- Neexistuje žádné zkušenosti marketplace pro vytvoření sady škálování virtuálního počítače. Můžete vytvořit škálování nastavit pomocí šablony.
- Nastavení škálování pro sady škálování virtuálního počítače nejsou k dispozici na portálu. Jako alternativní řešení, můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdílů verze prostředí PowerShell, je nutné použít `-Name` parametr místo `-VMScaleSetName`.

#### <a name="networking"></a>Sítě
- Nástroj pro vyrovnávání zatížení s veřejnou IP adresu nelze vytvořit pomocí portálu. Jako alternativní řešení můžete použít PowerShell k vytvoření nástroje pro vyrovnávání zatížení.
- Když vytvoříte Vyrovnávání zatížení sítě, musíte vytvořit pravidlo překladu adres sítě. Pokud to neuděláte, obdržíte chybu při pokusu přidat pravidlo NAT po vytvoření nástroje pro vyrovnávání zatížení.
- V části **sítě**, pokud kliknete na tlačítko **připojení** nastavit připojení k síti VPN **VNet-to-VNet** je uveden jako typ možné připojení. Nevybírejte tuto možnost. V současné době pouze **Site-to-site (IPsec)** možnost je podporována.
- Veřejnou IP adresu z virtuálního počítače (VM) nelze zrušit přidružení, po virtuálního počítače byla vytvořena a související s touto adresou IP. Zrušení přidružení se zobrazí postup, ale pořád přidruženy původní virtuální počítač dříve přiřazenou veřejnou IP adresu. K tomuto chování dochází i v případě, že je přiřadit IP adresu na nový virtuální počítač (obvykle označuje jako *prohození*). Všechny budoucí pokusy o připojení přes tuto IP adresu povede připojení do původně přidružený virtuální počítač a ne do nového. V současné době je nutné pouze použít nové veřejné IP adresy pro vytvoření nového virtuálního počítače.
- Operátory Azure zásobníku může nelze nasadit, odstraňovat, upravovat virtuální sítě nebo skupiny zabezpečení sítě. Tento problém je primárně zobrazit na následné aktualizace pokusy stejného balíčku. To je způsobeno problémem balení s aktualizaci, která je aktuálně šetření.
- Interní Vyrovnávání zatížení (ILB) nesprávně zpracovává adresy MAC pro virtuální počítače back-end, které dělí instance systému Linux.
 
#### <a name="sqlmysql"></a>SQL nebo MySQL 
- To může trvat až jednu hodinu, než klienti databáze můžete vytvářet v nové SQL nebo MySQL SKU. 
- Vytvoření položky přímo na SQL a MySQL hostitelské servery, které nejsou prováděné poskytovatelem prostředků není podporována a může mít za následek neodpovídající stavu.

#### <a name="app-service"></a>App Service
- Uživatel musí zaregistrovat zprostředkovatele prostředku úložiště dřív, než vytvoří jejich první funkce Azure v rámci předplatného.
 
#### <a name="usage-and-billing"></a>Využití a fakturace
- Veřejná data měření využití IP adres zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítka, který ukazuje vytvoření záznamu. Tato data v současné době nelze použít pro monitorování přesné veřejnou IP adresu využití.

#### <a name="identity"></a>Identita

V Azure Active Directory Federation Services (ADFS) nasazené prostředí, **azurestack\azurestackadmin** účet už není vlastníkem předplatného výchozí zprostředkovatel. Místo protokolování do **portál pro správu nebo koncový bod adminmanagement** s **azurestack\azurestackadmin**, můžete použít **azurestack\cloudadmin** účet, takže která spravujete a používáte výchozí zprostředkovatel odběru.

> [!IMPORTANT]
> I v **azurestack\cloudadmin** účet je vlastníkem předplatného poskytovatele výchozí v prostředí služby AD FS nasadit, nemá oprávnění pro připojení RDP na hostiteli. Nadále používat **azurestack\azurestackadmin** účet nebo účet místního správce pro přihlášení, přístup a spravovat hostitele, podle potřeby.


## <a name="build-201710201"></a>Sestavení 20171020.1

### <a name="improvements-and-fixes"></a>Vylepšení a opravy

Seznam vylepšení a opravy 20171020.1 sestavení najdete v sekci [vylepšení a opravy](azure-stack-update-1710.md#improvements-and-fixes) část v poznámkách k verzi 1710 zásobník Azure integrované systémy. Některé položky uvedené v části "Další kvalitou vylepšení a opravy" souvisí pouze s integrované systémy.

Navíc byly provedeny následující opravy:
- Opravit problém, kde poskytovatele výpočetních prostředků zobrazí neznámého stavu.
- Opravit problém, kde kvóty pravděpodobně nezobrazí na portálu správce po jejich vytvoření a později se pokusíte zobrazit podrobnosti o plánu.

### <a name="known-issues"></a>Známé problémy

#### <a name="powershell"></a>PowerShell
- Verze modulu PowerShell AzureRM 1.2.11 obsahuje seznam nejnovější změny. Informace o upgradu z 1.2.10 verze, najdete v článku [příručka k migraci](https://aka.ms/azspowershellmigration).
 
#### <a name="deployment"></a>Nasazení
- Čas serveru podle IP adresy musí zadat během nasazování.

#### <a name="infrastructure-management"></a>Správu infrastruktury
- Nepovolujte infrastruktura zálohování na **infrastruktura zálohování** okno.
- Baseboard management controller, (BMC) IP adresu a model se nezobrazí v uzlu jednotky škálování základní informace. Toto chování se očekává v Azure zásobníku Development Kit.

#### <a name="portal"></a>Portál
- Může se zobrazit prázdný řídicí panel portálu. Chcete-li obnovit řídicí panel, vyberte ikonu ozubené kolečko v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.
- Když máte zobrazeny vlastnosti skupiny prostředků, **přesunout** tlačítko k dispozici. Toto chování je očekávané. Přesunutí skupin prostředků mezi předplatnými není aktuálně podporován.
-  Pro jakýkoli pracovní postup, kde v rozevíracím seznamu vyberte předplatné, skupinu prostředků nebo umístění může zaznamenat jeden nebo více z následujících důvodů:

   - Může se zobrazit prázdný řádek v horní části seznamu. By měl mít pořád povolený výběr položky podle očekávání.
   - Pokud je seznam položek v rozevíracím seznamu krátký, asi nebudete moci zobrazit některé názvy položek.
   - Pokud máte více předplatných uživatele, rozevíracího seznamu skupiny prostředků může být prázdná. 

   Obejít poslední dva problémy, můžete zadat název předplatné nebo skupinu prostředků (pokud ho znáte), nebo můžete místo toho použít PowerShell.

- Zobrazí se **je vyžadována aktivace** upozornění s výzvou k registraci vaší Azure zásobníku Development Kit. Toto chování je očekávané.
- V **je vyžadována aktivace** upozornění podrobností o výstrahách, neklikejte na tento odkaz **AzureBridge** součásti. V takovém případě **přehled** okno se neúspěšně pokusí načíst, a nebude vypršení časového limitu.
- V portálu správce se může zobrazit **Chyba načítání klienty** došlo k chybě v **oznámení** oblasti. Tuto chybu můžete bezpečně ignorovat.
- Odstranění odběrů uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstraňte prostředky uživatele nebo skupinu celý prostředků a potom odstraňte odběry uživatele.
- Nemůžete se moci zobrazit oprávnění k předplatnému pomocí portálů zásobník Azure. Jako alternativní řešení můžete ověřit oprávnění pomocí prostředí PowerShell.
 
#### <a name="marketplace"></a>Marketplace
- Při pokusu o přidání položky do zásobníku Azure marketplace s použitím **přidat z Azure** možnost, všechny položky můžou být vidět ke stažení.
- Uživatelé mohou procházet na kompletní nabídku marketplace bez předplatného a uvidí položky pro správu jako plány a nabízí. Tyto položky jsou pro uživatele funkční.
 
#### <a name="compute"></a>Compute
- Uživatelé mají možnost vytvoření virtuálního počítače pomocí geograficky redundantní úložiště. Tato konfigurace způsobuje, že vytvoření virtuálního počítače k selhání. 
- Můžete nakonfigurovat dostupnost virtuálního počítače nastavit pouze u domény selhání jednoho a doméně aktualizace jednoho.
- Neexistuje žádné zkušenosti marketplace pro vytvoření sady škálování virtuálního počítače. Můžete vytvořit škálování nastavit pomocí šablony.
- Nastavení škálování pro sady škálování virtuálního počítače nejsou k dispozici na portálu. Jako alternativní řešení, můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdílů verze prostředí PowerShell, je nutné použít `-Name` parametr místo `-VMScaleSetName`.

#### <a name="networking"></a>Sítě
- Nástroj pro vyrovnávání zatížení s veřejnou IP adresu nelze vytvořit pomocí portálu. Jako alternativní řešení můžete použít PowerShell k vytvoření nástroje pro vyrovnávání zatížení.
- Když vytvoříte Vyrovnávání zatížení sítě, musíte vytvořit pravidlo překladu adres sítě. Pokud to neuděláte, obdržíte chybu při pokusu přidat pravidlo NAT po vytvoření nástroje pro vyrovnávání zatížení.
- V části **sítě**, pokud kliknete na tlačítko **připojení** nastavit připojení k síti VPN **VNet-to-VNet** je uveden jako typ možné připojení. Nevybírejte tuto možnost. V současné době pouze **Site-to-site (IPsec)** možnost je podporována.
- Veřejnou IP adresu z virtuálního počítače (VM) nelze zrušit přidružení, po virtuálního počítače byla vytvořena a související s touto adresou IP. Zrušení přidružení se zobrazí postup, ale pořád přidruženy původní virtuální počítač dříve přiřazenou veřejnou IP adresu. K tomuto chování dochází i v případě, že je přiřadit IP adresu na nový virtuální počítač (obvykle označuje jako *prohození*). Všechny budoucí pokusy o připojení přes tuto IP adresu povede připojení do původně přidružený virtuální počítač a ne do nového. V současné době je nutné pouze použít nové veřejné IP adresy pro vytvoření nového virtuálního počítače.
 
#### <a name="sqlmysql"></a>SQL nebo MySQL 
- To může trvat až jednu hodinu, než klienti databáze můžete vytvářet v nové SQL nebo MySQL SKU. 
- Vytvoření položky přímo na SQL a MySQL hostitelské servery, které nejsou prováděné poskytovatelem prostředků není podporována a může mít za následek neodpovídající stavu.

#### <a name="app-service"></a>App Service
- Uživatel musí zaregistrovat zprostředkovatele prostředku úložiště dřív, než vytvoří jejich první funkce Azure v rámci předplatného.
 
#### <a name="usage-and-billing"></a>Využití a fakturace
- Veřejná data měření využití IP adres zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítka, který ukazuje vytvoření záznamu. Tato data v současné době nelze použít pro monitorování přesné veřejnou IP adresu využití.

## <a name="build-201709283"></a>Sestavení 20170928.3

### <a name="known-issues"></a>Známé problémy

#### <a name="powershell"></a>PowerShell
- Verze modulu PowerShell AzureRM 1.2.11 obsahuje seznam nejnovější změny. Informace o upgradu z 1.2.10 verze, najdete v článku [příručka k migraci](https://aka.ms/azspowershellmigration).

#### <a name="deployment"></a>Nasazení
- Čas serveru podle IP adresy musí zadat během nasazování.

 #### <a name="infrastructure-management"></a>Správu infrastruktury
- Nepovolujte infrastruktura zálohování na **infrastruktura zálohování** okno.
- Zprostředkovatel prostředků výpočetního zobrazí neznámého stavu.
- Baseboard management controller, (BMC) IP adresu a model se nezobrazí v uzlu jednotky škálování základní informace. Toto chování se očekává v Azure zásobníku Development Kit. 
   
#### <a name="portal"></a>Portál
- Může se zobrazit prázdný řídicí panel portálu. Chcete-li obnovit řídicí panel, vyberte ikonu ozubené kolečko v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.
- Když máte zobrazeny vlastnosti skupiny prostředků, **přesunout** tlačítko k dispozici. Toto chování je očekávané. Přesunutí skupin prostředků mezi předplatnými není aktuálně podporován.
- Zobrazí se **je vyžadována aktivace** upozornění s výzvou k registraci vaší Azure zásobníku Development Kit. Toto chování je očekávané.
- V **je vyžadována aktivace** upozornění podrobností o výstrahách, neklikejte na tento odkaz **AzureBridge** součásti. V takovém případě **přehled** okno se neúspěšně pokusí načíst, a nebude vypršení časového limitu.
- Kvóty pravděpodobně nezobrazí na portálu správce po jejich vytvoření a potom se pokusíte novější zobrazení Podrobnosti o plánu. Jako alternativní řešení v **služeb a kvóty**, klikněte na tlačítko **přidat**a přidat novou položku.
- Odstranění odběrů uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstraňte prostředky uživatele nebo skupinu celý prostředků a potom odstraňte odběry uživatele.
- Nemůžete se moci zobrazit oprávnění k vašemu předplatnému používání portálů zásobník Azure. Jako alternativní řešení můžete ověřit oprávnění pomocí prostředí Powershell.
  
#### <a name="marketplace"></a>Marketplace
- Uživatelé mohou procházet na kompletní nabídku marketplace bez předplatného a uvidí položky pro správu jako plány a nabízí. Tyto položky jsou pro uživatele funkční.
 
#### <a name="compute"></a>Compute
- Uživatelé mají možnost vytvoření virtuálního počítače pomocí geograficky redundantní úložiště. Tato konfigurace způsobuje, že vytvoření virtuálního počítače k selhání.
- Můžete nakonfigurovat dostupnost virtuálního počítače nastavit pouze u domény selhání jednoho a doméně aktualizace jednoho.
- Neexistuje žádné zkušenosti marketplace pro vytvoření sady škálování virtuálního počítače. Můžete vytvořit škálování nastavit pomocí šablony.

#### <a name="networking"></a>Sítě
- Nástroj pro vyrovnávání zatížení s veřejnou IP adresu nelze vytvořit pomocí portálu. Jako alternativní řešení můžete použít PowerShell k vytvoření nástroje pro vyrovnávání zatížení.
- Když vytvoříte Vyrovnávání zatížení sítě, musíte vytvořit pravidlo překladu adres sítě. Pokud to neuděláte, obdržíte chybu při pokusu přidat pravidlo NAT po vytvoření nástroje pro vyrovnávání zatížení.
- V části **sítě**, pokud kliknete na tlačítko **připojení** nastavit připojení k síti VPN **VNet-to-VNet** je uveden jako typ možné připojení. Nevybírejte tuto možnost. V současné době pouze **Site-to-site (IPsec)** možnost je podporována.
- Veřejnou IP adresu z virtuálního počítače (VM) nelze zrušit přidružení, po virtuálního počítače byla vytvořena a související s touto adresou IP. Zrušení přidružení se zobrazí postup, ale pořád přidruženy původní virtuální počítač dříve přiřazenou veřejnou IP adresu. K tomuto chování dochází i v případě, že je přiřadit IP adresu na nový virtuální počítač (někdy označovány jako *prohození*). Všechny budoucí pokusy o připojení přes tuto IP adresu povede připojení do původně přidružený virtuální počítač a ne do nového. V současné době je nutné pouze použít nové veřejné IP adresy pro vytvoření nového virtuálního počítače.

#### <a name="sqlmysql"></a>SQL nebo MySQL
- To může trvat až jednu hodinu, než klienti databáze můžete vytvářet v nové SQL nebo MySQL SKU. 
- Vytvoření položky přímo na SQL a MySQL hostitelské servery, které nejsou prováděné poskytovatelem prostředků není podporována a může mít za následek neodpovídající stavu.

#### <a name="app-service"></a>App Service
- Uživatel musí zaregistrovat zprostředkovatele prostředku úložiště dřív, než vytvoří jejich první funkce Azure v rámci předplatného.
