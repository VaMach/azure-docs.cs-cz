<properties
   pageTitle="Začínáme s aplikací Azure AD Privileged Identity Management | Microsoft Azure"
   description="Přečtěte si, jak spravovat privilegované identity pomocí aplikace Azure Active Directory Privileged Identity Management na portálu Azure."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/29/2016"
   ms.author="kgremban"/>

# Začínáme s aplikací Azure AD Privileged Identity Management


Aplikace Azure Active Directory (AD) Privileged Identity Management vám umožní spravovat, řídit a monitorovat privilegované identity a přístup k prostředkům v Azure AD i v jiných online službách Microsoft, jako jsou Office 365 nebo Microsoft Intune.  

Tento článek vysvětluje, jak přidat aplikaci Azure AD PIM do řídicího panelu portálu Azure.

## Přidání aplikace Privileged Identity Management

Před použitím aplikace Azure AD Privileged Identity Management musíte tuto aplikaci přidat do řídicího panelu portálu Azure.

1. Přihlaste se k [portálu Azure](https://portal.azure.com/) jako globální správce adresáře.
2. Pokud má vaše organizace více než jeden adresář, klikněte na své uživatelské jméno v pravém horním rohu na portálu Azure a vyberte adresář, kde budete používat PIM.
3. Vyberte **Nové** > **Zabezpečení + Identita** > **Azure AD Privileged Identity Management**.

    ![Povolení PIM na portálu][1]

4. Zaškrtněte **Připnout na řídicí panel** a potom klikněte na **Vytvořit**. Aplikace Privileged Identity Management se otevře.


Pokud jste první, kdo bude ve vašem adresáři používat aplikaci Azure AD Privileged Identity Management, [průvodce zabezpečením](active-directory-privileged-identity-management-security-wizard.md) vás provede počátečním přiřazením. Automaticky se pak stanete prvním **správcem zabezpečení** a **správcem privilegovaných rolí** adresáře. Pouze správce privilegovaných rolí má přístup k této aplikaci za účelem správy přístupu pro další správce.  

Pokud jste byli přiřazeni k jedné nebo více rolím, budete mít k dispozici možnost **Aktivovat moje role**. Pokud jste správcem privilegovaných rolí, zobrazí se také možnost **Spravovat privilegované role**.  


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Další kroky

[Přehled aplikace Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md) obsahuje podrobné informace o tom, jak můžete spravovat přístup pro správu ve vaší organizaci.

[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png



<!--HONumber=Aug16_HO4-->


