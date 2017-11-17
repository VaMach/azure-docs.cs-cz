---
title: "Přehled služby Azure Cloud prostředí | Microsoft Docs"
description: "Přehled prostředí cloudu Azure."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: juluk
ms.openlocfilehash: 08ab3b38e4c1fbeb1fac67c5d1b6f6749f7a0a3e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="overview-of-azure-cloud-shell"></a>Přehled prostředí cloudu Azure
Prostředí Azure Cloud je interaktivní, přístupných prohlížeče prostředí pro správu prostředků Azure.
Nabízí flexibilitu zvolit prostředí shell, které nejlépe vyhovuje způsobu práce.
Uživatelé Linuxu si mohou vybrat Bash. Uživatelé Windows se mohou rozhodnout pro PowerShell.

Spustit prostřednictvím portálu Azure pomocí ikony prostředí cloudu:

![Spuštění portálu](media/overview/portal-launch-icon.png)

Z rozevíracího seznamu pro výběr prostředí využívají Bash nebo prostředí PowerShell:

![Bash v prostředí cloudu](media/overview/overview-bash-pic.png)

![Prostředí PowerShell v prostředí cloudu (Preview)](media/overview/overview-ps-pic.png)

## <a name="features"></a>Funkce
### <a name="browser-based-shell-experience"></a>Prostředí shell založené na prohlížeči
Cloudové prostředí umožňuje přístup k založené na prohlížeči příkazového řádku prostředí, vytvořené s nástroji úlohy správy Azure v paměti.
Můžete zadat využívání cloudové prostředí pro práci untethered z místního počítače způsobem, jenom do cloudu.

### <a name="choice-of-preferred-shell-experience"></a>Volba upřednostňované prostředí prostředí
Azure Cloud Shell vám dává možnost vybrat si prostředí, které nejlépe vyhovuje stylu vaší práce.
Uživatelé Linux můžete zvolit Bash v prostředí cloudu, zatímco uživatelé s Windows můžete zvolit prostředí PowerShell v prostředí cloudu (Preview).

### <a name="authenticated-and-configured-azure-workstation"></a>Ověřený a konfiguraci Azure pracovní stanice
Cloudové prostředí obsahuje spravované společností Microsoft, tak, aby je předem nainstalovaná pomocí oblíbené nástroje příkazového řádku a podpora jazyků, abyste mohli pracovat rychleji. Kromě toho cloudové prostředí bezpečně ověřuje automaticky pro okamžitý přístup k prostředkům prostřednictvím rutin 2.0 rozhraní příkazového řádku Azure nebo Azure PowerShell.

Zobrazit v seznamu úplné nástrojů [prostředí Bash](features.md#tools) a [prostředí PowerShell (Preview).](features-powershell.md#tools)

### <a name="multiple-access-points"></a>Několik přístupových bodů
Kromě cloudové prostředí, je k dispozici na portálu Azure je také přístupné:
* [Dokumentace Azure "Vyzkoušet" 2.0 rozhraní příkazového řádku](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Mobilní aplikace Azure](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Rozšíření sady Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-azure-files-storage"></a>Připojení úložiště Azure Files
Cloudové prostředí počítače jsou dočasné a v důsledku vyžadují připojit jako soubory Azure sdílenou složku `clouddrive` udržení adresáře $Home.
Při prvním spuštění prostředí cloudu vás vyzve k vytvoření prostředku skupiny, účet úložiště a sdílených složek vaším jménem. To je jednorázový krok a bude automaticky připojen pro všechny relace. Sdílení souborů lze mapovat a použije Bash a prostředí PowerShell v prostředí cloudu (Preview).

#### <a name="create-new-storage"></a>Vytvoření nového úložiště
![](media/overview/basic-storage.png)

Místně redundantní úložiště (LRS) účtu a sdílené složky souborů Azure vytvářet vaším jménem. Sdílenou složku Azure soubory se použije pro prostředí Bash a prostředí PowerShell, pokud se rozhodnete použít. Náklady na úložiště regulární použít.

Vaším jménem vytvoří tři zdroje:
1. Skupinu prostředků s názvem:`cloud-shell-storage-<region>`
2. Účet úložiště s názvem:`cs<uniqueGuid>`
3. Sdílené složky s názvem:`cs-<user>-<domain>-com-<uniqueGuid>`

> [!Note]
> Bash v prostředí cloudu vytvoří také výchozí obrázek 5 GB disk k uchování `$Home`. Všechny soubory v adresáři $Home například klíče SSH zůstávají v bitové kopii disku uživatele uložený ve sdílené složce vaší připojeného souboru. Použít osvědčené postupy při ukládání souborů v adresáři $Home a připojené sdílené složky.

#### <a name="use-existing-resources"></a>Používat existující prostředky
![](media/overview/advanced-storage.png)

Upřesňující možnost je určen k přidružení stávající prostředky pro cloudové prostředí.
Do příkazového řádku instalace úložiště klikněte na tlačítko "Zobrazit upřesňující nastavení" zobrazte další možnosti.
Rozevírací seznamy jsou filtrovány přiřazené oblast prostředí cloudu a místně nebo globálně redundantní úložiště účtů.

[Další informace o prostředí cloudové úložiště, aktualizace sdílené složky a nahrávání nebo stahování souborů.](persisting-shell-storage.md)

## <a name="concepts"></a>Koncepty
* Spuštění prostředí cloudu dočasné hostiteli k dispozici na každou relaci, jednotlivých uživatelů
* Cloudové prostředí vyprší po 20 minutách bez interaktivní aktivity
* Cloudové prostředí vyžaduje sdílení souborů pro připojit
* Cloudové prostředí používá stejné sdílené složky pro Bash a prostředí PowerShell
* Cloudové prostředí je přiřazený jeden počítač na uživatelský účet
* Máte nastavená oprávnění jako běžný uživatel Linux v Bash

Další informace o funkcích v [Bash v prostředí cloudu](features.md) a [prostředí PowerShell v prostředí cloudu (Preview)](features-powershell.md).

## <a name="examples"></a>Příklady
* Použít skripty pro automatizaci úloh správy Azure
* Současně spravovat prostředky Azure prostřednictvím portálu Azure a nástroje příkazového řádku Azure
* Test-Drive rutiny 2.0 rozhraní příkazového řádku Azure nebo Azure PowerShell

Vyzkoušejte tyto příklady v quickstarts pro [Bash v prostředí cloudu](quickstart.md) a [prostředí PowerShell v prostředí cloudu (Preview)](quickstart-powershell.md).

## <a name="pricing"></a>Ceny
Tento počítač hostování prostředí cloudu je bezplatná s předpoklad připojené sdílené složky Azure Files. Náklady na úložiště regulární použít.

## <a name="next-steps"></a>Další kroky
[Bash v prostředí cloudu rychlý start](quickstart.md) <br>
[Prostředí PowerShell v prostředí cloudu (Preview) rychlý start](quickstart-powershell.md)