---
title: "Zabezpečení dat a operace ve službě Azure Search | Microsoft Docs"
description: "Zabezpečení služby Azure Search podle SOC 2 dodržování předpisů, šifrování, ověřování a identita přístupu pomocí uživatele a skupiny identifikátory zabezpečení v Azure Search filtry."
services: search
documentationcenter: 
author: HeidiSteen
manager: cgronlun
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/19/2018
ms.author: heidist
ms.openlocfilehash: c3aa4883e33b1f3494f8502fe7f8b12f7d64a72f
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/23/2018
---
# <a name="security-and-controlled-access-in-azure-search"></a>Zabezpečení a řízený přístup do služby Azure Search

Služba Azure Search je [SOC 2 kompatibilní](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports), s komplexní zabezpečení architektura STA fyzické zabezpečení, šifrované přenosy, šifrované úložiště a ochrana softwaru celou platformu. Azure Search operačně přijímá pouze ověřené žádosti. Volitelně můžete přidat uživatelská řízení přístupu na obsah. Tento článek dotykem na zabezpečení v každé vrstvě, ale se zaměřuje především na tom, jak jsou zabezpečené data a operace ve službě Azure Search.

![Blokový diagram serveru vrstvy zabezpečení](media/search-security-overview/azsearch-security-diagram.png)

## <a name="physical-security"></a>Fyzické zabezpečení

Datovými centry Microsoftu špičkový fyzické zabezpečení a jsou kompatibilní s rozsáhlé portfolií standardů a nařízení. Pokud chcete dozvědět víc, přejděte na [globálních datových centrech](https://www.microsoft.com/cloud-platform/global-datacenters) stránky nebo shlédnout krátké video na data center zabezpečení.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Šifrované přenosu a uložení

Šifrování rozšiřuje v rámci celého kanálu indexování: od připojení, prostřednictvím přenosu a dolů indexovaná data uložená ve službě Azure Search.

| Vrstva zabezpečení | Popis |
|----------------|-------------|
| Šifrování během přenosu | Služba Azure Search naslouchá na portu HTTPS 443. Na platformě jsou šifrované připojení ke službám Azure. |
| Šifrování v klidovém stavu | Šifrování je plně internalized do procesu vytváření indexu se žádné měřitelný dopad na indexování čas dokončení nebo velikost indexu. Automaticky se objeví na všechny indexování, včetně přírůstkové aktualizace index, který není plně zašifrované (vytvořených před leden 2018).<br><br>Interně, je šifrování na základě [šifrování služby úložiště Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), pomocí 256 bitů [šifrování AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|
| [Dodržování předpisů SOC 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) | Všechny služby vyhledávání jsou plně AICPA SOC 2 předpisy, na všech datových center, které poskytuje Azure Search. Chcete-li zkontrolovat úplná sestava, přejděte na [Azure – a Azure Government SOC 2 typ II sestava](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). |

Šifrování je interní do služby Azure Search s certifikáty a šifrovacích klíčů interně spravuje Microsoft a použít univerzálně. Nelze zapnout šifrování nebo vypnout, spravovat nebo nahraďte váš vlastní klíče nebo zobrazit nastavení šifrování na portálu nebo prostřednictvím kódu programu. 

Šifrování v klidovém stavu byl oznámen v 24 leden 2018 a platí pro všechny úrovně služeb, včetně sdílených služeb (zdarma), ve všech oblastech. Úplné šifrování musí být indexy vytvořené před tímto datem vyřadit a znovu sestavit v pořadí pro šifrování proběhnout. Jinak se šifrují jenom nová data přidány po leden 24.

## <a name="azure-wide-logical-security"></a>Zabezpečení logické Azure

Několik mechanismů zabezpečení jsou dostupné přes zásobník Azure a proto automaticky dostupné pro prostředky Azure Search, které vytvoříte.

+ [Zámky na úrovni prostředků, aby se zabránilo odstranění nebo předplatné](../azure-resource-manager/resource-group-lock-resources.md)
+ [Na základě rolí řízení přístupu (RBAC) pro řízení přístupu k informacím a operace správy](../active-directory/role-based-access-control-what-is.md)

Nastavení úrovně přístupu konzistentně ve všech službách podpora všech služeb Azure řízení přístupu na základě role (RBAC). Například zobrazení citlivých dat, jako například klíč správce, je omezen na vlastníka a Přispěvatel role, zatímco zobrazení stavu služby je k dispozici pro členy žádné role. RBAC poskytuje vlastník, Přispěvatel a čtečky rolí. Ve výchozím nastavení všechny Správci služeb jsou členy role vlastníka.

## <a name="service-access-and-authentication"></a>Přístup k službě a ověřování

Zatímco Azure Search dědí zabezpečení bezpečnostní opatření na platformě Azure, je také vlastní ověřování na základě klíčů. Typ klíče (správce nebo dotazu) určuje úroveň přístupu. Odeslání platný klíč je považovaný za ověření požadavek pochází z důvěryhodné entity. 

Na každý požadavek, kde každý požadavek se skládá z povinných klíč, operace a objekt se vyžaduje ověření. Při zřetězen dohromady, je dostatečné pro zajištění zabezpečení úplné spektrum operací služby je dvě úrovně oprávnění (úplná nebo jen pro čtení) a kontext. 

|Klíč|Popis|Omezení|  
|---------|-----------------|------------|  
|Správa|Uděluje úplná práva ke všem operacím, včetně možnosti spravovat službu, vytvářet a odstraňovat indexy, indexery a zdroje dat.<br /><br /> Dva správce **klíče api Key**, označují jako *primární* a *sekundární* klíče na portálu, jsou generovány, pokud služba je vytvořena a můžete znovu vygeneruje jednotlivě na vyžádání . Máte dva klíče můžete vrátit přes jeden klíč při použití druhý klíč pro nepřetržitý přístup ke službě.<br /><br /> Správce klíčů jsou určené jenom v hlavičkách žádosti protokolu HTTP. Rozhraní api-key správce nelze umístit v adrese URL.|Maximálně 2 na služby|  
|Dotaz|Uděluje přístup jen pro čtení k indexům a dokumentům a obvykle se distribuují klientským aplikacím, které vydávají požadavky hledání.<br /><br /> Klíče dotazu jsou vytvořeny na vyžádání. Můžete je vytvořit ručně na portálu nebo prostřednictvím kódu programu prostřednictvím [REST API pro správu](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Klíče dotazu lze zadat v hlavičce požadavku HTTP pro vyhledávání, návrh nebo operace vyhledávání. Alternativně můžete předat klíč dotazu jako parametr na adresu URL. V závislosti na tom, jak klientské aplikace výrobky zpracovává požadavek může být snazší předat klíč jako parametr dotazu:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|50 pro službu|  

 Vizuální není žádný rozdíl mezi klíč správce a klíč dotazu. Oba klíče se, řetězce tvořený 32 náhodně generované alfanumerické znaky. Pokud ztratíte zaznamenávat, jaký typ klíče je zadán v aplikaci, můžete [zkontrolujte hodnoty klíče na portálu](https://portal.azure.com) nebo použít [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) vrátí hodnotu a typ klíče.  

> [!NOTE]  
>  Postup nízký zabezpečení k předávání citlivých dat, jako považuje `api-key` v identifikátoru URI požadavku. Z tohoto důvodu Azure Search přijímá pouze klíč dotazu jako `api-key` v dotazu řetězec a vy byste neměli tak, pokud obsah indexu by měly být veřejně dostupné. Obecně platí, doporučujeme předávání vaší `api-key` jako hlavičku požadavku.  

### <a name="how-to-find-the-access-keys-for-your-service"></a>Postup nalezení přístupové klíče služby

Můžete získat přístupové klíče na portálu nebo prostřednictvím [REST API pro správu](https://docs.microsoft.com/rest/api/searchmanagement/). Další informace najdete v tématu [Správa klíčů](search-manage.md#manage-api-keys).

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Seznam [vyhledávací služby](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) pro vaše předplatné.
3. Vyberte službu a na stránce služby najít **nastavení** >**klíče** zobrazíte klíče správce a dotazu.

![Stránky portálu, nastavení klíče oddílu](media/search-security-overview/settings-keys.png)

## <a name="index-access"></a>Index přístup

Ve službě Azure Search jednotlivé indexu není zabezpečitelných objektů. Místo toho přístup k indexu je určen ve vrstvě služby (přístup pro čtení nebo zápisu), spolu s kontext operace.

V případě přístupem koncových uživatelů můžete struktury požadavků na dotazy v aplikaci a připojte se pomocí klíč dotazu, který vytvoří každá žádost jen pro čtení a zahrnovat konkrétního indexu používané vaší aplikace. V žádosti o dotaz neexistuje žádná koncepce připojení indexy nebo přístupu k nim více indexů současně, všechny požadavky cíle jeden index podle definice. Struktura dotaz na žádost (klíč a jeden cílový index) jako takový definuje hranice zabezpečení.

Správce a vývojáře přístup k indexů je poskytujících blíže neurčené: obě potřebovat přístup pro zápis k vytvoření, odstranění a aktualizovat objekty spravované službou. Každý, kdo má klíč správce k službě můžou číst, upravit nebo odstranit žádný index v rámci stejné služby. Pro ochranu proti náhodnému nebo škodlivý odstranění indexů je vaše interní zdrojového kódu pro kód prostředky remedy zpětné nežádoucí index, odstranění nebo úpravy. Vyhledávání systému Azure má převzetí služeb při selhání v rámci clusteru, aby se zajistila dostupnost, ale nemá uložení nebo provést vlastní kódu umožňuje vytvořit nebo načíst indexy.

Taková řešení pro vyžadování hranic zabezpečení na úrovni index víceklientské architektury řešení, měl obvykle zahrnovat střední vrstvy, uvedeni zákazníci, kteří používala pro zpracování izolace index. Další informace o případ víceklientské použití najdete v tématu [vzory pro víceklientské aplikace SaaS a Azure Search návrhu](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>Přístup správce z klientské aplikace

REST API služby Azure Search správy je rozšíření Azure Resource Manager a sdílí jeho závislé součásti. Služba Active Directory jako takový je předpokladem pro správu služby Azure Search. Všechny požadavky na správu z kódu klienta musí být ověřeny pomocí Azure Active Directory předtím, než požadavek dosáhne Resource Manager.

Požadavky na data na koncový bod Azure Search služby, jako je vytvoření indexu (služby REST API Azure Search) nebo vyhledávání dokumentů (služby REST API Azure Search), použijte klíč rozhraní api v hlavičce žádosti.

Pokud kód aplikace zpracovává operace správy služeb a také data operací na indexy hledání nebo dokumenty, implementace dva přístupy k ověřování v kódu: nativní Azure Search a ověřování služby Active Directory přístupového klíče metodologie nutné pomocí Správce prostředků. 

Informace o vytváření struktury žádost ve službě Azure Search najdete v tématu [REST služby Azure Search](https://docs.microsoft.com/rest/api/searchservice/). Další informace o požadavcích na ověřování pro službu správce prostředků najdete v tématu [ověřování pomocí Správce prostředků rozhraní API k přístupu k odběrům](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Uživatelský přístup k obsahu indexu

Uživatelská přístup k obsahu indexu se implementuje pomocí filtrů zabezpečení na své dotazy, vrácení dokumenty spojené s danou zabezpečení identity. Místo předdefinované role a přiřazení rolí je řízení přístupu na základě identity implementovaný jako filtr, že výsledky dokumentů a obsah podle identity hledání ořízne. Následující tabulka popisuje dva přístupy pro výsledky hledání oříznutí neoprávněným obsahu.

| Přístup | Popis |
|----------|-------------|
|[Oříznutí zabezpečení podle identity filtry](search-security-trimming-for-azure-search.md)  | Dokumenty základní pracovní postup pro implementaci řízení přístupu identity uživatele. Popisuje přidání identifikátory zabezpečení do indexu a pak vysvětluje filtrování toto pole oříznout výsledky zakázaný obsah. |
|[Oříznutí zabezpečení podle identit Azure Active Directory](search-security-trimming-for-azure-search-with-aad.md)  | Tento článek rozšíří na předchozím článku, konkrétně postup pro načítání identit z Azure Active Directory (AAD), mezi [bezplatné služby](https://azure.microsoft.com/free/) v platformě Azure cloud. |

## <a name="table-permissioned-operations"></a>Tabulkou: Permissioned operace

Následující tabulka shrnuje operace povolené ve službě Azure Search a který klíč odemkne přístupu konkrétním operaci.

| Operace | Oprávnění |
|-----------|-------------------------|
| Vytvoření služby | Držitel předplatného Azure|
| Škálování služby | Klíč správce, RBAC vlastníkem nebo přispěvatelem na prostředek  |
| Odstranění služby | Klíč správce, RBAC vlastníkem nebo přispěvatelem na prostředek |
| Vytvořit, upravit, odstranit objekty služby: <br>Indexy a součásti (včetně definice analyzátor, vyhodnocování profily, možnosti CORS), indexery, zdroje dat, synonyma, trochu. | Klíč správce, RBAC vlastníkem nebo přispěvatelem na prostředek  |
| Dotazování indexu | Správce nebo dotaz, klíč (RBAC není k dispozici) |
| Dotaz na informace o systému, jako je například vrácení statistiky, počty a seznam objektů. | Klíč správce, RBAC na prostředku (vlastník, Přispěvatel, čtečky) |
| Správa Správce klíčů | Klíč správce, RBAC vlastníkem nebo přispěvatelem u daného prostředku. |
| Spravovat klíče dotazů |  Klíč správce, RBAC vlastníkem nebo přispěvatelem u daného prostředku. Čtečka RBAC můžete zobrazit klíče dotazu. |


## <a name="see-also"></a>Další informace najdete v tématech

+ [Získat spuštění rozhraní .NET (ukazuje, jak vytvořit index pomocí klíč správce)](search-create-index-dotnet.md)
+ [Získat spuštění REST (ukazuje, jak vytvořit index pomocí klíč správce)](search-create-index-rest-api.md)
+ [Řízení přístupu na základě identit pomocí filtrů Azure Search](search-security-trimming-for-azure-search.md)
+ [Řízení přístupu na základě identity Active Directory, které jsou pomocí filtrů Azure Search](search-security-trimming-for-azure-search-with-aad.md)
+ [Filtry ve službě Azure Search](search-filters.md)