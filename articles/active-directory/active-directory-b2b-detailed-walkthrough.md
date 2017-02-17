---
title: "Podrobný průvodce pro verzi Preview spolupráce B2B ve službě Azure Active Directory | Dokumentace Microsoftu"
description: "Spolupráce B2B ve službě Azure Active Directory podporuje vaše vztahy s ostatními společnostmi tím, že vašim obchodním partnerům umožní selektivní přístup ke podnikovým aplikacím"
services: active-directory
documentationcenter: 
author: viv-liu
manager: cliffdi
editor: 
tags: 
ms.assetid: 7ae68208-63c1-4128-8e44-43a4f56d34dc
ms.service: active-directory
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/09/2017
ms.author: viviali
translationtype: Human Translation
ms.sourcegitcommit: 874465b9ddb62d4cc8bfadf7cf07b9cfeea30da3
ms.openlocfilehash: a710e12d2694624b545893fccd1b34957060a236


---
# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Spolupráce B2B ve službě Azure Active Directory: Podrobný průvodce (verze Preview)
Tento průvodce ukazuje, jak je možné používat spolupráci B2B ve službě Azure AD. Jako správci IT ve společnosti Contoso chceme sdílet aplikace se zaměstnanci tří partnerských společností. Žádná z partnerských společností nemusí mít službu Azure AD.

* Alice ze společnosti Simple Partner Org
* Robert ze společnosti Medium Partner Org, potřebuje přístup k sadě aplikací
* Karolína ze společnosti Complex Partner Org, potřebuje přístup k sadě aplikací a členství ve skupinách společnosti Contoso

Poté, co uživatelům z partnerských společností zašleme pozvánky, je můžeme nakonfigurovat ve službě Azure AD, abychom jim mohli prostřednictvím portálu Azure přidělit přístup k aplikacím a členství ve skupinách. Začněme přidáním Alice.

## <a name="adding-alice-to-the-contoso-directory"></a>Přidání Alice do adresáře společnosti Contoso
1. Vytvořte soubor .csv se záhlavím jako na obrázku a vyplňte u Alice pouze sloupce **Email**, **DisplayName** a **InviteContactUsUrl**. **DisplayName** je jméno, které se zobrazí v pozvánce a také v adresáři společnosti Contoso ve službě Azure AD. **InviteContactUsUrl** je způsob, jak může Alice společnost Contoso kontaktovat. V následujícím příkladu odkazuje InviteContactUsUrl na profil společnosti Contoso na síti LinkedIn. Popisky na prvním řádku souboru .csv je nutné napsat přesně tak, jak určuje [referenční příručka pro formátování souboru .csv](active-directory-b2b-references-csv-file-format.md).  
   ![Příklad souboru .csv pro Alici](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)
2. Na portálu Azure přidejte uživatele do adresáře společnosti Contoso (Active Directory > Contoso > Uživatelé > Přidat uživatele). Z rozevíracího seznamu „Typ uživatele“ vyberte „Uživatelé v partnerských společnostech“. Nahrajte soubor .csv. Ujistěte se, že jste soubor .csv před nahráním zavřeli.  
   ![Nahrání souboru .csv pro Alici](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)
3. Alice je nyní představena jako externí uživatel v adresáři společnosti Contoso ve službě Azure AD.  
   ![Alice je uvedena ve službě Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)
4. Alice obdrží následující e-mail.  
   ![E-mailová pozvánka pro Alici](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)
5. Když Alice klikne na odkaz, zobrazí se jí výzva k přijetí pozvánky a k přihlášení pomocí pracovních přihlašovacích údajů. Pokud Alice v adresáři služby Azure AD není, bude vyzvána k registraci.  
   ![Registrace po odeslání pozvánky pro Alici](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)
6. Alice je přesměrována na panel přístupu k aplikacím, který je prázdný, dokud jí nebude přístup k aplikacím udělen.  
   ![Panel přístupu pro Alici](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Tento postup umožňuje nejjednodušší formu spolupráce B2B. Alici, jakožto uživateli v adresáři společnosti Contoso ve službě Azure AD, je možné udělit přístup k aplikacím a skupinám prostřednictvím portálu Azure. Nyní přidáme Roberta, který potřebuje přístup k aplikacím Moodle a Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Přidání Roberta do adresáře společnosti Contoso a udělení přístupu k aplikacím
1. K vyhledání ID aplikací Moodle a Salesforce použijte rozhraní Windows PowerShell s nainstalovaným modulem pro službu Azure AD. ID můžete získat pomocí rutiny: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` Ta vám ukáže seznam všech dostupných aplikací společnosti Contoso včetně atributů AppPrincialId.  
   ![Načtení ID aplikací pro Roberta](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)
2. Vytvořte soubor .csv, který obsahuje Robertovy atributy Email a DisplayName, **InviteAppID**, **InviteAppResources** a InviteContactUsUrl. Do pole **InviteAppResources** zadejte identifikátory AppPrincipalIds aplikací Moodle a Salesforce, které jste našli pomocí rozhraní PowerShell. Jednotlivá ID oddělte mezerou. Do pole **InviteAppId** zadejte identifikátor AppPrincipalId aplikace Moodle. Díky tomu se v e-mailu a na přihlašovací stránce bude zobrazovat logo aplikace Moodle.  
   ![Příklad souboru .csv pro Roberta](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)
3. Nahrajte soubor .csv prostřednictvím webu Azure Portal stejně, jako jste to udělali u Alice. Robert je nyní externím uživatelem v adresáři společnosti Contoso ve službě Azure AD.
4. Robert obdrží následující e-mail.  
   ![E-mailová pozvánka pro Roberta](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)
5. Robert klikne na odkaz a bude vyzván k přijetí pozvánky. Jakmile se přihlásí, bude přesměrován na panel přístupu a bude moci ihned používat aplikace Moodle a Salesforce.  
   ![Panel přístupu pro Roberta](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Jako další přidáme Karolínu, která potřebuje přístup k aplikacím a také členství ve skupinách v adresáři společnosti Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Přidání Karolíny do adresáře společnosti Contoso, udělení přístupu k aplikacím a udělení členství ve skupině
1. K vyhledání ID aplikací Moodle a Salesforce a ID skupin v rámci společnosti Contoso použijte rozhraní Windows PowerShell s nainstalovaným modulem pro službu Azure AD.

   * Získejte AppPrincipalId pomocí rutiny `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, stejně jako u Roberta.
   * Získejte ObjectId pro skupiny pomocí rutiny `Get-MsolGroup | fl DisplayName, ObjectId`. Tím se zobrazí seznam všech skupin společnosti Contoso včetně atributu ObjectId. ID skupiny také naleznete v podobě atributu ID objektu na kartě Vlastnosti dané skupiny v portálu Azure.  
     ![Získání ID aplikací a skupin pro Karolínu](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)
2. Vytvořte soubor .csv a vyplňte pro Karolínu sloupce Email, DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources** a InviteContactUsUrl. Do pole **InviteGroupResources** zadejte atributy ObjectId skupin MyGroup1 a Externals oddělené mezerou.  
   ![Příklad souboru .csv pro Karolínu](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)
3. Nahrajte soubor .csv prostřednictvím portálu Azure.
4. Karolína je uživatelkou v adresáři společnosti Contoso a také je členkou skupin MyGroup1 a Externals, jak je možné vidět v portálu Azure.  
   ![Karolína je uvedena ve skupině ve službě Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)
5. Karolína obdrží e-mail, který obsahuje odkaz pro přijetí pozvánky. Jakmile se přihlásí, bude přesměrována na panel přístupu k aplikacím, kde bude mít přístup k aplikacím Moodle a Salesforce.  

To je vše ohledně přidávání uživatelů z partnerských firem pro spolupráci B2B ve službě Azure AD. Tento průvodce vám ukázal, jak přidat uživatele Alici, Roberta a Karolínu do adresáře společnosti Contoso pomocí tří samostatných souborů .csv. Tento proces ještě můžete zjednodušit tím, že jednotlivé soubory .csv sloučíte do jednoho souboru.  
![Příklad souboru .csv pro Alici, Roberta a Karolínu](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Související články
Projděte si naše další články ohledně spolupráce B2B ve službě Azure AD:

* [Co je spolupráce B2B ve službě Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Jak to funguje](active-directory-b2b-how-it-works.md)
* [Referenční příručka pro formátování souboru .csv](active-directory-b2b-references-csv-file-format.md)
* [Formát tokenu externího uživatele](active-directory-b2b-references-external-user-token-format.md)
* [Změny atributů objektu externího uživatele](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Aktuální omezení verze Preview](active-directory-b2b-current-preview-limitations.md)
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)



<!--HONumber=Feb17_HO2-->


