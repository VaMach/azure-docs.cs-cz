---
title: "Správa adresáře služby Azure AD | Dokumentace Microsoftu"
description: "Vysvětluje, co je to klient služby Azure AD a jak spravovat službu Azure pomocí služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/25/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro;oldportal
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: d68b641ac934c3930366be3dbf89a86e9611c7b7
ms.contentlocale: cs-cz
ms.lasthandoff: 07/26/2017

---
# <a name="administer-your-azure-ad-directory"></a>Správa adresáře služby Azure AD
## <a name="what-is-an-azure-ad-tenant"></a>Co je klient služby Azure AD?
Na fyzickém pracovišti by se dal pojem klient definovat jako skupina nebo firma, která sídlí v budově. Vaše organizace může například vlastnit kancelářské prostory v budově. Tato budova může stát v ulici, kde sídlí několik dalších organizací. Vaše organizace může být považovaná za klienta této budovy. Budova je majetkem vaší organizace, dodává jí bezpečí a zajišťuje, abyste mohli bezpečně řídit své podnikání. Navíc je oddělená od ostatních firem v ulici. To zajistí, aby byla vaše organizace a její prostředky izolované od jiných organizací.

V prostředí cloudu se dá klient služby Azure AD definovat jako klient nebo organizace, která vlastní a spravuje konkrétní instanci dané cloudové služby. Microsoft Azure poskytuje identitu platformy, což znamená, že klient je jednoduše vyhrazenou instancí služby Azure Active Directory (Azure AD), kterou vaše organizace obdrží a vlastní od chvíle, kdy se přihlásí ke cloudové službě Microsoftu, například ke službě Azure nebo Office 365.

Každý adresář služby Azure AD je oddělený od ostatních adresářů služby Azure AD. Adresář služby Azure AD byl navržen tak, aby se jednalo o zabezpečený prostředek k použití pouze pro vaši organizaci – stejně jako je podniková kancelářská budova zabezpečeným prostředkem výhradně vaší organizace. Architektura služby Azure AD znemožňuje míchání dat zákazníků a informací o identitě. To znamená, že se uživatelé a správci jednoho adresáře služby Azure AD nemohou dostat – ať už omylem nebo záměrně – k datům v jiném adresáři.

![Správa služby Azure Active Directory][1]

## <a name="how-can-i-get-an-azure-ad-directory"></a>Jak získat adresář služby Azure AD?
Služba Azure AD poskytuje základní možnosti správy adresáře a identity pro většinu cloudových služeb Microsoftu, včetně těchto:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Adresář Azure AD získáte při registraci libovolné z těchto cloudových služeb Microsoftu. Podle potřeby můžete vytvářet další adresáře. Například můžete první adresář používat jako produkční a vytvořit další adresář pro testování nebo přípravu.

> [!NOTE]
> Doporučujeme, abyste pro registraci k dalším cloudovým službám Microsoftu používali účet správce přidružený k vaší organizaci, který jste použili při registraci k první službě.
> 
> 

Při první registraci ke cloudové službě Microsoftu budete vyzváni k zadání podrobností o vaší organizaci a registraci názvu internetové domény vaší organizace. Tyto informace se použijí k vytvoření nové instance adresáře služby Azure AD pro vaši organizaci. Stejný adresář se použije k ověřování pokusů o přihlášení, pokud se zaregistrujete k více cloudovým službám Microsoftu.

Další služby plně využívají integraci libovolných existujících uživatelských účtů, zásad, nastavení nebo místních adresářů, kterou si nakonfigurujete. To vám pomůže zlepšit efektivitu mezi místní infrastrukturou identity vaší organizace a službou Azure AD.

Pokud jste se například původně registrovali k předplatnému služby Microsoft Intune a provedli jste kroky nezbytné k další integraci místní služby Active Directory s adresářem služby Azure AD (nasazení synchronizace adresářů a/nebo serverů pro jednotné přihlašování), můžete se registrovat k jiné cloudové službě Microsoftu, například Office 365, která vám umožní využít stejných výhod integrace adresáře, které nyní využíváte ve službě Microsoft Intune.

Další informace o integraci místního adresáře se službou Azure AD najdete v článku o [integraci adresáře](active-directory-aadconnect.md).

### <a name="associate-an-azure-ad-directory-with-a-new-azure-subscription"></a>Přidružení adresáře služby Azure AD k novému předplatnému služby Azure
Nové předplatné služby Azure můžete přidružit ke stejnému adresáři, který používáte k ověřování přihlášení k existujícímu předplatnému služby Office 365 nebo Microsoft Intune. Přihlaste se k webu Azure Portal pomocí svého pracovního nebo školního účtu. Portál zobrazí zprávu, že se u tohoto účtu nepodařilo najít žádné předplatné. Klikněte na **Zaregistrovat se k Azure** a váš adresář bude možné spravovat v rámci portálu. Další informace naleznete v tématu [Správa adresáře pro předplatné služeb Office 365 ve službě Azure](active-directory-how-subscriptions-associated-directory.md#manage-the-directory-for-your-office-365-subscription-in-azure).

Video, které odpovídá na běžné otázky ohledně používání služby Azure AD, najdete v tématu [Azure Active Directory – běžné otázky ohledně registrace, přihlašování a používání](http://channel9.msdn.com/Series/Windows-Azure-Active-Directory/WAADCommonSignupsigninquestions).

### <a name="create-an-azure-ad-directory-by-signing-up-for-a-microsoft-cloud-service-as-an-organization"></a>Vytvoření adresáře služby Azure AD prostřednictvím registrace organizace ke cloudové službě Microsoftu
Pokud ještě nemáte předplatné cloudové služby Microsoftu, registrujte se prostřednictvím jednoho z odkazů níže. V rámci registrace k první službě se vám automaticky vytvoří adresář služby Azure AD.

* [Microsoft Azure](https://account.windowsazure.com/organization)
* [Office 365](http://products.office.com/business/compare-office-365-for-business-plans/)
* [Microsoft Intune](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0%20)

### <a name="manage-an-azure-provisioned-default-directory"></a>Správa výchozího adresáře poskytnutého službou Azure
Když se nyní zaregistrujete ke službě Azure, vytvoří se automaticky adresář a přidruží se k němu vaše předplatné. Pokud jste se ale ke službě Azure registrovali před říjnem roku 2013, adresář se automaticky nevytvořil. V takovém případě pro váš účet služba Azure pravděpodobně vytvořila výchozí adresář. Vaše předplatné bylo následně k tomuto výchozímu adresáři přidruženo.

Adresáře byly poskytnuty v říjnu roku 2013 v rámci celkového zlepšení bezpečnostního modelu služby Azure. Díky tomu mohou všichni zákazníci služby Azure využívat funkce organizační identity a je zajištěno, aby přístup k veškerým prostředkům služby Azure probíhal v kontextu uživatele v adresáři. Bez adresáře nemůžete službu Azure používat. Aby toho bylo možné dosáhnout, musel si každý uživatel, který se registroval před 7. červencem 2013 a neměl adresář, jeden adresář vytvořit. Pokud jste již měli adresář vytvořený, vaše předplatné se k němu přidružilo.

Za používání služby Azure AD se neplatí. Adresář je volným prostředkem. Existuje i dodatečná služba Azure Active Directory Premium, která má vlastní licencování a poskytuje další funkce, například firemní branding a samoobslužné resetování hesla.

Pokud chcete změnit zobrazovaný název vašeho adresáře, klikněte na adresář v portálu a pak klikněte na **Konfigurovat**. Můžete přidat nový adresář nebo odstranit adresář, který už nepotřebujete. Blíže se tomu budeme věnovat později v tomto tématu. Pokud chcete své předplatné přidružit k jinému adresáři, klikněte na **Nastavení** v levé navigaci a v dolní části stránky **Předplatné** klikněte na **Upravit adresář**. Můžete také vytvořit vlastní doménu pomocí názvu DNS, který jste si zaregistrovali, namísto výchozí domény *. onmicrosoft.com, která může být vhodnější při použití ve službě typu SharePoint Online.

## <a name="how-can-i-manage-directory-data"></a>Jak spravovat data adresáře
Jako správce jednoho nebo více předplatných cloudové služby Microsoftu můžete ke správě dat adresářů svých organizací použít [Centrum pro správu Azure AD](https://aad.portal.azure.com), Portál účtu Microsoft Intune nebo Centrum pro správu Office 365. Taky si můžete stáhnout a spustit rutiny [Modul Microsoft Azure Active Directory pro Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx), které vám pomohou spravovat data uložená ve službě Azure AD.

Pomocí kteréhokoli z těchto portálů (nebo rutin) můžete:

* vytvářet a spravovat účty uživatelů a skupin
* spravovat související cloudové služby, které si vaše organizace předplatila
* nastavit místní integraci s vaší adresářovou službou

[Centrum pro správu Azure AD](https://aad.portal.azure.com), Centrum pro správu Office 365, Portál účtu Microsoft Intune i rutiny služby Azure AD čtou a zapisují v rámci jediné sdílené instance služby Azure AD, která je přidružená k adresáři vaší organizace. Znázorňuje to následující obrázek. Portály (nebo rutiny) tímto způsobem fungují jako front-end rozhraní, které získává nebo upravuje data vašeho adresáře.

![][2]

Tyto portály účtů a přidružené rutiny služby Azure AD PowerShell, které se používají ke správě uživatelů a skupin, jsou postavené na platformě Azure AD.

Pokud změníte data vaší organizace prostřednictvím jednoho z těchto portálů, zatímco budete přihlášeni v kontextu jedné z těchto služeb, zobrazí se vám tato změna i v dalších portálech, jakmile se k nim v kontextu této služby přihlásíte. Tato data jsou totiž sdílena ve všech cloudových službách Microsoftu, které máte předplacené.
Pokud třeba v Centru pro správu Office 365 zablokujete určitému uživateli přihlašování, nebude se daný uživatel moci přihlásit k žádné službě, kterou má vaše organizace předplacenou. Pokud se na dotyčného uživatele podíváte v kontextu Portálu účtu Microsoft Intune, uvidíte, že je zablokovaný.

## <a name="how-can-i-add-and-manage-multiple-directories"></a>Jak přidat a spravovat více adresářů?
Adresář Azure AD můžete přidat na [portálu Azure AD Classic](https://manage.windowsazure.com). V podokně **Přehled** vyberte na levé straně rozšíření **Active Directory** a klikněte na **Nový**.

Každý adresář můžete spravovat jako plně nezávislý prostředek: každý adresář je rovnocenný, plně vybavený a logicky nezávislý na ostatních adresářích, které spravujete. Mezi adresáři neexistují žádné vztahy typu nadřazenost/podřazenost.  Tato nezávislost mezi adresáři zahrnuje nezávislost prostředků, nezávislost správy a nezávislost synchronizace.

* **Nezávislost prostředků**. Vytvoření nebo odstranění prostředku v jednom adresáři nemá žádný vliv na prostředky v jiných adresářích. Částečnou výjimku, která platí pro externí uživatele, popisujeme níže. Pokud používáte vlastní doménu „contoso.com“ u jednoho adresáře, nemůžete ji použít u jiných adresářů.
* **Nezávislost správy**.  Pokud uživatel adresáře „Contoso“, který nemá oprávnění správce, vytvoří adresář „Test“, pak:
  
  * Používá se nástroj pro synchronizaci adresáře k synchronizaci dat s jednou doménovou strukturou služby AD.
  * Správci adresáře Contoso nemají žádná přímá oprávnění pro správu adresáře Test, pokud jim správce adresáře Test oprávnění explicitně neudělí. Správci adresáře „Contoso“ můžou řídit přístup k adresáři „Test“ na základě ovládání uživatelského účtu, který vytvořil adresář „Test“.
    
    A pokud změníte (přidáte nebo odeberete) roli správce u uživatele v jednom adresáři, neovlivní tato změna žádnou roli správce, kterou tento uživatel může mít v jiném adresáři.
* **Nezávislost synchronizace**. Každý adresář služby Azure AD můžete nakonfigurovat nezávisle tak, aby se data synchronizovala z jediné instance jednoho z následujících:
  
  * Nástroj pro synchronizaci adresáře k synchronizaci dat s jednou doménovou strukturou služby AD.
  * Konektor služby Azure Active Directory pro nástroj Forefront Identity Manager k synchronizaci dat s jednou nebo více místními doménovými strukturami a/nebo zdroji dat mimo službu AD.

Všimněte si také, že na rozdíl od jiných prostředků služby Azure nejsou vaše adresáře podřízenými prostředky předplatného služby Azure. To znamená, že i v případě, že své předplatné služby Azure zrušíte nebo necháte vypršet, budete mít stále přístup k datům vašeho adresáře prostřednictvím rozhraní Azure AD PowerShell, Azure Graph API nebo dalších rozhraní, jako je například Centrum pro správu Office 365. K adresáři můžete také přidružit jiné předplatné.

## <a name="how-can-i-delete-an-azure-ad-directory"></a>Jak odstranit adresář služby Azure AD?
Adresář služby Azure AD může prostřednictvím portálu odstranit globální správce. Při odstranění adresáře se odstraní také všechny prostředky, které adresář obsahoval. Před odstraněním adresáře se proto ujistěte, že ho opravdu nepotřebujete.

> [!NOTE]
> Pokud je uživatel přihlášený prostřednictvím pracovního nebo školního účtu, nemůže se pokusit o odstranění vlastního domovského adresáře. Pokud je uživatel například přihlášený jako joe@contoso.onmicrosoft.com, nemůže odstranit adresář, který má jako výchozí doménu contoso.onmicrosoft.com.
> 
> 

### <a name="conditions-that-must-be-met-to-delete-an-azure-ad-directory"></a>Podmínky, které musí být splněny, aby bylo možné odstranit adresář služby Azure AD
Služba Azure AD vyžaduje, aby byly před odstraněním adresáře splněny určité podmínky. Tím se snižuje riziko, že odstranění adresáře negativně ovlivní uživatele nebo aplikace, například možnost uživatelů přihlásit se ke službě Office 365 nebo jejich přístup k prostředkům ve službě Azure. Pokud by například někdo neúmyslně odstranil adresář pro předplatné, ztratili by uživatelé přístup k prostředkům služby Azure daného předplatného.

Kontrolují se následující podmínky:

* Globální správce, který adresář odstraní, je jediným uživatelem v daném adresáři. Všichni ostatní uživatelé musí být před odstraněním adresáře odstraněni. Pokud uživatelé používají synchronizaci z místních adresářů, musí být tato synchronizace vypnuta a uživatelé musí být z adresáře cloudu odstraněni prostřednictvím Portálu pro správu nebo modulu Azure pro Windows PowerShell. Skupiny nebo kontakty, například kontakty přidané pomocí Centra pro správu Office 365, odstraňovat nemusíte.
* V adresáři nesmí být žádné aplikace. Všechny aplikace musí být před odstraněním adresáře odstraněny.
* K adresáři nesmí být přidruženo žádné předplatné online služby Microsoft, jako jsou Microsoft Azure, Office 365 nebo Azure AD Premium. Například pokud byl váš výchozí adresář vytvořený v Azure, nemůžete ho odstranit, pokud stále slouží k ověřování přihlášení k předplatnému služby Azure. Podobně nelze odstranit adresář, pokud k němu má přidružené předplatné jiný uživatel. Pokud předplatné chcete přidružit k jinému adresáři, přihlaste se k Portálu pro správu Azure a klikněte v levé navigaci na **Nastavení**. Poté v dolní části stránky **Předplatné** klikněte na **Upravit adresář**. Další informace o předplatných služby Azure najdete v článku o tom,  [jakým způsobem se předplatné služby Azure přidruží ke službě Azure AD](active-directory-how-subscriptions-associated-directory.md).

> [!NOTE]
> Pokud je uživatel přihlášený prostřednictvím pracovního nebo školního účtu, nemůže se pokusit o odstranění vlastního domovského adresáře. Pokud je uživatel například přihlášený jako joe@contoso.onmicrosoft.com, nemůže odstranit adresář, který má jako výchozí doménu contoso.onmicrosoft.com.
> 
> 

* S adresářem nesmí být propojení žádní poskytovatelé služby Multi-Factor Authentication.

## <a name="additional-resources"></a>Další zdroje
* [Fórum služby Azure AD](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD)
* [Fórum služby Azure Multi-Factor Authentication](https://social.msdn.microsoft.com/Forums/home?forum=windowsazureactiveauthentication)
* [Stackoverflow](http://stackoverflow.com/questions/tagged/azure)
* [Registrace organizace ke službě Azure](sign-up-organization.md)
* [Správa služby Azure AD pomocí rozhraní Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
* [Přiřazování rolí správce ve službě Azure AD](active-directory-assign-admin-roles.md)

<!--Image references-->
[1]: ./media/active-directory-administer/aad_portals.png
[2]: ./media/active-directory-administer/azure_tenants.png

