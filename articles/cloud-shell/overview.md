---
title: "Přehled služby Azure Cloud prostředí (Preview) | Microsoft Docs"
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
ms.date: 11/02/2017
ms.author: juluk
ms.openlocfilehash: 3acea56ea414f0c43333a02274e91226db29d454
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/06/2017
---
# <a name="overview-of-azure-cloud-shell-preview"></a>Přehled prostředí cloudu Azure (Preview)
Prostředí Azure Cloud je interaktivní, přístupných prohlížeče prostředí pro správu prostředků Azure.
Nabízí flexibilitu zvolit prostředí shell, které nejlépe vyhovuje způsobu práce.
Uživatelé Linuxu si mohou vybrat Bash. Uživatelé Windows se mohou rozhodnout pro PowerShell.

Spustit prostřednictvím portálu Azure pomocí ikony prostředí cloudu:

![Spuštění portálu](media/overview/portal-launch-icon.png)

Z rozevíracího seznamu pro výběr prostředí využívají Bash nebo prostředí PowerShell:

![Bash v prostředí cloudu](media/overview/overview-bash-pic.png)

![Prostředí PowerShell v prostředí cloudu](media/overview/overview-ps-pic.png)

## <a name="features"></a>Funkce
### <a name="browser-based-shell-experience"></a>Prostředí shell založené na prohlížeči
Cloudové prostředí umožňuje přístup k založené na prohlížeči příkazového řádku prostředí, vytvořené s nástroji úlohy správy Azure v paměti.
Můžete zadat využívání cloudové prostředí pro práci untethered z místního počítače způsobem, jenom do cloudu.

### <a name="choice-of-preferred-shell-experience"></a>Volba upřednostňované prostředí prostředí
Azure Cloud Shell vám dává možnost vybrat si prostředí, které nejlépe vyhovuje stylu vaší práce.
Uživatelé Linuxu si mohou vybrat Bash. Uživatelé Windows se mohou rozhodnout pro PowerShell.

### <a name="pre-configured-azure-workstation"></a>Předem nakonfigurovaná Azure pracovní stanice
Cloudové prostředí předem nainstalovaný pomocí oblíbené nástroje příkazového řádku a podpora jazyků, abyste mohli pracovat rychleji.

Zobrazení seznamu úplné nástrojů pro [prostředí Bash](features.md#tools) a [prostředí PowerShell.](features-powershell.md#tools)

### <a name="automatic-authentication"></a>Automatické ověření
Cloudové prostředí bezpečně ověřuje automaticky na každou relaci pro okamžitý přístup k prostředkům prostřednictvím rutin 2.0 rozhraní příkazového řádku Azure nebo Azure PowerShell.

### <a name="connect-your-azure-file-storage"></a>Připojení Azure File storage
Cloudové prostředí počítače jsou dočasné a v důsledku vyžadují připojit jako soubory Azure sdílenou složku `clouddrive` udržení adresáře $Home.
Při prvním spuštění prostředí cloudu vás vyzve k vytvoření prostředku skupiny, účet úložiště a sdílených složek vaším jménem. To je jednorázový krok a bude automaticky připojen pro všechny relace. Sdílení souborů lze mapovat a použije Bash a prostředí PowerShell v prostředí cloudu.

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
* Cloudové prostředí běží na dočasné počítači k dispozici na každou relaci, jednotlivých uživatelů
* Cloudové prostředí vyprší po 20 minutách bez interaktivní aktivity
* Cloudové prostředí lze přistupovat pouze u sdílené složky připojit
* Cloudová prostředí používá stejné sdílené složky pro Bash a prostředí PowerShell
* Cloudové prostředí je přiřazený jeden počítač na uživatelský účet
* Máte nastavená oprávnění jako běžný uživatel Linux (Bash)

Další informace o funkcích v [Bash v prostředí cloudu](features.md) a [prostředí PowerShell v prostředí cloudu](features-powershell.md).

## <a name="examples"></a>Příklady
* Použít skripty pro automatizaci úloh správy Azure
* Současně spravovat prostředky Azure prostřednictvím portálu Azure a nástroje příkazového řádku Azure
* Test-Drive rutiny 2.0 rozhraní příkazového řádku Azure nebo Azure PowerShell

Vyzkoušejte tyto příklady v quickstarts pro [Bash v prostředí cloudu](quickstart.md) a [prostředí PowerShell v prostředí cloudu](quickstart-powershell.md).

## <a name="pricing"></a>Ceny
Tento počítač hostování prostředí cloudu je bezplatná s předpoklad připojené sdílené složky Azure Files. Náklady na úložiště regulární použít.

## <a name="supported-browsers"></a>Podporované prohlížeče
Cloudové prostředí se doporučuje pro Chrome, okraj a Safari.
Když cloudové prostředí je podporované pro Chrome, Firefox, Safari, aplikace Internet Explorer a Edge, cloudové prostředí podléhá nastavení konkrétní prohlížeče.
