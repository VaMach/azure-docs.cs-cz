---
title: "Přehled identity pro Azure zásobníku | Microsoft Docs"
description: "Další informace o systémů identit, které můžete použít s Azure zásobníku."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/22/2018
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 609ea3300806f3cfed4a3285a096f59be491c684
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="overview-of-identity-for-azure-stack"></a>Přehled identity pro Azure zásobníku

Azure zásobníku vyžaduje Azure AD nebo Active Directory federovaný Services (AD FS), kterou zálohuje ve službě Active Directory (AD) jako zprostředkovatele identity. Následující koncepty a autorizace podrobnosti můžete provést volba mezi poskytovatelů identit. Výběr zprostředkovatele je jednorázové rozhodnutí, které můžete provést při prvním nasazení Azure zásobníku.  

Svou volbu mezi Azure AD a služby AD FS může být omezena režim, ve které nasazujete zásobník Azure: 
- Při nasazení v režimu připojení, můžete použít Azure AD ani AD FS. 
- Při nasazení v odpojeném režimu bez připojení k Internetu, je podporována pouze služba AD FS.

Další informace o těchto možnostech zobrazení v závislosti na prostředí zásobníku Azure v následujících článcích:
- V tématu Azure zásobníku Deployment Kit [Identity aspekty](azure-stack-datacenter-integration.md#identity-considerations).
- Azure zásobníku integrované systémy, najdete v části [plánování rozhodnutí pro Azure zásobníku nasazení integrované systémy](azure-stack-deployment-decisions.md).

 
## <a name="common-concepts-for-identity"></a>Běžné koncepty pro identitu
Následující části popisují běžné koncepty pro zprostředkovatele identity a jejich použití v Azure zásobníku.
![Terminologie pro zprostředkovatele identity](media/azure-stack-identity-overview/terminology.png)


### <a name="directories-tenants-and-organizations"></a>Adresáře klientů a o organizaci
Adresář je kontejner, který obsahuje informace o *uživatelé*, *aplikace*, *skupiny*, a *služby objekty*.
 
Adresář klienta je *organizace*, jako jsou Microsoft nebo vaše vlastní společnost. 
- Azure AD podporuje více klientů a může podporovat víc organizacím, každý svůj adresář. Pokud používáte Azure AD a máte víc klientů, můžete udělit pro aplikace a uživatele z jedné klientovi přístup do jiných klientů stejný adresář.
- Služba AD FS podporuje pouze jednoho klienta a proto jedné organizace. 

### <a name="users-and-groups"></a>Uživatelé a skupiny
Uživatelské účty (identity) jsou standardní účty, které ověření jednotlivce pomocí ID uživatele a heslo. Skupiny mohou obsahovat uživatele nebo jiné skupiny. 

Jak vytvořit a spravovat uživatele a skupiny závisí na řešení identit, které používáte. 

V zásobníku Azure, uživatelské účty: 
- Jsou vytvořené v  *username@domain*  formátu. I když služby AD FS mapuje uživatelské účty do služby Active Directory, služby AD FS nepodporuje použití  _&lt;domény >\<alias >_ formátu. 
- Můžete nastavit na používání služby Multi-Factor authentication. 
- Omezeny do adresáře, kde se nejprve zaregistrovat, což je adresáře své organizace.
- Je možné importovat z místních adresářů. Další informace najdete v tématu [integraci místních adresářů se službou Azure Active Directory](/azure/active-directory/connect/active-directory-aadconnect) v dokumentaci k Azure.  

Když na portál klienta vaší organizace, můžete použít https://portal.local.azurestack.external. 

### <a name="guest-users"></a>Uživatelé typu host
Uživatele typu Host jsou uživatelské účty od ostatních klientů adresáře, kterým byl udělen přístup k prostředkům ve vašem adresáři. Pro podporu uživatele guest, musíte použít Azure AD a povolit podporu pro více klientů. Pokud je podporovaná, můžete pozvat uživatele guest přístup k prostředkům ve vašem klientu adresáře, která umožňuje spolupráci s mimo organizacemi. 

Pozvaným uživatelům hosta operátorům cloudu a uživatelé můžou používat [spolupráce Azure AD B2B](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). Uživatelé získal přístup k dokumentům, prostředky a aplikace z adresáře pozvat při zachování kontroly nad vlastní prostředkům a datům. 

Jako uživatel guest můžou přihlásit do jiného adresáře klienta organizace. Uděláte to tak, musí tento název adresáře organizace připojit k portálu adresy URL.  Například pokud patří do domény contoso.com, ale se chcete přihlásit do adresáře společnosti Fabrikam, můžete použít https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.

### <a name="applications"></a>Aplikace
Můžete zaregistrovat aplikací do Azure AD ani AD FS a poskytněte jim uživatelům ve vaší organizaci. 

Použití patří:
- **Webovou aplikaci** – příklady portálu Azure a Azure Resource Manager.  Volání webového rozhraní API podporují. 
- **Nativní klient** – příklady prostředí Azure PowerShell, sady Visual Studio a rozhraní příkazového řádku Azure (CLI).

Aplikace podporují dva typy klientů: 
- **Jednoho klienta** aplikace podporují uživatele a služby pouze ze stejného adresáře, kde je registrovaná aplikace. 

  > [!NOTE]    
  > Vzhledem k tomu, že služba AD FS podporuje pouze jeden adresář, standardně jednoho klienta aplikace jsou aplikace, které vytvoříte v topologii služby AD FS.
- **Víceklientské** používá podporu aplikací uživatelům a službám z oba adresáře, kde je registrovaná aplikace a z dalších klienta adresářů.  S více klienty aplikace uživatelé jiného klienta adresáře (jiného klienta Azure AD) může přihlásit do vaší aplikace.     

  Další informace o víceklientské najdete v tématu [povolit víceklientský](azure-stack-enable-multitenancy.md).  

  Další informace o vývoji víceklientské aplikace najdete v tématu [víceklientské aplikace](/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Při registraci aplikace jsou vytvořeny dva objekty:
- **Objekt aplikace** – objekt aplikace je globální reprezentace aplikace přes všechny klienty. Tento vztah je 1: 1 s aplikací softwaru a pouze v adresáři, kde aplikace je první registry existuje.

 
   
- **Objekt zabezpečení služby** – hlavní název služby je vytvořený pro aplikaci v adresáři, kde je aplikaci nejprve zaregistrovat pověření. Hlavní název služby je vytvořen také v adresáři každý další klient, kde se používá tuto aplikaci. Tento vztah může být jeden mnoho s aplikací softwaru.   

Další informace o aplikaci a hlavní objekty služby najdete v tématu [aplikace a služby hlavní objekty ve službě Azure Active Directory (Azure AD)](/azure/active-directory/develop/active-directory-application-objects). 

### <a name="service-principals"></a>Objekty služby 
Hlavní název služby je sada *pověření* pro aplikace nebo služba, která uděluje přístup k prostředkům v Azure zásobníku. Použití objektu služby odděluje oprávnění aplikací z oprávnění uživatele, který používá aplikace.

Objekt služby se vytvoří v každém klientovi, kde se používá aplikace. Objekt služby vytváří identity pro přihlášení a přístup k prostředkům (například uživatelů), které jsou zabezpečeny tohoto klienta.   

- Single klientské aplikace má pouze jeden objekt služby, v adresáři, kde je prvním vytvoření.  Tento objekt zabezpečení služby se vytvoří a souhlasí má použít při registraci aplikace. 
- Víceklientské webovou aplikaci nebo API má hlavní název služby vytvořené v každého klienta, kde souhlasí uživatele z tohoto tenanta na používání aplikace.  

Přihlašovací údaje pro objekty služby může být buď klíč (vygenerovaný prostřednictvím portálu Azure) nebo certifikátu.  Použití certifikátu je vhodné pro automatizaci, protože certifikáty jsou považovány za bezpečnější než použití klíčů. 


> [!NOTE]    
> Při použití služby AD FS se zásobníkem Azure, můžete vytvořit pouze správce objekty služby. Se službou AD FS objekty služby vyžadují certifikáty a vytvoří se prostřednictvím privilegované koncového bodu (období). Další informace najdete v tématu, [poskytnout přístup aplikace k Azure zásobníku](azure-stack-create-service-principals.md).

Další informace o objektech služby pro Azure zásobníku, najdete v části [vytvořit objekty služby](azure-stack-create-service-principals.md).


### <a name="services"></a>Služby
Služby v zásobníku Azure, které spolupracují s poskytovateli Identity je zaregistrovaný jako aplikace pomocí zprostředkovatele identity. Podobně jako aplikace umožňuje registraci služby k ověřování pro systém identit. 

Použití všech služeb Azure [OpenID Connect](/azure/active-directory/develop/active-directory-protocols-openid-connect-code) protokoly a [webových tokenů JSON](/azure/active-directory/develop/active-directory-token-and-claims) (JWT) a vytvořit tak jeho identitu. Z důvodu konzistentní použití protokolů Azure AD a služby AD FS, můžete použít Azure [Active Directory Authentication Library](/azure/active-directory/develop/active-directory-authentication-libraries) (ADAL) k ověřování místní nebo do Azure (v případě připojené). ADAL také umožňuje používat nástroje, například Azure PowerShell a rozhraní příkazového řádku pro různé cloud a místní správy prostředků. 


### <a name="identities-and-your-identity-system"></a>Identity a systému identit 
Identit pro Azure zásobníku zahrnovat uživatelské účty, skupiny a objekty služby. Při instalaci Azure zásobníku několik předdefinovaných aplikacím a službám automatickou registraci v rámci zprostředkovatele identity v klientovi adresáře. Některé služby, které registrují se používají pro správu. Další služby jsou k dispozici pro uživatele. Registrace výchozí poskytnout základní služby identity, které mohou komunikovat navzájem a s identitami, můžete přidat později.
Pokud jste nastavili Azure AD s víceklientský, některé aplikace rozšíří do nového adresáře.  

## <a name="authentication-and-authorization"></a>Ověřování a autorizace
 

### <a name="authentication-by-applications-and-users"></a>Ověřování aplikace a uživatelé
  
![Identity mezi vrstvy zásobníkové Azure](media/azure-stack-identity-overview/identity-layers.png)

Pro aplikace a uživatelé se popisuje architekturu zásobník Azure čtyři vrstvy. Interakce mezi každou z těchto úrovní můžete použít různé typy ověřování.


|Vrstva    |Ověřování mezi vrstvami  |
|---------|---------|
|Nástroje a klienty, jako jsou portál pro správu     | Přístup k nebo upravit prostředek v zásobníku Azure, nástroje a klienti používají [webových tokenů JSON](/azure/active-directory/develop/active-directory-token-and-claims) (JWT), abyste mohli volat ke správě prostředků Azure.  <br><br> Azure Resource Manager ověří token JWT a prohlédne *deklarace identity* v vystavený token, který má odhad úrovně autorizace tohoto uživatele nebo instanční objekt má v zásobníku Azure.        |
|Azure Resource Manager a jeho základní služby     |Azure Resource Manager komunikuje se zprostředkovatelé prostředků pro přenos komunikace od uživatelů. <br><br> Přenosy použití *přímé imperativní* volání nebo *deklarativní* volá prostřednictvím [šablon Azure Resource Manageru](/azure/azure-stack/user/azure-stack-arm-templates.md).|
|Poskytovatelé prostředků     |Volání předaný zprostředkovatelé prostředků jsou zabezpečené pomocí ověřování pomocí certifikátů. <br><br>Azure Resource Manager a poskytovatele prostředků se pak zůstanou v komunikaci prostřednictvím rozhraní API. Poskytovatel prostředků pro každé volání přijatých ze Správce prostředků Azure, ověří volání tímto certifikátem.|
|Infrastruktury a obchodní logiky     |Zprostředkovatelé prostředků komunikovat s obchodní logiky a infrastrukturu pomocí režim ověřování podle svého výběru. Výchozí zprostředkovatelé prostředků dodávaných s zásobník Azure pomocí ověřování systému Windows pro zabezpečení této komunikace.|

![Informace potřebné pro ověřování](media/azure-stack-identity-overview/authentication.png)


### <a name="authenticate-to-azure-resource-manager"></a>Ověření do Azure Resource Manageru
Ověření pomocí zprostředkovatele identity a přijímat token JWT, musíte mít následující informace: 
1.  **Adresa URL pro systém identit (autorita)** – adresa URL, na které bude možné spojit zprostředkovatele identity. Například  *&lt;https://login.windows.net>*. 
2.  **Identifikátor ID URI aplikace pro Azure Resource Manager** – jedinečný identifikátor pro Azure Resource Manager, která je zaregistrovaná u poskytovatele identit a jedinečné pro každou instalaci zásobník Azure.
3.  **Přihlašovací údaje** – tato pověření, který používáte k ověření pomocí zprostředkovatele identity.  
4.  **Adresa URL pro Azure Resource Manager** – adresa URL je umístění služby Správce prostředků Azure. Například  *&lt;https://management.azure.com>* nebo  *&lt;https://management.local.azurestack.external>*.

Pokud objekt zabezpečení (klienta, aplikace nebo uživatele) provede požadavek ověřování pro přístup k prostředkům, musí obsahovat této žádosti:
- Přihlašovací údaje objektu zabezpečení.
- Identifikátor ID URI aplikace prostředku chtějí získat přístup.  

Přihlašovací údaje se ověří pomocí zprostředkovatele identity. Poskytovatel identity zároveň ověří, zda je identifikátor ID URI aplikace pro zaregistrovanou aplikaci a že objekt má dostatečná oprávnění pro získání tokenu pro tento prostředek. Pokud je požadavek platný, je oprávnění webového tokenu JSON. 

Token musí pak předejte v hlavičce požadavku na Azure Resource Manager. Azure Resource Manager nemá žádné konkrétní pořadí k následující ověření:
- Ověření *vystavitele* (iss) deklarace identity, abyste se ujistili, že token od zprostředkovatele identity správné. 
- Ověření *cílovou skupinu* deklarace identity (oblast), potvrďte, že byl token vydán pro Azure Resource Manager. 
- Ověřte, že je token JWT podepsaný certifikátem, který je nakonfigurovaný pomocí OpenID a který se označuje do Azure Resource Manageru. 
- Zkontrolujte *vydané v* (iat) a *vypršení platnosti* (exp) deklarace identity k potvrzení, že token je aktivní a nelze přijmout. 

Po dokončení všech ověření Azure Resource Manager používá *proti* (oid) a *skupiny* deklarace identity, aby seznam zdrojů, kteří mohou přistupovat k objektu zabezpečení. 

![Diagram protokolu tokenu exchange](media/azure-stack-identity-overview/token-exchange.png)


### <a name="use-role-based-access-control"></a>Pomocí řízení přístupu na základě rolí  
Na základě rolí řízení přístupu (RBAC) v zásobníku Azure je konzistentní s implementací v Microsoft Azure.  Můžete spravovat přístup k prostředkům přiřazením příslušné role RBAC uživatelům, skupinám a aplikacím. Informace o tom, jak používat funkci RBAC zásobníkem Azure naleznete v následujících článcích:
- [Začínáme s řízením přístupu na základě rolí na portálu Azure](/azure/active-directory/role-based-access-control-what-is).
- [Pomocí řízení přístupu na základě Role můžete spravovat přístup k prostředkům na předplatné](/azure/active-directory/role-based-access-control-configure).
- [Vytvořit vlastní role pro řízení přístupu](/azure/active-directory/role-based-access-control-custom-roles).
- [Správa řízení přístupu na základě rolí](azure-stack-manage-permissions.md) v Azure zásobníku.


### <a name="authenticate-with-azure-powershell"></a>Ověření pomocí Azure PowerShell  
Podrobnosti o použití prostředí Azure PowerShell k ověření pomocí Azure zásobníku naleznete na adrese [nakonfigurovat prostředí PowerShell Azure zásobník uživatele](azure-stack-powershell-configure-user.md).

### <a name="authenticate-with-azure-cli"></a>Ověření pomocí rozhraní příkazového řádku Azure
Podrobnosti o použití prostředí Azure PowerShell k ověření pomocí Azure zásobníku naleznete na adrese [instalace a konfigurace rozhraní příkazového řádku pro použití s Azure zásobníku](/azure/azure-stack/user/azure-stack-connect-cli.md).

## <a name="next-steps"></a>Další postup
- [Architektura identity](azure-stack-identity-architecture.md)   
- [Integrace Datacenter - Identity](azure-stack-integrate-identity.md)




