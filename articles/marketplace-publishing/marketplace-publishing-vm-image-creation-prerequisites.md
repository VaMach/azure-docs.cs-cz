---
title: "Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro Azure Marketplace | Microsoft Docs"
description: "Pochopili požadavky na vytvoření a nasazení bitové kopie virtuálního počítače v Azure Marketplace pro ostatní k nákupu."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 63c16966-0304-4b17-a715-368a0a5ccb2c
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: af3e2ad623d8d7bfafe676411f9ae3fbee78aab8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="technical-prerequisites-for-creating-a-virtual-machine-image-for-the-azure-marketplace"></a>Technické požadavky pro vytvoření bitové kopie virtuálního počítače pro Azure Marketplace
Přečtěte si důkladně před zahájením procesu a pochopit, kde a proč se provádí každý krok. Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytvoření nabídky. Tyto položky musí být vymazat z kontrola v tomto článku.  

## <a name="download-needed-tools--applications"></a>Stáhněte potřebné nástroje & aplikace
Musí mít připravené před zahájením procesu následující položky:

* V závislosti na tom, jaký operační systém cílení, nainstalujte [rutin prostředí Azure PowerShell](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WindowsAzurePowershellGet.3f.3f.3fnew.appids) nebo [Linux rozhraní příkazového řádku nástroje](https://go.microsoft.com/fwlink/?LinkId=253472&clcid=0x409) z [Azure stáhne](https://azure.microsoft.com/downloads/) stránky.
* Nainstalujte Azure Storage Explorer na webu CodePlex.
* Stáhněte a nainstalujte nástroj pro testování certifikační pro certifikaci Azure:
  * [http://go.microsoft.com/fwlink/?LinkId=526913](http://go.microsoft.com/fwlink/?LinkID=526913). Je nutné počítače se systémem Windows spusťte nástroj certifikace. Pokud jste počítači se systémem Windows k dispozici, můžete spustit nástroj pomocí systému Windows virtuálního počítače v Azure.

## <a name="platforms-supported"></a>Podporované platformy
Můžete vyvíjet založené na Azure virtuální počítače v systému Windows nebo Linux. Některé prvky proces publikování – například vytváření služby Azure kompatibilní virtuální pevný disk (VHD) – pomocí různých nástrojů a kroky v závislosti na tom, jaký operační systém, kterou používáte:  

* Pokud používáte Linux, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (systémem Linux)" z [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).
* Pokud používáte systém Windows, naleznete v části "Vytvoření služby Azure kompatibilní virtuální pevný disk (založené na Windows)" z [Průvodce publikování bitové kopie virtuálních počítačů](marketplace-publishing-vm-image-creation.md).

> [!NOTE]
> Potřebujete přístup pro počítač systému Windows:
> 
> * Spusťte nástroj Certifikační ověření.
> * Vytvořte adresu URL pro podpis sdíleného virtuálního pevného disku přístup pro odeslání certifikační virtuálního pevného disku.
> 
> 

## <a name="develop-your-vhd"></a>Vytvořte svůj disk VHD
Můžete vyvíjet virtuální pevné disky Azure v cloudu nebo místně:

* Vývoj v cloudu znamená, že všechny kroky vývoj se vzdáleně na virtuální pevný disk uložené v Azure.
* Místní vývoj vyžaduje stahování virtuální pevný disk a vývoj pomocí místní infrastruktury. I když je to možné, nedoporučujeme ji. Nezapomeňte, že vývoj pro Windows nebo SQL místní vyžaduje abyste měli kódy VLK relevantní místní. Nelze zahrnout nebo po vytvoření virtuálního počítače nainstalovat systém SQL Server. Vaši nabídku musí také založit na bitovou kopii schválené SQL z portálu Azure. Pokud se rozhodnete vyvíjet místně, je třeba provést některé kroky jinak než pokud byly vývoj v cloudu. Může hledání příslušných informací v [vytvořit bitovou kopii virtuálního počítače místní](marketplace-publishing-vm-image-creation-on-premise.md).

[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
