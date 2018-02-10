---
title: 'Kurz: Azure Active Directory integrace s Amazon Web Services (AWS) | Microsoft Docs'
description: "Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Amazon Web Services (AWS)."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/16/2017
ms.author: jeedes
ms.openlocfilehash: 0ff14365323d66a101e5847d7959045c3f20dea2
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Kurz: Azure Active Directory integrace s Amazon Web Services (AWS)

V tomto kurzu zjistěte, jak integrovat Amazon Web Services (AWS) s Azure Active Directory (Azure AD).

Integrace Amazon Web Services (AWS) s Azure AD poskytuje následující výhody:

- Můžete ovládat ve službě Azure AD, který má přístup k Amazon Web Services (AWS).
- Můžete povolit uživatelům automaticky získat přihlášení k Amazon Web Services (AWS) s jejich účty Azure AD.
- Můžete spravovat vaše účty v jednom centrálním místě – portál Azure.

Další informace o integraci aplikací SaaS v Azure AD najdete v tématu [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Požadavky

Ke konfiguraci integrace služby Azure AD pomocí Amazon Web Services (AWS), potřebujete následující položky:

- Předplatné služby Azure AD
- Amazon Web Services (AWS) jednotné přihlašování povolené předplatné

> [!NOTE]
> Nedoporučujeme používat produkčním prostředí pro testování kroky v tomto kurzu.

Chcete-li otestovat kroky v tomto kurzu, postupujte podle následujících doporučení:

- Nepoužívejte provozním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verze Azure AD, můžete [získat bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu můžete otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénáři uvedeném v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání Amazon Web Services (AWS) z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Přidat Amazon Web Services (AWS) z Galerie
Při konfiguraci integrace služby Amazon Web Services (AWS) do služby Azure AD, musíte přidat do seznamu spravovaných aplikací SaaS Amazon Web Services (AWS) z galerie.

**Pokud chcete přidat Amazon Web Services (AWS) z galerie, proveďte následující kroky:**

1. V  **[portál Azure](https://portal.azure.com)**, v levém navigačním podokně, vyberte **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte na **podnikové aplikace, které**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
3. Chcete-li přidat novou aplikaci, vyberte **novou aplikaci** tlačítko horní dialogové okno.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Amazon Web Services (AWS)**. Vyberte **Amazon Web Services (AWS)** z panelu výsledky a potom vyberte **přidat** tlačítko Přidat aplikaci.

    ![Amazon Web Services (AWS) v seznamu výsledků](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování Azure AD jednotné přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS) podle testovacího uživatele názvem "Britta Simon."

Azure AD pro jednotné přihlašování pro práci, musí vědět, kdo příslušného uživatele v Amazon Web Services (AWS) je pro uživatele ve službě Azure AD. Jinými slovy budete muset vytvořit propojení mezi uživatele Azure AD a související uživatelské Amazon Web Services (AWS).

K vytvoření odkazu, který je v Amazon Web Services (AWS), zadejte hodnotu **uživatelské jméno** stejnou hodnotu jako **uživatelské jméno** ve službě Azure AD. 

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující stavební bloky:

1. [Konfigurovat Azure AD jednotné přihlašování](#configure-azure-ad-single-sign-on) umožňující uživatelům používat tuto funkci.
2. [Vytvořit testovací uživatele Azure AD](#create-an-azure-ad-test-user) k testování Azure AD jednotné přihlašování s Britta Simon.
3. [Vytvořit testovací uživatele s Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user) tak, aby měl protějšek Britta Simon v Amazon Web Services (AWS), propojené služby Azure AD reprezentace daného uživatele.
4. [Přiřadit testovacího uživatele Azure AD](#assign-the-azure-ad-test-user) povolit Britta Simon používat Azure AD jednotné přihlašování.
5. [Test jednotného přihlašování](#test-single-sign-on) k ověření, že konfigurace funguje.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurovat Azure AD jednotné přihlašování

V této části můžete povolit Azure AD jednotného přihlašování na portálu Azure a nakonfigurovat jednotné přihlašování v aplikaci Amazon Web Services (AWS).

**Pokud chcete konfigurovat Azure AD jednotné přihlašování pomocí Amazon Web Services (AWS), proveďte následující kroky:**

1. Na portálu Azure na **Amazon Web Services (AWS)** stránky integrace aplikací, vyberte **jednotného přihlašování**.

    ![Konfigurace propojení přihlášení][4]

2. Pro povolení jednotného přihlašování, v **jednotného přihlašování** v dialogovém **režimu** rozevíracího seznamu vyberte **na základě SAML přihlašování**.
 
    ![Jediné přihlášení dialogové okno](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_samlbase.png)

3. V **Amazon Web Services (AWS) domény a adresy URL** části uživatel nemá žádné kroky provést, protože aplikace je už předem integrováno s Azure.

    ![Amazon Web Services (AWS) domény a adresy URL jednotné přihlašování informace](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

4. Softwarová aplikace Amazon Web Services (AWS) očekává SAML kontrolní výrazy ve specifickém formátu. Nakonfigurujte následující deklarace identity pro tuto aplikaci. Můžete spravovat hodnoty těchto atributů z **uživatelské atributy** části na stránce integrace aplikace. Následující snímek obrazovky ukazuje příklad:

    ![Nakonfigurujte jeden atribut přihlášení](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_attribute.png)  

5. V **uživatelské atributy** tématu **jednotného přihlašování** pole, nakonfigurujte atribut tokenu SAML, jak je vidět na předchozím obrázku a pak proveďte následující kroky:
    
    | Název atributu  | Hodnota atributu | Obor názvů |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Role            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Nakonfigurujte zřizování uživatelů ve službě Azure AD se načíst všechny role z konzole Amazon Web Services (AWS). Naleznete v následujících kroků zřizování.

    a. Chcete-li otevřít **přidat atribut** dialogové okno, vyberte **přidat atribut**.

    ![Nakonfigurujte jeden atribut přihlášení](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    ![Nakonfigurujte jeden atribut přihlášení](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    b. V **název** zadejte název atributu, který je uvedený na příslušném řádku.

    c. Z **hodnotu** seznamu, zadejte hodnotu atributu, který je uvedený na příslušném řádku.

    d. V **Namespace** zadejte hodnotu oboru názvů, které se zobrazí na příslušném řádku.
    
    d. Vyberte **Ok**.

6. V **SAML podpisový certifikát** vyberte **soubor XML s metadaty**. Uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

7. Vyberte **Uložit**.

    ![Konfigurovat jednotné přihlašování tlačítko Uložit](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. V okně jiný prohlížeč Přihlaste se k serveru vaší společnosti Amazon Web Services (AWS) jako správce.

9. Vyberte **konzoly domovské**.
   
    ![Konfigurace domovské přihlášení][11]

10. Vyberte **identita a správa přístupu**. 
   
    ![Konfiguraci identity přihlášení][12]

11. Vyberte **zprostředkovatelů Identity**. Potom vyberte **vytvoření zprostředkovatele**. 
   
    ![Konfigurace zprostředkovatele přihlášení][13]

12. V **konfigurace zprostředkovatele** dialogové okno pole, proveďte následující kroky: 
   
    ![Nakonfigurujte jeden n přihlašovací dialogové okno][14]
 
    a. Pro **typ zprostředkovatele**, vyberte **SAML**.

    b. V **název zprostředkovatele** zadejte název zprostředkovatele (například: *službou WAAD*).

    c. Nahrát váš stažené **soubor metadat** z portálu Azure vyberte **zvolit soubor**.

    d. Vyberte **dalším krokem**.

13. V **ověřte informace o poskytovateli** dialogové okno, vyberte **vytvořit**. 
    
    ![Konfigurace ověření přihlášení][15]

14. Vyberte **role**. Potom vyberte **vytvořit novou roli**. 
    
    ![Konfiguraci rolí přihlášení][16]

15. V **nastavit název Role** dialogové okno pole, proveďte následující kroky: 
    
    ![Konfigurace názvu přihlášení][17] 

    a. V **název Role** zadejte název role (například *TestUser*). 

    b. Vyberte **dalším krokem**.

16. V **vyberte typ Role** dialogové okno pole, proveďte následující kroky: 
    
    ![Nakonfigurujte typ role přihlášení][18] 

    a. Vyberte **Role pro přístup poskytovatele Identity**. 

    b. V **Grant webové jednotné přihlašování (WebSSO) přístup k poskytovatelům SAML** klikněte na tlačítko **vyberte**.

17. V **navázání vztahu důvěryhodnosti** dialogové okno pole, proveďte následující kroky:  
    
    ![Konfigurace důvěryhodnosti přihlášení][19] 

    a. Vyberte poskytovatele SAML jste předtím vytvořili (například: *službou WAAD*). 
  
    b. Vyberte **dalším krokem**.

18. V **ověřte důvěřovat Role** dialogové okno, vyberte **další krok**. 
    
    ![Konfigurovat vztah důvěryhodnosti Role přihlášení][32]

19. V **připojit zásady** dialogové okno, vyberte **další krok**.  
    
    ![Nakonfigurujte zásady přihlášení][33]

20. V **zkontrolujte** dialogové okno pole, proveďte následující kroky:   
    
    ![Konfigurace zkontrolujte přihlášení][34] 

    a. Vyberte **vytvořit roli**.

    b. Vytvořit tolik role podle potřeby a mapovat je na zprostředkovatele Identity.

21. Pomocí pověření účtu služby Amazon Web Services (AWS) pro načítání role z účtu Amazon Web Services (AWS) v zřizování uživatelů Azure AD. Chcete-li spustit tuto úlohu, otevřete konzolu Amazon Web Services (AWS) domácí.

22. Vyberte **služby** > **zabezpečení, Identity a dodržování předpisů** > **IAM**.

    ![Načítání role z účtu Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole1.png)

23. V části IAM vyberte **zásady** kartě.

    ![Načítání role z účtu Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole2.png)

24. Chcete-li vytvořit novou zásadu, vyberte **vytvořit zásadu**.

    ![Vytvoření nové zásady](./media/active-directory-saas-amazon-web-service-tutorial/fetchingrole3.png)
 
25. Pokud chcete vytvořit vlastní zásady načíst všechny role z účtů Amazon Web Services (AWS), proveďte následující kroky:

    ![Vytvoření nové zásady](./media/active-directory-saas-amazon-web-service-tutorial/policy1.png)

    a. V **vytvořit zásadu** vyberte **JSON** kartě.

    b. V dokumentu zásady přidejte následující kód JSON:
    
    ```
    
    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",
        
    "Action": [
        
    "iam: ListRoles"
        
    ],

    "Resource": "*"

    }

    ]

    }
    
    ```

    c. Chcete-li ověřit zásadu, vyberte **tlačítko zkontrolujte zásady**.

    ![Definovat nové zásady](./media/active-directory-saas-amazon-web-service-tutorial/policy5.png)

26. Definování **nové zásady** provedením následujících kroků:

    ![Definovat nové zásady](./media/active-directory-saas-amazon-web-service-tutorial/policy2.png)

    a. Zadejte **název zásady** jako **AzureAD_SSOUserRole_Policy**.

    b. Můžete zadat následující **popis** zásady: **tyto zásady vám umožní načíst role z účtů AWS**.
    
    c. Vyberte **vytvořit zásadu** tlačítko.
        
27. Pokud chcete vytvořit nový uživatelský účet ve službě Amazon Web Services (AWS) IAM, proveďte následující kroky:

    a. Vyberte **uživatelé** v konzole Amazon Web Services (AWS) IAM.

    ![Definovat nové zásady](./media/active-directory-saas-amazon-web-service-tutorial/policy3.png)
    
    b.To vytvořte nového uživatele, vyberte **přidat uživatele** tlačítko.

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/policy4.png)

    c. V **přidat uživatele** část, proveďte následující kroky:
    
    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/adduser1.png)
    
    * Zadejte **AzureADRoleManager** v poli uživatelské jméno.
    
    * Pro typ přístupu, vyberte **programový přístup** možnost. Tímto způsobem uživatele můžete volat rozhraní API a načíst role z účtu Amazon Web Services (AWS).
    
    * Vyberte **další oprávnění** tlačítko v pravém dolním rohu.

28. Vytvořte novou zásadu pro tohoto uživatele provedením následujících kroků:

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/adduser2.png)
    
    a. Vyberte **přímo připojit existující zásady** tlačítko.

    b. Vyhledejte nově vytvořenou zásadu v části filtru **AzureAD_SSOUserRole_Policy**.
    
    c. Vyberte **zásad**. Vyberte **Další: Zkontrolujte** tlačítko.

29. Projděte si zásadu pro připojené uživatele provedením následujících kroků:

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/adduser3.png)
    
    a. Zkontrolujte uživatelské jméno, typ přístupu a zásady, které jsou namapované na uživatele.
    
    b. Chcete-li vytvořit uživatele, vyberte **vytvořit uživatele** tlačítko v pravém dolním rohu vytvořit uživateli.

30. Stáhněte si přihlašovací údaje uživatele provedením následujících kroků:

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/adduser4.png)
    
    a. Zkopírujte uživatele **přístup klíče ID** a **tajný přístupový klíč**.
    
    b. Zadejte tyto přihlašovací údaje do části zřizování uživatelů Azure AD se načíst role z konzole Amazon Web Services (AWS).
    
    c. Vyberte **Zavřít** tlačítko v pravém dolním rohu.

31. Přejděte na **zřizování uživatelů** části Amazon Web Services (AWS) aplikace v portálu pro správu Azure AD.

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/provisioning.png)

32. Zadejte **přístupový klíč** a **tajný klíč** v **tajný klíč klienta** a **tajný klíč tokenu** pole v uvedeném pořadí.

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/provisioning1.png)
    
    a. Zadejte přístupový klíč pro uživatele Amazon Web Services (AWS) v **clientsecret** pole.
    
    b. Zadejte tajný klíč Amazon Web Services (AWS) uživatele v **tajný klíč tokenu** pole.
    
    c. Vyberte **Test připojení** tlačítko. Měli byste otestovat úspěšně toto připojení.

    d. Uložit nastavení výběrem **Uložit** tlačítka v horní části.
 
33. Ujistěte se, že vypnete Stav zřizování **na** v **nastavení**. To uděláte tak, že vyberete **na**a potom vyberete **Uložit** tlačítka v horní části.

    ![Přidání uživatele](./media/active-directory-saas-amazon-web-service-tutorial/provisioning2.png)

> [!TIP]
> Teď si můžete přečíst stručným verzi tyto pokyny uvnitř [portál Azure](https://portal.azure.com) při k nastavení aplikace. Po přidání této aplikace z **služby Active Directory > podnikové aplikace, které** vyberte **jednotné přihlašování** kartě. Přejděte k embedded dokumentace prostřednictvím **konfigurace** v dolní části. Můžete přečíst další informace o funkci embedded dokumentace v [Azure AD vložených dokumentaci]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovací uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na portálu Azure, názvem Britta Simon.

   ![Vytvořit testovací uživatele Azure AD][100]

**Vytvoření zkušebního uživatele ve službě Azure AD, proveďte následující kroky:**

1. Na portálu Azure, v levém podokně, vyberte **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**. Potom vyberte **všichni uživatelé**.

    !["Uživatelé a skupiny" a "Všichni uživatelé" odkazy](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, vyberte **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit hesla** zaškrtněte políčko a zapište si ji hodnotu, která se zobrazí v **heslo** pole.

    d. Vyberte **Vytvořit**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Vytvořit testovací uživatele s Amazon Web Services (AWS)

Cílem této části je vytvoření uživatele volat Britta Simon Amazon Web Services (AWS). Amazon Web Services (AWS) nepotřebuje uživatele, který bude vytvořen v jejich systému pro-jednotné přihlášení, takže není nutné provádět veškeré akce v tomto poli.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit testovacího uživatele Azure AD

V této části povolíte Britta Simon chcete použít Azure jednotné přihlašování je udělení přístupu k Amazon Web Services (AWS).

![Přiřadit role uživatele][200] 

**Pokud chcete přiřadit Britta Simon k Amazon Web Services (AWS), proveďte následující kroky:**

1. Na portálu Azure otevřete zobrazení aplikace. Pak přejděte do zobrazení adresáře a vyberte **podnikové aplikace, které**. Potom vyberte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

2. V seznamu aplikací vyberte **Amazon Web Services (Amazon Web Services (AWS)**.

    ![V seznamu aplikací na odkaz Amazon Web Services (AWS)](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)  

3. V nabídce na levé straně vyberte **uživatelů a skupin**.

    ![Odkaz "Uživatelé a skupiny"][202]

4. Vyberte **přidat** tlačítko. Potom v **přidat přiřazení** dialogové okno, vyberte **uživatelů a skupin**.

    ![V podokně Přidat přiřazení][203]

5. V **uživatelů a skupin** dialogové okno, vyberte **Britta Simon** v seznamu uživatelů.

6. V **uživatelů a skupin** dialogové okno, klikněte **vyberte** tlačítko. 

7. V **přidat přiřazení** dialogové okno, vyberte **přiřadit** tlačítko.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části otestovat vaše konfigurace Azure AD jeden přihlašování pomocí přístupového panelu.

Když vyberete dlaždici Amazon Web Services (AWS) na přístupovém panelu, jste měli získat automaticky přihlášeného k vaší aplikaci Amazon Web Services (AWS). Další informace o na přístupovém panelu najdete v tématu [Úvod k přístupovému panelu](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů k integraci aplikací SaaS v Azure Active Directory](active-directory-saas-tutorial-list.md)
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
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

