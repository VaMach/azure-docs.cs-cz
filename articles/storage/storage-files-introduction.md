---
title: "Úvod do služby Azure File Storage | Dokumentace Microsoftu"
description: "Přehled služby Azure File Storage, která umožňuje vytvářet a používat sdílené složky souborů sítě v Microsoft Cloudu s využitím oborových standardů."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 16fdd3aafef1a50554932a0e7843d347182c9b6a
ms.contentlocale: cs-cz
ms.lasthandoff: 07/20/2017

---

# <a name="introduction-to-azure-file-storage"></a>Úvod do služby Azure File Storage
Azure File Storage nabízí sdílené složky souborů sítě v cloudu s využitím standardních průmyslových protokolů [SMB (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) a [Samba/Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Sdílené složky Azure můžou současně připojit klienti, jako jsou například místní nasazení systémů Windows, macOS nebo Linux nebo služba Azure Virtual Machines. Účet úložiště pro obecné účely poskytuje přístup ke službě Azure File Storage a dalším službám, jako jsou například objekty blob, disky virtuálních počítačů Azure a fronty v rámci jednoho účtu.



## <a name="videos"></a>Videa
| Představení služby Azure File Storage (27 minut) | Kurz služby Azure File Storage (5 minut)  |
|-|-|
| [![Snímek obrazovky videa Představení služby Azure File Storage – kliknutím zahájíte přehrávání](media/storage-file-storage/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Snímek obrazovky videa Kurz služby Azure File Storage – kliknutím zahájíte přehrávání](media/storage-file-storage/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Proč je služba Azure File Storage užitečná
Azure File Storage umožňuje nahradit místně hostované souborové servery založené na Windows Serveru, Linuxu nebo NAS za cloudovou sdílenou složku bez operačního systému. To přináší následující výhody:

* **Sdílený přístup**. Sdílené složky Azure podporují standardní průmyslový protokol SMB, což znamená, že můžete bez problému nahradit místní sdílené složky za sdílené složky Azure bez starostí o kompatibilitu aplikací. Schopnost sdílet systém souborů mezi několika počítači, aplikacemi a instancemi představuje výraznou výhodu služby Azure File Storage pro aplikace potřebující možnost sdílení. 
* **Plně spravovaná**. Sdílené složky Azure je možné vytvářet bez nutnosti spravovat hardware nebo operační systém. To znamená, že se nemusíte starat o opravy operačního systému serveru pomocí důležitých upgradů zabezpečení ani o nahrazování vadných pevných disků.
* **Skriptování a nástroje**. Rutiny PowerShellu a Azure CLI je možné použít k vytváření, připojování a správě sdílených složek File Storage v rámci správy aplikací Azure. Sdílené složky Azure můžete vytvářet a spravovat pomocí webu Azure Portal a Azure Storage Exploreru. 
* **Odolnost**. Služba Azure File Storage je od základu vytvořena tak, aby byla vždy dostupná. Nahrazení místních sdílených složek za Azure File Storage znamená, že už se nebudete muset probouzet kvůli řešení místních výpadků napájení nebo problémů se sítí. 
* **Známá programovatelnost**. Aplikace spuštěné v Azure můžou k datům ve sdílené složce přistupovat přes [rozhraní API pro vstup/výstup souborového systému](https://msdn.microsoft.com/library/system.io.file.aspx). Vývojáři tedy můžou využít svoje dovednosti a znalosti kódu při migraci stávajících aplikací. Kromě rozhraní API pro vstup/výstup systému můžete použít [klientské knihovny pro Azure Storage](https://msdn.microsoft.com/library/azure/dn261237.aspx) nebo [rozhraní REST API pro Azure Storage](/rest/api/storageservices/file-service-rest-api).

Sdílené složky Azure umožňují:

* **Nahrazení místních souborových serverů:**  
    Azure File Storage je možné použít k úplnému nahrazení sdílených složek na tradičních místních souborových serverech nebo zařízeních NAS. Oblíbené operační systémy jako Windows, macOS a Linux můžou sdílenou složku Azure snadno připojit, ať jsou kdekoli na světě.

* **Migrace aplikací metodou „lift and shift“:**  
    Azure File Storage umožňuje snadnou migraci aplikací, které používají místní sdílené složky ke sdílení dat mezi částmi aplikace, metodou „lift and shift“ do cloudu. Aby to bylo možné, každý virtuální počítač se připojuje ke sdílené složce a potom může číst a zapisovat soubory stejně, jako by to prováděl s místní sdílenou složkou.

* **Zjednodušení vývoje pro cloud:**  
    Azure File Storage můžete použít mnoha různými způsoby pro zjednodušení nových projektů vývoje pro cloud.
    * **Sdílená nastavení aplikací:**  
        Běžným vzorem pro distribuované aplikace je mít konfigurační soubory v centralizovaném umístění, kde k nim může přistupovat mnoho různých virtuálních počítačů. Tyto konfigurační soubory teď můžou být uložené ve sdílené složce Azure a můžou je číst všechny instance aplikací. Tato nastavení je také možné spravovat přes rozhraní REST, které umožňuje přístup ke konfiguračním souborům po celém světě.

    * **Sdílená složka pro diagnostiku:**  
        Sdílenou složku Azure je také možné použít k ukládání diagnostických souborů, jako jsou protokoly, metriky a výpisy paměti. Jejich dostupnost prostřednictvím protokolu SMB i rozhraní REST umožňuje aplikacím sestavovat nebo využívat celou řadu analytických nástrojů pro zpracování a analýzu diagnostických dat.

    * **Vývoj, testování a ladění:**  
        Vývojáři nebo správci při práci na virtuálních počítačích v cloudu často potřebují sadu nástrojů nebo pomůcek. Instalace a distribuce těchto pomůcek na každý virtuální počítač, kde jsou potřeba, může být časově náročné. Se službou Azure File Storage můžou vývojáři nebo správci ukládat oblíbené nástroje ve sdílené složce, ke které se dá jednoduše připojit z jakéhokoli virtuálního počítače.
        
## <a name="how-does-it-work"></a>Jak to funguje?
Správa sdílených složek Azure je mnohem jednodušší než správa sdílených složek v místním prostředí. Následující diagram znázorňuje, z čeho se správa služby Azure File Storage skládá:

![Struktura souborů](../../includes/media/storage-file-concepts-include/files-concepts.png)

* **Účet služby Storage:** Veškerý přístup ke službě Azure Storage se provádí prostřednictvím účtu úložiště. Podrobné informace o kapacitě účtu úložiště najdete v článku Škálovatelnost a cíle výkonosti služby Azure Storage.
* **Sdílená složka:** Sdílená složka služby File Storage představuje sdílenou složku protokolu SMB v Azure. Všechny adresáře a soubory musí být vytvořeny v nadřazené sdílené složce. Účet může obsahovat neomezený počet sdílených složek a sdílená složka můžete obsahovat neomezený počet souborů až do 5 TB celkové kapacity sdílené složky.
* **Adresář:** Volitelná hierarchie adresářů.
* **Soubor:** Soubor ve sdílené složce. Soubor může mít velikost až 1 TB.
* **Formát adresy URL:** Soubory jsou adresovatelné v následujícím formátu adresy URL:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```
## <a name="next-steps"></a>Další kroky
* [Vytvoření sdílené složky Azure](storage-file-how-to-create-file-share.md)
* [Připojení v systému Windows](storage-file-how-to-use-files-windows.md)
* [Připojení v systému Linux](storage-how-to-use-files-linux.md)
* [Připojení v systému macOS](storage-file-how-to-use-files-mac.md)
* [Nejčastější dotazy](storage-files-faq.md)
* [Řešení potíží](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Koncepční články a videa
* [Azure File Storage: hladký cloudový souborový systém SMB pro Windows a Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Podpora nástrojů pro službu Azure File Storage
* [Použití Azure Powershell s Azure Storage](storage-powershell-guide-full.md)
* [Použití nástroje AzCopy s Microsoft Azure Storage](storage-use-azcopy.md)
* [Použití Azure CLI s Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="blog-posts"></a>Příspěvky na blozích
* [Úložiště Azure File je nyní dostupné pro veřejnost](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Uvnitř Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Představujeme službu Microsoft Azure File](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrace dat do služby Soubory Azure](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referenční informace
* [Klientská knihovna pro úložiště pro .NET – referenční informace](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [REST API služby File – referenční informace](http://msdn.microsoft.com/library/azure/dn167006.aspx)

