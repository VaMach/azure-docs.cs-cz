---
title: "Zabezpečení hesel ve službě Azure AD a resetování hesel, která byla zablokována inteligentním uzamčením hesla | Dokumentace Microsoftu"
description: "Vysvětluje, co je to klient služby Azure AD a jak spravovat službu Azure pomocí služby Azure Active Directory"
services: active-directory
documentationcenter: 
author: markvi
writer: v-lorisc
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/02/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 8e625a346c9495d436a99fcf9eadf8ffeffcfdff
ms.lasthandoff: 03/28/2017


---
# <a name="secure-passwords--in-azure-ad-and-reset-passwords-that-get-blocked-by-smart-password-lockout"></a>Zabezpečení hesel ve službě Azure AD a resetování hesel, která byla zablokována inteligentním uzamčením hesla
Tento článek popisuje osvědčené postupy, které můžete použít jako uživatel nebo jako správce k ochraně služby Azure Active Directory (Azure AD) a účtů služby Microsoft Account Service. 

 >[!NOTE]
 >Správci služby Azure AD můžou resetovat hesla uživatelů kliknutím na název adresáře. Z [Portálu pro správu Azure](https://manage.windowsazure.com) vyberte stránku Uživatelé, klikněte na jméno uživatele a na Resetovat heslo. 
 >

Azure AD zahrnuje následující běžné přístupy k zabezpečení hesel:
 *    Požadavky na délku hesla
 *    Požadavky na složitost hesla
 *    Pravidelné a opakované vypršení platnosti hesla 

Informace o možnostech správy hesel najdete v tématu [Správa hesel ve službě Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-manage-passwords). 

## <a name="azure-ad-password-protection"></a>Ochrana hesel Azure AD
Azure AD a Microsoft Account System používají ověřené přístupy k zajištění bezpečné ochrany hesel uživatelů a správců. 

Tato část popisuje, jak Azure AD chrání hesla pomocí následujících metod:
 *    Dynamicky zakázaná hesla
 *    Inteligentní uzamčení hesla

Informace o správě hesel na základě současných výzkumů najdete v dokumentu white paper [Password Guidance](http://aka.ms/passwordguidance) (Pokyny k heslům). 

### <a name="dynamically-banned-passwords"></a>Dynamicky zakázaná hesla
Azure AD a Microsoft Account System provádí ochranu hesel pomocí dynamického zakazování všech běžně používaných hesel. Tým Azure AD Identity Protection pravidelně analyzuje seznamy zakázaných hesel, aby bránil uživatelům ve zvolení běžně používaných hesel. Tato služba je k dispozici pro zákazníky Azure AD a Microsoft Account Service. 

Při vytváření hesel je pro správce důležité nutit uživatele k volbě neobvyklých hesel, která zahrnují jedinečné kombinace písmen, číslic a znaků. To pomáhá k tomu, aby prolomení uživatelských hesel bylo téměř nemožné. 

**Seznamy průniků**

Služba Azure AD se vždy snaží být o krok před kybernetickými zločinci. Jedním ze způsobů je bránit uživatelům ve vytváření hesel, které jsou na seznamu aktuálních útoků.

Tým Azure AD Identity Protection průběžně analyzuje hesla, která se běžně používají. Kybernetičtí zločinci také používají podobné strategie k úpravě svých útoků, jako je například vytváření [duhové tabulky](https://en.wikipedia.org/wiki/Rainbow_table) pro prolamování hodnot hash hesel. 

Microsoft průběžně analyzuje [úniky dat](https://www.privacyrights.org/data-breaches) a udržuje dynamicky aktualizovaný seznam zakázaných hesel, který zajišťuje, že citlivá hesla jsou zakázána dřív, než se stanou skutečnou hrozbou zákazníkům služby Azure AD. Další informace o našem aktuálním úsilí týkajícím se zabezpečení najdete ve zprávě [Microsoft Security Intelligence Report](https://www.microsoft.com/security/sir/default.aspx). 

### <a name="smart-password-lockout"></a>Inteligentní uzamčení hesla

Když Azure AD zjistí, že potenciální kybernetický zločinec se snaží o prolomení uživatelského hesla, uživatelský účet se uzamkne pomocí inteligentního uzamčení hesla. Azure AD vyhodnocuje riziko spojené s konkrétními přihlašovacími relacemi. 

Pomocí nejnovějších dat zabezpečení použijeme pro kybernetické hrozby sémantiku uzamčení. Tímto způsobem uživatelé nebudou uzamčeni v případě, když kybernetický zločinec prolomil uživatelská hesla ve vaší síti.

Pokud je uživatel uzamčen a vyloučen z Azure AD, vypadá jeho obrazovka podobně jako následující:

  ![Uzamčení a vyloučení z Azure AD](./media/active-directory-secure-passwords/locked-out-azuread.png)
  
A pro ostatní účty Microsoft vypadá jeho obrazovka podobně jako následující:

  ![Uzamčení a vyloučení z účtu Microsoft](./media/active-directory-secure-passwords/locked-out-ms-accounts.png)

Informace o správě hesel v Azure Active Directory najdete v tématu s popisem [fungování správy hesel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-how-it-works).

  >![NOTE] Pokud jste správce Azure AD, můžete chtít použít [Windows Hello](https://www.microsoft.com/en-us/windows/windows-hello), aby vaši uživatelé přestali vytvářet tradiční hesla úplně.
  >

## <a name="next-steps"></a>Další kroky
[Postup aktualizace vlastního hesla](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-update-your-own-password)<br>
[Základy správy identit Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>
[Jak získat provozní informace pomocí sestav správy hesel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-passwords-get-insights#view-password-reset-activity)



