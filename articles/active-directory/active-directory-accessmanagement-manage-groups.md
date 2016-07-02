<properties
   pageTitle="Managing groups in Azure Active Directory | Microsoft Azure"
   description="How to create and manage groups to manage Azure users using Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="05/26/2016"
   ms.author="curtand"/>


# Správa skupin ve službě Azure Active Directory

Jedna z funkcí správy uživatelů služby Azure Active Directory (Azure AD) je možnost vytváření skupin uživatelů. Skupinu potom můžete použít k přiřazení licencí ke třídě uživatelů. Skupiny můžete použít také k přiřazení přístupových oprávnění pro:

- prostředky, například objekty v adresáři
- prostředky, které jsou pro adresář externí, například aplikace SaaS, služby Azure, weby SharePointu nebo místní prostředky

Vlastník prostředku může navíc přístup k prostředku přiřadit i skupině služby Azure AD. Tím získají členové skupiny přístup k prostředku. Vlastník skupiny potom spravuje členství ve skupině. Vlastník prostředku deleguje vlastníkovi skupiny oprávnění přiřazovat uživatele k jeho prostředku.

## Vytvoření skupiny

Tuto úlohu můžete zvládnout pomocí portálu účtů Office 365, portálu účtů Windows Intune nebo portálu Azure Classic podle toho, jaké služby si vaše organizace předplatila. Další informace o používání portálů mimo platformu Azure ke správě služby Azure Active Directory najdete v článku [Správa adresáře služby Azure AD](active-directory-administer.md).

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Vyberte **Přidat skupinu**.

4. V okně **Přidat skupinu** zadejte název a popis skupiny.


## Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení

**Přidání jednotlivého uživatele do skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Otevřete skupinu, do které chcete přidat členy. Ve výchozím nastavení se zobrazí karta **Členové** vybrané skupiny.

4. Vyberte **Přidat členy**.

5. Na stránce **Přidat členy** vyberte jméno uživatele nebo skupiny, které chcete přidat jako člena této skupiny, a nezapomeňte toto jméno přidat i do podokna **Vybraní**.


**Odebrání jednotlivého uživatele ze skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Otevřete skupinu, ze které chcete odebrat členy.

4. Vyberte kartu **Členové**, vyberte jméno člena, kterého chcete z této skupiny odebrat, a potom klikněte na **Odebrat**.

6. V zobrazené výzvě potvrďte, že chcete tohoto člena ze skupiny odebrat.


## Dynamická správa členství ve skupině

Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo (které provádí pouze jedno porovnání), a určit tak uživatele, kteří se mají stát členy skupiny. Pokud je skupina přiřazena například k aplikaci SaaS a vy nastavíte pravidlo pro přidání uživatelů, kteří jsou na pozici „Obchodního zástupce“, budou mít všichni uživatelé v adresáři služby Azure AD s touto funkcí přístup k takové aplikaci SaaS.

> [AZURE.NOTE] Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365. Vnořené členství ve skupinách momentálně není podporované v případě přiřazování k aplikacím podle skupiny.
>
> Dynamické členství ve skupinách vyžaduje přiřazení licence služby Azure AD Premium následujícím uživatelům:
>
> - správce, který spravuje pravidlo pro skupinu
> - všichni uživatelé, kteří jsou pomocí pravidla vybraní jako členové skupiny

**Povolení dynamického členství ve skupině**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny** a otevřete skupinu, kterou chcete upravit.

3. Vyberte kartu **Konfigurace** a potom nastavte možnost **Povolit dynamické členství** na **Ano**.

4. Nastavte jedno jednoduché pravidlo pro skupinu, které bude řídit dynamické členství pro funkce v této skupině. Zkontrolujte, jestli jste vybrali možnost **Přidat uživatele, kde**, a potom vyberte vlastnost uživatele ze seznamu (například oddělení, pracovní funkce atd.),

5. V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá) a nakonec zadejte hodnotu vybrané vlastnosti uživatele.

Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Další informace

Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

* [Představení služby Azure Active Directory](active-directory-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)



<!---HONumber=Jun16_HO2-->


