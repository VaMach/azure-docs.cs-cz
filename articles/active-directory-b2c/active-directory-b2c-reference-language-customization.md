---
title: "Azure Active Directory B2C: Jazyk přizpůsobení pomocí | Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: 3c7c49ee5fbd98762da0eef6f02e7c2f036453cb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Přizpůsobení jazyka pomocí

>[!NOTE] 
>Tato funkce je ve verzi public preview.  Doporučuje se použít testovacím klientem, při použití této funkce.  Plánujeme není na všechny nejnovější změny z verze preview verze obecné dostupnosti, ale nemůžeme rezervovat práva k provedení těchto změn ke zlepšení funkci.  Až využijete možnost vyzkoušet funkci, zadejte prosím zpětnou vazbu pro vaše prostředí a jak bychom mohli je lepší.  Nástroj emotikona vzhled vpravo nahoře můžete poskytnout zpětnou vazbu prostřednictvím portálu Azure.   Pokud je obchodním požadavkem, abyste mohli přejít za provozu pomocí této funkce ve fázi preview, dejte nám vědět, vaše scénáře a my vám může poskytnout správné informace a pomoc.  Obraťte se na nás na adrese [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Přizpůsobení jazyk můžete změnit vám dobře slouží uživatele na jiný jazyk podle svých potřeb zákazníka.  Poskytujeme překladů pro 36 jazyků (viz [Další informace o](#additional-information)).  I když prostředí je k dispozici pouze pro jeden jazyk, můžete přizpůsobit jakýkoli text na stránkách podle svých potřeb.  

## <a name="how-does-language-customization-work"></a>Jak funguje jazyk přizpůsobení?
Vlastní nastavení jazyka umožňuje vyberte jazyky, které vám dobře slouží uživatele je k dispozici v.  Jakmile je funkce zapnutá, můžete zadat parametr řetězce dotazu, ui_locales, z vaší aplikace.  Při volání do Azure AD B2C, jsme převede stránku pro národní prostředí, které jste označili.  Pomocí typu konfigurace vám poskytuje úplnou kontrolu nad jazyky vám dobře slouží uživatele a ignoruje nastavení jazyka prohlížeče zákazníka. Můžete taky nemusí být nutné tuto úroveň kontroly nad jaké jazyky, najdete v části vašich zákazníků.  Pokud nezadáte parametr ui_locales, zkušeností zákazníků se závisí na nastavení svého prohlížeče.  Můžete řídit jazyky, které vám dobře slouží uživatele převádějí na přidáním v podporovaném jazyce.  Pokud prohlížeč zákazníka je nastavena na Zobrazit jazyk, že nechcete, aby pro podporu, zobrazí se místo toho jazyk, který jste vybrali jako výchozí hodnotu v podporované jazykové verze.

1. **Zadaný jazyk uživatelského rozhraní – národní prostředí** -Jakmile povolíte jazyk přizpůsobení, vám dobře slouží uživatele převádějí na jazyce určeném v tomto poli
2. **Požadovaný jazyk prohlížeče** – Pokud nebyly zadány žádné uživatelské rozhraní národní prostředí, překládá do prohlížeče požadovaný jazyk, **Pokud byl součástí podporované jazyky**
3. **Výchozí jazyk zásad** – Pokud prohlížeč neurčuje jazyk nebo Určuje jednu, která není podporována, překládá pro výchozí jazyk zásad

>[!NOTE]
>Pokud používáte vlastní uživatelské atributy, budete muset zadat vlastní překladů. Najdete v části "[přizpůsobit vaší řetězce](#customize-your-strings)' Podrobnosti.
>

## <a name="support-uilocales-requested-languages"></a>Podpora ui_locales požadované jazyky 
Když zapnete jazyk přizpůsobení na zásadu, nyní je možné určit jazyk cesty uživatele tak, že přidáte parametr ui_locales.
1. [Postupujte podle těchto kroků přejděte do okna s funkcemi B2C na portálu Azure.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Přejděte na zásadu, která chcete povolit pro překlad.
3. Klikněte na tlačítko **jazyk přizpůsobení**.
4. Přečtěte si upozornění pečlivě.  Jakmile bude povoleno, nelze vypnout, přizpůsobení jazyk'.
5. Zapnout funkci a klikněte na tlačítko **OK**.
6. Uložit vaše zásadu v levém horním rohu vaše **upravit zásady** okno.

## <a name="select-which-languages-your-user-journey-supports"></a>Vyberte jazyky, které podporuje vaše uživatelské cesty 
Vytvořte seznam povolených jazyků pro vaše uživatele cestu k převodu v, pokud není zadán parametr ui_locales.
1. Zajistěte, aby byl jazyk přizpůsobení z předchozích pokynů zapnout vaše zásady.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. Budete přesměrováni na vaši **podporované jazyky** okno.  Tady můžete vybrat **přidat jazyk**.
4. Vyberte všechny jazyky, které chcete podporovat.  

>[!NOTE]
>Pokud parametr ui_locales není k dispozici, pak stránce převádějí na jazyk prohlížeče zákazníka pouze v případě, že je v tomto seznamu
>

5. Klikněte na tlačítko **Ok** v dolní části
6. Zavřít **jazyk přizpůsobení** okno a **Uložit** vaše zásady.

## <a name="customize-your-strings"></a>Přizpůsobení vaší řetězce
'Jazyk vlastní nastavení' umožňuje přizpůsobit libovolného řetězce v vám dobře slouží uživatele.
1. Zajistěte, aby byl jazyk přizpůsobení z předchozích pokynů zapnout vaše zásady.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. V levé navigační nabídce vyberte **stahování obsahu**.
4. Vyberte stránku, kterou chcete upravit.
5. V rozevíracím seznamu vyberte jazyk, který chcete upravit.
6. Klikněte na **Stáhnout**. 

Tyto kroky poskytnout soubor JSON, který vám pomůže začít upravovat vaše řetězce.

### <a name="changing-any-string-on-the-page"></a>Změna libovolného řetězce na stránce
1. Otevřete soubor JSON stažený z předchozích pokynů v editoru JSON.
2. Najděte na element, který chcete změnit.  Můžete najít `StringId` řetězce, který hledáte, nebo Hledat `Value` chcete změnit.
3. Aktualizace `Value` atributem, co chcete zobrazit.
4. Soubor uložte a odešlete své změny.

### <a name="changing-extension-attributes"></a>Změna atributů rozšíření
Pokud chcete změnit řetězec pro vlastní uživatelský atribut, nebo chcete přidat do formátu JSON, je v následujícím formátu:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Pokud potřebujete k přepsání řetězec, nezapomeňte nastavit `Override` hodnotu `true`.  Pokud není hodnota změněna, tato položka je ignorována. 
>

Nahraďte `<ExtensionAttribute>` s názvem vaší atribut vlastní uživatele.  
Nahraďte `<ExtensionAttributeValue>` nové řetězcem, který se má zobrazit.

### <a name="using-localizedcollections"></a>Pomocí LocalizedCollections
Pokud chcete zadat seznam sadu hodnot pro odpovědi, je nutné vytvořit `LocalizedCollections`.  A `LocalizedCollections` je pole `Name` a `Value` páry.  `Name` Je určeno a `Value` je co je vrácený v deklaraci identity.  Chcete-li přidat `LocalizedCollections`, má následující formát:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": false,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```
>[!IMPORTANT]
>Pokud potřebujete k přepsání řetězec, nezapomeňte nastavit `Override` hodnotu `true`.  Pokud není hodnota změněna, tato položka je ignorována. 
>

* `ElementId`je uživatel atribut, který tato `LocalizedCollections` je odpovědí na
* `Name`je hodnota zobrazí uživateli
* `Value`Co je vrácený v deklaraci identity, pokud je vybraná tato možnost je

### <a name="upload-your-changes"></a>Odeslat změny
1. Jakmile dokončíte změny do souboru JSON, přejděte zpět do svého klienta B2C.
2. Z vaší **upravit zásady** vyberte **jazyk přizpůsobení**.
3. V levé navigační nabídce vyberte **nahrát obsah**.
4. Vyberte stránku, který chcete odeslat změny pro.
5. Pokud chcete nahrát jazyk, který jste zadali dříve JSON pro, budete muset odstranit předchozí položce.  Chcete-li odstranit kliknutím `...` na pravé straně tohoto jazyka a vyberte **odstranit**.
6. Klikněte na tlačítko **přidat** v levé horní části.
7. Vyberte jazyk souboru JSON.
8. Klikněte na tlačítko složky na pravé straně a vyhledejte souboru JSON.
9. Klikněte **nahrát** tlačítko v dolní části okna.
10. Přejděte zpět na vaše **upravit zásady** a klikněte na **Uložit**.

## <a name="additional-information"></a>Další informace
### <a name="recommendations-for-language-customization"></a>Doporučení pro 'jazyk přizpůsobení.
Doporučujeme, abyste jenom uvedení v položky k prostředkům jazyk pro řetězce, které chcete explicitně nahradit.  Jsme vynutit omezení velikosti k souboru, který se zkompiluje mimo všechny JSON překladů.  Pokud vaše soubory příliš velký, má dopad na výkon vaší uživatelské cesty.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Popisky přizpůsobení uživatelského rozhraní stránky jsou odebrány, jakmile je povoleno, přizpůsobení jazyk.
Když povolíte 'Jazyk přizpůsobení', se odeberou všechny předchozí úpravy štítků, které používají přizpůsobení uživatelského rozhraní stránky s výjimkou vlastní uživatelské atributy.  Tato změna se provádí aby nedocházelo ke konfliktům, ve kterém můžete upravit vaše řetězce.  Chcete-li změnit štítky a jiných řetězců tím, že nahrajete prostředků jazyka v 'jazyk přizpůsobení, můžete pokračovat.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Společnost Microsoft se zavazuje zajistit nejaktuálnější překladů pro vaše použití
Jsme bude nepřetržitě zlepšit překlady a zůstanou v dodržování předpisů pro vás.  Jsme se identifikovat chyby a změny v globálních terminologie a bezproblémově zkontrolujte aktualizace, které budou fungovat v vám dobře slouží uživatele.
### <a name="support-for-right-to-left-languages"></a>Podpora jazyků zprava doleva
Neexistuje žádná podpora pro zprava doleva jazyky, pokud budete potřebovat, tato funkce hlasovat prosím pro tuto funkci na [zpětnou vazbu Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Sociální překlady zprostředkovatele Identity
Poskytujeme parametru OIDC ui_locales sociálních přihlášení, ale není dodrženo některé poskytovateli sociální identity, včetně Facebook a Google. 
### <a name="browser-behavior"></a>Chování prohlížeče
Chrome a Firefox obě žádosti pro své jazykové sady a pokud je podporovaném jazyce, se zobrazí před výchozí.  Hraniční aktuálně neuvede v požadavku jazyk a přejde se rovnou na výchozí jazyk.
### <a name="known-issues"></a>Známé problémy
* Odesílání prostředky jazyk pro stránku vícefaktorového ověřování v zásadách upravit profil není aktuálně dostupná.
* `LocalizedCollections`nejsou generované hodnoty, pokud to vyžaduje typ odpovědi
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Co když chci, aby jazyk, který není podporován?
Doporučujeme hodláte umožňují rozšíření tuto funkci, která vám umožní nahrát prostředek JSON směrem "vlastní jazycích".  Tato funkce umožňuje zadat název a jazyk kódu pro žádný jazyk a zadejte *všechny* překladů pro požadovaný jazyk.  Pokud potřebujete tuto funkci, pošlete nám na váš scénář [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Jaké jazyky jsou podporovány?

| Jazyk              | Kód jazyka |
|-----------------------|---------------|
| Bengálském                | Bn            |
| čeština                 | cs            |
| dánština                | da            |
| němčina                | de            |
| řečtina                 | el            |
| Angličtina               | en            |
| španělština               | es            |
| finština               | fi            |
| francouzština                | fr            |
| Gudžarátština              | gu            |
| Hindština                 | Ahoj            |
| Chorvatština              | personální oddělení            |
| maďarština             | hu            |
| italština               | it            |
| japonština              | ja            |
| Kannadština               | KN            |
| korejština                | ko            |
| Malajalámština             | ml            |
| Maráthština               | MR            |
| Malajština                 | MS            |
| Norská Bokmål      | nb            |
| holandština                 | nl            |
| Paňdžábština               | Pa            |
| polština                | pl            |
| Portugalština – Brazílie   | pt-br         |
| Portugalština – Portugalsko | pt-pt         |
| rumunština              | ro            |
| ruština               | ru            |
| Slovenština                | Sk            |
| švédština               | sv            |
| Tamilština                 | tových            |
| Telugština                | te            |
| Thajština                  | tý            |
| turečtina               | tr            |
| -Čínština, zjednodušená čínština  | zh-hans       |
| Tradiční čínština – | zh-hant       |
