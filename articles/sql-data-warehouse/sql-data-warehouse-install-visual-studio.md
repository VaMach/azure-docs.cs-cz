---
title: Instalace sady Visual Studio a SSDT pro SQL Data Warehouse | Microsoft Docs
description: Instalace sady Visual Studio a SQL Server Development Tools (SSDT) pro Azure SQL Data Warehouse
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 12/14/2017
ms.author: kevin;barbkess
ms.openlocfilehash: e8170eefb2e359719684e08749180a4e7784f9b6
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/16/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalace sady Visual Studio a SSDT pro SQL Data Warehouse
K vývoji aplikací pro SQL Data Warehouse, doporučujeme používat nejnovější verzi sady Visual Studio s nejnovější verzi systému SQL Server Data Tools (SSDT).  Kvůli zpětné kompatibilitě se rovněž podporuje Visual Studio 2013 Update 5 s rozšířením SSDT.  

Používání sady Visual Studio s rozšířením SSDT vám umožní pomocí Průzkumníka objektů systému SQL Server vizuálně zkoumat tabulky, zobrazení, uložené procedury a celou řadu dalších objektů v SQL Data Warehouse a také spouštět dotazy.

> [!NOTE]
> SQL Data Warehouse zatím nepodporuje databázové projekty sady Visual Studio.  Tato funkce bude přidána v budoucí verzi. Pokud chcete dostávat pravidelné aktualizace na tuto funkci, prosím hlasovat o [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Krok 1: Instalace sady Visual Studio
Ke stažení a instalaci sady Visual Studio na následujících odkazech. Pokud již máte Visual Studio 2013 nebo novější verzi, můžete přeskočit ke kroku 2, instalaci rozšíření SSDT.

1. [Stáhněte si Visual Studio][].
2. Postupujte podle [instalaci sady Visual Studio] [ Installing Visual Studio] Průvodce na webu MSDN a zvolte výchozí konfigurace.

## <a name="step-2-install-ssdt"></a>Krok 2: Instalace rozšíření SSDT
Když budete chtít nainstalovat rozšíření SSDT pro Visual Studio, jednoduše podle následujícího postupu zkontrolujte dostupnost aktualizace SSDT ze sady Visual Studio.

1. V sadě Visual Studio klikněte na **nástroje** / **rozšíření a aktualizace...** / **Aktualizace**
2. Vyberte **Aktualizace produktu** a potom vyhledejte položku **Microsoft SQL Server Update for database tooling** (Aktualizace Microsoft SQL Serveru pro databázové nástroje).

Pokud se aktualizace nenajde, měli byste mít nainstalovanou nejnovější verzi.  Pokud chcete ověřit, že je rozšíření SSDT nainstalované, klikněte na **Nápověda** / **O sadě Microsoft Visual Studio** a vyhledejte v seznamu položku SQL Server Data Tools.  Nejnovější verze rozšíření SSDT je 14.0.60525.0.  Pokud není k dispozici ze sady Visual Studio možnost instalace, případně můžete navštívit [si rozšíření SSDT Stáhnout] [ SSDT Download] můžete stáhnout a nainstalovat ručně.

## <a name="next-steps"></a>Další kroky
Teď, když máte nejnovější verzi rozšíření SSDT, jste připraveni k [připojit] [ connect] do SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Stáhněte si Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
