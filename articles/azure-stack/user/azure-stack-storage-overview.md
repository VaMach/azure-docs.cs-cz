---
title: "Úvod do Azure zásobník úložiště"
description: "Další informace o Azure zásobník úložiště"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 092aba28-04bc-44c0-90e1-e79d82f4ff42
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/29/2018
ms.author: mabrigg
ms.openlocfilehash: dbc2ffb7540feb91b14c3d502e7f4007a0751ebd
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="introduction-to-azure-stack-storage"></a>Úvod do Azure zásobník úložiště

*Platí pro: Azure zásobníku integrované systémy a Azure zásobníku Development Kit*

## <a name="overview"></a>Přehled
Azure zásobníku úložiště je sada cloudových služeb úložiště včetně objektů BLOB, tabulek a front, které jsou konzistentní s služby Azure Storage.

## <a name="azure-stack-storage-services"></a>Služba Azure zásobník úložiště
Azure zásobníku úložiště poskytuje následující tři služby:

* **Blob Storage** 

    BLOB storage ukládá nestrukturované datové objekty. Objekt blob může být jakýkoli druh textu nebo binárních dat, jako je dokument, soubor médií nebo instalátor aplikace.
* **Úložiště Table** 

    Table storage ukládá strukturované datové sady. Table Storage je datové úložiště na bázi NoSQL typu klíč-atribut, které umožňuje rychlý vývoj a přístup k velkým objemům dat.
* **Queue Storage** 

    Queue storage poskytuje spolehlivé zasílání zpráv pro zpracování pracovního postupu a pro komunikaci mezi součástmi cloudových služeb.

Účet úložiště Azure zásobníku je zabezpečený účet, který poskytuje přístup ke službám v zásobníku úložiště Azure. Váš účet úložiště poskytuje jedinečný obor názvů pro vaše prostředky úložiště. Následující diagram znázorňuje vztahy mezi prostředky Azure zásobníku úložiště v účtu úložiště:

![Přehled služby Azure zásobník úložiště](media/azure-stack-storage-overview/AzureStackStorageOverview.png)


### <a name="blob-storage"></a>Blob Storage

Uživatelé s velkým množstvím nestrukturovaných dat ukládat v cloudu úložiště Blob nabízí efektivní a škálovatelné řešení. Úložiště Blob můžete použít k uložení například těchto druhů obsahu:

* Dokumenty
* Sociální data, jako jsou fotografie, videa, hudba nebo blogy
* Zálohy souborů, počítačů, databází a zařízení
* Obrázky a text pro webové aplikace
* Konfigurační dat pro cloudové aplikace
* Velké objemy dat, jako jsou protokoly a další velké datové sady

Každý objekt blob se organizuje do kontejneru. Kontejnery také nabízejí praktický způsob přiřazení zásad zabezpečení skupinám objektů. Účet úložiště může obsahovat libovolný počet kontejnerů a kontejner může obsahovat libovolný počet objektů BLOB až do limitu účtu úložiště.

Úložiště BLOB nabízí tři typy objektů blob: 
* **Objekty BLOB bloku** 

    Objekty blob bloku jsou optimalizované pro streamování a ukládání cloudových objektů a jsou dobrou volbou pro ukládání dokumentů, souborů médií, záloh atd.
* **Doplňovací objekty BLOB** 

    Doplňovací objekty blob jsou podobné objektům blob bloku, ale jsou optimalizované pro doplňovací operace. Doplňovací objekt blob se může aktualizovat jen přidáním nového bloku na konec. Doplňovací objekty blob jsou dobrou volbou pro takové scénáře, jako je například protokolování, kde se nová data potřebují zapisovat jen na konec objektu blob.
* **Objekty BLOB stránky** 

    Objekty BLOB stránky jsou optimalizované pro zastoupení disků IaaS a podporují náhodné zapisuje, což je velikost až 1 TB. Virtuální počítač Azure zásobníku připojen IaaS disk je virtuální pevný disk uložený jako objekt blob stránky.


### <a name="table-storage"></a>Úložiště Table
Moderní aplikace často potřebují datová úložiště s větší škálovatelností a flexibilitou, než potřebovaly starší generace softwaru. Úložiště Table nabízí vysoce dostupné, enormně škálovatelné úložiště, se kterým se vaše aplikace může automaticky škálovat podle požadavků uživatelů. Úložiště Table je úložiště Microsoftu na bázi NoSQL typu klíč/atribut – a na rozdíl od tradičních relačních databází je bez schématu. S datovým úložištěm bez schématu je snadné data přizpůsobovat měnícím se potřebám vaší aplikace. Úložiště Table se snadno používá, takže vývojáři můžou aplikace vytvářet rychle.

Úložiště Table je úložiště typu klíč-atribut – to znamená, že každá hodnota v tabulce je uložená se typovým názvem vlastnosti. název vlastnosti se může použít pro filtrování a upřesnění kritérií výběru. Kolekce vlastností a jejich hodnot tvoří entitu. Protože úložiště Table nemá schéma, dvě entity ve stejné tabulce můžou obsahovat různé kolekce vlastností a tyto vlastnosti můžou být různých typů.

Úložiště Table Storage můžete používat k ukládání flexibilních datových sad, například uživatelských dat pro webové aplikace, adresářů, informací o zařízení a dalších typů metadat, které vaše služba vyžaduje. Databáze NoSQL, jako je úložiště Table, nabízejí dnešním internetovým aplikacím oblíbenou alternativu tradičních relačních databází.

Účet úložiště může obsahovat libovolný počet tabulek a tabulka může obsahovat libovolný počet entit, až do limitu kapacity účtu úložiště.

### <a name="queue-storage"></a>Queue Storage
Při navrhování aplikací pro škálování ve větším měřítku jsou jednotlivé součásti aplikací často nepropojené, aby je bylo možné škálovat nezávisle. Queue Storage poskytuje spolehlivé řešení zasílání zpráv pro asynchronní komunikaci mezi součástmi aplikací bez ohledu na to, jestli běží v cloudu, v desktopovém prostředí, na místním serveru nebo na mobilním zařízení. Queue Storage také podporuje správu asynchronních úloh a pracovní postupy procesů sestavování buildů.

Účet úložiště může obsahovat libovolný počet front a fronty může obsahovat libovolný počet zpráv až do limitu kapacity účtu úložiště. Jednotlivé zprávy můžou mít velikost až 64 KB.

## <a name="next-steps"></a>Další postup
* [Konzistentní s Azure storage: rozdíly a důležité informace](azure-stack-acs-differences.md)

* Další informace o službě Azure Storage najdete v tématu [Úvod do Microsoft Azure Storage](../../storage/common/storage-introduction.md)

