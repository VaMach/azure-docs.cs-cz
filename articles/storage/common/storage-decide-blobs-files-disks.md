---
title: "Rozhodování o použití objektů BLOB služby Azure, Azure soubory nebo disky Azure"
description: "Další informace o různé způsoby, jak ukládat a přistupovat k datům v Azure a pomoci, že se že rozhodnete technologii, která má použít."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: e68f02484c0b8de8319a2d9a6d7e3a01ee06c69a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-disks"></a>Rozhodování o použití objektů BLOB služby Azure, Azure soubory nebo disky Azure

Microsoft Azure poskytuje několik funkcí ve službě Azure Storage k ukládání a přístup k datům v cloudu. Tento článek popisuje soubory Azure, objekty BLOB a disků a je navržená tak, abyste si mohli vybrat mezi těmito funkcemi.

## <a name="scenarios"></a>Scénáře

Následující tabulka porovnává souborů, objektů BLOB a disků a uvádí příklad scénáře vhodné pro každý.

| Funkce | Popis | Kdy je použít |
|--------------|-------------|-------------|
| **Soubory Azure** | Poskytuje rozhraní protokolu SMB, knihovny klienta a [rozhraní REST](/rest/api/storageservices/file-service-rest-api) umožňuje přístup odkudkoli k uložených souborů. | Chcete "navýšení a posunutí" aplikace do cloudu, což už používá rozhraní API systému nativní soubor sdílet data mezi nimi a dalších aplikací běžících v Azure.<br/><br/>Chcete uložit vývoj a ladicí nástroje, které je třeba získat přístup z více virtuálních počítačů. |
| **Azure Blobs** | Poskytuje klientské knihovny a [rozhraní REST](/rest/api/storageservices/blob-service-rest-api) umožňuje nestrukturovaných dat na ukládat a přistupovat v masivním měřítku v objekty BLOB bloku. | Chcete, aby vaše aplikace pro podporu streamování a scénáře náhodný přístup.<br/><br/>Chcete být schopni přistupovat data aplikací z libovolného místa. |
| **Disky systému Azure** | Poskytuje klientské knihovny a [rozhraní REST](/rest/api/compute/manageddisks/disks/disks-rest-api) data trvale ukládat a přistupovat z připojený virtuální pevný disk, který umožňuje. | Chcete navýšení a posunutí aplikace, které používají rozhraní API systému nativní souboru pro čtení a zápisu dat do trvalého disky.<br/><br/>Chcete uložit data, která nemusí být přístupná z mimo virtuální počítač, ke kterému je disk připojen. |

## <a name="comparison-files-and-blobs"></a>Porovnání: Soubory a objekty BLOB

Následující tabulka porovnává soubory Azure s Azure BLOB.  
  
||||  
|-|-|-|  
|**Atribut**|**Azure Blobs**|**Soubory Azure**|  
|Možnosti odolnost|LRS, ZRS (GRS a RA-GRS vyšší dostupnost)|LRS, GRS|  
|Usnadnění přístupu|Rozhraní REST API|Rozhraní REST API<br /><br /> SMB 2.1 a SMB 3.0 (systém souborů standardní rozhraní API)|  
|Připojení|Rozhraní REST API – po celém světě|Rozhraní REST API – po celém světě<br /><br /> Protokol SMB 2.1--v rámci oblasti<br /><br /> Protokol SMB 3.0 – po celém světě|  
|Koncové body|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Adresáře|Plochý obor názvů|Hodnota TRUE, directory objekty|  
|Rozlišování názvů|Rozlišovat malá a velká|Případ malých a velkých písmen, ale případu zachování|  
|Kapacita|Až 500 TB kontejnery|5 TB sdílené složky|  
|Propustnost|Až 60 MB za sekundu na jeden objekt blob bloku|Až 60 MB/s za sdílené složky|  
|Velikost objektu|Objekt blob až 200 GB nebo bloku|Až 1 TB nebo k souboru|  
|Fakturovaná kapacity|Podle zapsaných bajtů|Podle velikosti souboru|  
|Klientské knihovny|Více jazyků|Více jazyků|  
  
## <a name="comparison-files-and-disks"></a>Porovnání: Soubory a disky

Soubory Azure doplňují Azure disky. Disk lze připojit pouze k jedné virtuální počítač Azure v čase. Disky jsou pevného formátu virtuální pevné disky uložené jako objekty BLOB stránky ve službě Azure Storage a se používají pro virtuální počítač k ukládání trvalá data. Sdílené složky v souborech Azure je přístupná stejným způsobem, jako je přistupovat na místní disk (pomocí systému souborů nativní rozhraní API) a můžete sdílet mezi více virtuálních počítačů.  
 
Následující tabulka porovnává soubory Azure s Azure disky.  
 
||||  
|-|-|-|  
|**Atribut**|**Disky systému Azure**|**Soubory Azure**|  
|Rozsah|Výhradně pro jeden virtuální počítač|Sdílený přístup napříč více virtuálních počítačů|  
|Snímků a kopírování|Ano|Ne|  
|Konfigurace|Připojení při spuštění virtuálního počítače|Připojení po spuštění virtuálního počítače|  
|Authentication|Integrované|Nastavit pomocí příkazu net use|  
|Čištění|Automaticky|Ručně|  
|Přístup pomocí REST|Nelze získat přístup k souborů v rámci virtuální pevný disk|Soubory uložené ve sdílené složce je přístupná.|  
|Maximální velikost|4 TB disku|5 TB sdílení souborů a 1 TB soubor ve složce|  
|Maximální IOps 8KB|500 IOps|1000 IOps|  
|Propustnost|Až 60 MB/s na Disk|Až 60 MB/s na základě sdílené složky|  

## <a name="next-steps"></a>Další postup

Při rozhodování o tom, jak je vaše data uložené a získat přístup, měli byste také zvážit náklady související se situací. Další informace najdete v tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/).
  
Některé funkce protokolu SMB se nedají použít do cloudu. Další informace najdete v tématu [funkce nejsou podporovány službou Azure File](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Další informace o discích najdete v tématu [Správa disků a bitové kopie](../../virtual-machines/windows/about-disks-and-vhds.md) a [jak připojit datový Disk do virtuálního počítače s Windows](../../virtual-machines/windows/attach-managed-disk-portal.md).
