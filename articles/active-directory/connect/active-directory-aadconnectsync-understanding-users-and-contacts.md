---
title: "Synchronizace Azure AD Connect: Principy uživatelů a kontaktů | Microsoft Docs"
description: "Vysvětluje, uživatelů a kontaktů v synchronizaci Azure AD Connect."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 8d204647-213a-4519-bd62-49563c421602
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi;andkjell
ms.openlocfilehash: 0ad3194a0827c4ef68267ce5e3e3fcbe225e8a3d
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/18/2017
---
# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Synchronizace služby Azure AD Connect: Principy uživatelů a kontaktů
Existuje několik různých důvodů, proč by měla mít několik doménových struktur služby Active Directory a existuje několik různých nasazení topologie. Po fúze a akvizice zahrnovat běžné modely k nasazení účtu prostředků a GAL sync'ed doménových struktur. Ale i v případě, že jsou čistá modely, hybridní modely jsou společné také. Výchozí konfigurace v synchronizaci Azure AD Connect nepředpokládá žádné konkrétní modelu, ale v závislosti na tom, jak hledání shody uživatelů jste vybrali v instalační příručce, může být dodržen různé chování.

V tomto tématu provedeme chování výchozí konfigurace v určitých topologiích. Projdeme konfigurace a Editor pravidla synchronizace lze použít k podívejte se na konfiguraci.

Existuje několik obecná pravidla, která předpokládá konfigurace:

* Bez ohledu na to, které pořadí jsme importovat ze zdroje Active Directory konečný výsledek by měl být vždy stejné.
* Aktivní účet vždy přispějí přihlašovací údaje, včetně **userPrincipalName** a **sourceAnchor**.
* Deaktivovaného účtu přispějí userPrincipalName a sourceAnchor, pokud je propojená poštovní schránka, pokud neexistuje žádné aktivní účet, která se má najít.
* Účet s poštovní schránku propojenou se nikdy použije pro userPrincipalName a sourceAnchor. Předpokládá se, že bude aktivní účet později nalezeno.
* Objekt kontaktní může být zřízená Azure AD jako kontakt nebo jako uživatel. Nevíte skutečně dokud všech doménových struktur služby Active Directory zdroj byly zpracovány.

## <a name="contacts"></a>Kontakty
S kontakty představující uživatele v jiné doménové struktuře je běžné po fúze a akvizice kde je řešení GALSync přemostění dvou nebo více doménovými strukturami systému Exchange. Objekt kontaktu vždycky připojuje z prostoru konektoru do úložiště metaverse pomocí atributu e-mailu. Pokud již existuje objekt kontaktní nebo objektu uživatele se stejnou adresou e-mailu, objekty jsou propojeny. To je nakonfigurovaný v pravidle **v ze služby Active Directory – obraťte se na připojení**. Je také na pravidlo s názvem **v ze služby Active Directory – běžné kontaktujte** s tok atributů pro atribut úložiště metaverse **sourceObjectType** s konstanta **kontaktujte**. Toto pravidlo má velmi nízkou prioritu, pokud uživatelských objektů je připojený k stejného objektu úložiště metaverse, bude toto pravidlo **v ze služby Active Directory – běžné uživatele** přispějí hodnotu uživatele na tento atribut. S tímto pravidlem bude mít tento atribut hodnotu kontaktu, pokud žádný uživatel připojil a hodnota uživatele Pokud byl nalezen nejméně jeden uživatel.

Pro zřizování objekt do služby Azure AD, odchozí pravidlo **Out aad – obraťte se na připojení** vytvoří objekt kontaktu, pokud atribut úložiště metaverse **sourceObjectType** je nastaven na **obraťte se na**. Pokud tento atribut je nastaven na **uživatele**, pak pravidlo **Out aad – připojení uživatele k** vytvoří objekt uživatele.
Je možné, že objekt povýší z obraťte se na uživatele při další zdroje Active Directory byly naimportovány a synchronizovány.

Například v topologii GALSync jsme najdete kontaktní objekty pro všechny uživatele v druhé doménové struktury při jsme importu první doménovou strukturu. To bude dvoufázové instalace nové kontaktní objekty v konektoru AAD. Když jsme později importovat a synchronizovat druhé doménové struktury, jsme se najít skutečné uživatele a připojte je k stávající objekty úložiště metaverse. Potom jsme se odstranit objekt kontaktu v AAD a místo toho vytvořte nový objekt uživatele.

Pokud máte topologii, kde jsou uživatelé vyjádřené kontakty, ujistěte se, zda že jste vybrali pro vyhledání uživatelů na atribut mail v instalační příručce. Pokud vyberete jinou možnost, bude mít závislé konfigurace aplikace pořadí. Kontaktujte objekty se vždy připojí na atribut mail, ale uživatelské objekty se připojí pouze na atribut mail, pokud byla tato možnost vybrána v v instalační příručce. Může pak skončili s dva různé objekty v úložišti metaverse s stejný atribut mail Pokud kontaktní objekt byl importován před objektu user. Při exportu do služby Azure AD bude vyvolána k chybě. Toto chování je záměrné a může naznačovat chybná data nebo že topologii nebyl správně identifikovány během instalace.

## <a name="disabled-accounts"></a>Zakázané účty
Zakázané účty jsou také synchronizovány do Azure AD. Zakázané účty jsou společné pro představují prostředky v systému Exchange, například konferenční místnosti. Jedinou výjimkou je uživatelé s poštovní schránku propojenou; Jak jsme uvedli tyto se nikdy zřídit účet do služby Azure AD.

Za předpokladu je, že pokud je účet zakázaný uživatel nalezen, pak jsme nebude najít jiný účet služby active později a objekt se zřizuje do služby Azure AD s userPrincipalName a sourceAnchor nalezen. V případě, že jiný aktivní účet se připojí ke stejnému objektu úložiště metaverse, bude použita jeho userPrincipalName a sourceAnchor.

## <a name="changing-sourceanchor"></a>Změna sourceAnchor
Pokud objekt byly exportovány do služby Azure AD a změnit sourceAnchor už není povoleno. Když byl objekt exportovat atribut úložiště metaverse **cloudSourceAnchor** nastavena **sourceAnchor** hodnota přijata službou Azure AD. Pokud **sourceAnchor** mění a neshodují **cloudSourceAnchor**, pravidlo **Out aad – připojení uživatele k** vyvolá výjimku chyba **došlo ke změně atribut sourceAnchor**. V takovém případě konfigurace nebo data musí být opraveny tak stejné sourceAnchor je k dispozici v úložišti metaverse znovu před objekt lze znovu synchronizovat.

## <a name="additional-resources"></a>Další zdroje
* [Azure AD Connect Sync: Možnosti přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

