---
title: "Sledování změn s Azure Log Analytics | Microsoft Docs"
description: "Řešení sledování změn v analýzy protokolů pomáhá identifikovat software a služby systému Windows změny ve vašem prostředí."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f8040d5d-3c89-4f0c-8520-751c00251cb7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 57af000e47188786a77cdb84ebb6ffb5c50eafaa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="track-software-changes-in-your-environment-with-the-change-tracking-solution"></a>Sledování změn softwaru ve vašem prostředí do řešení pro sledování změn

![Změnit symbol sledování](./media/log-analytics-change-tracking/change-tracking-symbol.png)

Tento článek vám pomůže používat řešení sledování změn v analýzy protokolů snadno identifikovat změny ve vašem prostředí. Řešení sleduje změny softwaru Windows a Linux, soubory systému Windows a klíčů registru, služby systému Windows a Linux démoni. Identifikace změny konfigurace vám mohou pomoci přesně určit provozní problémy.

Nainstalujete řešení aktualizovat typ agenta, který jste nainstalovali. Změny nainstalovaného softwaru, služby systému Windows a Linux démoni na monitorované servery jsou přečíst. Data se pak, odešlou se službou analýzy protokolů v cloudu pro zpracování. Logika se použije pro přijatá data a cloudové služby zaznamenává data. Podle informací uvedených na řídicím panelu sledování změn, uvidíte snadno změny, které byly provedeny v serverové infrastruktuře.

## <a name="installing-and-configuring-the-solution"></a>Instalace a konfigurace řešení
Použijte následující informace k instalaci a konfiguraci řešení.

* Musíte mít [Windows](log-analytics-windows-agents.md), [nástroje Operations Manager](log-analytics-om-agents.md), nebo [Linux](log-analytics-linux-agents.md) agent na každém počítači, ve které chcete sledovat změny.
* Přidat do pracovního prostoru OMS z tohoto řešení pro sledování změn [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ChangeTrackingOMS?tab=Overview). Nebo můžete přidat řešení podle informací uvedených v [řešení přidat analýzy protokolů z Galerie řešení](log-analytics-add-solutions.md). Není nutná žádná další konfigurace.

### <a name="configure-linux-files-to-track"></a>Konfigurace souborů Linux ke sledování
Pomocí následujícího postupu můžete nakonfigurovat soubory pro sledování počítačů se systémem Linux.

1. Na portálu OMS, klikněte na tlačítko **nastavení** (symbol ozubené kolečko).
2. Na **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování souboru Linux**.
3. Ve skupinovém rámečku, aby bylo možné sledování změn v souboru Linux, zadejte celou cestu, včetně jeho názvu souboru, který chcete sledovat a pak klikněte na **přidat** symbol. Například: "/etc/*.conf"
4. Klikněte na **Uložit**.  

> [!NOTE]
> Soubor Linux sledování obsahuje další možnosti, včetně directory sledování recrusion prostřednictvím adresářů a sledování zástupný znak.

### <a name="configure-windows-files-to-track"></a>Konfigurovat soubory systému Windows ke sledování
Pomocí následujícího postupu můžete nakonfigurovat soubory sledovat na počítače se systémem Windows.

1. Na portálu OMS, klikněte na tlačítko **nastavení** (symbol ozubené kolečko).
2. Na **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování souboru Windows**.
3. V části, aby bylo možné sledování změn v souboru systému Windows, zadejte celou cestu, včetně jeho názvu souboru, který chcete sledovat a pak klikněte na **přidat** symbol. Například: C:\Program Files (x86) \Internet Explorer\iexplore.exe nebo C:\Windows\System32\drivers\etc\hosts.
4. Klikněte na **Uložit**.  
   ![Sledování změn souborů systému Windows](./media/log-analytics-change-tracking/windows-file-change-tracking.png)

### <a name="configure-windows-registry-keys-to-track"></a>Nakonfigurovat klíče registru Windows ke sledování
Pomocí následujících kroků konfigurace klíče registru sledování v počítačích se systémem Windows.

1. Na portálu OMS, klikněte na tlačítko **nastavení** (symbol ozubené kolečko).
2. Na **nastavení** klikněte na tlačítko **Data**a potom klikněte na **sledování registru Windows**.
3. V části, aby bylo možné sledování změn v registru systému Windows, zadejte celý klíč, který chcete sledovat a pak klikněte na **přidat** symbol.
4. Klikněte na **Uložit**.  
   ![Sledování změn registru systému Windows](./media/log-analytics-change-tracking/windows-registry-change-tracking.png)

### <a name="explanation-of-linux-file-collection-properties"></a>Vysvětlení vlastností kolekce Linux souboru
1. **Typ**
   * **Soubor** (sestavu metadata souboru - velikost, datum změny, hodnota hash, atd.)
   * **Adresář** (sestava metadat adresáře - velikost, datum změny, atd.)
2. **Odkazy** (zpracování Linux symlink odkazy na další soubory nebo adresáře)
   * **Ignorovat** (ignorovat během recurions tak, aby neobsahoval soubory nebo adresáře odkazuje symbolických odkazů)
   * **Postupujte podle** (podle během rekurze zahrnout také soubory nebo adresáře odkazuje symbolických odkazů)
   * **Spravovat** (podle symbolických odkazů a změnit způsob zpracování vrácená obsahu)

   > [!NOTE]   
   > Možnost "Manage" odkazy se nedoporučuje. Načtení souboru obsahu není podporováno.

3. **Recurse** (Recurse prostřednictvím úrovně složky a sledovat všechny soubory, které splňuje příkaz cesta)
4. **Sudo** (Povolit přístup k souborům nebo adresářům, které vyžadují oprávnění sudo)

### <a name="limitations"></a>Omezení
Řešení sledování změn v současné době nepodporuje následující položky:

* Složky (adresáře) pro Windows Sledování souboru
* Rekurze pro sledování souboru systému Windows
* Zástupné znaky pro sledování souborů systému Windows
* Proměnné cest
* Systémy souborů síť
* Obsah souboru

Jiná omezení:

* **Maximální velikost souboru** sloupec a hodnoty se nepoužívá v aktuální implementace.
* Pokud jste v cyklu 30 minut kolekce shromažďovat více než 2 500 soubory, může docházet ke snížení výkonu řešení.
* Když síťový provoz vysoké, záznamy změn může trvat maximálně šest hodin k zobrazení.
* Pokud upravíte konfiguraci, zatímco počítač je vypnutý, můžete umístit počítač změny souborů, které byly součástí předchozí konfiguraci.

## <a name="change-tracking-data-collection-details"></a>Změňte podrobnosti pro kolekce dat sledování
Sledování změn shromažďuje inventář softwaru a metadata služby systému Windows pomocí agentů, které jste povolili.

Následující tabulka uvádí metody shromažďování dat a další podrobnosti o tom, jak se data shromažďují pro sledování změn.

| Platforma | Přímé agenta | Agent nástroje Operations Manager | Agenta systému Linux | Azure Storage | Nástroj Operations Manager vyžaduje? | Dat agenta nástroje Operations Manager odeslána prostřednictvím skupiny pro správu | Frekvence kolekce |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Systém Windows a Linux | &#8226; | &#8226; | &#8226; |  |  | &#8226; | 5 minut až 50 minut v závislosti na daný typ změny. Další informace najdete v následující tabulce. |


Následující tabulka uvádí četnost shromažďování dat pro typy změn.

| **změnit typ** | **frekvence** | **Nemá****agenta****odeslat rozdíly, když se najde?**  |
| --- | --- | --- |
| Registru systému Windows | 50 minut | Ne |
| Soubor systému Windows | 30 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Soubor Linux | 15 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Služby pro Windows | 30 minut | Ano, každých 30 minut, kdy jsou zjištěny změny. Každých 24 hodin snímku se pošle bez ohledu na změnu. Ano, snímku se odesílají i tam, kde neexistují žádné změny. |
| Démoni Linux | 5 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |
| Windows software | 30 minut | Ano, každých 30 minut, kdy jsou zjištěny změny. Každých 24 hodin snímku se pošle bez ohledu na změnu. Ano, snímku se odesílají i tam, kde neexistují žádné změny. |
| Softwaru platformy Linux | 5 minut | Ano. Pokud se nezměnila za 24 hodin, se budou odesílat snímku. |

### <a name="registry-key-change-tracking"></a>Sledování změn klíče registru

Analýzy protokolů provede registru systému Windows, monitorování a sledování do řešení pro sledování změn. Účelem monitorování změn klíče registru je ke kotvícímu bodu body rozšiřitelnosti, kde můžete aktivovat kód třetích stran a malwarem. Následující seznam obsahuje výchozí hodnota klíče registru, které sleduje řešení a proč je každý sledovaný.

- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Startup
    - Monitorování skripty, které spustí při spuštění.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Group Policy\Scripts\Shutdown
    - Monitorování skripty, které běží při vypnutí.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Run
    - Monitoruje klíče, které jsou načteny před uživatel přihlásí k účtu systému Windows. Používá se pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Microsoft\Active Setup\Installed součásti
    - Sleduje změny nastavení aplikace.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\ShellEx\ContextMenuHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\Shellex\CopyHookHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Classes\Directory\Background\ShellEx\ContextMenuHandlers
    - Monitorování běžných automatické spuštění položky, které připojit přímo do Průzkumníka Windows a obvykle spuštění v rámci procesu Explorer.exe.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitorování pro ikonu překrytí registraci obslužné rutiny.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers
    - Monitorování pro ikonu překrytí registrace obslužné rutiny pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows\CurrentVersion\Explorer\Browser pomocné objekty
    - Monitorování pro moduly nový prohlížeč Pomocník objekt plug-in pro Internet Explorer. Použít pro přístup k modelu objektu dokumentu (DOM) aktuální stránky a k řízení navigace.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\Browser pomocné objekty
    - Monitorování pro moduly nový prohlížeč Pomocník objekt plug-in pro Internet Explorer. Použít pro přístup k modelu objektu dokumentu (DOM) aktuální stránky a k řízení navigace pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Internet Explorer\Extensions
    - Monitorování pro nové rozšíření, Internet Explorer, jako je například vlastní nástroj nabídky a tlačítka panelu nástrojů vlastní.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Internet Explorer\Extensions
    - Monitorování pro nové rozšíření, Internet Explorer, jako jsou nabídky vlastního nástroje a vlastní panel nástrojů tlačítka pro 32bitové aplikace spuštěné na 64bitových počítačích.
- Nastavení HKEY\_místní\_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitoruje 32bitové ovladače přidružené wavemapper, wave1 a wave2, msacm.imaadpcm, .msadpcm, .msgsm610 a čtyřznakového. Podobně jako v oddílu [ovladače] v systému. Soubor INI.
- Nastavení HKEY\_místní\_MACHINE\Software\Wow6432Node\Microsoft\Windows NT\CurrentVersion\Drivers32
    - Monitorování 32bitové ovladače přidružené wavemapper, wave1 a wave2, msacm.imaadpcm, .msadpcm, .msgsm610 a čtyřznakového pro 32bitové aplikace spuštěné na 64bitových počítačích. Podobně jako v oddílu [ovladače] v systému. Soubor INI.
- Nastavení HKEY\_místní\_MACHINE\System\CurrentControlSet\Control\Session Manager\KnownDlls
    - Monitoruje seznam známých nebo běžně používané systémové knihovny DLL; Tento systém brání osobám v zneužitím slabé aplikace directory oprávnění podle vyřazením trojský kůň verze systémové knihovny DLL.
- Nastavení HKEY\_místní\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Notify
    - Monitoruje seznam balíčků schopný přijímat oznámení událostí z přihlášení do systému Windows, model podporu interaktivní přihlášení pro operační systém Windows.


## <a name="use-change-tracking"></a>Pomocí sledování změn
Po instalaci řešení můžete zobrazit souhrnné informace o změnách pro monitorované servery pomocí **sledování změn** na dlaždici **přehled** stránky v OMS.

![Obrázek sledování změn dlaždice](./media/log-analytics-change-tracking/change-tracking-tile.png)

Můžete zobrazit změny v konfiguraci infrastruktury a potom přejít k podrobnostem podrobnosti v těchto kategoriích:

* Změny podle konfigurace pro software a služby systému Windows
* Změny softwaru aplikací a aktualizací pro jednotlivé servery
* Celkový počet změny softwaru pro jednotlivé aplikace
* Balíčky Linux
* Změny pro jednotlivé servery služby Windows
* Změny linuxového démona

![Obrázek sledování změn řídicí panel](./media/log-analytics-change-tracking/change-tracking-dash01.png)

![Obrázek sledování změn řídicí panel](./media/log-analytics-change-tracking/change-tracking-dash02.png)

### <a name="to-view-changes-for-any-change-type"></a>Chcete-li zobrazit změny pro libovolný typ
1. Na **přehled** klikněte na tlačítko **sledování změn** dlaždici.
2. Na **změnit sledování** řídicí panel, zkontrolujte souhrnné informace v jednom z okna typ změny a pak klikněte na jednu Chcete-li zobrazit podrobné informace o jeho **hledání protokolů** stránky.
3. Na všech stránkách vyhledávání protokolu můžete zobrazit výsledky čas, podrobné výsledky a historii hledání protokolu. Můžete také filtrovat podle omezující vlastnosti výsledky upřesněte.

## <a name="next-steps"></a>Další kroky
* Použití [přihlásit analýzy protokolů hledání](log-analytics-log-searches.md) zobrazíte podrobné sledování dat změn.
