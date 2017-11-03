---
title: "Předplatné služby Azure pomocí klienta služby Office 365 | Microsoft Docs"
description: "Informace o postupu přidání adresář služby Office 365 (klient) k předplatnému Azure."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/13/2017
ms.author: cjiang
ms.openlocfilehash: e6300932d044ec9a4f88eb5bd5977220ed11d513
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="link-an-office-365-tenant-to-an-azure-subscription"></a>Klient služby Office 365 propojit předplatné Azure
Propojte samostatné předplatných Azure a Office 365, máte přístup klienta Office 365 z vašeho předplatného Azure. Odkaz vašich předplatných, přihlaste se k Azure pomocí účtu správce služby Azure, přidejte adresář a přidejte Office 365 pracovní nebo školní účty pro klienta služby Azure Active Directory.

**Chcete přesunout existující předplatné Office 365 pracovního nebo školního účtu?** Pokud jste zaregistrovali do služby Azure pomocí osobního účtu Microsoft a chcete používat ji nebo Přihlaste se pomocí účtu Office 365, důrazně doporučujeme přenosu předplatného. V tématu [Azure přenos vlastnictví předplatného na jiný účet](billing-subscription-transfer.md). 

**Chcete se přihlásit k Azure pomocí Office 365?** V tématu [registraci do Azure pomocí účtu Office 365](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Než začnete
* Musíte mít pověření správce služby předplatného Azure. Účty správce společné nelze provést některé kroky v tomto článku. Chcete-li změnit správce služby, [postup přidání nebo změna role Správce služby Azure](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Musíte mít pověření pro globálního správce klienta Office 365.
* E-mailovou adresu správce služby nesmí být v klientovi Office 365.
* E-mailovou adresu správce služeb nesmí odpovídat všechny globální správce klienta Office 365.
* Pokud používáte e-mailovou adresu, která je účet Microsoft a účet organizace, dočasně změňte na služby správce vašeho předplatného Azure a použít jiný účet Microsoft. Můžete vytvořit účet Microsoft v [stránky registraci účtu Microsoft](https://signup.live.com/).

## <a name="link-office-365-tenant-to-azure-subscription"></a>Klienta Office 365 odkaz pro předplatné Azure
Chcete-li přidružit klienta Office 365 pro předplatné Azure, postupujte takto:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>Krok 1: Přidání klienta Office 365 k předplatnému Azure

1. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.

    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
2. V levém podokně vyberte **služby ACTIVE DIRECTORY**. Byste neměli vidět klienta Office 365. Pokud se zobrazí, přejděte k [krok 2: Změňte adresář přidružený k předplatnému Azure](#Step2).
   
   ![Snímek obrazovky Active Directory položka](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)
3. Vyberte **nový** > **DIRECTORY** > **vlastní vytvořit**.
   
    ![Vytvořit vlastní snímek obrazovky nástroje Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
4. Na **adresáře přidat** v části **DIRECTORY**, vyberte **použít existující adresář**. Potom vyberte **nyní mě můžete odhlásit**a vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Snímek obrazovky "Použít existující adresář"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
5. Poté, co jste se odhlásili, přihlaste se pomocí přihlašovacích údajů globálního správce vašeho tenanta Office 365.
   
    ![Přihlášení globálního správce Office 365 – snímek obrazovky](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
6. Vyberte **pokračovat**.
   
    ![Snímek obrazovky ověření](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
7. Vyberte **Odhlásit**.
   
    ![Snímek obrazovky odhlášení](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
8. Přihlaste se k [portál Azure classic](https://manage.windowsazure.com/) s přihlašovacími údaji správce služby.
   
    ![Snímek obrazovky Azure přihlásit](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
9. Měli byste vidět vašeho tenanta Office 365 v řídicím panelu.
   
    ![Snímek obrazovky řídicí panel](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Krok 2: Změňte adresář přidružený k předplatnému Azure
   
1. Vyberte **nastavení**.
   
    ![Ikona snímek Azure classic nastavení portálu](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
2. Vyberte předplatné Azure a pak vyberte **upravit adresář**.

    ![Snímek obrazovky Azure předplatné upravit adresář](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
3. Vyberte **Další** ![další ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Snímek obrazovky "Změna přidružené adresáře"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
4. Zkontrolujte ovlivněných účty. Všechny spolusprávci a [řízení přístupu na základě Role (RBAC)](../active-directory/role-based-access-control-configure.md) uživatelé s přiřazenou přístupem v existující skupiny prostředků se odeberou. Uvádí, odebrání spolusprávci pouze upozornění, které se zobrazí.
      
    ![Snímek obrazovky, který zobrazuje účty spolusprávcem odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Snímek obrazovky zobrazující uživatelský účet příklad odeberou.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
5. Vyberte **Complete** ![dokončení ikonu](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-admins-to-the-azure-subscription"></a>Krok 3: Přidání vaší organizace účtů Office 365 jako správci k předplatnému Azure
   
Přidání správce k předplatnému Azure naleznete v tématu [přidání nebo změna role Správce služby Azure, které spravují předplatné nebo služby](billing-add-change-azure-subscription-administrator.md)

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) získat rychle vyřešit problém.

