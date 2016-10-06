<properties
    pageTitle="Managing groups in Azure Active Directory | Microsoft Azure"
    description="How to create and manage groups to manage Azure users using Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>


# Správa skupin ve službě Azure Active Directory

Jedna z funkcí správy uživatelů služby Azure Active Directory (Azure AD) je možnost vytváření skupin uživatelů. Pomocí skupiny můžete provádět úlohy správy, jako je přiřazení licencí nebo oprávnění víc uživatelům současně. Skupiny můžete použít také k přiřazení přístupových oprávnění pro:

- prostředky, například objekty v adresáři
- Prostředky, které jsou pro adresář externí, třeba aplikace SaaS, služby Azure, weby SharePoint nebo místní prostředky

Vlastník prostředku může navíc přístup k prostředku přiřadit i skupině služby Azure AD, která patří někomu jinému. Tím získají členové této skupiny přístup k prostředku. Vlastník skupiny potom spravuje členství ve skupině. Vlastník prostředku deleguje vlastníkovi skupiny oprávnění přiřazovat uživatele k jeho prostředku.

## Vytvoření skupiny

V závislosti na tom, jaké služby má vaše organizace předplacené, můžete vytvořit skupinu pomocí některé z těchto služeb:
- Portál Azure Classic
- Portál účtu služeb Office 365
- Portál účtu Windows Intune

Budeme popisovat úlohy prováděné na portálu Azure Classic. Další informace o používání portálů mimo Azure ke správě adresáře služby Azure AD najdete v článku [Správa adresáře služby Azure AD](active-directory-administer.md).

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Vyberte **Přidat skupinu**.

4. V okně **Přidat skupinu** zadejte název a popis skupiny.


## Přidání nebo odebrání jednotlivých uživatelů ve skupině zabezpečení

**Přidání jednotlivého uživatele do skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Otevřete skupinu, do které chcete přidat členy. Pro vybranou skupinu otevřete kartu **Členové**, pokud ještě není zobrazená.

4. Vyberte **Přidat členy**.

5. Na stránce **Přidat členy** vyberte jméno uživatele nebo název skupiny, které chcete přidat jako člena této skupiny. Nezapomeňte toto jméno nebo název přidat i do podokna **Vybraní**.


**Odebrání jednotlivého uživatele ze skupiny**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny**.

3. Otevřete skupinu, ze které chcete odebrat členy.

4. Vyberte kartu **Členové**, vyberte jméno člena, kterého chcete z této skupiny odebrat, a potom klikněte na **Odebrat**.

6. V zobrazené výzvě potvrďte, že chcete tohoto člena ze skupiny odebrat.


## Dynamická správa členství ve skupině

Ve službě Azure AD můžete velmi snadno nastavit jednoduché pravidlo a určit tak uživatele, kteří se mají stát členy skupiny. Jednoduché pravidlo je takové pravidlo, které provádí jenom jedno porovnání. Pokud je třeba některá skupina přiřazená aplikaci SaaS, můžete vytvořit pravidlo, které zajistí přidání uživatelů s názvem pracovní pozice „Obchodní zástupce“. Toto pravidlo potom udělí přístup do této aplikace SaaS všem uživatelům s touto pracovní pozicí ve vašem adresáři.

Když se změní libovolné atributy uživatele, systém vyhodnotí všechna dynamická pravidla skupin v adresáři a zjistí, zda změna uživatele aktivuje nějaké přidání do skupiny nebo odebrání ze skupiny. Pokud uživatel splňuje pravidlo pro skupinu, je do této skupiny přidán jako člen. Pokud již uživatel pravidlo pro skupinu, ve které je členem, nesplňuje, je jeho členství ve skupině odebráno.

> [AZURE.NOTE] Pravidlo pro dynamické členství můžete nastavit pro skupiny zabezpečení nebo pro skupiny Office 365. Vnořené členství ve skupinách se v současné době nepodporuje v případě přiřazování k aplikacím podle skupiny.
>
> Dynamické členství ve skupinách vyžaduje přiřazení licence služby Azure AD Premium následujícím uživatelům:
>
> - správce, který spravuje pravidlo pro skupinu
> - Všichni členové skupiny

**Povolení dynamického členství ve skupině**

1. Na [portálu Azure Classic](https://manage.windowsazure.com) vyberte **Active Directory** a potom vyberte název adresáře své organizace.

2. Vyberte kartu **Skupiny** a otevřete skupinu, kterou chcete upravit.

3. Vyberte kartu **Konfigurace** a potom nastavte možnost **Povolit dynamické členství** na **Ano**.

4. Nastavte jedno jednoduché pravidlo pro skupinu, které bude řídit dynamické členství pro funkce v této skupině. Zkontrolujte, jestli jste vybrali možnost **Přidat uživatele, kde**, a potom vyberte vlastnost uživatele ze seznamu (například oddělení, pracovní funkce atd.),

5. V dalším kroku vyberte podmínku (nerovná se, rovná se, nezačíná, začíná, neobsahuje, obsahuje, neodpovídá, odpovídá).

6. Zadejte hodnotu vybrané vlastnosti uživatele.

Další informace o vytváření *rozšířených* pravidel (která můžou obsahovat několik porovnání) pro dynamické členství ve skupině najdete v článku o [používání atributů k vytvoření rozšířených pravidel](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Další informace

Následující články poskytují další informace o službě Azure Active Directory.

* [Správa přístupu k prostředkům pomocí skupin služby Azure Active Directory](active-directory-manage-groups.md)

* [Rutiny Azure Active Directory pro konfiguraci nastavení skupiny](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)

* [Představení služby Azure Active Directory](active-directory-whatis.md)

* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)


<!--HONumber=Sep16_HO4-->


