---
title: "Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth | Dokumentace Microsoftu"
description: "Naučte se vytvářet poskytovatele ověřování Azure Multi-Factor Auth."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: a7dd5030-7d40-4654-8fbd-88e53ddc1ef5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/28/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 977640041f4b58a751848c96e2aa48eb2b284154
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017

---

# <a name="getting-started-with-an-azure-multi-factor-auth-provider"></a>Začínáme s poskytovatelem ověřování Azure Multi-Factor Auth
Dvoustupňové ověřování je k dispozici ve výchozím nastavení pro globální správce, kteří mají uživatele služeb Azure Active Directory a Office 365. Pokud ale budete chtít využívat výhod [pokročilých funkcí](multi-factor-authentication-whats-next.md) měli byste si zakoupit plnou verzi Azure Multi-Factor Authentication (MFA).

Poskytovatel ověřování Azure Multi-Factor Auth umožňuje využít výhod funkcí poskytovaných plnou verzí Azure MFA. Je určený pro uživatele, kteří **nemají licence prostřednictvím Azure MFA, Azure AD Premium nebo Enterprise Mobility + Security (EMS)**.  Azure MFA, Azure AD Premium a EMS zahrnují plnou verzi Azure MFA ve výchozím nastavení. Pokud máte licence, nepotřebujete poskytovatele Azure Multi-Factor Auth.

Pokud si chcete stáhnout sadu SDK, budete potřebovat poskytovatele Azure Multi-Factor Auth.

> [!IMPORTANT]
> Pokud chcete stáhnout sadu SDK, vytvořte poskytovatele Azure Multi-Factor Auth i v případě, že máte licence Azure MFA, AAD Premium nebo EMS.  Pokud vytvoříte poskytovatele Azure Multi-Factor Auth pro tento účel a máte už licence, nezapomeňte poskytovatele vytvořit podle modelu **Na povoleného uživatele**. Potom propojte poskytovatele s adresářem, ve kterém jsou uložené licence Azure MFA, Azure AD Premium nebo EMS. Tato konfigurace zajistí, že nebudete dostávat faktury, pokud nemáte více jedinečných uživatelů provádějících dvoustupňové ověřování než počet vlastněných licencí.

## <a name="what-is-an-azure-multi-factor-auth-provider"></a>Co je poskytovatele ověřování Azure Multi-Factor Auth?

Pokud nemáte licence pro Azure Multi-Factor Authentication, můžete vytvořit poskytovatele ověřování tak, aby pro vaše uživatele vyžadoval dvoustupňové ověření. Pokud vyvíjíte vlastní aplikaci a chcete povolit Azure MFA, vytvořte poskytovatele ověřování a [stáhněte sadu SDK](multi-factor-authentication-sdk.md).

Existují dva typy poskytovatelů ověřování. Rozdíl mezi nimi spočívá v tom, jak se účtuje předplatné Azure. Možnost podle ověření zjišťuje počet ověření za měsíc v rámci vašeho tenanta. Tato možnost je vhodná, pokud máte řadu uživatelů, kteří se ověřují jenom občas, například pokud vícefaktorové ověřování vyžadujete pro vlastní aplikaci. Možnost podle uživatelů zjišťuje počet jednotlivců za měsíc, kteří ve vašem tenantovi provedli dvoustupňové ověření. Tato možnost je vhodná, pokud máte uživatelé s licencemi, ale potřebujete rozšířit vícefaktorového ověřování pro další uživatele nad rámec licenčních omezení.

## <a name="create-a-multi-factor-auth-provider"></a>Vytvoření poskytovatele Multi-Factor Auth
K vytvoření poskytovatele ověřování Azure Multi-Factor Auth použijte následující postup.

1. Přihlaste se jako správce do [portálu Azure Classic](https://manage.windowsazure.com).
2. Vlevo vyberte možnost **Active Directory**.
3. Na stránce služby Active Directory nahoře vyberte možnost **Poskytovatelé Multi-Factor Authentication**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider1.png)

4. V dolní části, klikněte na tlačítko **Nový**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider2.png)

5. V části App Services vyberte **Poskytovatel Multi-Factor Auth**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider3.png)

6. Vyberte možnost **Rychle vytvořit**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider4.png)

7. Vyplňte následující pole a vyberte možnost **Vytvořit**.
   1. **Název** – název poskytovatele Multi-Factor Auth.
   2. **Model použití** – Zvolte jednu ze dvou možností:
      * Za ověření – nákupní model, který účtuje za ověření. Obvykle se používá pro scénáře, které používají Azure Multi-Factor Authentication v aplikaci zaměřené na spotřebitele.
      * Pro povolené uživatele – nákupní model, který účtuje za povoleného uživatele. Obvykle se používá pro přístup zaměstnanců k aplikacím, například Office 365. Tuto možnost vyberte, pokud máte uživatele, kteří už mají licence na Azure MFA.
   3. **Adresář** – klient Azure Active Directory, ke kterému je přidružen poskytovatel Multi-Factor Authentication. Uvědomte si následující skutečnosti:
      * Pro vytvoření poskytovatele Multi-Factor Auth nepotřebujete adresář Azure AD. Pokud plánujete používat pouze server Azure Multi-Factor Authentication nebo SDK, ponechte pole prázdné.
      * Poskytovatel Multi-Factor Auth musí být přidružen k adresáři služby Azure AD, aby mohl využívat výhod pokročilých funkcí.
      * Azure AD Connect, AAD Sync nebo DirSync jsou požadavek pouze v případě, že synchronizujete prostředí služby Active Directory v místě s adresářem služby Azure AD.  Pokud používáte pouze adresář služby Azure AD, který není synchronizován, pak tento postup není třeba.
        
        ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider5.png)

8. Po kliknutí na tlačítko vytvořit se vytvoří poskytovatel MFA a měla by se zobrazit zpráva s oznámením: **Poskytovatel Multi-Factor Authentication byl úspěšně vytvořen**. Klikněte na tlačítko **OK**.
   
   ![Vytvoření poskytovatele MFA](./media/multi-factor-authentication-get-started-auth-provider/authprovider6.png)  

