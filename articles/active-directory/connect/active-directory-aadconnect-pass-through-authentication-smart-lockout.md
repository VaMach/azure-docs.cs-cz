---
title: "Azure AD Connect: Ověřování průchozí – inteligentní uzamčení | Microsoft Docs"
description: "Tento článek popisuje, jak předávací ověřování Azure Active Directory (Azure AD) chrání vaše místní účty před útoky hrubou silou hesla v cloudu."
services: active-directory
keywords: "Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro Azure AD, jednotné přihlašování, jednotné přihlašování"
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: billmath
ms.openlocfilehash: 771741fd7da8c9b6932851851aaca148f9596643
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/24/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a>Předávací ověřování Azure Active Directory: Inteligentní uzamčení

## <a name="overview"></a>Přehled

Azure AD chrání před útoky hrubou silou heslo a originální uživatelům bránit v zamykají mimo jejich aplikace Office 365 a SaaS. Tato funkce volána **inteligentní uzamčení**, je podporováno, pokud používáte ověřování průchozí jako způsob přihlášení. Inteligentní uzamčení je ve výchozím nastavení povolena pro všechny klienty a chráníte uživatelské účty všech čas; není potřeba ji zapnout.

Inteligentní uzamčení funguje udržovat přehled o neúspěšných pokusů o přihlášení a po určitou **prahové hodnoty počtu uzamčení**, počáteční **doba trvání uzamčení**. Všechny pokusů o přihlášení před útočníkem během doby trvání uzamčení odmítají. Pokud pokračuje útoku, následné neúspěšné pokusy o přihlášení po dobu trvání uzamčení končí způsobí delší doby uzamčení.

>[!NOTE]
>Prahové hodnoty počtu uzamčení výchozí hodnota je 10 neúspěšných pokusů o přihlášení, a doba trvání uzamčení výchozí hodnota je 60 sekund.

Inteligentní uzamčení také rozlišuje přihlášení z originálního uživatelů a útočníci a pouze zámky se útočníci ve většině případů. Tato funkce zabrání útočníkům neoprávněnému uzamčení uživatele originálního. Používáme po přihlášení chování, zařízení uživatelů & prohlížeče a jinými signály odlišit uživatele originálního a útočníkům. Jsme jsou neustále zlepšování našich algoritmy.

Vzhledem k tomu, že předávací ověřování předává žádosti o ověření hesla do vaší místní služby Active Directory (AD), budete muset útočníkům zabránit v uzamykání účty AD vašich uživatelů. Vzhledem k tomu, že máte vlastní zásady AD uzamčení účtu (konkrétně [ **prahovou hodnotu uzamknutí účtu** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) a [ **zásady resetování čítač účet uzamčení po** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), budete muset nakonfigurovat prahové hodnoty počtu uzamčení Azure AD a doba trvání uzamčení hodnoty správně filtrovat útoky v cloudu než dosáhnou místní AD.

>[!NOTE]
>Funkce uzamčení inteligentní je zdarma a je _na_ ve výchozím nastavení pro všechny zákazníky. Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD však musí mít alespoň jednu licenci Azure AD Premium P2 vašeho klienta. Nepotřebujete licenci Azure AD Premium P2 _na uživatele_ získat funkci inteligentního uzamčení pomocí předávacího ověřování.

Zajistit, aby vaši uživatelé místní účty AD se dobře chráněný, musíte zajistit, aby:

1.  Prahová hodnota pro uzamčení Azure AD je _méně_ než prahovou hodnotu uzamknutí účtu na AD. Doporučujeme nastavit hodnoty tak, aby na AD prahovou hodnotu uzamknutí účtu alespoň dvě nebo tři krát prahové hodnoty počtu uzamčení Azure AD.
2.  Doba trvání uzamčení Azure AD (vyjádřený v sekundách) je _delší_ než AD na resetovat účet uzamčení čítač po (vyjádřený v minutách).

>[!IMPORTANT]
>Správce aktuálně nemůžete odemknout uživatelské účty cloudu, pokud budou mít byl uzamčen schopnosti inteligentní uzamčení. Budou mít čekání po dobu trvání uzamčení vyprší.

## <a name="verify-your-ad-account-lockout-policies"></a>Zkontrolujte vaše zásady uzamčení účtu služby AD

Postupujte podle následujících pokynů k ověření zásad uzamčení účtu AD:

1.  Spusťte nástroj pro správu zásad skupiny.
2.  Upravte zásady skupiny, které se použije pro všechny uživatele, například výchozí zásada domény.
3.  Přejděte na Konfigurace uživatele\Zásady\Nastavení systému Windows\Nastavení zabezpečení\Zásady zamknutí zásada pro uzamčení počítače.
4.  Ověřte hodnoty prahovou hodnotu uzamknutí účtu a resetování čítač účet uzamčení po.

![Zásady uzamčení účtu AD](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-the-graph-api-to-manage-your-tenants-smart-lockout-values-needs-premium-license"></a>Pomocí rozhraní Graph API ke správě vašeho klienta inteligentní uzamčení hodnoty (musí licencí Premium)

>[!IMPORTANT]
>Úprava prahové hodnoty počtu uzamčení a doba trvání uzamčení hodnotami pomocí rozhraní Graph API služby Azure AD je funkce Azure AD Premium P2. Je také musí být globálním správcem na vašeho klienta.

Můžete použít [grafu Explorer](https://developer.microsoft.com/graph/graph-explorer) číst, nastavit a aktualizujte hodnoty inteligentní uzamčení Azure AD. Ale můžete také provést tyto operace prostřednictvím kódu programu.

### <a name="read-smart-lockout-values"></a>Inteligentní uzamčení pro čtení hodnoty

Postupujte podle těchto kroků načíst hodnoty inteligentní uzamčení vašeho klienta:

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".
3. Žádost o rozhraní Graph API nakonfigurovat následujícím způsobem: nastavit verzi "Beta verze", typ požadavku na "GET" a adresu URL pro `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Klikněte na tlačítko "Spustit dotaz" zobrazíte hodnoty inteligentní uzamčení vašeho klienta. Pokud jste nenastavili před hodnoty vašeho klienta, zobrazí prázdnou sadou.

### <a name="set-smart-lockout-values"></a>Nastavení hodnot inteligentní uzamčení

Postupujte podle těchto kroků můžete nastavte hodnoty inteligentní uzamčení vašeho klienta (poprvé pouze):

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".
3. Nakonfigurujte rozhraní Graph API požadavek takto: nastavte verzi na "BETA", "POST" a adresa URL pro typ žádosti `https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.
4. Zkopírujte a vložte následující požadavku JSON do pole "Textu žádosti".
5. Klikněte na tlačítko "Spustit dotaz" nastavit hodnoty inteligentní uzamčení vašeho klienta.

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
>Pokud je nepoužíváte, můžete nechat **BannedPasswordList** a **EnableBannedPasswordCheck** hodnoty jako prázdný ("") a "false" v uvedeném pořadí.

Ověřte, že jste si nastavili hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).

### <a name="update-smart-lockout-values"></a>Aktualizujte hodnoty inteligentní uzamčení

Postupujte podle těchto kroků provedete aktualizaci hodnoty inteligentní uzamčení vašeho klienta (Pokud jste již zadali, je před):

1. Průzkumník grafu se přihlaste jako globální správce tenanta. Pokud se zobrazí výzva, udělení přístupu požadovaná oprávnění.
2. Klikněte na "Upravit oprávnění" a vyberte oprávnění "Directory.ReadWrite.All".
3. [Postupujte podle těchto kroků načíst hodnoty inteligentní uzamčení vašeho klienta](#read-smart-lockout-values). Kopírování `id` hodnota (GUID) položky se zobrazovaným názvem"" jako "PasswordRuleSettings".
4. Rozhraní Graph API žádosti nakonfigurovat následujícím způsobem: nastavit verzi "Beta verze", typ požadavku na "OPRAVIT" a adresu URL pro `https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -pomocí identifikátoru GUID z kroku 3 pro `<id>`.
5. Zkopírujte a vložte následující požadavku JSON do pole "Textu žádosti". Inteligentní uzamčení hodnoty podle potřeby změňte.
6. Klikněte na tlačítko "Spustit dotaz" aktualizujte hodnoty inteligentní uzamčení vašeho klienta.

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

Ověřte, že jste aktualizovali hodnoty inteligentní uzamčení vašeho klienta správně pomocí [tyto kroky](#read-smart-lockout-values).

## <a name="next-steps"></a>Další kroky
- [**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – pro vyplnění žádosti o nové funkce.
