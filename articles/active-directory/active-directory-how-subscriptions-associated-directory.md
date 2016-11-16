---
title: "Jak je předplatné Azure propojeno se službou Azure Active Directory | Dokumentace Microsoftu"
description: "Popisuje přihlášení do služby Microsoft Azure a související problémy, jako je například vztah mezi předplatným služby Azure a službou Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 73d58df13d3265312b371a02e12fbb9342fb3980


---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Jak je předplatné Azure propojeno se službou Azure Active Directory
V tomto článku najdete informace o přihlašování do služby Microsoft Azure a souvisejících problémech, jako je například vztah mezi předplatným služby Azure a službou Azure Active Directory (Azure AD).

## <a name="accounts-that-you-can-use-to-sign-in"></a>Účty, které můžete použít pro přihlášení
Začněme s účty, které můžete použít pro přihlášení Existují dva typy: účet Microsoft (dříve označovaný jako Microsoft Live ID) a pracovní nebo školní účet, což je účet uložený ve službě Azure AD.

| Účet Microsoft | Účet Azure AD |
| --- | --- |
| Systém identit uživatelů společnosti Microsoft |Systém identit podniků společnosti Microsoft |
| Ověřování přístupu do služeb orientovaných na uživatele, například Hotmail nebo MSN |Ověřování přístupu do služeb orientovaných na podniky, například Office 365 |
| Uživatelé vytváří vlastní účty Microsoft, například při registraci e-mailu |Společnosti a organizace vytváří a spravují vlastní pracovní nebo školní účty |
| Identity jsou vytvořeny a uloženy v systému účtů Microsoft |Identity se vytváří pomocí služby Azure nebo jiných služeb, např. Office 365, a jsou uloženy v instanci služby Azure AD, která je přiřazená k organizaci |

I když služba Azure původně umožňovala přístup pouze uživatelům účtů Microsoft, nyní umožňuje přístup uživatelům z *obou* systémů. Provedli jsme to tak, že jsme všechny vlastnosti služby Azur nechali ověřovat službou Azure AD, která ověřuje organizační uživatele, a také jsme vytvořili federační vztah, kde Azure AD nechává spotřebitelské uživatele ověřovat systémem identit uživatelů účtů Microsoft. V důsledku toho dokáže služba Azure AD ověřit hostitelské účty Microsoft i nativní účty Azure AD.

Zde se například uživatel s účtem Microsoft přihlašuje na portál Azure Classic.

> [!NOTE]
> Aby se mohl přihlásit na portál Azure Classic, musí mít adresa msmith@hotmail.com předplatné služby Azure. Účet musí být buď správce služby, nebo spolusprávce předplatného.
> 
> 

![][1]

Protože je tato hotmailová adresa uživatelský účet, přihlášení bude ověřeno systémem identit uživatelů účtů Microsoft. Systém identit Azure AD důvěřuje ověření provedenému systémem účtů Microsoft a vydá token k přístupu ke službám Azure.

## <a name="how-an-azure-subscription-is-related-to-azure-ad"></a>Jak předplatné služby Azure souvisí se službou Azure AD
Každé předplatné služby Azure má vztah důvěryhodnosti s instancí služby Azure AD. To znamená, že tomuto adresáři svěřuje ověřování uživatelů, služeb i zařízení. Několik předplatných může důvěřovat stejnému adresáři, ale jedno předplatné důvěřuje pouze jednomu adresáři. Na kartě Nastavení můžete zobrazit, který adresář je pro vaše předplatné důvěryhodný. [Nastavení předplatného můžete upravit](active-directory-understanding-resource-access.md) a změnit adresář, kterému bude předplatné důvěřovat.

Vztah důvěryhodnosti, který má předplatné s adresářem, se liší od vztahu, který má předplatné se všemi ostatními prostředky ve službě Azure (webové stránky, databáze apod.), což jsou pro předplatné spíše podřízené prostředky. Pokud platnost předplatného vyprší, zastaví se i přístup k těmto dalším prostředkům přidruženým k předplatnému. Adresář však ve službě Azure zůstane a vy k němu můžete přidružit jiné předplatné a pokračovat ve správě uživatelů adresáře.

A podobně i rozšíření Azure AD, které se zobrazí v rámci předplatného, nefunguje stejně jako ostatní rozšíření na portálu Azure Classic. Další rozšíření na portálu Azure Classic jsou vymezena podle předplatného služby Azure. To, co se zobrazuje v rozšíření Azure AD, se neodlišuje na základě předplatného – adresáře se zobrazují pouze na základě přihlášeného uživatele.

Všichni uživatelé mají jeden domovský adresář, který je ověřuje, ale mohou být i hosté ostatních adresářů. V rozšíření Azure AD se zobrazí každý adresář, kterého je váš uživatelský účet členem. Nezobrazí se žádný adresář, jehož členem váš účet není. Adresář může vystavovat tokeny pro pracovní nebo školní účty ve službě Azure AD nebo pro uživatele účtů Microsoft (protože je služba Azure AD federovaná se systémem účtů Microsoft).

Tento diagram znázorňuje předplatné pro Michaela Smithe po přihlášení pomocí pracovního účtu pro Contoso.

![][2]

## <a name="how-to-manage-a-subscription-and-a-directory"></a>Správa předplatného a adresáře
Správní role pro předplatné služby Azure spravují prostředky, které jsou svázané s předplatným služby Azure. Tyto role a doporučené postupy pro správu předplatného jsou popsané v kapitole [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles.md).

Ve výchozím nastavení máte po registraci přiřazenou roli správce služby. Pokud se ostatní potřebují přihlašovat ke službám a používat je v rámci stejného předplatného, můžete je přidat jako spolusprávce. Správci služeb a spolusprávci mohou být účty Microsoft nebo pracovní či školní účty z adresáře, který je přidružen k předplatnému služby Azure.

Azure AD má ke správě adresáře a funkcím souvisejícím s identitou k dispozici jinou sadu správních rolí. Globální správce adresáře může do adresáře například přidávat uživatele a skupiny, nebo vyžadovat vícefaktorové ověřování pro uživatele. Uživatel, který vytvoří adresář, je přiřazen k roli globálního správce a může ostatním uživatelům přiřazovat správní role.

Stejně jako u správců předplatného, mohou mít správní roli účty Microsoft nebo pracovní či školní účty. Správní role služby Azure AD jsou také používány dalšími službami, například Office 365 nebo Microsoft Intune. Další informace naleznete v tématu [Přiřazení rolí správce](active-directory-assign-admin-roles.md).

Je zde však nutné zdůraznit, že správci předplatného služby Azure a správci adresáře služby Azure AD představují dva samostatné koncepty.  Správci předplatného služby Azure mohu spravovat prostředky ve službě Azure a zobrazovat rozšíření Active Directory na portálu Azure Classic (protože portál Azure Classic je prostředek služby Azure). Správci adresáře mohou spravovat vlastnosti v adresáři.

Jedna osoba může mít obě role, ale není to povinné. Uživateli lze přiřadit roli globálního správce adresáře, ale nelze ho přiřadit jako správce nebo spolusprávce služeb v předplatném služby Azure. Pokud uživatel není správce předplatného, nemůže se přihlásit na portál Azure Classic. Úlohy pro správu adresáře ale může provádět pomocí jiných nástrojů, například Azure AD PowerShell nebo Centrum pro správu Office 365.

## <a name="why-cant-i-manage-the-directory-with-my-current-user-account"></a>Proč nemůžu spravovat adresář pomocí aktuálního uživatelského účtu?
Uživatel se někdy může pokusit přihlásit na portál Azure Classic pomocí pracovního nebo školního účtu před registrací předplatného služby Azure. V takovém případě uživatel obdrží zprávu, že pro tento účet neexistuje předplatné. Zpráva bude obsahovat odkaz k zahájení bezplatné zkušební verze předplatného.

Po registraci bezplatné zkušební verze se uživateli na portálu Azure Classic zobrazí adresář pro organizaci, nebude ho ale moct spravovat (tzn. přidávat uživatele nebo upravovat vlastnosti stávajících uživatelů), protože není globální správce adresáře. Předplatné uživateli umožní používat portál Azure Classic a zobrazit rozšíření Azure Active Directory, ale ke správě adresáře jsou potřebná další oprávnění globálního správce.

## <a name="using-your-work-or-school-account-to-manage-an-azure-subscription-that-was-created-by-using-a-microsoft-account"></a>Použití pracovního nebo školního účtu ke správě předplatného služby Azure, které bylo vytvořeno pomocí účtu Microsoft
Osvědčený postup je [Registrace organizace ke službě Azure](sign-up-organization.md) a používání ke správě prostředků ve službě Azure pracovní nebo školní účet: Pracovní nebo školní účty jsou upřednostňované, protože můžou být centrálně spravované organizací, která je vydala, mají více funkcí než účty Microsoft a jsou přímo ověřovány službou Azure AD. Stejný účet poskytuje přístup k dalším online službám společnosti Microsoft, které jsou nabízeny podnikům a organizacím, jako je Office 365 nebo Microsoft Intune. Pokud již máte účet, který používáte s těmito ostatními službami, pravděpodobně ho budete chtít použít i ve službě Azure. Také již budete mít instanci Active Directory podporující služby, kterým má předplatné služby Azure důvěřovat.

Pracovní nebo školní účty je také možné spravovat více způsoby než účty Microsoft. Správce může například obnovit heslo pracovního nebo školního účtu nebo pro něj vyžadovat vícefaktorové ověření.

V některých případech můžete chtít, aby uživatelé spotřebitelského účtu Microsoft z vaší organizace mohli spravovat prostředky přidružené k předplatnému služby Azure. Další informace o přecházení mezi účty, které spravují předplatné či adresáře, najdete v tématu [Správa adresáře pro předplatné služeb Office 365 ve službě Azure](#manage-the-directory-for-your-office-365-subscription-in-azure).

## <a name="signing-in-when-you-used-your-work-email-for-your-microsoft-account"></a>Přihlášení po použití pracovního e-mailu pro účet Microsoft
Pokud jste v určitém okamžiku v minulosti vytvořili uživatelský účet Microsoft pomocí pracovního e-mailu jako identifikátoru uživatele, můžete být na stránce požádáni o výběr systému účtů Microsoft Azure nebo systému účtů Microsoft.

![][3]

Máte uživatelské účty se stejným názvem, jeden ve službě Azure AD a druhý v systému uživatelských účtů Microsoft. Měli byste vybrat účet přidružený předplatnému služby Azure, které chcete použít. Pokud se zobrazí chyba s oznámením, že předplatné pro tohoto uživatele neexistuje, pravděpodobně jste právě zvolili nesprávnou možnost. Odhlaste se a akci opakujte. Další informace o chybách, které mohou bránit přihlášení, naleznete v části [Řešení potíží s chybami „Nepodařilo se najít žádné předplatné spojené s vaším účtem“](https://social.msdn.microsoft.com/Forums/en-US/f952f398-f700-41a1-8729-be49599dd7e2/troubleshooting-we-were-unable-to-find-any-subscriptions-associated-with-your-account-errors-in?forum=windowsazuremanagement).

## <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Správa adresáře pro předplatné služeb Office 365 ve službě Azure
Řekněme, že jste si zaregistrovali služby Office 365 před službou Azure. Nyní chcete spravovat adresář pro předplatné služeb Office 365 na portálu Azure Classic. Můžete to provést dvěma způsoby v závislosti na tom, zda máte nebo nemáte registrovanou službu Azure.

### <a name="i-do-not-have-a-subscription-for-azure"></a>Nemám předplatné služby Azure
V tomto případě si [registrujte službu Azure](sign-up-organization.md) pomocí stejného pracovního nebo školního účtu, jaký používáte k přihlášení do služeb Office 365. Ve formuláři pro registraci do služby Azure se znovu vyplní příslušné informace z účtu Office 365. Účet bude v předplatném přiřazen do role správce služeb.  

### <a name="i-do-have-a-subscription-for-azure-using-my-microsoft-account"></a>Mám předplatné služby Azure pomocí účtu Microsoft
Pokud jste si pomocí pracovního nebo školního účtu zaregistrovali služby Office 365 a potom pomocí účtu Microsoft zaregistrovali službu Azure, máte dva adresáře: jeden pro práci nebo školu a jeden výchozí adresář, který byl vytvořen při registraci služby Azure.

Abyste mohli na portálu Azure Classic spravovat oba adresáře, postupujte následovně.

> [!NOTE]
> Kroky je možné dokončit, jen když je uživatel přihlášený pomocí účtu Microsoft. Pokud je uživatel přihlášený pomocí pracovního nebo školního účtu, možnost **Použít existující adresář** není k dispozici, protože pracovní a školní účet může být ověřen pouze pomocí jeho domovského adresáře (to znamená adresáře, ve kterém je pracovní nebo školní účet uložen a který vlastní práce nebo škola).
> 
> 

1. Přihlášení na portálu Azure Classic pomocí účtu Microsoft.
2. Klikněte na tlačítko **Nový** > **App Services** > **Active Directory** > **Adresář** > **Vytvořit vlastní**.
3. Klikněte na tlačítko **Použít existující adresář** a zaškrtnutím políčka **Nyní mě můžete odhlásit** akci dokončete.
4. Přihlaste se na portál Azure Classic  pomocí účtu, který má práva globálního správce pro pracovní nebo školní adresář.
5. Po zobrazení výzvy **Používat adresář Contoso se službou Azure?** klikněte na tlačítko **Pokračovat**.
6. Klikněte na tlačítko **Odhlásit**.
7. Přihlaste se znovu na portál Azure Classic pomocí účtu Microsoft. V rozšíření Active Directory se zobrazí oba adresáře.

## <a name="next-steps"></a>Další kroky
* Další informace o tom, jak změnit správce pro předplatné služby Azure naleznete v tématu [Postup přidání nebo změna role správce služby Azure](../billing-add-change-azure-subscription-administrator.md)
* Další informace o tom, jak se přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům ve službě Azure](active-directory-understanding-resource-access.md)
* Další informace o tom, jak přiřadit role ve službě Azure AD, najdete v tématu [Přiřazení rolí správce ve službě Azure Active Directory](active-directory-assign-admin-roles.md)
* [Registrace organizace ke službě Azure](sign-up-organization.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG



<!--HONumber=Nov16_HO2-->


