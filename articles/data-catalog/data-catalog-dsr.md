---
title: "Podporované zdroje dat v Azure Data Catalog | Microsoft Docs"
description: "V tomto článku jsou uvedeny specifikace zdrojů dat aktuálně podporované."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Podporované zdroje dat v Azure Data Catalog

Metadata můžete publikovat pomocí veřejné rozhraní API nebo klikněte na-jednou registrace Nástroje, nebo když ručně zadáte informace přímo do Azure Data Catalog webový portál. Následující tabulka shrnuje všechny zdroje dat, které jsou podporovány v katalogu dnes a možnosti publikování pro každou. Externí data nástroje, které všechny zdroje dat, můžete spustit z našich zkušeností portál "open in" jsou také uvedeny. Druhá tabulka obsahuje další technické specifikace každé připojení vlastnosti zdroje dat.


## <a name="list-of-supported-data-sources"></a>Seznam podporovaných zdrojů dat

<table>
    <tr>
       <td><b>Objekt zdroje dat</b></td>
       <td><b>Rozhraní API</b></td>
       <td><b>Ruční zadání</b></td>
       <td><b>Nástroj pro registraci</b></td>
       <td><b>Nástroje pro Open in</b></td>
       <td><b>Poznámky k</b></td>
    </tr>
    <tr>
      <td>Adresář Azure Data Lake Store</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Soubor Azure Data Lake Store</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Blob Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Adresář úložiště Azure</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulky Azure Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td>HDFS adresáře</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HDFS souboru</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulku Hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení Hive</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení MySQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení databáze Oracle</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Jiné (Obecné asset)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulky Azure SQL Data Warehouse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení SQL Data Warehouse</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Dimenze služby SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services klíčového ukazatele výkonu</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Míra SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulky SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Aplikace Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sestavy služby SQL Server Reporting Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Prohlížeč</font></td>
      <td><font size=2>Pouze servery v nativním režimu. Režim serveru SharePoint není podporována.</font></td>
    </tr>
    <tr>
      <td>Tabulka systému SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení systému SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Nástroje aplikace Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení SAP HANA</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení DB2</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Soubor systému souborů</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Adresář serveru FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Soubor FTP</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sestavy HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Koncový bod HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Soubor protokolu HTTP</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sada entit OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Funkce OData</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka PostgreSQL</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL zobrazení</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení SAP HANA</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> Objekt služby Salesforce</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Seznam serveru SharePoint </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Cosmos DB kolekce</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Obecná tabulka rozhraní ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Obecná zobrazení rozhraní ODBC</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka Cassandra</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></td>
    </tr>
    <tr>
      <td>Cassandra zobrazení</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></td>
    </tr>
    <tr>
      <td>Tabulka Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Zobrazení Sybase</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabulka MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></td>
    </tr>
    <tr>
      <td>Zobrazení MongoDB</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Publikovat jako obecný asset rozhraní ODBC</font></td>
    </tr>
</table>

Pokud potřebujete podporu pro další zdroje, podat žádost o funkci [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


## <a name="data-source-reference-specification"></a>Specifikace odkaz na zdroj dat
> [!NOTE]
> **DSL struktura** sloupci v následující tabulce jsou uvedeny pouze vlastnosti připojení pro kontejner objektů a dat "address" používané Azure Data Catalog. To znamená balík vlastností "address" může obsahovat další vlastnosti připojení zdroje dat, které Azure Data Catalog potrvají, ale nepoužívá.

<table>
    <tr>
       <td><b>Typ zdroje</b></td>
       <td><b>Typ prostředku</b></td>
       <td><b>Typy objektů</b></td>
       <td><b>Struktura DSL<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Kontejner</td>
      <td>Data Lake</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Ověřování: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Table</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Ověřování: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kontejner</td>
      <td>Kontejner</td>
      <td>
        <font size=2>Protokol:-objektů BLOB služby azure <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Objekt BLOB, adresář</td>
      <td>
        <font size=2>Protokol:-objektů BLOB služby azure <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kontejner <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Kontejner</td>
      <td>Kontejner</td>
      <td>
        <font size=2>Protokol: azure – tabulky <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        <font size=2>Protokol: azure – tabulky <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domény <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;účet <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Jméno</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Kontejner</td>
      <td>Virtuální cluster</td>
      <td>
        <font size=2>Protokol: cosmos <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Table</td>
      <td>Datový proud, sadu datového proudu, zobrazení</td>
      <td>
        <font size=2>Protokol: cosmos <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Kontejner</td>
      <td>Lokality</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Sestava</td>
      <td>Sestavy, řídicí panel</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: db2 <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: db2 <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </td>
    </tr>
    <tr>
      <td>Systém souborů</td>
      <td>Table</td>
      <td>File</td>
      <td>
        <font size=2>Protokol: soubor <br>Ověřování: {none, basic, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Table</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protokol: ftp <br>Ověřování: {none, basic, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Kontejner</td>
      <td>Cluster</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Ověřování: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Table</td>
      <td>Adresář, soubor</td>
      <td>
        <font size=2>Protokol: webhdfs <br>Ověřování: {základní, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: hive <br>Ověřování: {HDInsight, basic, uživatelské jméno, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: hive <br>Ověřování: {HDInsight, basic, uživatelské jméno, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Kontejner</td>
      <td>Lokality</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Sestava</td>
      <td>Sestavy, řídicí panel</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Table</td>
      <td>Koncový bod, soubor</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: mysql <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: mysql <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Kontejner</td>
      <td>Kontejneru entit</td>
      <td>
        <font size=2>Protokol: odata <br>Ověřování: {none, basic, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Table</td>
      <td>Sady entit, – funkce</td>
      <td>
        <font size=2>Protokol: odata <br>Ověřování: {none, basic, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;prostředek</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: oracle <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: oracle <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: postgresql <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: postgresql <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Kontejner</td>
      <td>Lokality</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Sestava</td>
      <td>Sestavy, řídicí panel</td>
      <td>
        <font size=2>Protokol: http <br>Ověřování: {none, basic, windows, oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Table</td>
      <td>Data hybridní webové aplikace</td>
      <td>
        <font size=2>Protokol: power dotazu <br>Ověřování: {oauth} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Table</td>
      <td>Objekt</td>
      <td>
        <font size=2>Protokol: salesforce-com <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;– Třída <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Název položky</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Kontejner</td>
      <td>Server</td>
      <td>
        <font size=2>Protokol: sap hana-sql <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Table</td>
      <td>Zobrazení</td>
      <td>
        <font size=2>Protokol: sap hana-sql <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Table</td>
      <td>Seznam</td>
      <td>
        <font size=2>Protokolů: sharepoint – seznam <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Příkaz</td>
      <td>Uložené procedury</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Funkce vracející tabulku</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Příkaz</td>
      <td>Uložené procedury</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Funkce vracející tabulku</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: tds. <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Míra</td>
      <td>Míra</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Table</td>
      <td>Dimenze</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {{dimenze}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulkové</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulkové</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>KLÍČOVÝ UKAZATEL VÝKONU</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {klíčový ukazatel výkonu}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulkové</td>
      <td>Míra</td>
      <td>Míra</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabulkové</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        <font size=2>Protokolu: analýza služby <br>Ověřování: {windows, basic, anonymní, none} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Kontejner</td>
      <td>Server</td>
      <td>
        <font size=2>Protokol: reporting services <br>Ověřování: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Sestava</td>
      <td>Sestava</td>
      <td>
        <font size=2>Protokol: reporting services <br>Ověřování: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Cesta <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: teradata <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: teradata <br>Ověřování: {protokol, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Kontejner</td>
      <td>Model</td>
      <td>
        <font size="2">Protokol: mssql-mds <br>Ověřování: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Table</td>
      <td>Entita</td>
      <td>
        <font size="2">Protokol: mssql-mds <br>Ověřování: {windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entity</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: dokument db <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Kolekce</td>
      <td>Kolekce</td>
      <td>
        <font size=2>Protokol: dokument db <br>Ověřování: {azure přístupový klíč} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Adresa URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;kolekce</font>
      </td>
    </tr>
    <tr>
      <td>Obecná rozhraní ODBC</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>Protokol: rozhraní odbc <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Obecná rozhraní ODBC</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>Protokol: rozhraní odbc <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Možnosti <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Kontejner</td>
      <td>Databáze</td>
      <td>
        <font size=2>protokol: sybase <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Table</td>
      <td>Zobrazení tabulky</td>
      <td>
        <font size=2>protokol: sybase <br>Ověřování: {základní, windows} <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;databáze <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schéma <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objekt</font>
      </td>
    </tr>
    <tr>
      <td>Jiné (žádná z výše uvedených)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protokol: Obecné – prostředek <br>Adresa: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID</font>
      </td>
    </tr>
</table>
