---
title: "Poznámky k verzi služby Azure Data Catalog | Microsoft Docs"
description: "Poznámky k verzi pro Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: ba79bf465fc7775c44b370de287bfca5e29beccd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-release-notes"></a>Poznámky k verzi služby Azure Data Catalog
## <a name="notes-for-the-november-20-2015-release-of-azure-data-catalog"></a>Poznámky pro 20. listopadu 2015 verzi služby Azure Data Catalog
### <a name="opening-data-sources-in-power-bi-desktop"></a>Otevírání zdroje dat v Power BI Desktop
Při použití volby "Otevřít v Power BI Desktop" z **Azure Data Catalog** portálu, uživatelé setkat s jedním z dva problémy v aplikaci Power BI Desktop:

* Zobrazí se dialogové okno s názvem "Nelze pro otevřete dokument"
* Otevře aplikace Power BI Desktop, ale soubor zdá být prázdné.

Pro každé situaci lze vyřešit problém stahuje a instaluje nejnovější verze Power BI Desktop z [PowerBI.com](https://powerbi.com).

## <a name="notes-for-the-november-13-2015-release-of-azure-data-catalog"></a>Poznámky pro 13. listopadu 2015 verzi služby Azure Data Catalog
### <a name="registering-and-connecting-to-teradata"></a>Registrace a připojení k Teradata
Při připojování k Teradata zdroje dat uživatelům musí mít nainstalované správné ovladače Teradata ODBC odpovídající bitová verze (32bitová nebo 64bitová verze) softwaru používán.

Od této ADC datum vydání, nejnovější [ovladač Teradata ODBC pro windows (verze 15.10)](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) je kompatibilní s Office 2013, ale ne při Office 2016.

## <a name="notes-for-the-july-13-2015-release-of-azure-data-catalog"></a>Poznámky pro 13. července 2015 verzi služby Azure Data Catalog
### <a name="registering-and-connecting-to-oracle-database"></a>Registrace a připojení k databázi Oracle
Při připojování ke zdrojům dat Oracle Database uživatelům musí mít nainstalované správné ovladače Oracle odpovídající bitová verze (32bitová nebo 64bitová verze) softwaru používá.

* Při registraci zdroje dat Oracle na počítači se systémem Windows 32-bit, použije se 32bitové ovladače Oracle
* Při registraci zdroje dat Oracle na počítači se systémem Windows 64-bit, použije se 64bitové ovladače Oracle
* Při připojování ke zdrojům dat Oracle pomocí aplikace Excel v počítači se systémem 32bitovou verzi systému Microsoft Office, včetně na 64bitovém systému Windows, bude použit 32bitové ovladače Oracle
* Při připojení ke zdrojům dat Oracle pomocí aplikace Excel v počítači se systémem 64bitové verze systému Microsoft Office, budou použity ovladače Oracle 64-bit

### <a name="registering-and-connecting-to-sql-server-reporting-services"></a>Registrace a připojení k SQL Server Reporting Services
Podpora pro zdroje dat služby SQL Server Reporting Services (SSRS) je aktuálně omezená nativního režimu jen pro servery. Podpora služby SSRS v režimu serveru SharePoint přidá v novější verzi.

### <a name="opening-data-assets-in-excel"></a>Otevírání datových prostředků v aplikaci Excel
Při otevírání datových prostředků v aplikaci Microsoft Excel z **Azure Data Catalog** portálu, může se uživatelům výzva s **oznámení o zabezpečení aplikace Microsoft Excel** dialogové okno. Toto je standardní, můžete vybrat očekávané chování a uživatelé **povolit** pokračujte.

Další informace najdete v tématu [povolit nebo zakázat výstrahy zabezpečení týkající se propojení a souborů z podezřelé weby](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy server, zásad konfigurace a data registrace zdroje
Uživatelé mohou nastat situace, kdy se nemohou přihlásit k portálu Azure Data Catalog, ale při pokusu o přihlášení k nástroj registrace zdroje dat narazí chybovou zprávu, která je zabraňuje v přihlášení.

Existují dvě možné příčiny tohoto problému chování:

**Příčina 1: Konfigurace služby Active Directory Federation Services** nástroj registrace zdroje dat používá ověřování založené na formulářích k ověření přihlášení uživatelů pro službu Active Directory. Pro úspěšné přihlášení musí být povolené ověřování pomocí formulářů ve globální zásady ověřování správcem služby Active Directory.

V některých situacích může dojít toto chování chyba, pouze pokud je uživatel v podnikové síti, nebo jenom v případě, že se uživatel připojuje z vnějšku firemní sítě. Globální zásady ověřování umožňuje metody ověřování pro intranetové a připojení k síti extranet povolení samostatně. Pokud je ověřování pomocí formulářů není povoleno pro síť, ze kterého se uživatel připojuje, může dojít k chybám přihlášení.

Další informace najdete v tématu [konfigurace zásad ověřování](https://technet.microsoft.com/library/dn486781.aspx).

**Příčina 2: Konfigurace proxy serveru sítě** Pokud podniková síť používá proxy server, nástroj pro registraci nemusí být možné se připojit k Azure Active Directory prostřednictvím proxy serveru. Uživatele můžete zajistit, aby nástroj pro registraci úpravou konfiguračního souboru nástroje, přidání této části do souboru:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


Vyhledejte soubor RegistrationTool.exe.config, spusťte nástroj pro registraci a pak otevřete nástroj Správce úloh systému Windows. Na kartě Podrobnosti ve Správci úloh klikněte pravým tlačítkem na RegistrationTool.exe a rozbalovací nabídce vyberte příkaz Otevřít umístění souboru.
