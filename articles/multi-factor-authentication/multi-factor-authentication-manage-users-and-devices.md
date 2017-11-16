---
title: "Správci spravovat uživatele a zařízení – Azure MFA | Microsoft Docs"
description: "Popisuje postup změny nastavení uživatele jako vynucení uživatelům znovu provést proces výš."
documentationcenter: 
services: multi-factor-authentication
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: aac3b922-7cc1-428c-9044-273579aa7b5a
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.openlocfilehash: cec582462d6f9b3ae7fbee0dd2a3ab66d7b0d1da
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>Spravovat uživatelská nastavení s Azure Multi-Factor Authentication v cloudu
Jako správce můžete spravovat následující nastavení uživatelů a zařízení:

* Vyžadovat uživatelé znovu zadali způsoby kontaktování
* Odstranit hesla aplikací
* Na všechny důvěryhodné zařízení vyžadovat vícefaktorové ověřování 

## <a name="require-users-to-provide-contact-methods-again"></a>Vyžadovat uživatelé znovu zadali způsoby kontaktování
Toto nastavení donutí uživatele k dokončení procesu registrace znovu. Neprohlížečové aplikace i nadále fungovat, pokud má uživatel hesla aplikací pro ně.  Hesla uživatelů aplikace můžete odstranit také výběrem **odstranit všechna stávající hesla aplikací vytvořená vybranými uživateli**.

### <a name="how-to-require-users-to-provide-contact-methods-again"></a>Jak budou muset uživatelé znovu zadali způsoby kontaktování
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication. 
4. Zaškrtněte políčko vedle uživatelů, které chcete spravovat. Zobrazí seznam možností rychlý krok na pravé straně. 
5. Vyberte **spravovat uživatelská nastavení**.
6. Zaškrtněte políčko pro **vyžadují Vybraní uživatelé znovu zadali způsoby kontaktování**.
   ![Zadali způsoby kontaktování](./media/multi-factor-authentication-manage-users-and-devices/reproofup.png)
7. Klikněte na **Uložit**.
8. Klikněte na tlačítko **zavřete**.

## <a name="delete-users-existing-app-passwords"></a>Odstranit uživatele existující hesla aplikací
Tato nastavení odstraní všechna hesla aplikací, která uživatel vytvoří. Neprohlížečové aplikace, které byly přidruženy tato hesla aplikace přestat pracovat teprve po vytvoření nové heslo aplikace.

### <a name="how-to-delete-users-existing-app-passwords"></a>Postup odstranění uživatelé existující hesla aplikací
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication. 
6. Zaškrtněte políčko vedle uživatelů, které chcete spravovat. Zobrazí seznam možností rychlý krok na pravé straně. 
7. Vyberte **spravovat uživatelská nastavení**.
8. Zaškrtněte políčko pro **odstranit všechna stávající hesla aplikací vytvořená vybranými uživateli**.
   ![Odstranit hesla aplikací](./media/multi-factor-authentication-manage-users-and-devices/deleteapppasswords.png)
9. Klikněte na **Uložit**.
10. Klikněte na tlačítko **zavřete**.

## <a name="restore-mfa-on-all-remembered-devices-for-a-user"></a>Obnovit vícefaktorové ověřování u všech zapamatovaných zařízení pro uživatele
Mezi konfigurovatelné funkce služby Azure Multi-Factor Authentication je udělení uživatelům možnost označte zařízení jako důvěryhodný. Další informace najdete v tématu [nastavení konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md#remember-multi-factor-authentication-for-devices-that-users-trust).

Uživatelé mohou vyjádření výslovného nesouhlasu dvoustupňové ověřování pro konfigurovat počet dní na svých zařízeních regulární. Pokud dojde k ohrožení bezpečnosti účtu nebo o důvěryhodné zařízení dojde ke ztrátě, musíte být schopni odebrat důvěryhodný stav a vyžadovat dvoustupňové ověření znovu.

**Obnovení služby Multi-Factor authentication u všech zapamatovaných zařízení** nastavení znamená, že uživatel se bude postiženy provést dvoustupňové ověření při příštím přihlášení, bez ohledu na to, jestli se rozhodli označit jejich zařízení jako důvěryhodný. 

### <a name="how-to-restore-mfa-on-all-suspended-devices-for-a-user"></a>Postup obnovení MFA na všechny pozastavené zařízení pro uživatele
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na levé straně vyberte **Azure Active Directory** > **uživatelů a skupin** > **všichni uživatelé**.
3. Vyberte **služby Multi-Factor Authentication**. Otevře se stránka služby Multi-Factor authentication. 
6. Zaškrtněte políčko vedle uživatelů, které chcete spravovat. Zobrazí seznam možností rychlý krok na pravé straně. 
7. Vyberte **spravovat uživatelská nastavení**.
8. Zaškrtněte políčko pro **obnovení služby Multi-Factor authentication u všech zapamatovaných zařízení**
   ![odstranit hesla aplikací](./media/multi-factor-authentication-manage-users-and-devices/rememberdevices.png)
9. Klikněte na **Uložit**.
10. Klikněte na tlačítko **zavřete**.

## <a name="next-steps"></a>Další kroky

- Další informace o tom, jak [nastavení konfigurace Azure Multi-Factor Authentication](multi-factor-authentication-whats-next.md)

- Pokud budou uživatelé potřebovat pomoc, je směrem nasměrujte [uživatelská příručka pro dvoustupňové ověření](./end-user/multi-factor-authentication-end-user.md)
