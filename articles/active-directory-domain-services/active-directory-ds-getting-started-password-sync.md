<properties
    pageTitle="Azure AD Domain Services: Povolení synchronizace hesel | Microsoft Azure"
    description="Začínáme se službou Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="maheshu"/>

# Azure AD Domain Services *(Preview)* – Povolení synchronizace hesel do služby Azure AD Domain Services

## Úloha 5: Povolení synchronizace hesel do služby AAD Domain Services u výhradně cloudového adresáře služby Azure AD
Jakmile jste u svého klienta služby Azure AD povolili službu Azure AD Domain Services, dalším krokem je povolit synchronizaci přihlašovacích údajů do služby Azure AD Domain Services. To uživatelům umožní přihlásit se k spravované doméně pomocí podnikových přihlašovacích údajů.

Potřebný postup se liší podle toho, jestli má vaše organizace výhradně cloudový adresář Azure AD, nebo jestli má nastavenou synchronizaci s místním adresářem pomocí služby Azure AD Connect.

<br>

> [AZURE.SELECTOR]
- [Výhradně cloudový adresář Azure AD](active-directory-ds-getting-started-password-sync.md)
- [Synchronizovaný adresář Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Povolení generování hodnot hash přihlašovacích údajů protokolů NTLM a Kerberos u výhradně cloudového adresáře služby Azure AD
Pokud má vaše organizace výhradně cloudový adresář služby Azure AD, uživatelé, kteří potřebují používat službu Azure AD Domain Services, si budou muset změnit heslo. Tento proces změny hesla způsobí, že služba Azure AD vygeneruje hodnoty hash přihlašovacích údajů vyžadované službou Azure AD Domain Services pro ověřování pomocí protokolu Kerberos a NTLM. Můžete buď ukončit platnost hesel všech uživatelů v klientovi, kteří potřebují používat službu Azure AD Domain Services, nebo dát těmto uživatelům pokyny ke změně hesla.

Toto jsou pokyny, které je nutné dát koncovým uživatelům, aby si změnili hesla:

1. Přejděte na stránku přístupového panelu Azure AD své organizace. Ta je obvykle dostupná na adrese [http://myapps.microsoft.com](http://myapps.microsoft.com).

2. Vyberte na této stránce kartu **profilu**.

3. Kliknutím na dlaždici **Změnit heslo** na této stránce zahajte změnu hesla.

    ![Vytvořte virtuální síť pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password.png)

4. Otevře se stránka **změny hesla**. Uživatelé můžou zadat své stávající (staré) heslo a pokračovat ve změně hesla.

    ![Vytvořte virtuální síť pro službu Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Jakmile si uživatelé změní heslo, nové heslo bude zanedlouho použitelné ve službě Azure AD Domain Services. Po několika minutách se uživatelé mohou přihlašovat k počítačům připojeným k spravované doméně pomocí nově změněného hesla.


<br>

## Související obsah

- [Povolení synchronizace hesel do služby AAD Domain Services u synchronizovaného adresáře služby Azure AD](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Správa spravované domény služby Azure AD Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Připojení virtuálního počítače s Windows k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Připojení virtuálního počítače se systémem Red Hat Enterprise Linux k spravované doméně služby Azure AD Domain Services](active-directory-ds-admin-guide-join-rhel-linux-vm.md)



<!--HONumber=Jun16_HO2-->


