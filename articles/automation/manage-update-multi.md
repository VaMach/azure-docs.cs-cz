---
title: "Správa aktualizací pro několik virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Toto téma popisuje, jak se spravují aktualizace pro virtuální počítače Azure."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: f97b28d1588e959728163f7ab16d2550a79f610e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="manage-updates-for-multiple-machines"></a>Správa aktualizací pro několik počítačů

Správa aktualizací umožňuje spravovat aktualizace a opravy pro počítače s Windows nebo Linuxem.
Ze svého účtu [Azure Automation](automation-offering-get-started.md) můžete rychle připojit počítače, posoudit stav dostupných aktualizací, naplánovat instalaci požadovaných aktualizací a zkontrolovat výsledky nasazení, abyste ověřili, že se aktualizace úspěšně nainstalovaly na všech virtuálních počítačích s povolenou správou aktualizací.

## <a name="prerequisites"></a>Požadavky

Chcete-li použít správu aktualizací, budete potřebovat:

* Účet Azure Automation. Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Začínáme s Azure Automation](automation-offering-get-started.md).

* Virtuální počítač nebo počítač s nainstalovaným jedním z podporovaných operačních systémů.

## <a name="supported-operating-systems"></a>Podporované operační systémy

Správa aktualizací je podporována v následujících operačních systémech.

### <a name="windows"></a>Windows

* Windows Server 2008 nebo vyšší a nasazení aktualizací na Windows Server 2008 R2 SP1 a vyšší.  Možnosti instalace Server Core a Nano Server nejsou podporované.

    > [!NOTE]
    > Podpora pro nasazování aktualizací do Windows Serveru 2008 R2 SP1 vyžaduje .NET Framework 4.5 a WMF 5.0 nebo novější.
    > 
* Klientské operační systémy Windows nejsou podporované.

Agenti Windows musí být buď nakonfigurovaní na komunikaci se službou Windows Server Update Services (WSUS), nebo musí mít přístup ke službě Microsoft Update.

> [!NOTE]
> Agenta Windows není možné spravovat současně s nástrojem System Center Configuration Manager.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) a 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) a 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) a 12 (x64)  
* Ubuntu 12.04 LTS a novější x86/x64   

> [!NOTE]  
> Pokud se chcete vyhnout tomu, aby se aktualizace používaly mimo časové období údržby v Ubuntu, změňte konfiguraci Unattended-Upgrade tak, aby automatické aktualizace byly zakázány. Informace o postupu této konfigurace najdete v tématu [Téma Automatické aktualizace v příručce k Ubuntu Serveru](https://help.ubuntu.com/lts/serverguide/automatic-updates.html).

Agenty Linux musí mít přístup k úložišti aktualizací.

> [!NOTE]
> Agent OMS pro Linux nakonfigurovaný k ukládání dat do více pracovních prostorů OMS se v tomto řešení nepodporuje.  
>

## <a name="enable-update-management-for-azure-virtual-machines"></a>Povolení správy aktualizací pro virtuální počítače Azure

1. Na webu Azure Portal otevřete účet Automation.
2. Na levé straně obrazovky vyberte **Správa aktualizací**.
3. V horní části obrazovky klikněte na **Přidat virtuální počítač Azure**.
    ![Připojení virtuálních počítačů](./media/manage-update-multi/update-onboard-vm.png)
4. Vyberte virtuální počítač, který chcete připojit. Zobrazí se obrazovka **Povolit řešení Update Management**.
5. Klikněte na **Povolit**.

   ![Povolení správy aktualizací](./media/manage-update-multi/update-enable.png)

Správa aktualizací pro váš virtuální počítač je povolena.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Povolení správy aktualizací pro počítače a virtuální počítače jiné než Azure

Pokyny k povolení správy aktualizací pro počítače a virtuální počítače s Windows jiné než Azure najdete v popisu [připojení počítačů s Windows ke službě Log Analytics v Azure](../log-analytics/log-analytics-windows-agents.md).

Pokyny k povolení správy aktualizací pro počítače a virtuální počítače s Linuxem jiné než Azure najdete v popisu [připojení počítačů s Linuxem k sadě OMS (Operations Management Suite)](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-update-assessment"></a>Zobrazení posouzení aktualizací

Po povolení **správy aktualizací** se zobrazí obrazovka **Správa aktualizací**. Na kartě **Chybějící aktualizace** můžete zobrazit seznam chybějících aktualizací.

## <a name="data-collection"></a>Shromažďování dat

Agenti nainstalovaní na virtuálních počítačích a počítačích shromažďují data o aktualizacích a odesílají je do správy aktualizací Azure.

### <a name="supported-agents"></a>Podporovaní agenti

Následující tabulka popisuje připojené zdroje, které toto řešení podporuje.

| Připojený zdroj | Podporuje se | Popis |
| --- | --- | --- |
| Agenti systému Windows |Ano |Správa aktualizací shromažďuje informace o aktualizacích systému pro agenty Windows a inicializuje instalaci požadovaných aktualizací. |
| Agenti systému Linux |Ano |Správa aktualizací shromažďuje informace o aktualizacích systému od agentů systému Linux a zahajuje instalaci požadovaných aktualizací v podporovaných distribucích. |
| Skupina pro správu Operations Manageru |Ano |Správa aktualizací shromažďuje informace o aktualizacích systému z agentů v připojené skupině pro správu. |
| Účet služby Azure Storage |Ne |Úložiště Azure neobsahuje informace o aktualizacích systému. |

### <a name="collection-frequency"></a>Četnost shromažďování dat

Pro každý spravovaný počítač s Windows se kontrola provádí dvakrát denně. Každých 15 minut se volá rozhraní Windows API pro zadání dotazu na čas poslední aktualizace, podle kterého zjistí, jestli se změnil stav, a pokud ano, zahájí se kontrola kompatibility.  Pro každý spravovaný počítač s Linuxem se kontrola provádí každé tři hodiny.

Může trvat 30 minut až 6 hodin, než se na řídicím panelu zobrazí aktualizovaná data ze spravovaných počítačů.

## <a name="schedule-an-update-deployment"></a>Naplánování nasazení aktualizace

Pokud chcete nainstalovat aktualizace, naplánujte nasazení odpovídající vašemu plánu vydávání a časovému intervalu pro správu a údržbu.
Můžete zvolit typy aktualizací, které budou součástí nasazení. Můžete například zahrnout důležité aktualizace nebo aktualizace zabezpečení a vyloučit kumulativní aktualizace.

Naplánujte nové nasazení aktualizací pro jeden nebo více virtuálních počítačů kliknutím na **Naplánovat nasazení aktualizací** v horní části obrazovky **Správa aktualizací**. Na obrazovce **Nové nasazení aktualizací** zadejte následující:

* **Název** – Zadejte jedinečný název pro identifikaci nasazení aktualizací.
* **Typ operačního systému** – Vyberte Windows nebo Linux.
* **Počítače k aktualizaci** – Vyberte virtuální počítače, které chcete aktualizovat.

  ![Výběr virtuálních počítačů k aktualizaci](./media/manage-update-multi/update-select-computers.png)

* **Klasifikace aktualizací** – Vyberte typy softwaru, které se zahrnou do nasazení aktualizací. Typy klasifikace jsou:
  * Důležité aktualizace
  * Aktualizace zabezpečení
  * Kumulativní aktualizace
  * Balíčky funkcí
  * Aktualizace Service Pack
  * Aktualizace definic
  * Nástroje
  * Aktualizace
* **Nastavení plánu** – Můžete přijmout výchozí datum a čas, což je 30 minut od aktuálního času, nebo zadat jiný čas.
   Můžete také určit, jestli nasazení proběhne jednou, nebo nastavit plán opakování. Pokud chcete nastavit plán opakování, klikněte na možnost Opakovat v části Opakování.

   ![Obrazovka nastavení plánu aktualizací](./media/manage-update-multi/update-set-schedule.png)

* **Časové období údržby (minuty)** – Zadejte časové období, ve kterém má dojít k nasazení aktualizací.  Pomůžete tím zajistit, že se změny provedou v rámci vašich definovaných časových intervalů pro správu a údržbu.

Jakmile dokončíte konfiguraci plánu, klikněte na tlačítko **Vytvořit** a vrátíte se na řídicí panel stavu.
Všimněte si, že v tabulce **Naplánováno** se zobrazí plán nasazení, který jste právě vytvořili.

> [!WARNING]
> V případě aktualizací, které vyžadují restartování, proběhne restartování virtuálního počítače automaticky.

## <a name="view-results-of-an-update-deployment"></a>Zobrazení výsledků nasazení aktualizací

Po spuštění naplánovaného nasazení se stav tohoto nasazení zobrazí na kartě **Nasazení aktualizací** na obrazovce **Správa aktualizací**.
Pokud je nasazení aktuálně spuštěno, jeho stav je **Probíhající**. Po úspěšném dokončení se změní na **Úspěch**.
Pokud u jedné nebo více aktualizací v nasazení dojde k chybě, stav je **Částečné selhání**.

![Stav nasazení aktualizací ](./media/manage-update-multi/update-view-results.png)

Kliknutím na dokončené nasazení aktualizací zobrazíte řídicí panel pro toto nasazení aktualizací.

Na dlaždici **Výsledky aktualizací** je souhrn celkového počtu aktualizací a výsledků nasazení na virtuálním počítači.
V tabulce vpravo je podrobný rozpis všech aktualizací a výsledků instalace, které můžou mít jednu z následujících hodnot:

* Nebyl proveden pokus – aktualizace se nenainstalovala, protože podle definovaného trvání časového období údržby nebylo k dispozici dostatek času.
* Úspěch – aktualizace byla úspěšná.
* Neúspěch – aktualizace se nezdařila.

Kliknutím na **Všechny protokoly** zobrazíte všechny položky protokolu, které toto nasazení vytvořilo.

Kliknutím na **Výstup** zobrazíte datový proud úlohy runbooku zodpovědného za správu nasazení aktualizací na cílovém virtuálním počítači.

Kliknutím na **Chyby** zobrazíte podrobné informace o případných chybách nasazení.

Podrobné informace o protokolech, výstupu a informace o chybách najdete v tématu [Update Management](../operations-management-suite/oms-solution-update-management.md).

## <a name="next-steps"></a>Další kroky

* Další informace o správě aktualizací najdete v tématu věnovaném řešení [Update Management](../operations-management-suite/oms-solution-update-management.md).
