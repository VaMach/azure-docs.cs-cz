---
title: "Azure AD Domain Services: Povolení synchronizace hesel | Dokumentace Microsoftu"
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
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: e4b1907b95576468654703c843a5f6e06846814b
ms.lasthandoff: 03/10/2017


---
# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Povolení synchronizace hesel do služby Azure AD Domain Services
V předchozích úlohách jste pro svého tenanta Azure AD povolili službu Azure AD Domain Service. Dalším krokem je povolení synchronizace hodnot hash přihlašovacích údajů požadovaných pro ověřování protokolů NTLM a Kerberos do služby Azure AD Domain Services. Jakmile je synchronizace přihlašovacích údajů nastavená, uživatelé se do spravované domény mohou přihlásit pomocí podnikových přihlašovacích údajů.

Potřebný postup se liší podle toho, jestli má vaše organizace výhradně cloudového tenanta Azure AD, nebo jestli má nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect.

<br>

> [!div class="op_single_selector"]
> * [Výhradně cloudový tenant Azure AD](active-directory-ds-getting-started-password-sync.md)
> * [Synchronizovaný tenant Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Úloha 5: Povolení synchronizace hesel do služby AAD Domain Services u výhradně cloudového tananta Azure AD
Azure AD Domain Services k ověřování uživatelů na spravované doméně vyžaduje hodnoty hash přihlašovacích údajů ve formátu vhodném pro ověřování protokolů NTLM a Kerberos. Pokud u svého tenanta službu AAD Domain Services nepovolíte, Azure AD nebude generovat ani ukládat hodnoty hash přihlašovacích údajů ve formátu požadovaném pro ověřování protokolů NTLM a Kerberos. Ze zřejmých bezpečnostních důvodů Azure AD také neukládá přihlašovací údaje jako nešifrovaný text. Proto v Azure AD neexistuje způsob, jak vygenerovat tyto hodnoty hash přihlašovacích údajů protokolů NTLM nebo Kerberos na základě stávajících přihlašovacích údajů uživatelů.

> [!NOTE]
> Pokud má vaše organizace výhradně cloudového tenanta Azure AD, uživatelé, kteří potřebují používat službu Azure AD Domain Services, si musí změnit heslo.
>
>

Tento proces změny hesla způsobí, že služba Azure AD vygeneruje hodnoty hash přihlašovacích údajů vyžadované službou Azure AD Domain Services pro ověřování pomocí protokolu Kerberos a NTLM. Můžete buď ukončit platnost hesel všech uživatelů v tenantovi, kteří potřebují používat službu Azure AD Domain Services, nebo dát těmto uživatelům pokyny ke změně hesla.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Povolení generování hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos u výhradně cloudového tenanta Azure AD
Tyto pokyny je nutné poskytnout koncovým uživatelům, aby si mohli změnit hesla:

1. Přejděte na stránku přístupového panelu služby Azure AD své organizace na webu [http://myapps.microsoft.com](http://myapps.microsoft.com).
2. Vyberte na této stránce kartu **profilu**.
3. Na této stránce klikněte na dlaždici **Změnit heslo**.

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Pokud se na stránce přístupového panelu možnost **Změnit heslo** nezobrazí, ujistěte se, že je ve vaší organizaci nakonfigurovaná [správa hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md).
   >
   >
4. Na stránce **Změnit heslo** zadejte stávající (staré) heslo, poté zadejte nové heslo a potvrďte jej. Klikněte na **Odeslat**.

    ![Vytvoření virtuální sítě pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Jakmile si změníte heslo, nové heslo bude zanedlouho použitelné ve službě Azure AD Domain Services. Po několika minutách (obvykle přibližně 20 minut) se můžete přihlašovat k počítačům připojeným k spravované doméně pomocí nově změněného hesla.

<br>

## <a name="related-content"></a>Související obsah
* [Postup aktualizace vlastního hesla](../active-directory/active-directory-passwords-update-your-own-password.md#how-to-reset-your-password)
* [Začínáme se správou hesel v Azure AD](../active-directory/active-directory-passwords-getting-started.md).
* [Povolení synchronizace hesel do služby AAD Domain Services u synchronizovaného tenanta Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)
* [Připojení virtuálního počítače s Windows k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)
* [Připojení virtuálního počítače se systémem Red Hat Enterprise Linux k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

