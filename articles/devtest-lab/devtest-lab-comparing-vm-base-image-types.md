---
title: "Porovnání vlastních bitových kopií a vzorce v DevTest Labs | Microsoft Docs"
description: "Další informace o rozdílech mezi vlastních bitových kopií a vzorce jako virtuální počítač základny, abyste se mohli rozhodnout, které z nich nejlépe vyhovuje prostředí."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: a3cb259a-7d80-40ec-8ee8-45105704d589
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: tarcher
ms.openlocfilehash: ff771abc26c08f0adb977c29739d2f5c91924b21
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="comparing-custom-images-and-formulas-in-devtest-labs"></a>Porovnání vlastních bitových kopií a vzorce v DevTest Labs
Obě [vlastních bitových kopií](devtest-lab-create-template.md) a [vzorce](devtest-lab-manage-formulas.md) lze použít jako základ pro [vytvořit nové virtuální počítače](devtest-lab-add-vm-with-artifacts.md). Však mezi vlastních bitových kopií a vzorce klíče rozdíl je, že vlastní image je jednoduše image podle virtuální pevný disk, když vzorec je obrázek, který založené na virtuální pevný disk *kromě* předkonfigurované nastavení – například velikost virtuálního počítače, virtuální sítě, podsítě a artefakty. Tyto předem nakonfigurovaných nastavení se nastaví se výchozí hodnoty, které je možné přepsat v době vytvoření virtuálního počítače. Tento článek vysvětluje některé (specialisté) výhody a nevýhody (cons) pomocí vlastních bitových kopií a kdy vzorce.

## <a name="custom-image-pros-and-cons"></a>Vlastní image výhody a nevýhody
Vlastní Image poskytují statické, neměnné způsob, jak vytvořit virtuální počítače z požadované prostředí. 

**Odborníci na**

* Zřizování virtuálních počítačů z vlastní image je rychlé, protože nic změny po je spuštěné virtuální počítač z bitové kopie. Jinými slovy nejsou žádné nastavení, které se použijí jako vlastní image je právě obrázek bez nastavení. 
* Virtuální počítače vytvořené z jedné vlastní image jsou identické.

**Cons**

* Pokud budete potřebovat aktualizace některých aspektů vlastní image, je nutné znovu vytvořit bitovou kopii.  

## <a name="formula-pros-and-cons"></a>Vzorce výhody a nevýhody
Vzorce zadejte dynamické způsob, jak vytvořit virtuální počítače z požadované nastavení nebo konfigurace.

**Odborníci na**

* Změny v prostředí se dají zachytit za chodu prostřednictvím artefakty. Například pokud chcete nainstalovat s nejnovější bity z vaší verze kanálu virtuálního počítače nebo zařazení nejnovější kód z úložiště, je jednoduše zadat artefakt nasadí nejnovější bits nebo využívá nejnovější kód ve vzorci společně s cíl základní bitovou kopii. Vždy, když tento vzorec se používá k vytvoření virtuálních počítačů, nejnovější bits nebo kód jsou zařazeny do virtuálního počítače jejich nasazení. 
* Vzorce můžete definovat výchozí nastavení, které nemůžete zadat vlastní Image – například velikosti virtuálních počítačů a nastavení virtuální sítě. 
* Nastavení uložená v vzorec se zobrazují jako výchozí hodnoty, ale můžete upravovat, když je virtuální počítač vytvořený. 

**Cons**

* Vytvoření virtuálního počítače ze vzorce může trvat delší dobu než vytvoření virtuálního počítače z vlastní image.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Příspěvky blogu související
* [Vlastní Image nebo vzorce?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Další kroky
- [DevTest Labs – nejčastější dotazy](devtest-lab-faq.md)