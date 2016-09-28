<properties
    pageTitle="Používání Outlooku v Azure RemoteAppu | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat a používat Outlook v Azure RemoteAppu | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# Používání Outlooku v Azure RemoteAppu

> [AZURE.IMPORTANT]
> Azure RemoteApp se přestává používat. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).

Azure RemoteApp podporuje Microsoft Outlook O365. Další informace o [fungování Office v Azure RemoteAppu](remoteapp-officesubscription.md). Existuje několik doporučených nastavení pro Outlook, když se používá v Azure RemoteAppu.

## Režim mezipaměti
Režim mezipaměti je při používání Outlooku v Azure RemoteAppu doporučenou konfigurací. Když nakonfigurujete účet Outlooku 2013, aby používal režim Cached Exchange, funguje Outlook 2013 z místní kopie uživatelovy schránky Microsoft Exchange, která je uložená v datovém souboru offline (soubor OST) v počítači uživatele společně s offline adresářem. Poštovní schránka a offline adresář v mezipaměti jsou pravidelně aktualizovány ze služby O365. Další informace o [rozdílech mezi režimem mezipaměti a online režimem](https://technet.microsoft.com/library/jj683103.aspx).

Uživatel může zvolit **režim Cached Exchange** nebo **online režim** během úvodního nastavení účtu nebo při změně nastavení účtu. Také můžete jeden nebo druhý režim nasadit pomocí nástroje pro přizpůsobení sady Office (OCT) nebo zásad skupiny.  

Přečtěte si [podrobné pokyny k povolení režimu mezipaměti](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## Hledání
Použití hledání v Outlooku v Azure RemoteAppu má určitá omezení. Azure RemoteApp používá k obsluze uživatelských relací fond virtuálních počítačů. Indexování pro hledání závisí na ID počítače, které se pro různé virtuální počítače liší. Při každém přihlášení uživatele do Azure RemoteAppu může být uživatel přesměrován na nový virtuální počítač. To znamená, že pokud povolíte místní vyhledávání, spustí se indexer při každé změně ID počítače (když je uživatel na odlišném virtuálním počítači). V závislosti na velikosti souboru .OST může indexer běžet dlouhou dobu a spotřebovávat zdroje potřebné pro ostatní aplikace. Hledání je pak nejen pomalé, ale také nemusí vracet výsledky. Pomocí profilu účtu v režimu Online lze tento problém obejít, ale celkový výkon utrpí z důvodu nedostatku místní mezipaměti (viz výše uvedený odkaz pro další informace o rozdílu mezi mezipamětí a online režimem). Bohužel však v Outlooku 2013 nelze ve výchozím nastavení zakázat indexované/místní hledání a povolit online hledání.

Outlook 2016 nabízí řešení tohoto problému tím, že přináší nové možnosti služby hledání pro poštovní schránky hostované na serveru Exchange 2016 (nebo hostované v Office 365). Tato funkce využívá výsledky hledání služby v místní mezipaměti (OST). Outlook se může v některých scénářích vrátit zpět k použití indexeru pro místní hledání, ale většina hledání probíhá pomocí této nové funkce služby hledání. Pokud je pro vás hledání v poště důležité, Azure RemoteApp proto doporučuje používat Outlook 2016.



<!--HONumber=Sep16_HO3-->


