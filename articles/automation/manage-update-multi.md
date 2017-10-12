---
title: "Správa aktualizací pro několik virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Připojte virtuální počítače Azure pro účely správy aktualizací."
services: operations-management-suite
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: eslesar
ms.openlocfilehash: 89bf87f27fdf276068cba261fc6ae1660307e0b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Správa aktualizací pro několik virtuálních počítačů Azure

Správa aktualizací umožňuje spravovat aktualizace a opravy pro virtuální počítače Azure.
Ze svého účtu [Azure Automation](automation-offering-get-started.md) můžete rychle připojit virtuální počítače, posoudit stav dostupných aktualizací, naplánovat instalaci požadovaných aktualizací a zkontrolovat výsledky nasazení, abyste ověřili, že se aktualizace úspěšně použily na všech virtuálních počítačích s povolenou správou aktualizací.

## <a name="prerequisites"></a>Požadavky

K dokončení kroků v této příručce budete potřebovat:

* Účet Azure Automation. Pokyny k vytvoření účtu Azure Automation Spustit jako najdete v tématu [Účet Spustit jako pro Azure](automation-sec-configure-azure-runas-account.md).
* Virtuální počítač Azure Resource Manageru (ne Classic). Pokyny k vytvoření virtuálního počítače najdete v tématu [Vytvoření vašeho prvního virtuálního počítače s Windows na webu Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md).

## <a name="enable-update-management-for-azure-virtual-machines"></a>Povolení správy aktualizací pro virtuální počítače Azure

1. Na webu Azure Portal otevřete účet Automation.
2. Na levé straně obrazovky vyberte **Správa aktualizací**.
3. V horní části obrazovky klikněte na **Přidat virtuální počítač Azure**.
    ![Připojení virtuálních počítačů](./media/manage-update-multi/update-onboard-vm.png)
4. Vyberte virtuální počítač, který chcete připojit. Zobrazí se obrazovka **Povolit řešení Update Management**.
5. Klikněte na **Povolit**.

   ![Povolení správy aktualizací](./media/manage-update-multi/update-enable.png)

Správa aktualizací pro váš virtuální počítač je povolena.

## <a name="view-update-assessment"></a>Zobrazení posouzení aktualizací

Po povolení **správy aktualizací** se zobrazí obrazovka **Správa aktualizací**. Na kartě **Chybějící aktualizace** můžete zobrazit seznam chybějících aktualizací.

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

## <a name="next-steps"></a>Další kroky

* Další informace o správě aktualizací najdete v tématu věnovaném řešení [Update Management](../operations-management-suite/oms-solution-update-management.md).