---
title: "Konektor prostředí PowerShell | Microsoft Docs"
description: "Tento článek popisuje, jak nakonfigurovat konektor služby společnosti Microsoft Windows PowerShell."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2caf8dd8a657f116df0342893763829676602cd6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="windows-powershell-connector-technical-reference"></a>Technické informace o konektoru služby Windows PowerShell
Tento článek popisuje Windows PowerShell Connector. Se článek vztahuje následující produkty:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manageru 2010 R2 (FIM2010R2)
  * Musíte použít opravu hotfix 4.1.3671.0 nebo novější [KB3092178](https://support.microsoft.com/kb/3092178).

Pro MIM2016 a FIM2010R2, je k dispozici ke stažení z konektoru [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Přehled konektoru prostředí PowerShell
PowerShell Connector umožňuje integraci synchronizační službu s externími systémy, které nabízí rozhraními API založenými na prostředí Windows PowerShell. Konektor poskytuje most mezi funkce agenta pro správu na základě volání extensible connectivity 2 (ECMA2) framework a prostředí Windows PowerShell. Další informace o rozhraní ECMA najdete v tématu [Extensible Connectivity 2.2 agenta managementu](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Požadavky
Než použijete tento konektor, ujistěte se, že máte k dispozici následující na serveru synchronizace:

* 4.5.2 rozhraní Microsoft .NET Framework nebo novější
* Prostředí Windows PowerShell 2.0, 3.0 nebo 4.0

Jsou zásady provádění na serveru služby synchronizace musí nakonfigurovat pro povolení konektoru ke spouštění skriptů prostředí Windows PowerShell. Pokud je spuštěn konektor jsou digitálně podepsané, skripty konfiguraci zásady spouštění tak, že spustíte tento příkaz:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Vytvořit nový konektor
Chcete-li vytvořit konektor prostředí Windows PowerShell ve službě synchronizace, je nutné zadat řadu skriptů prostředí Windows PowerShell, které jsou spouštěny kroky požadoval synchronizační služby. V závislosti na zdroji dat, ke kterým se připojujete a funkci, kterou požadujete se liší skripty, které je nutné implementovat. V této části jsou popsané všechny skripty, které se dají implementovat a když jsou povinné.

Prostředí Windows PowerShell konektor je určený k ukládání těchto skriptů v databázi služby synchronizace. I když je možné spouštět skripty, které jsou uloženy v systému souborů, je snazší vložit do těla každý skript přímo v konfiguraci konektoru.

Vytvořit konektor prostředí PowerShell, v **synchronizační služba** vyberte **agenta pro správu** a **vytvořit**. Vyberte **prostředí PowerShell (Microsoft)** konektor.

![Vytvořit konektor](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Připojení
Zadejte parametry konfigurace pro připojení ke vzdálenému systému. Tyto hodnoty jsou bezpečně uložené pomocí služby synchronizace a budou dostupné pro skripty prostředí Windows PowerShell při spuštění konektoru.

![Připojení](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Můžete nakonfigurovat následující parametry připojení:

**Připojení**

| Parametr | Výchozí hodnota | Účel |
| --- | --- | --- |
| Server |<Blank> |Název serveru, který konektor musí připojit k. |
| Doména |<Blank> |Doména pověření, která se uloží pro použití při spuštění konektoru. |
| Uživatel |<Blank> |Uživatelské jméno pověření k uložení pro použití při spuštění konektoru. |
| Heslo |<Blank> |Heslo pověření, která se uloží pro použití při spuštění konektoru. |
| Zosobnit účet konektoru |False |V případě hodnoty true synchronizační služba běží v kontextu pověření zadaná skriptů prostředí Windows PowerShell. Pokud je to možné, doporučujeme **$Credentials** parametr se předává pro každý skript se používá namísto zosobnění. Další informace o další oprávnění, které jsou nutné pro tuto možnost použijte, najdete v tématu [další konfiguraci pro zosobnění](#additional-configuration-for-impersonation). |
| Načíst profil uživatele při zosobnění |False |Dá pokyn Windows načíst profil uživatele konektoru přihlašovacích údajů během zosobnění. Pokud se zosobněným uživatelem má cestovní profil, konektor nenačte profilu roamingu. Další informace o další oprávnění, které jsou nutné k použití tohoto parametru najdete v tématu [další konfiguraci pro zosobnění](#additional-configuration-for-impersonation). |
| Typ přihlášení při zosobnění |Žádné |Typ přihlášení během zosobnění. Další informace najdete v tématu [dwLogonType] [ dw] dokumentaci. |
| Pouze podepsaných skriptů |False |V případě hodnoty true konektor prostředí Windows PowerShell ověří, že každý skript má platný digitální podpis. Pokud je hodnota false, zajistěte, aby byl zásady spouštění prostředí Windows PowerShell serveru služby synchronizace RemoteSigned nebo bez omezení. |

**Běžné modulu**  
Konektor umožňuje ukládání sdílených modul prostředí Windows PowerShell v konfiguraci. Pokud konektor spouští skript, modul Windows PowerShell je extrahován do systému souborů tak, aby jej bylo možné importovat každý skript.

Pro Import, Export a synchronizace hesel skripty je běžné modulu extrahovány do složky MAData konektoru. Schéma, ověřování, hierarchie a oddílu zjišťování skriptů je běžné modulu extrahovat ke složce % TEMP %. V obou případech je název extrahované běžné modulu skriptu podle nastavení běžný název modulu skriptu.

Chcete-li načíst modul s názvem FIMPowerShellConnectorModule.psm1 ve složce MAData, použijte následující příkaz:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Chcete-li načíst modul s názvem FIMPowerShellConnectorModule.psm1 ve složce % TEMP %, použijte následující příkaz:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Ověření parametru**  
Skript ověření je volitelné skript prostředí Windows PowerShell, které je možné zajistit, že jsou platné parametry konfigurace konektoru zadaný správcem. Ověřující server, přihlašovací údaje pro připojení a připojení parametry jsou běžné použití ověřovacího skriptu. Ověření je volán po následující karty a dialogová okna jsou upraveny:

* Připojení
* Globální parametry
* Oddíl konfigurace

Ověření skriptu přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Na kartě Konfigurace nebo dialogové okno, který aktivoval žádosti o ověření. |
| ConfigParameters |[Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |

Ověření skriptu by měl vrátit a jeden objekt ParameterValidationResult do kanálu.

**Schema Discovery**  
Skript zjišťování schématu je povinný. Tento skript vrátí typy objektů, atributy a atribut omezení, které služba synchronizace používá při konfiguraci pravidla toku atributu. Skript zjišťování schématu se spustí při vytváření konektoru a naplní schématu konektor. Používá se také pomocí akce aktualizovat schéma Synchronization Service Manager.

Skript zjišťování schématu přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection] [ keyk] [řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |

Skript musí vracet jedné [schématu] [ schema] objektu do kanálu. Objekt schématu se skládá z [elementu SchemaType] [ schemaT] objekty, které reprezentují typy objektů (například: uživatelé a skupiny). Objekt elementu SchemaType obsahuje kolekci [SchemaAttribute] [ schemaA] objekty, které představují atributy (například: křestní jméno, příjmení a poštovní adresa) typu.

**Další parametry**  
Kromě standardní konfigurace nastavení můžete definovat další vlastní konfiguraci nastavení, které jsou specifické pro instance konektoru. Tyto parametry mohou být zadané na konektoru, oddíl nebo kroku spuštění úrovně a k němu přistupovat z příslušných skript prostředí Windows PowerShell. Vlastní nastavení konfigurace mohou být uloženy v databázi služby synchronizace ve formátu prostého textu nebo může být zašifrovaná. Služba synchronizace automaticky šifruje a dešifruje zabezpečené nastavení konfigurace v případě potřeby.

Zadejte vlastní nastavení konfigurace, název každého parametru oddělte čárkou (,).

Pro přístup k vlastní nastavení konfigurace ze skriptu, musí přípona názvu podtržítko ( \_ ) a obor parametru (globální, oddíl nebo RunStep). Například pro přístup k parametr globální název souboru, použijte tento fragment kódu:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Možnosti
Na kartě Možnosti návrháře agenta správy definuje chování a funkce konektoru. Na této kartě provedených výběrů nelze upravovat, když tento konektor byl vytvořen. Tato tabulka uvádí nastavení schopností.

![Možnosti](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Schopnost | Popis |
| --- | --- |
| [Rozlišující název stylu][dnstyle] |Označuje, zda konektor podporuje rozlišujícími názvy, a pokud ano, jaké stylu. |
| [Typ exportu][exportT] |Určuje typ objektů, které mají být zobrazena skripty pro Export. <li>Při změně atributu, AttributeReplace – obsahuje úplnou sadu hodnot pro atribut s více hodnotami.</li><li>AttributeUpdate – obsahuje pouze rozdíly s více hodnotami atributu při změně atributu.</li><li>MultivaluedReferenceAttributeUpdate – obsahuje úplnou sadu hodnot pro více hodnot atributů – referenční dokumentace a rozdílů pouze pro atributy typu odkaz s více hodnotami.</li><li>ObjectReplace – obsahuje všechny atributy pro objekt, pokud žádné změny atributů</li> |
| [Normalizaci dat][DataNorm] |Dá pokyn synchronizační službu k normalizaci ukotvení atributy předtím, než jsou k dispozici pro skripty. |
| [Objekt potvrzení][oconf] |Konfiguruje chování nástroje čekajícího importu v synchronizační služby. <li>Normální – výchozí chování, která očekává všechny exportovaný změny být potvrzen pomocí importu</li><li>NoDeleteConfirmation – když je odstraněn objekt, neexistuje žádný čekajícího importu vygenerovat.</li><li>NoAddAndDeleteConfirmation – při vytvoření nebo odstranění objektu není žádná čekajícího importu vygenerovat.</li> |
| Používat jako kotvu rozlišující název |Pokud je nastaven styl rozlišující název LDAP, atribut kotvy pro prostoru konektoru je také rozlišující název. |
| Souběžných operací několik konektorů |Pokud je zaškrtnuto, můžete současně spustit více konektorů prostředí Windows PowerShell. |
| Oddíly |Pokud je zaškrtnuto, konektor podporuje víc oddílů a zjišťování oddílu. |
| Hierarchie |Pokud je zaškrtnuto, konektor podporuje hierarchickou strukturu styl LDAP. |
| Povolit Import |Pokud je zaškrtnuto, konektor importuje data prostřednictvím import skriptů. |
| Povolit rozdílový Import |Pokud je zaškrtnuto, konektor může požádat o rozdílů z import skriptů. |
| Povolit Export |Pokud je zaškrtnuto, konektor exportuje data prostřednictvím export skripty. |
| Povolit úplnou Export |Pokud je zaškrtnuto, skripty export podporovala export prostoru celý konektoru. Chcete-li použít tuto možnost, povolit Export musí také kontrolovat. |
| Žádné hodnoty odkaz při průchodu první exportu |Pokud je zaškrtnuto, exportují se v druhé fázi exportu atributy typu odkaz. |
| Povolit přejmenování objektu |Pokud je zaškrtnuto, můžete upravit rozlišujícími názvy. |
| Přidat jako nahradit odstranění |Pokud je zaškrtnuto, delete přidáte operace jsou exportovány jako jeden nahrazení. |
| Povolit operace s heslem |Pokud je zaškrtnuto, jsou podporovány skripty synchronizace hesel. |
| Povolit Export hesla při prvním průchodu |Pokud je zaškrtnuto, exportují se při vytvoření objektu hesla nastavit během zřizování. |

### <a name="global-parameters"></a>Globální parametry
Karta globální parametry v Návrháři agenta správy umožňuje konfigurovat skripty prostředí Windows PowerShell, které spouštějí konektorem. Můžete také nakonfigurovat globální hodnoty pro vlastní nastavení, které jsou uvedené na kartě připojení.

**Oddíl zjišťování**  
Oddíl je samostatný obor názvů v rámci jedné sdílené schématu. Například všechny domény ve službě Active Directory je oddílu v rámci jedné doménové struktuře. Oddíl je logické seskupení pro import a export operace. Import a Export mít oddílu, jako v tomto kontextu se stane kontextu a všechny operace. Oddíly jsou by mělo představují hierarchie v protokolu LDAP. Rozlišující název oddílu se používá v importu, jsou všechny vrácených objektů v rámci oboru oddílu. Rozlišující název oddílu se také používá při zřizování z úložiště metaverse do prostoru konektoru k určení oddíl, který objekt by měly být přidružené během exportu.

Skript zjišťování oddílu přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |

Skript musí vrátit, buď jediný [oddílu] [ part] objekt nebo seznam [T] objekty oddílů do kanálu.

**Zjištění hierarchie**  
Skript zjišťování hierarchie se používá pouze v případě schopností rozlišující název stylu LDAP. Skript se používá k umožňují procházet a vyberte sadu kontejnery, který je považován za v nebo mimo rozsah pro import a export operace. Skript by měl poskytovat pouze seznam uzlů, které jsou přímo podřízené kořenového uzlu zadaný do skriptu.

Skript zjišťování hierarchie přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| ParentNode |[HierarchyNode][hn] |Kořenový uzel hierarchie, pod kterou by měla vrátit skript přímé podřízené objekty. |

Skript musí vrátit buď jeden podřízený objekt HierarchyNode nebo seznam [T] podřízené objekty HierarchyNode do kanálu.

#### <a name="import"></a>Import
Konektory, které podporují operace importu musí implementovat tři skripty.

**Zahájení importu**  
Spuštění skriptu importu begin na začátku na import spustit krok. Během tohoto kroku můžete vytvořit připojení ke zdrojovém systému a udělat přípravné kroky před importem dat z připojený systém.

Začátek importního skriptu přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Skript informuje o typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky. |
| Typy |[Schema][schema] |Schéma pro prostoru konektoru, který není importován. |

Skript musí vracet jedné [OpenImportConnectionResults] [ oicres] objektu do kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Umožňuje importovat Data**  
Import dat je volán konektorem dokud skriptu určuje, že neexistuje žádná další data pro import. Konektor prostředí Windows PowerShell má velikost stránky objektů 9 999. Pokud váš skript vrátí víc než 9 999 objekty pro import, musí podporovat stránkování. Zpřístupňuje konektor, vlastnost vlastní data, můžete použít k úložišti vodoznak tak, že pokaždé, když skriptu pro import dat je volána, váš skript obnoví importu objektů, kde bylo přerušeno.

Import dat skript přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Obsahuje vodoznak (CustomData), který se dá použít během stránkovaného importy a importuje delta. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Skript informuje o typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky. |
| Typy |[Schema][schema] |Schéma pro prostoru konektoru, který není importován. |

Import dat skriptu musíte napsat seznam [[CSEntryChange][csec]] objektu do kanálu. Tato kolekce se skládá z CSEntryChange atributy, které představují každý importovaný objekt. Během spuštění úplný Import tuto kolekci měli kompletní CSEntryChange objektů, které mají všechny atributy pro každý objekt. Během importu rozdílů by měly obsahovat objekt CSEntryChange buď úrovně rozdílů atribut pro každý objekt, který chcete importovat nebo dokončení reprezentace objektů, které se změnily (nahraďte režim).

**End Import**  
Po dokončení importu spustit spuštění skriptu End importovat. Tento skript by měl provést jakékoli úlohy čištění nezbytné (například zavřít připojení k systémům) a reakce na selhání.

Importovat skript end přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Skript informuje o typ importu spustit (Rozdílová nebo úplné), oddíl, hierarchie, vodoznak, velikost očekávané stránky. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Informuje o důvod, proč byla ukončena, import skript. |

Skript musí vracet jedné [CloseImportConnectionResults] [ cicres] objektu do kanálu, například:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Export
Identické s architekturou import konektoru, konektory, které podporují Export musí implementovat tři skripty.

**Zahájení exportu**  
Skripty pro export begin běží na začátku na krok exportu. Během tohoto kroku můžete vytvořit připojení k systému zdrojového a proveďte všechny přípravné kroky před exportem dat do připojený systém.

Skripty pro export begin přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Skript informuje o typu export spustit (Rozdílová nebo úplné), oddíl, hierarchie a velikost očekávané stránky. |
| Typy |[Schema][schema] |Schéma pro prostoru konektoru, která je exportována. |

Skript nesmí vrátit žádný výstup do kanálu.

**Export dat**  
Služba synchronizace volá exportovat Data skriptu jako tolikrát, kolikrát je třeba zpracovat všechny čekající exporty. Pokud prostoru konektoru další čekající exporty než velikost stránky konektoru, skripty pro export dat může volat vícekrát a které by mohly mít více než jednou pro stejný objekt.

Skripty pro export dat přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| CSEntries |IList[CSEntryChange][csec] |Seznam všech prostoru konektoru objekty s čekající exporty mají být zpracovány během této relace. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Skript informuje o typu export spustit (Rozdílová nebo úplné), oddíl, hierarchie a velikost očekávané stránky. |
| Typy |[Schema][schema] |Schéma pro prostoru konektoru, která je exportována. |

Skripty pro export dat musí vracet [PutExportEntriesResults] [ peeres] objektu do kanálu. Tento objekt není nutné zahrnout výsledek informace pro každý konektor exportovaný nenastane chybu nebo ke změně atributu ukotvení. Chcete-li například vrátí objekt PutExportEntriesResults do kanálu:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**End Export**  
V závěru exportu, spuštění, spuštění skriptu End exportovat. Tento skript by měl provést jakékoli úlohy čištění nezbytné (například zavřít připojení k systémům) a reakce na selhání.

Skripty pro export end přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Skript informuje o typu export spustit (Rozdílová nebo úplné), oddíl, hierarchie a velikost očekávané stránky. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Skript informuje o důvod, proč exportu bylo ukončeno. |

Skript nesmí vrátit žádný výstup do kanálu.

#### <a name="password-synchronization"></a>Synchronizace hesel
Konektory prostředí Windows PowerShell můžete použít jako cíl pro změny nebo resetování hesla.

Skript heslo přijímá následující parametry z konektoru nástroje:

| Název | Typ dat | Popis |
| --- | --- | --- |
| ConfigParameters |[Kolekci KeyedCollection][keyk][řetězec, [ConfigParameter][cp]] |Tabulka parametry konfigurace pro konektor. |
| Přihlašovací údaj |[Přihlašovací údaje][pscred] |Obsahuje všechny přihlašovací údaje správce zadali na kartě připojení. |
| Oddíl |[Oddíl][part] |Oddíl adresáře, který je CSEntry v. |
| CSEntry |[CSEntry][cse] |Položka místa konektoru pro objekt, který je přijala změny hesla nebo resetování. |
| Typ operace |Řetězec |Určuje, zda operaci provést obnovení (**SetPassword**) nebo ke změně (**Změna hesla byla**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Příznaky, které určují určený heslo resetovat chování. Tento parametr je k dispozici, pokud je typ operace pouze **SetPassword**. |
| Původní_heslo |Řetězec |Naplní objektu staré heslo pro změny hesla. Tento parametr je k dispozici, pokud je typ operace pouze **Změna hesla byla**. |
| Nové_heslo |Řetězec |Obsahuje nové heslo objektu, který musí nastavit skript. |

Skript heslo neočekává se, že výsledky zřetězením příkazů Windows Powershellu. Pokud dojde k chybě ve skriptu heslo, mají skript vyvolat jeden z následujících výjimek pro synchronizační službu informovat o problému:

* [PasswordPolicyViolationException] [ pwdex1] – vygeneruje se, pokud heslo nesplňuje zásady hesel v systému připojený.
* [PasswordIllFormedException] [ pwdex2] – vygeneruje se, pokud heslo není přijatelné pro připojený systém.
* [PasswordExtension] [ pwdex3] – vyvolána pro všechny chyby ve skriptu heslo.

## <a name="sample-connectors"></a>Ukázka konektory
Úplný přehled k dispozici ukázkový konektory, najdete v části [Windows PowerShell Connector ukázka konektor kolekce][samp].

## <a name="other-notes"></a>Další poznámky
### <a name="additional-configuration-for-impersonation"></a>Další konfiguraci pro zosobnění
Udělte následující oprávnění na serveru služby synchronizace zosobnit uživatele, který patří:

Přístup pro čtení k tyto klíče registru:

* HKEY_USERS\\[SynchronizationServiceServiceAccountSID]\Software\Microsoft\PowerShell
* HKEY_USERS\\[SynchronizationServiceServiceAccountSID]\Environment

Pokud chcete zjistit identifikátor zabezpečení (SID) účtu služby synchronizační služba, spusťte následující příkazy prostředí PowerShell:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Přístup pro čtení do následující složky systému souborů:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft Forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Nahraďte název konektoru prostředí Windows PowerShell pro zástupný symbol {Názevkonektoru}.

## <a name="troubleshooting"></a>Řešení potíží
* Informace o tom, jak povolit protokolování pro řešení potíží s konektoru najdete v tématu [postup povolení trasování ETW pro konektory](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
