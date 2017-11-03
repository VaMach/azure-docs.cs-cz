---
title: "Řešení potíží s Microsoft Azure zásobníku | Microsoft Docs"
description: "Azure zásobníku řešení potíží."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 3b40a657ee8eb391d14a38cb95acc0729a8dda21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Řešení potíží s Microsoft Azure zásobníku

*Platí pro: Azure zásobníku Development Kit*

Tento dokument obsahuje společné informace o odstraňování potíží pro Azure zásobníku. 

Protože Azure zásobníku technické Development Kit je poskytován jako prostředí pro testování, neexistuje žádná oficiální podpora zákazníků služby podpory společnosti Microsoft.  Pokud se objevil problém není zdokumentovaný, nezapomeňte zaškrtnout [fórum MSDN zásobník Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) informace a další pomoc.  

Doporučení pro odstraňování problémů, které jsou popsané v této části jsou odvozené z několika zdrojů a může nebo nemusí vyřešit konkrétní problém. Protože je k dispozici jsou příklady kódu a nejde zaručit očekávané výsledky. V této části se může časté úpravy a aktualizace, jak jsou implementované vylepšení produktu.

## <a name="deployment"></a>Nasazení
### <a name="deployment-failure"></a>Nasazení se nezdařilo.
Pokud dojde k chybě během instalace, můžete použít možnost spustit znovu skriptu nasazení restartovat nasazení z vadný krok.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Na konci nasazení relaci prostředí PowerShell je stále otevřen a nezobrazí žádný výstup
Toto chování je pravděpodobně právě výsledek výchozí chování příkazové okno prostředí PowerShell, pokud byla vybrána. Nasazení development kit ve skutečnosti proběhla úspěšně, ale skript byla pozastavena při výběru okna. Můžete ověřit, že je tomu tak, že vyhledá slovo "Vyberte" v záhlaví příkazové okno.  Stisknutím klávesy ESC zrušte jeho výběr a po ní se má zobrazit zpráva dokončení.

## <a name="templates"></a>Šablony
### <a name="azure-template-wont-deploy-to-azure-stack"></a>Šablony Azure nenasadí do Azure zásobníku
Ujistěte se, že:

* Šablona musí používat službu Microsoft Azure, která je již k dispozici nebo ve verzi preview v zásobníku Azure.
* Rozhraní API používaná pro konkrétní prostředek podporuje místní instanci Azure zásobníku a zda cílíte na platné umístění ("local" v zásobníku Azure development kit vs "East US" nebo "– jih, Indie" v Azure).
* Prostudování [v tomto článku](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md) o rutiny Test-AzureRmResourceGroupDeployment který catch malé rozdíly v syntaxi Azure Resource Manager.

Můžete také již součástí šablony Azure zásobníku [úložiště GitHub](http://aka.ms/AzureStackGitHub/) můžete začít pracovat.

## <a name="virtual-machines"></a>Virtuální počítače
### <a name="default-image-and-gallery-item"></a>Výchozí image a Galerie položku
Před nasazením virtuálních počítačů v zásobníku Azure je nejprve nutno přidat položku bitové kopie a galerii systému Windows Server.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Po restartování počítače Moje zásobník Azure hostitele se nemusí spustit některé virtuální počítače automaticky.
Po restartování počítače hostiteli, můžete si všimnout, že nejsou ihned k dispozici služby Azure zásobníku.  Důvodem je, že Azure zásobníku [infrastrukturu virtuálních počítačů](azure-stack-architecture.md#virtual-machine-roles) a RPs málo chvíli trvat ke kontrole konzistence, ale server spustí automaticky.

Můžete zjistit, že klienta, které virtuální počítače se po restartu hostitele zásobník Azure development kit nespouštějte automaticky.  To se o známý problém a vyžaduje jen pár Ruční postup jejich převedení do online režimu:

1.  Na hostiteli zásobník Azure development kit spustit **Správce clusteru převzetí služeb při selhání** z nabídky Start.
2.  Vyberte cluster **S Cluster.azurestack.local**.
3.  Vyberte **role**.
4.  Virtuální počítače klienta se zobrazí v *Uložit* stavu.  Jakmile se všechny virtuální počítače infrastruktury jsou spuštěné, klikněte pravým tlačítkem na virtuální počítače klientů a vyberte **spustit** obnovit virtuální počítač.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Odstranili některé virtuální počítače ale stále vidět soubory virtuálního pevného disku na disk. Je toto chování očekávané?
Ano, toto chování se očekává se. Protože byl navržen tímto způsobem:

* Při odstranění virtuálního počítače, virtuální pevné disky nejsou odstraněny. Disky jsou samostatné prostředky ve skupině prostředků.
* Při odstranění účtu úložiště získá odstranění je viditelná okamžitě prostřednictvím Správce Azure Resource Manager (portál, prostředí PowerShell), ale disky, které mohou obsahovat jsou uchovány stále v úložišti, dokud se nespustí uvolňování paměti.

Pokud se zobrazí "oddělena" virtuálních pevných disků, je důležité vědět, pokud jsou součástí složky pro účet úložiště, který byl odstraněn. Pokud účet úložiště nebyl odstraněn, je normální, že jsou stále existuje.

Další informace o konfiguraci uchování recyklaci prahovou hodnotu a na vyžádání v [spravovat účty úložiště](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Úložiště
### <a name="storage-reclamation"></a>Recyklace úložiště
Může trvat až 14 hodin regenerovaný kapacity objeví na portálu. Recyklace místa závisí na různých faktorech včetně procento využití interní kontejner souborů v úložišti objektů blob bloku. Proto v závislosti na tom, kolik dat je odstraněn, není zaručeno na množství místa, které může uvolnit, když běží systém uvolňování paměti.

## <a name="powershell"></a>PowerShell
### <a name="resource-providers-not-registered"></a>Zprostředkovatelé prostředků Neregistrovaný
Při připojování k odběrům klienta pomocí prostředí PowerShell, si všimnete, že nejsou automaticky registrované poskytovatele prostředků. Použití [připojení modulu](https://github.com/Azure/AzureStack-Tools/tree/master/Connect), nebo spuštěním následujícího příkazu z prostředí PowerShell (po jste [nainstalujte a připojte](azure-stack-connect-powershell.md) jako klient): 
  
       Get-AzureRMResourceProvider | Register-AzureRmResourceProvider

## <a name="cli"></a>Rozhraní příkazového řádku

* Jednofaktorovému interaktivním režimu rozhraní příkazového řádku `az interactive` příkaz není dosud podporován v zásobníku Azure.
* Seznam dostupných v zásobníku Azure bitové kopie virtuálních počítačů, použijte `az vm images list --all` příkaz místo `az vm image list` příkaz. Určení `--all` možnost zajišťuje, že odpovědi vrátí pouze obrázky, které jsou k dispozici ve vašem prostředí Azure zásobníku. 
* Aliasy bitové kopie virtuálního počítače, které jsou k dispozici v Azure nemusí být k dispozici do protokolů Azure. Pokud pomocí bitové kopie virtuálních počítačů, je nutné použít parametr celý název URN (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) namísto alias bitové kopie. A tento URNmust odpovídat specifikace image odvozené z `az vm images list` příkaz.
* Ve výchozím nastavení používá rozhraní příkazového řádku 2.0 "Standard_DS1_v2" jako výchozí velikost bitové kopie virtuálního počítače. Ale ještě tato velikost není k dispozici v Azure zásobníku, takže budete muset zadat `--size` parametr explicitně při vytváření virtuálního počítače. Můžete získat seznam velikostí virtuálních počítačů, které jsou k dispozici v zásobníku Azure pomocí `az vm list-sizes --location <locationName>` příkaz.


## <a name="windows-azure-pack-connector"></a>Konektor služby Windows Azure Pack
* Pokud změníte heslo účtu azurestackadmin poté, co nasadíte zásobník Azure development kit, můžete nakonfigurovat již více cloudu režimu. Proto nebude možné se připojit k Windows Azure Pack cílovém prostředí.
* Po nastavení režimu více cloudu:
    * Uživatel může vidět řídicí panel, až po jejich Resetovat nastavení portálu. (V portálu user portal, klikněte na ikonu portálu nastavení (ozubené kolečko ikonu v pravém horním rohu). V části **obnovit výchozí nastavení**, klikněte na tlačítko **použít**.)
    * Názvy řídicího panelu se nezobrazí. Pokud k tomuto problému dochází, je třeba ručně přidat je zpět.
    * Některé dlaždice se nemusí zobrazit správně při jejich nejprve přidat na řídicí panel. Chcete-li tento problém vyřešit, aktualizujte stránku prohlížeče.



