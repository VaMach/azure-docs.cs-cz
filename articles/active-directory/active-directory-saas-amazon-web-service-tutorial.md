---
title: 'Kurz: Azure Active Directory integrace s Amazon Web Services (AWS) | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 0fb9c8f428368271b548e3f174726fa01ea910c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Kurz: Azure Active Directory integrace s Amazon Web Services (AWS)

V tomto kurzu zjistěte, jak integrovat Amazon Web Services (AWS) s Azure Active Directory (Azure AD).

Integrace Amazon Web Services (AWS) s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Amazon Web Services (AWS)
- Můžete povolit uživatelům, aby automaticky získat přihlášeného k Amazon Web Services (AWS) (jednotné přihlášení) s jejich účty Azure AD
- Můžete spravovat vaše účty v jednom centrálním místě - portálu Azure

Pokud chcete vědět, další informace o integraci aplikací SaaS v Azure AD, najdete v části [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD pomocí Amazon Web Services (AWS), potřebujete následující položky:

- Předplatné služby Azure AD
- Amazon Web Services (AWS) jednotného přihlašování povolené předplatné

> [!NOTE]
> K testování kroky v tomto kurzu, nedoporučujeme používání provozním prostředí.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle těchto doporučení:

- Provozním prostředí byste neměli používat, pokud je to nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete získat zkušební jeden měsíc [zde](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Amazon Web Services (AWS) z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Přidání Amazon Web Services (AWS) z Galerie
Při konfiguraci integrace služby Amazon Web Services (AWS) do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS Amazon Web Services (AWS) z galerie.

**Pokud chcete přidat Amazon Web Services (AWS) z galerie, proveďte následující kroky:**

1. V  **[portálu Azure](https://portal.azure.com)**, v levém navigačním panelu klikněte na tlačítko **Azure Active Directory** ikonu. 

    ![Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
3. Klikněte na tlačítko **přidat** tlačítko horní dialogové okno.

    ![Aplikace][3]

4. Do vyhledávacího pole zadejte **Amazon Web Services (AWS)**.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Na panelu výsledků vyberte **Amazon Web Services (AWS)**a potom klikněte na **přidat** tlačítko Přidat aplikaci.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS) podle testovacího uživatele názvem "Britta Simon".

Azure AD pro jednotné přihlašování pro práci, musí vědět, co uživatel protějškem v Amazon Web Services (AWS) je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah propojení mezi uživatele Azure AD a související uživatelské Amazon Web Services (AWS).

Tento vztah propojení se navazuje se hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** Amazon Web Services (AWS).

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), je třeba dokončit následující stavební bloky:

1. **[Konfigurace Azure AD jednotné přihlašování](#configuring-azure-ad-single-sign-on)**  – Pokud chcete povolit uživatelům tuto funkci používat.
2. **[Vytváření testovacího uživatele Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytváření testovacího uživatele Amazon Web Services](#creating-an-amazon-web-services-test-user)**  – Pokud chcete mít v Amazon Web Services (AWS), propojené služby Azure AD reprezentace jí protějšek Britta Simon.
4. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotné přihlašování.
5. **[Testování jednotné přihlašování](#testing-single-sign-on)**  – Pokud chcete ověřit, zda je funkční konfigurace.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Amazon Web Services (AWS).

**Pokud chcete konfigurovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující kroky:**

1. Na portálu Azure na **Amazon Web Services (AWS)** stránky integrace aplikací, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurovat jednotné přihlašování][4]

2. Na **jednotného přihlašování** dialogové okno, jako **režimu** vyberte **na základě SAML přihlašování** umožňující jednotného přihlašování.
 
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Na **Amazon Web Services (AWS) domény a adresy URL** části uživatel nemusí provádět žádné kroky, protože aplikace je už předem integrováno s Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Na **SAML podpisový certifikát** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.
    
    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. Aplikace Software Amazon Web Services (AWS) očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z "**uživatelské atributy**" části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad pro tento.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. V **uživatelské atributy** části na **jednotného přihlašování** dialogové okno, nakonfigurujte atribut tokenu SAML, jak je znázorněno na obrázku výše a proveďte následující kroky:
    
    | Název atributu  | Hodnota atributu | obor názvů |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | User.userPrincipalName | https://AWS.Amazon.com/SAML/Attributes |
    | Role            | User.assignedroles |  https://AWS.Amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Musíte nakonfigurovat zřizování uživatelů ve službě Azure AD se načíst všechny role z konzoly AWS. Naleznete v následujících zřizování kroků.

    a. Klikněte na tlačítko **přidat atribut** otevřete **přidat atribut** dialogové okno.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. V **název** textovému poli, zadejte název atributu, který je uvedený na příslušném řádku.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku. Přidejte hodnotu Namespace jako ve výše uvedených.
    
    d. Klikněte na tlačítko **OK**.

7. Klikněte na tlačítko **Uložit** tlačítko pro uložení nastavení v Azure.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. V okně jiný prohlížeč přihlašování k webu společnosti Amazon Web Services (AWS) jako správce.

9. Klikněte na tlačítko **konzoly domovské**.
   
    ![Konfigurovat jednotné přihlašování][11]

10. Klikněte na tlačítko **IAM** z **zabezpečení, Identity a dodržování předpisů** služby.
   
    ![Konfigurovat jednotné přihlašování][12]

11. Klikněte na tlačítko **zprostředkovatelů Identity**a potom klikněte na **vytvoření zprostředkovatele**.
   
    ![Konfigurovat jednotné přihlašování][13]

12. Na **konfigurace zprostředkovatele** dialogové okno stránky, proveďte následující kroky:
   
    ![Konfigurovat jednotné přihlašování][14]
 
    a. Jako **typ zprostředkovatele**, vyberte **SAML**.

    b. V **název zprostředkovatele** textovému poli, zadejte název zprostředkovatele (např: *službou WAAD*).

    c. Chcete-li nahrát soubor stažený metadata, klikněte na tlačítko **zvolit soubor**.

    d. Klikněte na tlačítko **dalším krokem**.

13. Na **ověřte informace o poskytovateli** dialogové okno stránky, klikněte na tlačítko **vytvořit**. 
    
    ![Konfigurovat jednotné přihlašování][15]

14. Klikněte na tlačítko **role**a potom klikněte na **vytvořit novou roli**. 
    
    ![Konfigurovat jednotné přihlašování][16]

15. Na **nastavit název Role** dialogové okno, proveďte následující kroky: 
    
    ![Konfigurovat jednotné přihlašování][17] 

    a. V **název Role** textovému poli, zadejte název role (např: *TestUser*). 

    b. Klikněte na tlačítko **dalším krokem**.

16. Na **vyberte typ Role** dialogové okno, proveďte následující kroky: 
    
    ![Konfigurovat jednotné přihlašování][18] 

    a. Vyberte **Role pro přístup poskytovatele Identity**. 

    b. V **Grant webové jednotné přihlašování (WebSSO) přístup k poskytovatelům SAML** klikněte na tlačítko **vyberte**.

17. Na **navázání vztahu důvěryhodnosti** dialogové okno, proveďte následující kroky:  
    
    ![Konfigurovat jednotné přihlašování][19] 

    a. Jako poskytovatel SAML, vyberte poskytovateli SAML, který jste vytvořili dříve (např: *službou WAAD*)
  
    b. Klikněte na tlačítko **dalším krokem**.

18. Na **ověřte důvěřovat Role** dialogové okno, klikněte na tlačítko **další krok**.
    
    ![Konfigurovat jednotné přihlašování][32]

19. Na **připojit zásady** dialogové okno, klikněte na tlačítko **další krok**.
    
    ![Konfigurovat jednotné přihlašování][33]

20. Na **zkontrolujte** dialogové okno, proveďte následující kroky:
    
    ![Konfigurovat jednotné přihlašování][34]
 
    a. Klikněte na tlačítko **vytvořit roli**.

    b. Vytvořit tolik role podle potřeby a jejich namapování na zprostředkovatele Identity.

21. Teď nakonfigurujte zřizování načíst všechny role z AWS uživatelů

    a. V konzole AWS přihlášení pomocí kořenového účtu

    b. V pravém horním rohu klikněte na své jméno a potom klikněte na **Moje zabezpečovací pověření** možnost. Tím se otevře na obrazovce jako upozornění. Klikněte na tlačítko **zabezpečovací pověření** tlačítko předat na obrazovce.
        
       ![Konfigurovat jednotné přihlašování][36]

       ![Konfigurovat jednotné přihlašování][37]

    c. V části přístupových klíčů klepněte **vytvořit nový přístupový klíč** tlačítko. To vygeneruje ID přístupu klíč a hodnotu tokenu.
    
       ![Konfigurovat jednotné přihlašování][38]

    d. Zkopírujte oba tyto hodnoty a také, tak, aby neztratili ho stáhnout.

    e. Na portálu Azure, na stránce integrace aplikace Amazon Web Services (AWS), klikněte na tlačítko **zřizování**.
        
       ![Konfigurovat jednotné přihlašování][35]

    f. Nastavit režim zřizování **automatické**
        
       ![Konfigurovat jednotné přihlašování][39]

    g. Nyní ve **clientsecret** a **tajný klíč tokenu** vložte odpovídající hodnoty, které jste zkopírovali z konzoly AWS.
    
    h. Můžete kliknout na **otestovat připojení** tlačítko Testovat připojení. Po úspěšné, můžete začít konektor zřizování.
       
       ![Konfigurovat jednotné přihlašování][40]

    i. Teď povolit stav zřizování na **na**. Tím se spustí načítání role z aplikace.

       ![Konfigurovat jednotné přihlašování][41]

    > [!NOTE]
    > Spuštění služby Azure AD zřizování každých po určité době synchronizaci tyto role z AWS. Měli byste vidět všechny zprostředkovatele Identity připojena AWS rolí do služby Azure AD a můžete je použít při přiřazování aplikace uživatelům nebo skupinám.

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Vytváření testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

![Vytvořit uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. V **portál Azure**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé** zobrazíte seznam uživatelů.
    
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. V horní části okna klikněte na tlačítko **přidat** otevřete **uživatele** dialogové okno.
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Na **uživatele** dialogové okno stránky, proveďte následující kroky:
 
    ![Vytváření testovacího uživatele Azure AD](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. V **název** textovému poli, typ **BrittaSimon**.

    b. V **uživatelské jméno** textovému poli, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit hesla** a poznamenejte si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Vytváření testovacího uživatele Amazon Web Services

Pokud chcete povolit uživatelům Azure AD přihlášení k Amazon Web Services (AWS), se musí zřízená do Amazon Web Services (AWS). V případě Amazon Web Services (AWS) zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, proveďte následující kroky:**

1. Přihlaste se k vaší **Amazon Web Services (AWS)** společnosti lokality jako správce.

2. Klikněte **konzoly domovské** ikonu. 
   
    ![Konfigurovat jednotné přihlašování][11]

3. Klikněte na tlačítko identita a správa přístupu. 
   
    ![Konfigurovat jednotné přihlašování][28]

4. Na řídicím panelu klikněte na tlačítko **uživatelé**a potom klikněte na **vytvořte nové uživatele**. 
   
    ![Konfigurovat jednotné přihlašování][29]

5. V dialogovém okně Vytvořit uživatele proveďte následující kroky: 
   
    ![Konfigurovat jednotné přihlašování][30]   
    
    a. V **zadejte uživatelská jména** textová pole, zadejte uživatelské jméno Brita Simon (userprincipalname) ve službě Azure AD.

    b. Klikněte na tlačítko **vytvořit.**
        
### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části povolíte Britta Simon používat Azure jednotné přihlašování tak, že udělíte přístup k Amazon Web Services (AWS).

![Přiřadit uživatele][200] 

**Pokud chcete přiřadit Britta Simon k Amazon Web Services (AWS), proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace, které** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Amazon Web Services (AWS)**.

    ![Konfigurovat jednotné přihlašování](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogové okno.

    ![Přiřadit uživatele][203]

5. Na **uživatelů a skupin** dialogovém okně, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogové okno.

7. Na **vybrat roli** , vyberte příslušnou roli pro uživatele. Tyto role jsou zobrazeny s názvem role a název zprostředkovatele identity. Tímto způsobem můžete snadno identifikovat tyto role z AWS.

8. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogové okno.
    
### <a name="testing-single-sign-on"></a>Testování jednotné přihlašování

V této části můžete vyzkoušet Azure AD jeden přihlašování konfiguraci pomocí přístupového panelu.

Když kliknete na dlaždici Amazon Web Services (AWS) na přístupovém panelu, jste měli získat automaticky přihlášení k aplikaci Amazon Web Services (AWS). 

## <a name="additional-resources"></a>Další zdroje

* [Seznam kurzů k integraci aplikací SaaS službou Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
