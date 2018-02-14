---
title: "Jak přidat skripty do plánu obnovení v Azure Site Recovery | Microsoft Docs"
description: "Popisuje požadavky přidávání nový skript do plánu obnovení v nástroji VMM do Azure"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: shons
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/13/2017
ms.author: ruturaj
ms.openlocfilehash: 60c6eebd9323028c63f42bd8a0996e3c0a2a1cf1
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="add-vmm-scripts-to-a-recovery-plan"></a>Přidat skripty VMM do plánu obnovení


Tento článek obsahuje informace o vytvoření a přidání VMM skripty pro plány obnovení v [Azure Site Recovery](site-recovery-overview.md).

Jakékoli dotazy nebo připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites-of-adding-a-script-to-recovery-plan"></a>Požadavky pro přidání skript do plánu obnovení

Můžete použít skripty prostředí PowerShell v plánu obnovení. Musíte je vytvořit a umístit je v knihovně VMM byla přístupná z plánu obnovení. Tady jsou některé aspekty při psaní skriptu.

* Zkontrolujte, jestli skripty používají bloků try-catch tak, aby pohodlné zpracování výjimky.
    - Pokud dojde k výjimce ve skriptu, přestane pracovat a úloha ukazuje, jak se nezdařilo.
    - Pokud dojde k chybě, všechny zbývající část skriptu nefunguje.
    - Pokud dojde k chybě, když spustíte neplánované převzetí služeb při selhání, pokračuje plánu obnovení.
    - Pokud dojde k chybě při spuštění plánovaného převzetí služeb při selhání, zastaví se plán obnovení. Je třeba opravit skriptu, zkontrolujte, zda pracuje podle očekávání a znovu spusťte obnovení znovu naplánujte.
        - Příkaz Write-Host nefunguje v skript plánu obnovení a skript selže. Pokud chcete vytvořit výstup, vytvořte proxy skript, který zase spustí hlavního skriptu. Ujistěte se, že všechny je výstup pomocí >> příkaz.
        - Skript časového limitu, pokud není vrátit do 600 sekund.
        - Pokud nic je zapsán do STDERR, skript je klasifikován jako neúspěšná. Tyto informace se zobrazí v podrobnostech provádění skriptu.

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
> Zásady spouštění byste měli nastavit na možnost Nepoužívat na 64-bit PowerShell jenom. Pokud jste nastavili pro 32bitová verze prostředí PowerShell, se skripty není exeute.

## <a name="add-the-script-to-the-vmm-library"></a>Přidejte skript do knihovny VMM

Pokud máte VMM zdrojovou lokalitu, můžete vytvořit skript na serveru VMM a její zahrnutí do plánu obnovení.

1. Vytvořte novou složku ve sdílené složce knihovny. Například \<VMMServerName > \MSSCVMMLibrary\RPScripts. Umístit na zdrojové a cílové servery VMM.
2. Vytvoření skriptu (například RPScript) a zkontrolujte, že funguje podle očekávání.
3. Umístěte skript do umístění \<VMMServerName > \MSSCVMMLibrary na zdrojové a cílové servery VMM.

## <a name="add-the-script-to-a-recovery-plan"></a>Přidejte skript do plánu obnovení

Tento skript můžete přidat do skupiny po přidána virtuálních počítačů nebo skupin replikace a vytvořit plán.

1. Otevřete plánu obnovení.
2. Klikněte na položku v **krok** seznamu a pak klikněte na tlačítko **skriptu** nebo **ruční akce**.
3. Určete, zda chcete přidat skript nebo akce před nebo po vybranou položku. Použití **nahoru** a **přesunout dolů** tlačítka Přesunout pozici skript nahoru nebo dolů.
4. Pokud chcete přidat skript VMM, vyberte **převzetí služeb při selhání do skriptu VMM**. V **cestu ke skriptu**, zadejte relativní cestu ke sdílené složce. V následujícím příkladu VMM, zadejte cestu: **\RPScripts\RPScript.PS1**.
5. Pokud přidáte Azure automation spustit adresáře, zadejte účet Azure Automation, ve kterém je sada runbook a vyberte odpovídající runbooku Azure skript.
6. Proveďte testovací převzetí služeb v plánu obnovení, abyste měli jistotu, že skript funguje podle očekávání.


## <a name="next-steps"></a>Další postup

[Další informace](site-recovery-failover.md) o spuštění převzetí služeb při selhání.
