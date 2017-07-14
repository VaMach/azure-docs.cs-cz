---
title: "Co je služba Azure Analysis Services | Dokumentace Microsoftu"
description: "Získejte celkový přehled o službě Analysis Services v Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 83d7a29c-57ae-4aa0-8327-72dd8f00247d
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 34726377836d00d484ca01edb098f6c7cbfa9dbf
ms.contentlocale: cs-cz
ms.lasthandoff: 06/17/2017


---
# Co je služba Azure Analysis Services?
<a id="what-is-azure-analysis-services" class="xliff"></a>
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Služba Azure Analysis Services, která je založená na osvědčeném analytickém modulu Microsoft SQL Server Analysis Services, zajišťuje modelování dat na podnikové úrovni v cloudu. 

Podívejte se na toto video a zjistěte, jak služba Azure Analysis Services zapadá do celkových možností BI společnosti Microsoft a jaké výhody vám může přinést převedení datových modelů do cloudu.


>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>


## Vytvořeno na základě SQL Server Analysis Services
<a id="built-on-sql-server-analysis-services" class="xliff"></a>
Služba Azure Analysis Services je kompatibilní se stejnou edicí SQL Server Analysis Services Enterprise Edition, kterou už znáte. Azure Analysis Services podporuje tabulkové modely na úrovni kompatibility 1200 nebo vyšší. Podporují se DirectQuery, oddíly, zabezpečení na úrovni řádku, obousměrné relace a překlady.

## Používejte známé nástroje.
<a id="use-the-tools-you-already-know" class="xliff"></a>
![Vývojářské nástroje BI](./media/analysis-services-overview/aas-overview-dev-tools.png)

Při vytváření datových modelů pro Azure Analysis Services můžete používat stejné nástroje jako pro SQL Server Analysis Services. Vytvářejte a nasazujte modely pomocí [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx). Spravujte servery a modelové databáze pomocí aplikace [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). A automatizujte úlohy pomocí šablon [PowerShellu](analysis-services-powershell.md) a [Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md). 

## Podporuje nejnovější funkce
<a id="supports-the-latest-features" class="xliff"></a>
Azure Analysis Services podporuje tabulkové modely na úrovni kompatibility 1200 a 1400 Preview.

**Tabulkový 1200** – model 1200 byl poprvé součástí služby SQL Server 2016 Analysis Services a představil tabulkový objektový model (TOM) pro popis modelových objektů, jako jsou například tabulky, sloupce a relace. Tabulkový objektový model je přístupný ve formátu JSON prostřednictvím [jazyka TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) a jazyka DDL AMO v oboru názvů [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

**Tabulkový 1400 Preview** – představuje podporu řádků podrobností, zabezpečení na úrovni objektů, nepravidelné hierarchie, moderní prostředí získávání dat pro datové připojení a mnoho dalších vylepšení. Abyste mohli využívat všechny nejnovější funkce, budete muset použít nejnovější [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx). Vzhledem k tomu, že je model Tabulkový 1400 stále ve verzi Preview, mění se velmi rychle. Nejnovější informace najdete v našem [blogovém příspěvku](https://azure.microsoft.com/blog/1400-models-in-azure-as/).

## Zdroje dat
<a id="data-sources" class="xliff"></a>
Datové modely nasazené na servery v Azure podporují připojení ke zdrojům dat místně ve vaší organizaci nebo na cloudu. Kombinací dat z místních a cloudových zdrojů dat získáte hybridní řešení BI.

Vzhledem k tomu, že server je v cloudu, je připojení ke cloudovým zdrojům dat bezproblémové. [Místní brána dat](analysis-services-gateway.md) zajistí při připojování k místním zdrojům dat rychlé a zabezpečené připojení k serveru v cloudu. Další informace o podporovaných místních zdrojích dat najdete v tématu [Zdroje dat podporované ve službě Azure Analysis Services](analysis-services-datasource.md).


## Prozkoumávejte data odkudkoli
<a id="explore-your-data-from-anywhere" class="xliff"></a>
Připojte se a získejte data ze serverů prakticky odkudkoli. Služba Azure Analysis Services podporuje připojení z aplikace Power BI Desktop, Excelu, vlastních aplikací a nástrojů založených na prohlížeči.

![Vizualizace dat](./media/analysis-services-overview/aas-overview-visualization.png)


## Zabezpečení
<a id="secure" class="xliff"></a>
#### Ověřování uživatelů
<a id="user-authentication" class="xliff"></a>
Ověřování uživatelů pro Azure Analysis Services zařizuje služba [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Při pokusu o přihlášení k databázi Azure Analysis Services používají uživatelé identitu účtu organizace s přístupem k databázi, ke které se snaží získat přístup. Tyto identity uživatelů musí být členy výchozí služby Azure Active Directory pro předplatné, ve kterém se server služby Azure Analysis Services nachází. [Integrace adresáře](https://technet.microsoft.com/library/jj573653.aspx) mezi AAD a místní službou Active Directory je skvělým způsobem, jak pro uživatele získat přístup k databázi Azure Analysis Services. Nevyžaduje se ale pro všechny scénáře.

Uživatelé se přihlašují hlavním názvem uživatele (UPN) svého účtu a heslem. Při synchronizaci s místní službou Active Directory je hlavním názvem uživatele často e-mailová adresa jejich organizace.

Oprávnění pro správu prostředků serveru Azure Analysis Services se zpracovávají prostřednictvím přiřazování uživatelů do rolí v rámci předplatného Azure. Ve výchozím nastavení mají správci předplatného oprávnění vlastníka prostředku serveru v Azure. Další uživatele je možné přidat pomocí Azure Resource Manageru.

#### Zabezpečení dat
<a id="data-security" class="xliff"></a>
Služba Azure Analysis Services využívá úložiště Azure Blob Storage k zachování úložiště a metadat pro databáze služby Analysis Services. Datové soubory jsou v rámci objektu Blob šifrované pomocí šifrování na straně serveru Azure Blob. Při použití režimu přímých dotazů se ukládají jenom metada. Ke skutečným datům se přistupuje ze zdroje dat v době dotazu.

#### Místní zdroje dat
<a id="on-premises-data-sources" class="xliff"></a>
Zabezpečeného přístupu k datům nacházejícím se místně ve vaší organizaci se dá dosáhnout instalací a konfigurací [místní brány dat](analysis-services-gateway.md). Brány poskytují přístup k datům pro režimy přímých dotazů a v paměti. Když se model služby Azure Analysis Services připojuje k místnímu zdroji dat, vytvoří se dotaz společně se zašifrovanými přihlašovacími údaji pro místní zdroj dat. Cloudová služba brány analyzuje dotaz a nabídne žádost sběrnici Azure Service Bus. Místní brána zadá dotazy sběrnici Azure Service Bus na čekající žádosti. Brána pak získá dotaz, dešifruje přihlašovací údaje a připojí se ke zdroji dat pro provedení. Výsledky se pak odešlou ze zdroje dat zpět do brány a potom dále do databáze Azure Analysis Services.

Služba Azure Analysis Services se řídí [podmínkami služeb Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [prohlášením o zásadách ochrany osobních údajů služeb Microsoft Online](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Pokud se o službě Azure Security chcete dozvědět víc, podívejte se na [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## Podpora
<a id="get-help" class="xliff"></a>

### Dokumentace
<a id="documentation" class="xliff"></a>
Službu Azure Analysis Services je jednoduché nastavit a spravovat. Všechny potřebné informace týkající se vytvoření a správy serveru najdete zde. Vytváření datového modelu pro nasazení na server je velmi podobné vytváření datového modelu nasazovaného na místní server. [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) obsahuje rozsáhlou knihovnu článků s koncepty, postupy, kurzy a odkazy.

### Videa
<a id="videos" class="xliff"></a>
Užitečná videa najdete v části [Azure Analysis Services na webu Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

### Blogy
<a id="blogs" class="xliff"></a>
Všechno se rychle mění. Nejnovější informace vždy najdete na [blogu týmu Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/) a [blogu o Azure](https://azure.microsoft.com/blog/).

### Komunita
<a id="community" class="xliff"></a>
Služba Analysis Services má velmi aktivní komunitu uživatelů. Připojte se ke konverzaci na [fóru Azure Analysis Services](https://aka.ms/azureanalysisservicesforum).

## Váš názor
<a id="feedback" class="xliff"></a>
Máte návrhy nebo požadavky na funkce? Své komentáře pište na web [Zpětná vazba ke službě Azure Analysis Services](https://aka.ms/azureanalysisservicesfeedback).

Máte návrhy týkající se dokumentace? Komentáře můžete přidávat pomocí Livefyre v dolní části každého článku.

## Další kroky
<a id="next-steps" class="xliff"></a>
Teď, když už víte o Azure Analysis Services víc, je čas začít. Zjistěte, jak [vytvořit server](analysis-services-create-server.md) v Azure a [nasadit na něj tabulkový model](analysis-services-deploy.md).

