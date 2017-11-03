---
title: "Správa zásad testovacího prostředí v Azure DevTest Labs | Microsoft Docs"
description: "Naučte se definovat zásady testovacího prostředí, například velikosti virtuálních počítačů, maximální virtuálních počítačů na uživatele a vypnutí automatizace."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: tarcher
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 328a4d893637d7150807855e118b485a2c3bbfc5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Správa všech zásad pro testovací prostředí v Azure DevTest Labs

Azure DevTest Labs umožňuje řídit náklady a minimalizovat odpady ve vaší laboratoře podle Správa zásad (nastavení) pro každý testovací prostředí. Tento článek krok za krokem podrobně vysvětluje postup nastavení jednotlivých zásad.  

## <a name="set-allowed-virtual-machine-sizes"></a>Sada povolené velikosti virtuálních počítačů
Zásady pro nastavení povolené velikosti virtuálních počítačů pomáhá minimalizovat odpady testovacího prostředí tím, že vám nastavit, které velikosti virtuálních počítačů jsou povoleny v testovacím prostředí. Tato zásada je aktivována, lze nastavit pouze velikosti virtuálních počítačů z tohoto seznamu k vytvoření virtuálních počítačů.

1. V tomto prostředí **konfiguraci a zásady** vyberte **povolené velikosti virtuálních počítačů**.
   
    ![Velikosti povolené virtuální počítače](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.

1. Pokud povolíte tuto zásadu, vyberte jeden nebo více velikosti virtuálních počítačů, které lze vytvořit ve svém testovacím prostředí.

1. Vyberte **Uložit**.

## <a name="set-virtual-machines-per-user"></a>Sada virtuálních počítačů na uživatele
Zásady pro **virtuálních počítačů na uživatele** umožňuje určit maximální počet virtuálních počítačů, které je možné vytvářet podle jednotlivých uživatelů. Pokud se uživatel pokusí o vytvoření nebo deklarací identity virtuálního počítače, když byla splněna limit počtu uživatelů, chybová zpráva označuje, že virtuální počítač nelze vytvořit vyžádaná. 

1. V tomto prostředí **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na uživatele**.
   
    ![Virtuální počítače na uživatele](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Vyberte **Ano** omezit počet virtuálních počítačů na uživatele. Pokud nechcete omezit počet virtuálních počítačů na uživatele, vyberte **ne**. Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel. 

1. Vyberte **Ano** omezit počet virtuálních počítačů, které můžete použít SSD (solid-state disk). Pokud nechcete omezit počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**. Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD. 

1. Vyberte **Uložit**.

## <a name="set-virtual-machines-per-lab"></a>Sada virtuálních počítačů na testovacího prostředí
Zásady pro **virtuálních počítačů na testovacím** umožňuje určit maximální počet virtuálních počítačů, které lze vytvořit pro aktuální testovací prostředí. Pokud se uživatel pokusí o vytvoření virtuálního počítače, když byla splněna limit testovacího prostředí, chybová zpráva označuje, že virtuální počítač nelze vytvořit. 

1. V tomto prostředí **konfiguraci a zásady** nabídce vyberte možnost **virtuálních počítačů na testovacím**.
   
    ![Virtuální počítače na testovacího prostředí](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Vyberte **Ano** omezit počet virtuálních počítačů za testovacího prostředí. Pokud nechcete omezit počet virtuálních počítačů za testovacího prostředí, vyberte **ne**. Pokud vyberete **Ano**, zadejte číselnou hodnotu, která určuje maximální počet virtuálních počítačů, které můžou vytvořit nebo jeden uživatel. 

1. Vyberte **Ano** omezit počet virtuálních počítačů, které můžete použít SSD (solid-state disk). Pokud nechcete omezit počet virtuálních počítačů, které můžete použít SSD, vyberte **ne**. Pokud vyberete **Ano**, zadejte hodnotu, která určuje maximální počet virtuálních počítačů, které lze vytvořit pomocí SSD. 

1. Vyberte **Uložit**.

## <a name="set-auto-shutdown"></a>Sada auto vypnutí
Vypnutí automatického zásada pomáhá minimalizovat odpady testovacího prostředí tím, že se vám zadejte dobu, kterou vypnout virtuální počítače v tomto testovacím prostředí.

1. V tomto prostředí **konfiguraci a zásady** vyberte **Auto-shutdown**.
   
    ![Vypnutí automatického](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.

1. Pokud povolíte tuto zásadu, zadejte dobu (a časového pásma) a ukončí se všechny virtuální počítače v aktuálním prostředí.

1. Zadejte **Ano** nebo **ne** pro možnost Odeslat oznámení 15 minut před časem zadaný auto vypnutí. Pokud zadáte **Ano**, zadejte koncový bod adresy URL webhooku pro příjem oznámení. Další informace o webhooky najdete v tématu [vytvoření webhooku nebo funkce rozhraní API Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Vyberte **Uložit**.

    Ve výchozím nastavení, jakmile bude povoleno, tato zásada platí pro všechny virtuální počítače v aktuálním prostředí. Chcete-li toto nastavení odebrat z konkrétní virtuální počítač, otevřete okno Virtuálního počítače a změňte jeho **Auto-shutdown** nastavení 

## <a name="set-auto-start"></a>Sada automatického – spuštění
Automaticky spouštěná zásady umožňuje zadat při virtuálních počítačů v aktuálním prostředí by měl být spuštěn.  

1. V tomto prostředí **konfiguraci a zásady** vyberte **automatického spuštění**.
   
    ![Automatické spuštění](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Vyberte **na** Chcete-li povolit tuto zásadu a **vypnout** ji zakázat.

3. Pokud povolíte tuto zásadu, zadejte naplánovaný čas zahájení, časové pásmo a dny v týdnu, pro kterou platí čas. 

4. Vyberte **Uložit**.

    Jakmile bude povoleno, není tato zásada použitá automaticky všechny virtuální počítače v aktuálním prostředí. Chcete-li použít tato nastavení pro konkrétní virtuální počítač, otevřete okno Virtuálního počítače a změňte jeho **automatického spuštění** nastavení 

## <a name="set-expiration-date"></a>Nastavit datum vypršení platnosti
Můžete nastavit vypršení platnosti datum, kdy jste [vytvořit virtuální počítač](devtest-lab-add-vm.md). V **upřesňující nastavení**, zvolte ikonu Kalendář zadat datum, na kterém virtuální počítač se automaticky odstraní.  Ve výchozím nastavení virtuální počítač nikdy nevyprší.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Další kroky
Jakmile jste definovaný a použít různá nastavení zásad virtuálních počítačů pro testovací prostředí, zde jsou další vyzkoušejte:

* [Pochopení sdílené IP adresy](devtest-lab-shared-ip.md) -vysvětluje, jak sdílené IP adresy se používají v DevTest Labs, chcete-li minimalizovat počet veřejných IP adres, které jsou požadované pro připojení k testovacího prostředí virtuálních počítačů.
* [Konfigurovat náklady na správu](devtest-lab-configure-cost-management.md) -ukazuje, jak používat **měsíční Trend odhadované náklady na** grafu  
  Chcete-li zobrazit aktuální měsíc je odhadované náklady na začátku a předpokládané náklady na koncový měsíc.
* [Vytvořit vlastní image](devtest-lab-create-template.md) – při vytváření virtuálního počítače, zadejte základ, který může být buď vlastní image nebo image pořízenou prostřednictvím Marketplace. Tento článek ukazuje, jak vytvořit vlastní obrázek ze souboru virtuálního pevného disku.
* [Konfigurace Marketplace image](devtest-lab-configure-marketplace-images.md) – Azure DevTest Labs podporuje vytváření virtuálních počítačů založené na imagích Azure Marketplace. Tento článek ukazuje, jak určit, které, pokud existuje, může být Azure Marketplace Image používá při vytváření virtuálních počítačů v testovacím prostředí.
* [Vytvoření virtuálního počítače v testovacím prostředí](devtest-lab-add-vm-with-artifacts.md) -znázorňuje, jak vytvořit virtuální počítač z bitové kopie základní (buď vlastní nebo Marketplace) a postup práce s artefakty ve vašem virtuálním počítači.

