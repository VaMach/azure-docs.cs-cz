<properties
   pageTitle="Instalace sady Visual Studio a SSDT pro SQL Data Warehouse | Microsoft Azure"
   description="Instalace sady Visual Studio a SQL Server Development Tools (SSDT) pro Azure SQL Data Warehouse"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/01/2016"
   ms.author="sonyama;barbkess"/>

# Instalace sady Visual Studio 2015 a SSDT pro SQL Data Warehouse

Pro vývoj aplikací pro SQL Data Warehouse doporučujeme používat Visual Studio 2015 s nejnovější verzí rozšíření SQL Server Data Tools (SSDT).  Visual Studio 2013 s rozšířením SSDT je podporováno pro zpětnou kompatibilitu.  

Používání sady Visual Studio s rozšířením SSDT vám umožní pomocí Průzkumníka objektů systému SQL Server vizuálně zkoumat tabulky, zobrazení, uložené procedury a celou řadu dalších objektů v SQL Data Warehouse a také spouštět dotazy.

> [AZURE.NOTE] SQL Data Warehouse zatím nepodporuje databázové projekty sady Visual Studio.  Tato funkce bude přidána v budoucí verzi.

## Krok 1: Instalace sady Visual Studio 2015

Z těchto odkazů si stáhněte Visual Studio 2015 a nainstalujte si ho. Pokud už máte nainstalovanou sadu Visual Studio 2013 nebo 2015, můžete přeskočit ke kroku 2, instalaci rozšíření SSDT.

1. [Stáhněte si Visual Studio 2015][].
2. Postupujte podle pokynů průvodce [instalací sady Visual Studio][] na webu MSDN a zvolte výchozí konfigurace.

## Krok 2: Instalace rozšíření SSDT

Když budete chtít nainstalovat rozšíření SSDT pro Visual Studio, jednoduše podle následujícího postupu zkontrolujte dostupnost aktualizace SSDT ze sady Visual Studio.

1. V sadě Visual Studio klikněte na **Nástroje** / **Rozšíření a aktualizace...** / **Aktualizace**.
2. Vyberte **Aktualizace produktu** a potom vyhledejte položku **Microsoft SQL Server Update for database tooling** (Aktualizace Microsoft SQL Serveru pro databázové nástroje).

Pokud se aktualizace nenajde, měli byste mít nainstalovanou nejnovější verzi.  Pokud chcete ověřit, že je rozšíření SSDT nainstalované, klikněte na **Nápověda** / **O sadě Microsoft Visual Studio** a vyhledejte v seznamu položku SQL Server Data Tools.  Nejnovější verze rozšíření SSDT je 14.0.60525.0.  Pokud v sadě Visual Studio nebude dostupná možnost instalace, můžete navštívit stránku pro [stažení rozšíření SSDT][] a z ní si rozšíření SSDT stáhnout a nainstalovat ručně.

## Další kroky

Teď, když máte nejnovější verzi rozšíření SSDT, se můžete [připojit][] ke své službě SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[připojit]: ./sql-data-warehouse-get-started-connect.md

<!--Other-->
[Stáhněte si Visual Studio 2015]: https://www.visualstudio.com/downloads/
[instalací sady Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[stažení rozšíření SSDT]: https://msdn.microsoft.com/library/mt204009.aspx



<!--HONumber=Jun16_HO2-->


