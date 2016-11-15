---
title: "Používání Outlooku v Azure RemoteAppu | Dokumentace Microsoftu"
description: "Zjistěte, jak nakonfigurovat a používat Outlook v Azure RemoteAppu | Microsoft Azure"
services: remoteapp
documentationcenter: 
author: pavithir
manager: mbaldwin
ms.assetid: cb2a498f-9539-4522-a874-542114926a61
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/15/2016
ms.author: elizapo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1ec1d88b72d2f34dc7515d4387d09a46b4da7184


---
# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Používání Outlooku v Azure RemoteAppu
> [!IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Azure RemoteApp podporuje Microsoft Outlook O365. Další informace o [fungování Office v Azure RemoteAppu](remoteapp-officesubscription.md). Existuje několik doporučených nastavení pro Outlook, když se používá v Azure RemoteAppu.

## <a name="cached-mode"></a>Režim mezipaměti
Režim mezipaměti je při používání Outlooku v Azure RemoteAppu doporučenou konfigurací. Když nakonfigurujete účet Outlooku 2013, aby používal režim Cached Exchange, funguje Outlook 2013 z místní kopie uživatelovy schránky Microsoft Exchange, která je uložená v datovém souboru offline (soubor OST) v počítači uživatele společně s offline adresářem. Poštovní schránka a offline adresář v mezipaměti jsou pravidelně aktualizovány ze služby O365. Další informace o [rozdílech mezi režimem mezipaměti a online režimem](https://technet.microsoft.com/library/jj683103.aspx).

Uživatel může zvolit **režim Cached Exchange** nebo **online režim** během úvodního nastavení účtu nebo při změně nastavení účtu. Také můžete jeden nebo druhý režim nasadit pomocí nástroje pro přizpůsobení sady Office (OCT) nebo zásad skupiny.  

Přečtěte si [podrobné pokyny k povolení režimu mezipaměti](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Hledání
Použití hledání v Outlooku v Azure RemoteAppu má určitá omezení. Azure RemoteApp používá k obsluze uživatelských relací fond virtuálních počítačů. Indexování pro hledání závisí na ID počítače, které se pro různé virtuální počítače liší. Při každém přihlášení uživatele do Azure RemoteAppu může být uživatel přesměrován na nový virtuální počítač. To znamená, že pokud povolíte místní vyhledávání, spustí se indexer při každé změně ID počítače (když je uživatel na odlišném virtuálním počítači). V závislosti na velikosti souboru .OST může indexer běžet dlouhou dobu a spotřebovávat zdroje potřebné pro ostatní aplikace. Hledání je pak nejen pomalé, ale také nemusí vracet výsledky. Pomocí profilu účtu v režimu Online lze tento problém obejít, ale celkový výkon utrpí z důvodu nedostatku místní mezipaměti (viz výše uvedený odkaz pro další informace o rozdílu mezi mezipamětí a online režimem). Bohužel však v Outlooku 2013 nelze ve výchozím nastavení zakázat indexované/místní hledání a povolit online hledání.




<!--HONumber=Nov16_HO2-->


