---
title: "Podmíněný přístup pro Azure Active Directory | Microsoft Docs"
description: "Pomocí podmíněného řízení přístupu ve službě Azure Active Directory zkontrolujte za určitých podmínek při ověřování pro přístup k aplikacím."
services: active-directory
keywords: "podmíněný přístup k aplikacím, podmíněného přístupu s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/27/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 4cf30130907151ade9eaf9db28748b8141dac8e7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="conditional-access-in-azure-active-directory"></a>Podmíněný přístup v Azure Active Directory

První mobilní, cloudové první světě Azure Active Directory umožňuje jednotné přihlašování k zařízení, aplikacím a službám odkudkoli. S jak narůstá počet zařízení (včetně BYOD), použít vypnout podnikových sítích a aplikace SaaS 3. stran, odborníci v oblasti IT potýkají s dva dosáhnout cíle:

- Umožnit koncovým uživatelům k dosažení produktivity. kdykoli a kdekoli
- Ochrana podnikových prostředků kdykoli

Pokud chcete zvýšit produktivitu, Azure Active Directory poskytuje uživatelům s širokou škálu možností pro přístup k vaší podnikové prostředky. Azure Active Directory se správou aplikací přístup, umožňuje pouze zkontrolujte *příslušní lidé* můžete přístup k vaší aplikace. Co dělat, pokud chcete mít větší kontrolu nad jak příslušní lidé přistupují k prostředkům za určitých podmínek? Co když i máte podmínky, za kterých chcete zablokovat přístup k určitým aplikacím i pro *pravým osoby*? Například když může to být OK můžete příslušní lidé přistupují k určitým aplikacím z důvěryhodné sítě; však nemusí chcete, aby tyto aplikace přistupovat ze sítě, kterým nedůvěřujete. Tyto otázky pomocí podmíněného přístupu můžete vyřešit.

Podmíněný přístup je funkce služby Azure Active Directory, která umožňuje vynutit ovládacích prvků na přístup k aplikacím ve vašem prostředí na základě určitých podmínek. S ovládacími prvky můžete buď tie další požadavky na přístup, nebo můžete ho blokovat. Implementace podmíněného přístupu je založená na zásadách. O přístupu na základě zásad usnadňuje prostředí konfigurace, protože postupuje způsob, jakým si myslíte o požadavků na přístup.  

Obvykle můžete definovat požadavků na přístup pomocí příkazů, které jsou založeny na vzoru následující:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/10.png)

Když nahradíte dva výskyty "*to*" reálného informace, máte příklad prohlášení o zásadách, pravděpodobně bude vypadat snadno dokážete:

*Když dodavatelů se pokoušíte získat přístup ze sítě, které nejsou důvěryhodné naší cloudové aplikace, pak Blokujte přístup.*

Prohlášení o zásadách výše označuje power podmíněného přístupu. Když povolíte dodavatelů v podstatě přístup cloudových aplikací (**kdo**), s podmíněným přístupem můžete také definovat podmínky, za kterých je možné přístup (**jak**).

V rámci Azure Active Directory podmíněný přístup,

- "**v takovém případě**" se nazývá **podmínky – příkaz**
- "**Udělejte to**" se nazývá **ovládací prvky**

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/11.png)

Kombinace příkaz podmínky s ovládacími prvky představuje zásady podmíněného přístupu.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/12.png)


## <a name="controls"></a>Ovládací prvky

V zásadách podmíněného přístupu ovládací prvky definovat, co je, že, musí dojít v případě, že příkaz podmínky má byly splněny.  
S ovládacími prvky můžete blokovat přístup nebo povolit přístup s další požadavky.
Pokud budete konfigurovat zásadu, která umožňuje přístup, je nutné vybrat alespoň jeden požadavek.  

Existují dva typy ovládacích prvků: 

- **Ovládací prvky grant** – ovládací prvky Grant řídí, zda může uživatel dokončí ověření a přístup k prostředku, který se pokouší přihlásit k. Pokud máte více ovládacích prvků vybraná, můžete nakonfigurovat, jestli všechny z nich jsou povinné, při zpracování vaší zásady.
Aktuální implementace služby Azure Active Directory umožňuje nakonfigurovat následující požadavky řízení grant:

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/73.png)

- **Ovládací prvky relace** -relace řídí povolit omezení prostředí v rámci cloudové aplikace. Ovládací prvky relace vynucuje cloudových aplikací a spoléhá na další informace, které poskytuje Azure AD do aplikace o relaci.

    ![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/31.png)


Další informace najdete v tématu [ovládacích prvků v Azure Active Directory podmíněného přístupu](active-directory-conditional-access-controls.md).


## <a name="condition-statement"></a>Příkaz podmínky

V předchozí části obsahuje zavedla podporované možnosti blokovat nebo omezení přístupu k prostředkům v podobě ovládacích prvků. V zásadách podmíněného přístupu zadejte kritéria, která musí být splněny pro vaše ovládací prvky, které se má použít v podobě příkaz podmínky.  

Do vaší příkaz podmínky může zahrnovat následující přiřazení:

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/07.png)


### <a name="who"></a>Kdo?

Pokud budete konfigurovat zásadu podmíněného přístupu, je nutné vybrat uživatele nebo skupiny, které vaše zásada se vztahuje na. V řadě případů budete chtít vaše ovládací prvky má být použita pro konkrétní skupinu uživatelů. V příkazu podmínku můžete definovat této sady výběrem požadovaných uživatelů a skupin, které vaše zásada se vztahuje na. V případě potřeby můžete také výslovně vyloučit sadu uživatelů ze zásady výjimky je.  

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/08.png)



### <a name="what"></a>Co?

Pokud budete konfigurovat zásadu podmíněného přístupu, je nutné vybrat cloudové aplikace, které vaše zásada se vztahuje na.
Ve vašem prostředí z hlediska ochrany vyžaduje další pozornost než jiné jsou obvykle určité aplikace. Tímto je ovlivněn, například aplikace, které mají přístup k citlivým datům.
Pokud vyberete cloudové aplikace, definujte rozsah cloudové aplikace, které vaše zásada se vztahuje na. V případě potřeby můžete také výslovně vyloučili sadu aplikací z vaší zásady.

![Ovládací prvek](./media/active-directory-conditional-access-azure-portal/09.png)

Úplný seznam cloudových aplikací v požadované zásady podmíněného přístupu můžete použít, najdete v článku [technické informace o Azure Active Directory podmíněného přístupu](active-directory-conditional-access-technical-reference.md#cloud-apps-assignments).

### <a name="how"></a>Jak?

Také přístup k aplikacím probíhá podle podmínek, které můžete řídit, může být pro nastavení dalších ovládacích prvků na tom, jak cloudové aplikace přistupují uživatelé není nutné. Věcí však může vypadat jinak, pokud se provádí přístup k vaší cloudové aplikace, například v ze sítě, které nejsou důvěryhodné nebo zařízení, která nejsou kompatibilní. V příkazu podmínku můžete definovat určité podmínky přístupu, které mají další požadavky na tom, jak se provádí přístup k aplikacím.

![Podmínky](./media/active-directory-conditional-access-azure-portal/01.png)


## <a name="conditions"></a>Podmínky

V aktuální implementace služby Azure Active Directory můžete definovat podmínky v následujících oblastech:

- Riziko přihlášení
- Platformy zařízení
- Umístění
- Klientské aplikace


![Podmínky](./media/active-directory-conditional-access-azure-portal/01.png)

### <a name="sign-in-risk"></a>Riziko přihlášení

Riziko přihlášení je objekt, který se používá služba Azure Active Directory pro trasování pravděpodobnost, že u přihlášení pokus nebyla provedena legitimní vlastníkem uživatelského účtu. V tomto objektu pravděpodobnost (vysoká, střední nebo nízká) je uložen v podobě atributu s názvem [úroveň rizika přihlašovací](active-directory-reporting-risk-events.md#risk-level). Tento objekt je generován během přihlášení uživatele, pokud byly zjištěny rizika přihlášení pomocí služby Azure Active Directory. Další informace najdete v tématu popisujícím [riziková přihlášení](active-directory-identityprotection.md#risky-sign-ins).  
Úroveň rizika počítané přihlášení můžete použít jako podmínku v zásadách podmíněného přístupu. 

![Podmínky](./media/active-directory-conditional-access-azure-portal/22.png)

### <a name="device-platforms"></a>Platformy zařízení

Operační systém, který běží na vašem zařízení je charakterizovaná platformu zařízení:

- Android
- iOS
- telefon se systémem Windows
- Windows
- systému macOS (preview). 

![Podmínky](./media/active-directory-conditional-access-azure-portal/02.png)

Můžete definovat platformy zařízení, které jsou zahrnuty i platformy zařízení, které jsou vyloučené ze zásad.  
Pokud chcete použít v zásadách platformy zařízení, nejprve změňte konfigurace přepínačů **Ano**a vyberte všechny nebo platformy jednotlivých zařízení se zásady vztahují. Pokud vyberete platformy jednotlivých zařízení, zásady má vliv pouze na těchto platformách. V tomto případě nejsou zásady dopad na přihlášení na jiných podporovaných platforem.


### <a name="locations"></a>Umístění

Umístění máte možnost definovat podmínky, které jsou založeny na kde bylo inicializováno pokus o připojení. Položky v seznamu umístění jsou buď **s názvem umístění** nebo **MFA důvěryhodné IP adresy**.  

**S názvem umístění** je funkce služby Azure Active Directory, který umožňuje definovat popisky pro pokusy o připojení umístění nebyly provedeny z. Zadat umístění, buď konfigurací IP adresy rozsahů adres, nebo vyberte zemi nebo oblast.  

![Podmínky](./media/active-directory-conditional-access-azure-portal/42.png)

Kromě toho můžete označit jako důvěryhodné umístění s názvem umístění. Pro zásady podmíněného přístupu, důvěryhodném umístění je další možností filtru, který vám umožní vybrat *všech důvěryhodných umístění* v podmínce vaše umístění.
Pojmenované umístění jsou také důležité v kontextu detekce [rizik události](active-directory-reporting-risk-events.md) snížit počet pozitivních false pro nemožná cesta do netypických míst riziko událostí. 

Počet pojmenované umístění, které můžete konfigurovat, je omezené velikost je související objekt ve službě Azure AD. Můžete nakonfigurovat:
 
 - Jedno umístění s názvem s maximálně 500 rozsahy IP adres
 - Nesmí být delší než 60 s názvem umístění (preview) s jeden rozsah IP adres přiřazené ke každému z nich 

Další informace najdete v tématu [s názvem umístění v Azure Active Directory](active-directory-named-locations.md).


**MFA důvěryhodné IP adresy** je funkce služby Multi-Factor authentication, která umožňuje definovat důvěryhodné rozsahy IP adres reprezentující místní intranet vaší organizace. Při konfiguraci umístění podmínku důvěryhodných IP adres umožňuje rozlišovat připojení z vaší podnikové síti a všech jiných umístění. Další informace najdete v tématu [důvěryhodné IP adresy](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

Zásady podmíněného přístupu můžete:

- Zahrnout
    - Jakékoli umístění
    - Všechny důvěryhodné umístění
    - Vybrané umístění
- Vyloučení
    - Všechny důvěryhodné umístění
    - Vybrané umístění
     
![Podmínky](./media/active-directory-conditional-access-azure-portal/03.png)


### <a name="client-apps"></a>Klientské aplikace

Klientská aplikace může být buď na obecné úrovni aplikace (webový prohlížeč, mobilní aplikace, klient plocha), jste použili pro připojení k Azure Active Directory nebo můžete konkrétně vybrat protokolu Exchange Active Sync.  
Starší verze ověřování odkazuje na klienty, kteří používají základní ověřování, jako je starší klienty Office, které nepoužívají moderní ověřování. Pomocí starší verze ověřování není aktuálně podporuje podmíněný přístup.

![Podmínky](./media/active-directory-conditional-access-azure-portal/04.png)


Úplný seznam klientských aplikací v požadované zásady podmíněného přístupu můžete použít, najdete v článku [technické informace o Azure Active Directory podmíněného přístupu](active-directory-conditional-access-technical-reference.md#client-apps-condition).




## <a name="common-scenarios"></a>Obvyklé scénáře

### <a name="requiring-multi-factor-authentication-for-apps"></a>Vyžadování vícefaktorového ověřování pro aplikace

Mnoho prostředí mít aplikace, které vyžadují vyšší úroveň ochrany než jiné.
To platí, například pro aplikace, které mají přístup k citlivým datům.
Pokud chcete přidat další vrstvu ochrany do těchto aplikací, můžete nakonfigurovat zásady podmíněného přístupu, který vyžaduje službu Multi-Factor authentication, když uživatelé přistupují těchto aplikací.


### <a name="requiring-multi-factor-authentication-for-access-from-networks-that-are-not-trusted"></a>Vyžadování vícefaktorového ověřování pro přístup ze sítě, které nejsou důvěryhodné

Tento scénář je podobný předchozímu scénáři, protože ho přidá požadavek pro službu Multi-Factor authentication.
Hlavní rozdíl je však podmínky pro tento požadavek.  
Během fokus předchozím scénáři je pro aplikace s přístupem k datům sensitve, je aktivní tohoto scénáře důvěryhodného umístění.  
Jinými slovy může mít požadavek pro službu Multi-Factor authentication, pokud uživatel ze sítě, kterým nedůvěřujete přístupu k aplikaci.


### <a name="only-trusted-devices-can-access-office-365-services"></a>Jenom důvěryhodné zařízení mají přístup ke službám Office 365

Pokud ve svém prostředí používáte Intune, můžete okamžitě začít používat rozhraní zásad podmíněného přístupu v konzole Azure.

Mnoho zákazníků Intune použití podmíněného přístupu k zajištění, že pouze důvěryhodné zařízení mají přístup ke službám Office 365. To znamená, že mobilní zařízení jsou zaregistrovaná v Intune a splňovat požadavky zásad dodržování předpisů, a že jsou počítače s Windows připojený k doméně místní. Klíče zlepšování je, že není nutné nastavit stejné zásady pro každou služeb Office 365.  Když vytvoříte novou zásadu, nakonfigurujte cloudových aplikací na každý ze O365 aplikace, které chcete chránit pomocí podmíněného přístupu.

## <a name="next-steps"></a>Další kroky

- Pokud chcete vědět, jak konfigurovat zásadu podmíněného přístupu, najdete v článku [Začínáme s podmíněným přístupem v Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup v Azure Active Directory](active-directory-conditional-access-best-practices.md). 
