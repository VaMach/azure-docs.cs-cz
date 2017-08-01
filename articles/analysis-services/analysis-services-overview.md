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
ms.date: 07/03/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 625baa2df8b137779ed846c584a138cc15e89a3f
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---
# <a name="what-is-azure-analysis-services"></a>Co je služba Azure Analysis Services?
![Azure Analysis Services](./media/analysis-services-overview/aas-overview-aas-icon.png)

Azure Analysis Services poskytuje modelování dat na podnikové úrovni v cloudu. Je to plně spravovaná platforma jako služba (PaaS) úzce integrovaná se službami datové platformy Azure. Díky svému vysoce optimalizovanému jádru pro analýzu dat OLAP poskytuje služba Analysis Services vrstvu bohatého sémantického modelu mezi rozsáhlými, komplexními a často různorodými zdroji dat. Díky spojování dat můžete definovat sémantický datový model a vytvořit vysoce přizpůsobené a výkonné analýzy, které se stanou motorem bohatých, interaktivních analýz v moderních klientských nástrojích.

![Zdroje dat](./media/analysis-services-overview/aas-overview-data-sources.png)


Podívejte se na [toto video](https://sec.ch9.ms/ch9/d6dd/a1cda46b-ef03-4cea-8f11-68da23c5d6dd/AzureASoverview_high.mp4) a zjistěte, jak služba Azure Analysis Services zapadá do celkových možností BI společnosti Microsoft a jaké výhody vám může přinést převedení datových modelů do cloudu.

<!--
>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/Azure-Analysis-Services-overview/player]
>
>
-->

## <a name="built-on-sql-server-analysis-services"></a>Vytvořeno na základě SQL Server Analysis Services
Služba Azure Analysis Services je kompatibilní se stejnou edicí SQL Server Analysis Services Enterprise Edition, kterou už znáte. Azure Analysis Services podporuje tabulkové modely na úrovni kompatibility 1200 a 1400. Podporují se oddíly, zabezpečení na úrovni řádku, obousměrné relace i překlady. Režimy In-Memory a DirectQuery znamenají bleskově rychlé dotazy nad obrovskými a komplexními datovými sadami.

Tabulkové modely nabízejí rychlý vývoj a jsou vysoce přizpůsobitelné. Pro vývojáře tabulkové modely zahrnují tabulkový objektový model (TOM) pro popis objektů modelu. Tabulkový objektový model (TOM) je přístupný ve formátu JSON prostřednictvím [jazyka TMSL (Tabular Model Scripting Language)](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) a jazyka DDL AMO v oboru názvů [Microsoft.AnalysisServices.Tabular](https://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx).

Nové funkce v tabulkových modelech 1400 podporují řádky podrobností, zabezpečení na úrovni objektů, nepravidelné hierarchie, moderní prostředí v rozšíření SSDT pro získávání dat pro datové připojení a mnoho dalších vylepšení. A protože základní metadata modelu jsou stejná, stávající řešení tabulkových modelů v místním prostředí můžete snadno migrovat do cloudu.


## <a name="better-with-azure"></a>Lepší s Azure
Azure Analysis Services se integruje s mnoha datovými službami Azure a tím umožňuje sestavovat sofistikovaná analytická řešení.

Azure Analysis Services může využívat data ze služeb Azure SQL Database, Azure SQL Data Warehouse a Azure Blob Storage. V Azure můžete vytvářet podniková řešení datových skladů pomocí hvězdicovitého modelu, v jehož středu je služba SQL Data Warehouse a několik modelů BI okolo cílí na různé obchodní skupiny nebo předmětné oblasti.

Pomocí služby Azure Data Factory můžete orchestrovat přesun a transformaci dat, což je základní schopnost každého podnikového řešení BI nebo analýzy. Azure Analysis Services je možné integrovat do jakéhokoli kanálu Azure Data Factory vložením aktivity, která načte data do modelu. Můžete také použít služby Azure Automation a Azure Functions k provádění prosté orchestrace modelů pomocí vlastního kódu.

Služba Azure Analysis Services je také úzce integrovaná s Azure Active Directory a poskytuje zabezpečený přístup na základě role k vašim datům.

## <a name="pricing"></a>Ceny
Služba Azure Analysis Services je dostupná v úrovních Developer, Basic a Standard. Ceny plánů na jednotlivých úrovních se liší podle výpočetního výkonu, jednotek QPU a velikosti paměti. Při vytváření serveru si vyberete plán na nějaké úrovni. Plány můžete měnit na vyšší nebo nižší v rámci stejné úrovně nebo upgradovat na vyšší úroveň, není však možné downgradovat z vyšší úrovně na nižší.

![Upgrade úrovně](./media/analysis-services-overview/aas-overview-tier-up.png)

Úrovně můžete měnit za běhu na webu Azure Portal nebo pomocí rutiny PowerShellu Set-AzureRmAnalysisServicesServer. Další informace o různých plánech a úrovních a cenovou kalkulačku, která vám pomůže s určením správného plánu, najdete v tématu [Ceny služby Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="scale-resources"></a>Škálování prostředků
Vertikálně navyšujte nebo snižujte kapacitu vašeho serveru nebo ho pozastavte. Použijte web Azure Portal nebo mějte úplnou kontrolu za běhu pomocí PowerShellu. Platíte jenom za to, co používáte.

Při vytváření nových serverů použijte rutinu [New-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver) k nastavení vašeho plánu. U existujících serverů můžete plán změnit pomocí rutiny [Set-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver). Nepoužíváte službu pořád? Můžete ji pozastavit na portálu nebo pomocí rutiny [Suspend-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver). Znovu ji spustíte rutinou [Resume-AzureRmAnalysisServicesServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver). Platíte jenom za čas, kdy je server aktivní.


## <a name="regions"></a>Oblasti
Servery služby Azure Analysis Services je možné vytvořit v následujících [oblastech Azure](https://azure.microsoft.com/regions/):

| Amerika | Evropa | Asie a Tichomoří |
|----------|--------|--------------|
|  Brazílie – jih<br> Střední Kanada<br> Východní USA 2<br> Střed USA – sever<br> Střed USA – jih<br> Západní střed USA<br> Západní USA | Severní Evropa<br> Spojené království – jih<br> Západní Evropa |   Austrálie – jihozápad<br> Japonsko – východ<br> Jihovýchodní Asie<br> Indie – západ  |

Neustále se přidávají nové oblasti, takže tento seznam nemusí být kompletní. Umístění volíte při vytváření serveru na webu Azure Portal nebo pomocí šablon Azure Resource Manageru. Pokud chcete získat nejlepší výkon, zvolte umístění co nejblíže vaší největší uživatelské základně. Zajistěte [vysokou dostupnost](analysis-services-bcdr.md) nasazením modelů na redundantních serverech ve více oblastech.

## <a name="get-up-and-running-quickly"></a>Rychlé zprovoznění
Pomocí webu Azure Portal můžete [vytvořit server](analysis-services-create-server.md) během několika minut. A pomocí PowerShellu a šablon Azure Resource Manageru můžete zřizovat servery s využitím deklarativní šablony. S jedinou šablonou můžete nasadit několik služeb společně s dalšími komponentami Azure, jako jsou například účty úložiště.  Další informace najdete v tématu [Nasazení prostředků pomocí šablon Resource Manageru a Azure PowerShellu](../azure-resource-manager/resource-group-template-deploy.md).

Jakmile máte vytvořený server, můžete vytvořit tabulkový model přímo na webu Azure Portal. Pomocí nové (ve verzi Preview) funkce Webový návrhář se můžete připojit ke službě Azure SQL Database, zdroji dat Azure SQL Data Warehouse nebo importovat soubor .pbix aplikace Power BI Desktop. Relace mezi tabulkami se vytvoří automaticky a vy můžete vytvářet míry nebo upravovat soubor model.bim ve formátu JSON přímo z prohlížeče.

## <a name="migrate-existing-tabular-models"></a>Migrace existujících tabulkových modelů
Pokud už máte existující řešení modelu SQL Server Analysis Services v místním prostředí, můžete je migrovat do služby Azure Analysis Services bez významných změn. Pokud chcete provést migraci, pomocí rozšíření SSDT můžete nasadit model na server. Nebo můžete v aplikaci SSMS použít zálohování a obnovení nebo jazyk TMSL.

Pokud máte místní zdroje dat, je potřeba nainstalovat a nakonfigurovat [místní bránu dat](analysis-services-gateway.md). Pokud už máte nakonfigurované role a členy rolí, vaše role se budou migrovat, ale členy rolí budete muset znovu přidat pomocí aplikace SSMS nebo PowerShellu.


## <a name="data-sources"></a>Zdroje dat
Azure Analysis Services podporuje připojení ke zdrojům dat v místním prostředí vaší organizace i v cloudu. Kombinací dat z místních a cloudových zdrojů dat získáte hybridní řešení. 

Nové tabulkové modely 1400 využívají moderní funkci Získání dat v rozšíření SSDT založenou na dotazovacím jazyku se vzorci M. S funkcí Získání dat máte více funkcí pro transformaci a mashup dat a možnost vytvářet a upravovat vlastní pokročilé dotazy v jazyce vzorců M. Například u tabulkových modelů 1400 můžete modelovat na základě datových souborů ve službě Azure Blob Storage.

Azure Analysis Services podporuje použití [DirectQuery](https://docs.microsoft.com/sql/analysis-services/tabular-models/directquery-mode-ssas-tabular) pro přímé připojení k relačním databázím Azure SQL Database, Azure SQL Data Warehouse, SQL Server, SQL Server Data Warehouse, Oracle a Teradata.

Další informace najdete v tématu [Podporované zdroje dat ve službě Azure Analysis Services](analysis-services-datasource.md).

## <a name="use-the-tools-you-already-know"></a>Používejte známé nástroje.

![Vývojářské nástroje BI](./media/analysis-services-overview/aas-overview-dev-tools.png)

#### <a name="sql-server-data-tools-ssdt-for-visual-studio"></a>SQL Server Data Tools (SSDT) pro Visual Studio
Vyvíjejte a nasazujte modely pomocí bezplatného rozšíření [SQL Server Data Tools (SSDT) pro Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx). Rozšíření SSDT zahrnuje šablony projektů Analysis Services, které vám pomůžou s rychlým zprovozněním. Rozšíření SSDT teď pro tabulkové modely 1400 zahrnuje moderní funkci Získání dat pro dotazování a mashup zdrojů dat. Pokud už znáte funkci Získání dat v aplikacích Power BI Desktop a Excel 2016, tak už víte, jak snadné je vytvářet vysoce přizpůsobené dotazy na zdroj dat.

S novým rozšířením SSDT a tabulkovými modely 1400 už není nutné instalovat místní instanci služby Analysis Services pro hostování databáze pracovního prostoru. Rozšíření SSDT teď zahrnuje vlastní integrované jádro služby Analysis Services a databázi. Až budete připraveni, můžete provést nasazení na servery v Azure přímo z rozšíření SSDT. Rozšíření SSDT se navíc aktualizuje každý měsíc, takže můžete rychle začít používat nejnovější funkce.

#### <a name="sql-server-management-studio"></a>SQL Server Management Studio
Spravujte servery a modelové databáze pomocí aplikace [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Připojte se k vašim serverům v cloudu. Spouštějte skripty jazyka TMSL přímo z okna dotazu XMLA a automatizujte pomocí nich úlohy. Nové funkce se přidávají rychle – aplikace SSMS se aktualizuje každý měsíc.

#### <a name="powershell"></a>PowerShell
Úlohy správy prostředků serveru, jako je například vytváření serverů, pozastavení nebo obnovení operací serveru nebo změna úrovně služby, používají rutiny Azure Resource Manageru (AzureRM). Ostatní úlohy správy databází, jako je například přidávání nebo odebírání členů rolí, zpracování nebo spouštění skriptů jazyka TMSL, používají rutiny v modulu SqlServer. Moduly AzureRM a SQLServer jsou k dispozici v [Galerii prostředí PowerShell](https://www.powershellgallery.com/).


## <a name="secure"></a>Zabezpečení
![Vizualizace dat](./media/analysis-services-overview/aas-overview-secure.png)

#### <a name="authentication"></a>Ověřování
Ověřování uživatelů pro Azure Analysis Services zařizuje služba [Azure Active Directory (AAD)](../active-directory/active-directory-whatis.md). Při pokusu o přihlášení k databázi Azure Analysis Services používají uživatelé identitu účtu organizace s přístupem k databázi, ke které se snaží získat přístup. Tyto identity uživatelů musí být členy výchozí služby Azure Active Directory pro předplatné, ve kterém se server služby Azure Analysis Services nachází. Další informace najdete v tématu [Ověřování a uživatelská oprávnění](analysis-services-manage-users.md).

#### <a name="data-security"></a>Zabezpečení dat
Služba Azure Analysis Services využívá úložiště Azure Blob Storage k zachování úložiště a metadat pro databáze služby Analysis Services. Datové soubory jsou v rámci objektu Blob šifrované pomocí šifrování na straně serveru Azure Blob. Při použití režimu přímých dotazů se ukládají jenom metada. Ke skutečným datům se přistupuje ze zdroje dat v době dotazu.

#### <a name="on-premises-data-sources"></a>Místní zdroje dat
Zabezpečeného přístupu k datům nacházejícím se místně ve vaší organizaci se dosahuje instalací a konfigurací [místní brány dat](analysis-services-gateway.md). Brány poskytují přístup k datům pro režimy přímých dotazů a v paměti. Když se model služby Azure Analysis Services připojuje k místnímu zdroji dat, vytvoří se dotaz společně se zašifrovanými přihlašovacími údaji pro místní zdroj dat. Cloudová služba brány analyzuje dotaz a nabídne žádost sběrnici Azure Service Bus. Místní brána zadá dotazy sběrnici Azure Service Bus na čekající žádosti. Brána pak získá dotaz, dešifruje přihlašovací údaje a připojí se ke zdroji dat pro provedení. Výsledky se pak odešlou ze zdroje dat zpět do brány a potom dále do databáze Azure Analysis Services.

Služba Azure Analysis Services se řídí [podmínkami služeb Microsoft Online Services](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) a [prohlášením o zásadách ochrany osobních údajů služeb Microsoft Online](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).
Pokud se o službě Azure Security chcete dozvědět víc, podívejte se na [Microsoft Trust Center](https://www.microsoft.com/trustcenter/Security/AzureSecurity).

## <a name="client-connections"></a>Připojení klientů
![Vizualizace dat](./media/analysis-services-overview/aas-overview-clients.png)

Moderní nástroje pro zkoumání a vizualizaci dat, jako jsou Power BI, Excel a další nástroje třetích stran, poskytují koncovým uživatelům vysoce interaktivní a vizuálně bohaté přehledy o datech modelu.

Klienti pro připojení k serverům služby Analysis Services používají [klientské knihovny](analysis-services-data-providers.md) MSOLAP, AMO nebo ADOMD. Klientské aplikace Microsoftu jako Power BI Desktop a Excel instalují všechny tři klientské knihovny. Mějte však na paměti, že v závislosti na verzi nebo četnosti aktualizací nemusí klientské knihovny být nejnovější verze, kterou vyžaduje služba Azure Analysis Services. To samé platí pro vlastní aplikace a další rozhraní, jako jsou AsCmd, Tom nebo ADOMD.NET. Tyto aplikace obvykle vyžadují ruční instalaci těchto knihoven jako součást balíčku.


## <a name="get-help"></a>Podpora

#### <a name="documentation"></a>Dokumentace
Službu Azure Analysis Services je jednoduché nastavit a spravovat. Všechny potřebné informace týkající se vytvoření a správy služeb serveru najdete zde. Vytváření datového modelu pro nasazení na server je velmi podobné vytváření datového modelu nasazovaného na místní server. [Analysis Services](https://docs.microsoft.com/sql/analysis-services/analysis-services) obsahuje rozsáhlou knihovnu článků s koncepty, postupy, kurzy a odkazy.

#### <a name="videos"></a>Videa
Užitečná videa najdete v části [Azure Analysis Services na webu Channel 9](https://channel9.msdn.com/series/Azure-Analysis-Services).

#### <a name="blogs"></a>Blogy
Všechno se rychle mění. Nejnovější informace vždy najdete na [blogu týmu Analysis Services](https://blogs.msdn.microsoft.com/analysisservices/) a [blogu o Azure](https://azure.microsoft.com/blog/).

#### <a name="community"></a>Komunita
Služba Analysis Services má velmi aktivní komunitu uživatelů. Připojte se ke konverzaci na [fóru Azure Analysis Services](https://aka.ms/azureanalysisservicesforum).

## <a name="feedback"></a>Váš názor
Máte návrhy nebo požadavky na funkce? Své komentáře pište na web [Zpětná vazba ke službě Azure Analysis Services](https://aka.ms/azureanalysisservicesfeedback).

Máte návrhy týkající se dokumentace? Komentáře můžete přidávat pomocí Livefyre v dolní části každého článku.

## <a name="next-steps"></a>Další kroky
Teď, když už víte o Azure Analysis Services víc, je čas začít. Zjistěte, jak [vytvořit server](analysis-services-create-server.md) v Azure. Když už je váš server připravený, projděte si [kurz Adventure Works](tutorials/aas-adventure-works-tutorial.md), ve kterém se dozvíte, jak vytvořit plně funkční tabulkový model a nasadit ho na váš server.

