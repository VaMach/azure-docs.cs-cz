---
title: "Vytvořit plány obnovení pro převzetí služeb při selhání a obnovení v Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak vytvořit a přizpůsobení plánů obnovení v Azure Site Recovery k převzetí služeb při selhání a obnovení virtuálních počítačů a fyzických serverů"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: raynew
ms.openlocfilehash: 202e0ac8be36e9156ec16fadc1b722f4eb3d1432
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/19/2017
---
# <a name="create-recovery-plans"></a>Vytvořit plány obnovení


Tento článek obsahuje informace o vytváření a přizpůsobení plánů obnovení v [Azure Site Recovery](site-recovery-overview.md).

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Vytvořte plány obnovení provést následující akce:

* Definujte skupiny počítačů, které převzetí služeb při selhání společně a pak spusťte společně.
* Modelu závislosti mezi počítači, když seskupíte společně do obnovení naplánujte skupiny. Například převzetí služeb při selhání a zprovoznit konkrétní aplikaci, seskupit všechny virtuální počítače pro tuto aplikaci do stejné skupiny plánu obnovení.
* Spusťte převzetí služeb při selhání. Můžete spustit test plánované, nebo neplánované převzetí služeb při selhání pro plán obnovení.


## <a name="create-a-recovery-plan"></a>Vytvoření plánu obnovení

1. Klikněte na tlačítko **plány obnovení** > **vytvoření plánu obnovení**.
   Zadejte název plánu obnovení a zdroj a cíl. Zdrojové umístění musí mít virtuální počítače, které jsou povolené pro převzetí služeb při selhání a obnovení.

    - VMM VMM replikace, vyberte **typ zdroje** > **VMM**a zdrojové a cílové servery VMM. Klikněte na tlačítko **technologie Hyper-V** zobrazíte cloudy, které jsou chráněné.
    - Pro nástroj VMM do Azure, vyberte **typ zdroje** > **VMM**.  Vyberte zdrojový server VMM a **Azure** jako cíl.
    - Pro replikaci technologie Hyper-V do Azure (bez VMM), vyberte **typ zdroje** > **technologie Hyper-V lokality**. Vyberte lokalitu jako zdroj, a **Azure** jako cíl.
    - Pro virtuální počítače VMware nebo fyzický místní server Azure, vyberte konfigurační server jako zdroj, a **Azure** jako cíl.
    - Pro plán obnovení Azure do Azure vyberte oblast Azure jako zdroj a sekundární oblasti Azure jako cíl. Sekundární oblasti jsou pouze ty, na které jsou chráněné virtuální počítače.
2. V **vybrat virtuální počítače**, vyberte virtuální počítače (nebo replikační skupiny), který chcete přidat do výchozí skupiny (skupina 1) v plánu obnovení.

## <a name="customize-and-extend-recovery-plans"></a>Přizpůsobit a rozšířit plánů obnovení

Můžete přizpůsobit a rozšířit plány obnovení:

- **Přidat nové skupiny**– přidat do skupiny výchozí skupiny plánu obnovení dodatečné (až 7) a poté přidejte další počítače nebo skupiny replikace do těchto skupin plánu obnovení. Skupiny jsou číslované v pořadí, ve kterém je přidat. Virtuální počítač nebo skupinu replikace můžete zahrnuty pouze do jedné obnovení plán skupiny.
- **Přidání ručně prováděné akce**– můžete přidat ručně prováděné akce, které spustit před nebo po skupiny pro plán obnovení. Při spuštění plánu obnovení, zastaví se v okamžiku, kdy jste vložili manuální akce. Dialogové okno zobrazí výzvu k určení, že je dokončená manuální akce.
- **Skript přidáte**– můžete přidat skripty, které spustit před nebo po skupiny pro plán obnovení. Při přidání skript přidá novou sadu akcí pro skupinu. Například se vytvoří sadu předběžné kroky 1. skupina s názvem: 1. skupina: předběžné kroky. Objeví se všechny předběžné kroky v této sadě. Pokud máte server VMM nasazený, můžete přidat pouze skript v primární lokalitě.
- **Přidat runbooky Azure**– můžete rozšířit plány obnovení s runbooky služby Azure. Například k automatizaci úloh, nebo vytvořte krokování obnovení. [Další informace](site-recovery-runbook-automation.md).

## <a name="add-scripts"></a>Přidat skripty

Můžete použít skripty prostředí PowerShell v plánu obnovení.

 - Zkontrolujte, jestli skripty používají bloků try-catch tak, aby pohodlné zpracování výjimky.
    - Pokud dojde k výjimce ve skriptu, přestane pracovat a úloha ukazuje, jak se nezdařilo.
    - Pokud dojde k chybě, všechny zbývající část skriptu nefunguje.
    - Pokud dojde k chybě, když spustíte neplánované převzetí služeb při selhání, pokračuje plánu obnovení.
    - Pokud dojde k chybě při spuštění plánovaného převzetí služeb při selhání, zastaví se plán obnovení. Je třeba opravit skriptu, zkontrolujte, zda pracuje podle očekávání a znovu spusťte obnovení znovu naplánujte.
- Příkaz Write-Host nefunguje v skript plánu obnovení a skript selže. Pokud chcete vytvořit výstup, vytvořte proxy skript, který zase spustí hlavního skriptu. Ujistěte se, že všechny je výstup pomocí >> příkaz.
  * Skript časového limitu, pokud není vrátit do 600 sekund.
  * Pokud nic je zapsán do STDERR, skript je klasifikován jako neúspěšná. Tyto informace se zobrazí v podrobnostech provádění skriptu.

Pokud používáte VMM ve vašem nasazení:

* V kontextu účtu služby VMM spustit skripty v plánu obnovení. Ujistěte se, že tento účet má oprávnění ke čtení pro vzdálené sdílené složce, ve kterém je uložený skript. Otestujte skript, který chcete spustit na úrovni oprávnění účtu služby VMM.
* Rutin služby VMM se dodávají v modulu Windows PowerShell. Modul je nainstalován při instalaci konzole VMM. Je možné načíst do vašeho skriptu, pomocí následujícího příkazu ve skriptu:
   - Import-Module-Name virtualmachinemanager. [Další informace](https://technet.microsoft.com/library/hh875013.aspx).
* Zajistěte, že abyste měli aspoň jeden server knihovny ve vašem nasazení VMM. Ve výchozím nastavení cesta ke sdílené složce knihovny pro VMM server nachází místně na serveru VMM MSCVMMLibrary názvem složky.
    * Pokud vaše cesta ke sdílené složce knihovny vzdáleného (nebo místní, ale není sdílený s MSCVMMLibrary), konfigurovat sdílení takto (pomocí \\libserver2.contoso.com\share\ jako příklad):
      * Otevřete Editor registru a přejděte do **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure lokality Recovery\Registration**.
      * Upravit hodnotu **ScriptLibraryPath** a umístěte jej jako \\libserver2.contoso.com\share\. Zadejte úplný plně kvalifikovaný název domény. Zadejte oprávnění k umístění sdílené složky. Všimněte si, že se jedná o kořenového uzlu sdílené složky. **Tuto kontrolu můžete procházet knihovně na kořenový uzel ve VMM. Cesta, která otevře bude kořen cesty – ten, který jste bude muset používat v proměnné**.
      * Ujistěte se, že provedete test skriptu pomocí uživatelského účtu, který má stejné oprávnění jako účet služby VMM. Tato kontrola ověřuje, že samostatné otestovat skripty spustit stejným způsobem, jak se v rámci plánů obnovení. Na serveru VMM nastavte zásady spouštění obejít následujícím způsobem:
        * Otevřete **64bitová verze Windows PowerShell** konzole použitím zvýšených oprávnění.
        * Typ: **nepoužívat Set-executionpolicy**. [Další informace](https://technet.microsoft.com/library/ee176961.aspx).

> [!IMPORTANT]
> Zásady spouštění byste měli nastavit na možnost Nepoužívat na 64-bit powershell jenom. Pokud jste nastavili pro 32bitová verze prostředí powershell, se skripty není exeute.

## <a name="add-a-script-or-manual-action-to-a-plan"></a>Akce skriptu nebo ručně přidat do plánu

Po přidána virtuálních počítačů nebo skupin replikace a vytvořit plán, můžete přidat skript do výchozí skupiny plánu obnovení.

1. Otevřete plánu obnovení.
2. Klikněte na položku v **krok** seznamu a pak klikněte na tlačítko **skriptu** nebo **ruční akce**.
3. Určete, zda chcete přidat skript nebo akce před nebo po vybranou položku. Použití **nahoru** a **přesunout dolů** tlačítka Přesunout pozici skript nahoru nebo dolů.
4. Pokud chcete přidat skript VMM, vyberte **převzetí služeb při selhání do skriptu VMM**. V **cestu ke skriptu**, zadejte relativní cestu ke sdílené složce. V následujícím příkladu VMM, zadejte cestu: **\RPScripts\RPScript.PS1**.
5. Pokud přidáte Azure automation spustit adresáře, zadejte účet Azure Automation, ve kterém je sada runbook a vyberte odpovídající runbooku Azure skript.
6. Proveďte převzetí služeb při selhání plánu obnovení, abyste měli jistotu, že skript funguje podle očekávání.


### <a name="add-a-vmm-script"></a>Přidejte skript VMM

Pokud máte VMM zdrojovou lokalitu, můžete vytvořit skript na serveru VMM a její zahrnutí do plánu obnovení.

1. Vytvořte novou složku ve sdílené složce knihovny. Například \<VMMServerName > \MSSCVMMLibrary\RPScripts. Umístit na zdrojové a cílové servery VMM.
2. Vytvoření skriptu (například RPScript) a zkontrolujte, že funguje podle očekávání.
3. Umístěte skript do umístění \<VMMServerName > \MSSCVMMLibrary na zdrojové a cílové servery VMM.


## <a name="next-steps"></a>Další kroky

[Další informace](site-recovery-failover.md) o spuštění převzetí služeb při selhání.
