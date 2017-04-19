---
title: "Azure Active Directory Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
description: "Začínáme se službou Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: c300ba45cd530e5a606786aa7b2b254c2ed32fcd
ms.openlocfilehash: 51e4665b6372859021237144a0bf7f6763a2638b
ms.lasthandoff: 04/14/2017


---
# <a name="enable-password-synchronization-with-azure-active-directory-domain-services"></a>Povolení synchronizace hesel se službou Azure Active Directory Domain Services
V předchozích úlohách jste povolili službu Azure Active Directory Domain Services (AD DS) pro klienta služby Azure Active Directory (Azure AD). Dalším krokem je povolení synchronizace hodnot hash přihlašovacích údajů, které se vyžadují pro ověřování pomocí protokolů NT LAN Manager (NTLM) a Kerberos při synchronizaci se službou Azure Active Directory Domain Services. Po nastavení synchronizace přihlašovacích údajů se uživatelé mohou přihlásit ke spravované doméně s použitím podnikových přihlašovacích údajů.

Postupy se liší podle toho, jestli má vaše organizace výhradně cloudového tenanta Azure AD, nebo jestli má nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect.

> [!div class="op_single_selector"]
> * [Výhradně cloudový tenant Azure AD](active-directory-ds-getting-started-password-sync.md)
> * [Synchronizovaný tenant Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

## <a name="task-5-enable-password-synchronization-with-azure-active-directory-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Úloha 5: Povolení synchronizace hesel se službou Azure Active Directory Domain Services u výhradně cloudového tenanta Azure AD
K ověřování uživatelů ve spravované doméně služba Azure Active Directory Domain Services potřebuje hodnoty hash přihlašovacích údajů ve formátu vhodném pro ověřování pomocí protokolů NTLM a Kerberos. Pokud u svého tenanta službu Azure Active Directory Domain Services nepovolíte, Azure AD nebude generovat ani ukládat hodnoty hash přihlašovacích údajů ve formátu požadovaném pro ověřování pomocí protokolů NTLM a Kerberos. Ze zřejmých bezpečnostních důvodů Azure AD také neukládá přihlašovací údaje jako nešifrovaný text. Proto v Azure AD neexistuje způsob, jak vygenerovat tyto hodnoty hash přihlašovacích údajů protokolů NTLM nebo Kerberos na základě stávajících přihlašovacích údajů uživatelů.

> [!NOTE]
> Pokud má vaše organizace výhradně cloudového tenanta Azure AD, uživatelé, kteří potřebují používat službu Azure Active Directory Domain Services, si musí změnit heslo.
>
>

Tento proces změny hesla způsobí, že služba Azure AD vygeneruje hodnoty hash přihlašovacích údajů vyžadované službou Azure Active Directory Domain Services pro ověřování pomocí protokolů Kerberos a NTLM. Můžete buď ukončit platnost hesel všech uživatelů v tenantovi, kteří potřebují používat službu Azure Active Directory Domain Services, nebo jim dát pokyny ke změně hesel.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Povolení generování hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos u výhradně cloudového tenanta Azure AD
Tyto pokyny je nutné poskytnout uživatelům, aby si mohli změnit hesla:

1. Přejděte na stránku s [přístupovým panelem Azure AD](http://myapps.microsoft.com) své organizace.
2. V okně s přístupovým panelem vyberte kartu **Profil**.
3. Klikněte na dlaždici **Změnit heslo**.

    ![Dlaždice „Změnit heslo“ na přístupovém panelu Azure AD](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Pokud se v okně s přístupovým panelem možnost **Změnit heslo** nezobrazí, ujistěte se, že je ve vaší organizaci nakonfigurovaná [správa hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. Na stránce **Změnit heslo** zadejte stávající (staré) heslo, poté zadejte nové heslo a potvrďte je.

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klikněte na **Odeslat**.

Několik minut poté, co si změníte heslo, je možné používat ve službě Azure Active Directory Domain Services nové heslo. Po několika dalších minutách (obvykle přibližně 20 minut) se můžete přihlašovat k počítačům připojeným k spravované doméně s použitím nově změněného hesla.

## <a name="next-steps"></a>Další kroky
* [Postup aktualizace vlastního hesla](../active-directory/active-directory-passwords-update-your-own-password.md#reset-my-password)
* [Začínáme se správou hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md)
* [Povolení synchronizace hesel do služby Azure Active Directory Domain Services u synchronizovaného tenanta Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Správa spravované domény služby Azure Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Připojení virtuálního počítače s Windows k spravované doméně služby Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Připojení virtuálního počítače se systémem Red Hat Enterprise Linux k spravované doméně služby Azure Active Directory Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

