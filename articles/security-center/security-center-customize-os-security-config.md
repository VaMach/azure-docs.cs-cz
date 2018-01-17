---
title: "Přizpůsobení operačního systému konfigurace zabezpečení v Azure Security Center [Preview] | Microsoft Docs"
description: "V tomto článku se naučíte, jak přizpůsobit hodnocení security center"
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: terrylan
ms.openlocfilehash: 3af59e1b38e70494dd9dc17e2682d31cf7b7d361
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/17/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>Přizpůsobení operačního systému konfigurace zabezpečení v Azure Security Center [Preview]

Zjistěte, jak přizpůsobit hodnocení konfigurace zabezpečení operačního systému v Azure Security Center pomocí tohoto průvodce.

## <a name="what-are-os-security-configurations"></a>Jaké jsou konfigurace zabezpečení operačního systému?

Konfigurace zabezpečení monitorování Azure Security Center pomocí sady více než 150 doporučená pravidla pro posílení zabezpečení operačního systému, včetně pravidel souvisejících s brány firewall, auditování, zásady pro hesla a další. Pokud je počítač je zjištěn ohrožené konfigurace, vygeneruje se doporučení zabezpečení.

Přizpůsobení pravidel pomáhá organizacím určovat, které možnosti konfigurace mají pro jejich prostředí vhodnější. Tato funkce umožňuje uživatelům nastavit vlastní hodnocení zásady a použít na všech příslušných počítačích v rámci předplatného.

> [!NOTE]
> - Aktuálně přizpůsobení konfigurace zabezpečení operačního systému je k dispozici pro Windows Server 2008, 2008 R2, 2012, jenom 2012R2 operační systémy.
- Konfigurace platí pro všechny virtuální počítače a počítače připojeny k všechny pracovní prostory v části vybraného předplatného.
- Přizpůsobení konfigurace zabezpečení operačního systému je k dispozici pouze na úrovni Standard Security Center.
>
>

Tom, jak přizpůsobit pravidla konfigurace zabezpečení operačního systému?

Pravidla konfigurace zabezpečení operačního systému můžete přizpůsobit tak, že povolení a zakázání konkrétní pravidlo, změna požadované nastavení pro existující pravidlo a přidat nové pravidlo založené na typech podporovaných pravidlo (registr, zásady auditu a zásady zabezpečení). V současné době požadované nastavení musí být přesná hodnota.

Nová pravidla musí být ve stejné formátu a struktura jako ostatní existující pravidla stejného typu.

> [!NOTE]
> K přizpůsobení konfigurace zabezpečení operačního systému, musí být přiřazenou roli vlastník předplatného, Přispěvatel předplatné nebo správce zabezpečení.
>
>

## <a name="customize-security-configuration"></a>Přizpůsobení konfigurace zabezpečení

Chcete-li přizpůsobit výchozí konfigurace zabezpečení operačního systému ve službě Security Center:

1.  Otevřete řídicí panel **Security Center**.

2.  V části v hlavní nabídce Security Center, vyberte **zásady zabezpečení**.  **Security Center – zásady zabezpečení** otevře.

3.  Vyberte předplatné, se kterou chcete provést vlastní nastavení pro.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. V části **součásti zásad**, vyberte **upravit konfigurace zabezpečení**.

6.  **Úprava konfigurací zabezpečení** otevře. Postupujte podle kroků vyznačené na obrazovce stáhnout, upravit a odeslat změněný soubor.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Ve výchozím nastavení, je konfigurační soubor, který stáhnete v *json* formátu. Pokyny pro úpravu tohoto souboru, přejděte na [přizpůsobení konfiguračního souboru](#customize-the-configuration-file).
  >

7. Po úspěšném uložení souboru, konfigurace platí pro všechny virtuální počítače a počítače připojeny k všech pracovních prostorů v rámci vybraného předplatného. Tento proces může trvat delší dobu, obvykle na několik minut, ale můžete může trvat déle, protože závisí na velikosti infrastruktury. Vyberte **Uložit** potvrzení změn, jinak zásady se neukládá.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

V libovolném bodě můžete aktuální konfiguraci zásad resetovat do výchozího stavu pro zásadu výběrem **resetovat** možnost **pravidla upravit konfiguraci zabezpečení operačního systému**. Pokud zvolíte tuto možnost, zobrazí se následující automaticky otevírané okno Výstraha. Vyberte **Ano** k potvrzení.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Přizpůsobení konfiguračního souboru

V souboru přizpůsobení všechny podporované verze operačního systému je sada pravidel (ruleset). Každá sada pravidel má svou vlastní název a jedinečné ID, jak je znázorněno v následujícím příkladu:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Tento soubor byl upraven pomocí sady Visual Studio, ale můžete také použít Poznámkový blok, dokud máte modulu plug-in prohlížeče JSON, nainstalovány.
>
>

Při úpravě tohoto souboru, můžete upravit jedno pravidlo nebo všechny z nich. Každý ruleset zahrnuje *pravidla* oddíl, který obsahuje pravidla, rozdělit do 3 kategorií pravidel: registr, zásady auditu a zásady zabezpečení, jak je vidět níže:

![](media/security-center-customize-os-security-config/rules-section.png)

Každá kategorie obsahuje vlastní sadu atributů. Pro existující pravidla můžete provést změny v těm, které jsou následující:

- expectedValue: datového typu pole tohoto atributu musí odpovídat podporované hodnoty podle jednotlivých typů pravidel, například:

  - baselineRegistryRules: hodnota by měla odpovídat [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) definované v tomto pravidle.

  - baselineAuditPolicyRules: hodnota musí být řetězcovou hodnotu, jednu z následujících:

    - Úspěchy a chyby

    - Úspěch

  - baselineSecurityPolicyRules: hodnota musí být řetězcovou hodnotu, jednu z následujících:

    - "Nikdo"

    - Seznam povolených skupin uživatelů, například: "Správci, Backup Operators"

-   Stav: řetězec, který může obsahovat možnosti "Povoleno" nebo "Zakázáno". Pro tuto verzi privátní Preview verzi řetězec je malá a velká písmena.

Jedná se o pouze pole, které lze konfigurovat. Pokud jste porušují formát souboru nebo velikost, nebudete moci uložit změny. K této chybě dojde, pokud soubor nelze zpracovat:

![](media/security-center-customize-os-security-config/invalid-json.png)

V tématu [kódy chyb](#error-codes) seznam potenciální chyby.

Zde máte mezi některé příklady těchto pravidel. Atributy 'expectedValue' a 'stav' lze změnit:

**Část pravidla:** baselineRegistryRules
```
{

    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

}
```

**Část pravidla:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"expectedValue": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

**Pravidla částech:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"expectedValue": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft",
"state": "Enabled"
},
```

Existují některá pravidla, které jsou duplikované pro různé typy operačního systému. Duplicitní pravidla mají stejné originalId.

## <a name="adding-a-new-custom-rule"></a>Přidání nové vlastní pravidlo

Můžete také vytvořit nové pravidlo. Před vytvořením nové pravidlo, mějte na paměti následující omezení:

-   Verze schématu *baselineId* a *baselineName* nelze změnit.

-   Nelze odebrat, RuleSet.

-   Nelze přidat RuleSet.

-   Maximální počet pravidel povolené (i výchozí pravidla), která je 1 000 pravidel.

Nová vlastní pravidla jsou označené jako nový vlastní zdroj (! = "Microsoft"). *RuleId* pole může být null nebo prázdný. Pokud je prázdná, Microsoft generuje jeden. Pokud není prázdná, musí mít platný identifikátor GUID jedinečný mezi všechna pravidla (výchozí a vlastní). Přečtěte si omezení týkající se základní pole níže:

-   *originalId* -může být null nebo prázdný. Pokud *originalId* je prázdná, by mělo být platný identifikátor GUID.

-   *cceId* -může být null nebo prázdný. Pokud *cceId* je prázdná, musí být jedinečný.

-   *Typ pravidla* – jeden z: registru, AuditPolicy nebo SecurityPolicy.

-   *Závažnost* – jeden z: Neznámý kritická, upozornění nebo informativní.

-   *analyzeOperation* -musí mít hodnotu.

-   *auditPolicyId* – musí být platný identifikátor GUID.

-   *regValueType* -musí mít jednu z: Int, Long, řetězec nebo MultipleString.

> [!NOTE]
> Musí být Hive *LocalMachine*.
>
>

Příklad nové vlastní pravidlo:

**Registru**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
   }
```
**Zásady zabezpečení**:
```
{

   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Zásady auditu:**
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Selhání nahrávání souborů

Pokud odeslaná konfigurační soubor je neplatný z důvodu chyby ve formátování nebo hodnoty, zobrazí se chyba selhání. Můžete si stáhnout opravit a opravte chyby před opětovným odesláním opravené konfiguračního souboru csv zprávu podrobné chyby.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Příklad souboru chyby:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Kódy chyb

Níže uvedeného seznamu obsahuje všechny potenciální chyby:

| **Chyba**                                | **Popis**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Vlastnost 'schemaVersion' byl nalezen neplatný nebo prázdný. Hodnota musí nastavit na {0}.                                                         |
| BaselineInvalidStringError               | Vlastnost '{0}' nemůže obsahovat '\\n ".                                                                                                         |
| BaselineNullRuleError                    | Seznam standardních hodnot konfigurace pravidel obsahuje pravidla s hodnotou "null".                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE-ID: {0} není jedinečný.                                                                                                                  |
| BaselineRuleEmptyProperty                | Vlastnost: {0}' je chybí nebo je neplatný.                                                                                                       |
| BaselineRuleIdNotInDefault               | Toto pravidlo má zdrojová vlastnost "Microsoft", ale nebyl nalezen v ruleset výchozí Microsoft.                                                   |
| BaselineRuleIdNotUniqueError             | Pravidlo Id není jedinečné.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Byla nalezena neplatná vlastnost: {0}. Hodnota není platný identifikátor Guid.                                                                             |
| BaselineRuleInvalidHive                  | Hive musí být LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Název pravidla není jedinečný.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Pravidlo nebylo nalezeno v novou konfiguraci. Pravidlo nelze odstranit.                                                                     |
| BaselineRuleNotFoundError                | Pravidlo nebylo nalezeno ve výchozí sada pravidla standardních hodnot.                                                                                        |
| BaselineRuleNotInPlace                   | Pravidlo odpovídá výchozí pravidlo s typu {0} a je uveden v seznamu {1}.                                                                       |
| BaselineRulePropertyTooLong              | Vlastnost: {0}' je příliš dlouhý. Maximální povolená délka: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | Očekávaná hodnota: {0} se neshoduje typ hodnoty registru, který je definován.                                                              |
| BaselineRulesetAdded                     | Sada pravidla s id: {0} nebyl nalezen ve výchozí konfiguraci. Nelze přidat sadu pravidel.                                               |
| BaselineRulesetIdMustBeUnique            | Sada pravidel daného směrného plánu {0}' musí být jedinečný.                                                                                           |
| BaselineRulesetNotFound                  | Nastavit pravidla s id: {0} a názvem objekt {1}' nebyl nalezen v dané konfiguraci. Sada pravidla nelze odstranit.                                |
| BaselineRuleSourceNotMatch               | Pravidlo s id: {0} je již definován.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Výchozí pravidlo typ je: {0}.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Skutečný typ pravidla je: {0}, ale typ pravidla vlastnost je: {1}.                                                                          |
| BaselineRuleUnpermittedChangesError      | Povoleny jsou pouze vlastnosti 'expectedValue' a 'stav' se musí změnit.                                                                       |
| BaselineTooManyRules                     | Maximální povolený počet vlastní pravidla je pravidla {0}. Danou konfiguraci obsahuje pravidla {1}. (výchozí pravidla {2} + {3} vlastní pravidla. |
| ErrorNoConfigurationStatus               | Nenašel se žádný stav konfigurace. Uveďte prosím stav požadované konfigurace - "Výchozí" nebo "Vlastní".                                    |
| ErrorNonEmptyRulesetOnDefault            | Stav konfigurace je nastaven výchozí. Seznam BaselineRulesets musí být null nebo prázdný.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Stav daného konfigurace je "Vlastní", ale vlastnost 'baselineRulesets' má hodnotu null nebo prázdný.                                             |
| ErrorParsingBaselineConfig               | Danou konfiguraci je neplatná. Minimálně jeden z hodnot fronty definovaných má hodnotu null nebo neplatného typu.                                  |
| ErrorParsingIsDefaultProperty            | Daný 'configurationStatus' Hodnota '{0}' je neplatná. Hodnota může být pouze "Výchozí" nebo "Vlastní".                                         |
| InCompatibleViewVersion                  | Zobrazení verze: {0} není k dispozici pro tento typ pracovního prostoru.                                                                                   |
| InvalidBaselineConfigurationGeneralError | S jedné nebo více chybám ověření typ byl nalezen daného směrného plánu konfigurace.                                                          |
| ViewConversionError                      | Zobrazení je starší verze tohoto pracovního prostoru podporována. Zobrazit Chyba převodu: {0}                                                                 |

Pokud nemáte dostatečná oprávnění, může dojít k chybě došlo k obecné chybě:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Další postup
Tento článek ukázal, jak přizpůsobit hodnocení konfigurace zabezpečení operačního systému ve službě Security Center. Další informace o konfiguraci pravidla a nápravě najdete v tématu:

- [Security Center běžné identifikátorů konfigurace a standardních pravidel](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Security Center používá CCE (Common Configuration výčtu) k přiřazení jedinečné identifikátory pro konfigurační pravidla. Přejděte [CCE](https://nvd.nist.gov/config/cce/index) lokality pro další informace.
- [Opravit konfigurace zabezpečení](security-center-remediate-os-vulnerabilities.md) ukazuje, jak vyřešit chyby zabezpečení, když vaše konfigurace operačního systému neodpovídá pravidla zabezpečení doporučené konfigurace.
