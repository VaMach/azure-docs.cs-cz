---
title: "Azure spravované aplikace na webu Marketplace | Microsoft Docs"
description: "Popisuje Azure spravované aplikace, které jsou dostupné přes Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Azure spravovaných aplikací na webu Marketplace

 MSPs, nezávislí dodavatelé softwaru a systémových integrátorech (si) můžete použít Azure spravované aplikace na nabídku svá řešení pro všechny zákazníky využívající Azure Marketplace. Taková řešení snížit údržby a režijní náklady na údržbu zákazníků. Vydavatelé můžete prodeje, infrastruktury a software přes Marketplace. Službám a podpoře provozní se můžete připojit k spravovaných aplikací. Další informace najdete v tématu [spravované aplikace přehled](managed-application-overview.md).

Tento článek vysvětluje, jak MSP, ISV nebo SI můžete publikovat aplikaci na Marketplace s cílem a co nejvíce zpřístupnili zákazníkům.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Předpoklady pro publikování spravované aplikace

Předpoklady pro výpis v Marketplace:

* Technické

    *  Informace o základní strukturu a syntaxe šablon Azure Resource Manageru najdete v tématu [šablon Azure Resource Manageru](resource-group-authoring-templates.md).
    *  Chcete-li zobrazit úplnou šablonu řešení, najdete v části [šablony Azure Quickstart](https://azure.microsoft.com/en-us/documentation/templates/) nebo [úložiště šablony rychlý Start](https://github.com/azure/azure-quickstart-templates).
    *  Informace o tom, jak vytvořit rozhraní pro zákazníky, kteří nasazují aplikace prostřednictvím Marketplace najdete v tématu [vytvoření uživatelského rozhraní definice souboru](managed-application-createuidefinition-overview.md).

* Běžné uživatele (obchodní požadavky)

    *   Vaše společnost nebo její dceřiné společnosti, musí být umístěny v určité zemi, kde je podporováno prodeje podle Marketplace.
    *   Tento produkt musí mít licenci způsobem, který je kompatibilní s modely fakturace nepodporuje Marketplace.
    *   Jste zodpovědná za zpřístupnění technické podpory zákazníků vyvineme způsobem. Podporu může být volná, placené, nebo prostřednictvím komunity podporovat.
    *   Jste zodpovědní za licencování softwaru a všechny závislosti software třetích stran.
    *   Je nutné zadat obsah, který splňuje kritéria pro vaši nabídku uveden v Marketplace. a na portálu Azure.
    *   Budete muset souhlasit s podmínkami Azure Marketplace zapojení zásady a vydavatele smlouvy.
    *   Budete muset souhlasit s splňovat podmínky použití, prohlášení o ochraně osobních údajů Microsoft a certifikované smlouvy programu služby Microsoft Azure.

## <a name="create-a-new-azure-application-offer"></a>Vytvořit novou aplikaci Azure nabídka

Po splňujete požadavky, jste připraveni vytvořit nabídku spravované aplikace. Podívejme rychlý přehled o nabídku a SKU.

### <a name="offer"></a>Nabídka

Nabídka pro spravované aplikace odpovídá na třídu produktu nabídky od vydavatele. Pokud máte nový typ řešení nebo aplikace, kterou chcete zpřístupnit v Marketplace, můžete ho nastavit jako novou nabídku. Nabídka je kolekce SKU. Všechny nabídky se zobrazí jako vlastní entity v Marketplace.

### <a name="sku"></a>Skladová jednotka (SKU)

SKU je nejmenší nabízené ke koupi jednotkou nabídku. Skladová položka v rámci stejné třídy produktu (nabídka) můžete použít k rozlišení mezi:

* Různé funkce, které jsou podporovány.
* Jestli je nabídka spravované nebo nespravované.
* Modely fakturace, které jsou podporovány.

V případě nadřazené nabídky na Marketplace se zobrazí SKU. Zobrazí se jako vlastní nabízené ke koupi entity na portálu Azure.

### <a name="set-up-an-offer"></a>Nastavit nabídky

1. Přihlaste se k [portál Cloud Partner](https://cloudpartner.azure.com/).

2. V navigačním podokně na levé straně vyberte **+ nové nabídky** > **aplikací Azure**.

    ![Nová nabídka](./media/managed-application-author-marketplace/newOffer.png)

3. Vyplnění formuláře, které se zobrazují na levé straně v **Editor** zobrazení. Povinná pole jsou označené červenou hvězdičkou (*).

    ![Nabídka nastavení](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Čtyři hlavní forms slouží k vytváření spravované aplikaci:

    a. Nabídka nastavení

    b. SKU

    c. Marketplace

    d. Podpora

Tyto formuláře jsou popsány podrobněji v následujících částech.

## <a name="offer-settings-form"></a>Nabídka nastavení formuláře
Tento základní formulář použijte pro zadání nastavení nabídky.

1. Vyplňte **nabízejí nastavení** formuláře. Různých polí jsou:

    a. **ID nabídky**: Tento jedinečný identifikátor identifikuje nabídku v rámci profilu vydavatele. Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy. Může být složené jenom malé alfanumerické znaky nebo pomlčky (-). ID nemůže končit pomlčkou. Jeho omezeno na maximálně 50 znaků. Po nabídku přejde za provozu, toto pole je uzamčené.

    b. **ID vydavatele**: pomocí tohoto rozevíracího seznamu vyberte vydavatele profil, který chcete publikovat v rámci této nabídky. Po nabídku přejde za provozu, toto pole je uzamčené.

    c. **Název**: Tento název zobrazení pro danou nabídku se zobrazí v Marketplace. a na portálu. Může mít maximálně 50 znaků. Zahrnují srozumitelný název značky pro svůj produkt. Nezadávejte název společnosti, pokud to není, jak je na trh. Pokud jste marketingové v rámci této nabídky na vlastní web, ujistěte se, zda je název přesně jak se zobrazuje na vašem webu.

2. Vyberte **Uložit** k rozdělanou práci uložit. 

## <a name="skus-form"></a>SKU formuláře
Dalším krokem je přidání SKU pro vaši nabídku.

1. Vyberte **SKU** > **nové SKU**. 

    ![Vyberte nové SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Zadejte **SKU ID**. SKU ID je jedinečný identifikátor pro SKU v rámci nabídku. Toto ID je viditelná v adresách URL produktu, šablony Resource Manageru a fakturace sestavy. Může být složené jenom malé alfanumerické znaky nebo pomlčky (-). ID nemůže končit pomlčkou a jeho omezeno na maximálně 50 znaků. Po nabídku přejde za provozu, toto pole je uzamčené. Můžete mít více SKU v rámci nabídku. Je nutné SKU pro každé bitové kopie, které chcete publikovat.

3. Vyplňte **SKU podrobnosti** část v následující podobě:

    ![Zadejte nové SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Vyplňte následující pole:
    
    a. **Název**: Zadejte název pro tento SKU. Tento název se zobrazí v galerii pro tuto položku.

    b. **Souhrn**: Zadejte krátký souhrn pro tento SKU. Tento text se zobrazí pod názvem.

    c. **Popis**: Zadejte podrobný popis o verze SKU.

    d. **Typ SKU**: povolené hodnoty jsou **spravované aplikace** a **šablony řešení**. Pro tento případ, vyberte **spravované aplikace**.

4. Vyplňte **podrobnosti balíčku** část v následující podobě:

    ![Balíček](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Vyplňte následující pole:

    a. **Aktuální verze**: Zadejte verzi balíčku můžete nahrávat na server. Musí být ve formátu `{number}.{number}.{number}{number}`.

    b. **Vyberte soubor balíčku**: Tento balíček obsahuje následující soubory, které jsou komprimované a do souboru .zip:
    * **applianceMainTemplate.json**: soubor šablony nasazení, který se používá k nasazení řešení nebo aplikace. Informace o tom, jak vytvořit nasazení soubory šablony najdete v tématu [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: Tento soubor se používá na portálu Azure ke generování uživatelského rozhraní, které slouží ke zřízení tohoto řešení aplikace. Další informace najdete v tématu [začít pracovat s CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: Tento soubor šablony obsahuje pouze Microsoft.Solution/appliances prostředků. Soubor mainTemplate zahrnuje následující vlastnosti:

        *  **druh**: použití **Marketplace** spravovaných aplikací na webu Marketplace.
        *  **ManagedResourceGroupId**: Tato skupina prostředků v předplatném zákazníka je, kde jsou všechny prostředky, které jsou definované v applianceMainTemplate.json nasazen.
        *  **PublisherPackageId**: Tento řetězec jednoznačně identifikuje balíčku. Zadejte hodnotu ve formátu `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Získat **nabízejí ID** a **ID vydavatele** z portálu pro publikování, jak je znázorněno na následujícím obrázku:

![ID nabídky](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Získat **SKU ID**, jak je znázorněno na následujícím obrázku:

![SKU ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Získání balíčku **verze**, jak je znázorněno na následujícím obrázku:

![Verze balíčku](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Podle předchozích ukázkách, hodnota **PublisherPackageId** je `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Ukázka mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Tento balíček měl obsahovat všechny vnořené šablony nebo skripty, které jsou požadovány k přidělení úspěšně této aplikace. MainTemplate.json, applianceMainTemplate.json a applianceCreateUIDefinition.json soubory musí být přítomen v kořenové složce.

* **Povolení**: definuje tato vlastnost, který získá přístup a úroveň přístupu k prostředkům v rámci předplatných zákazníků. Vydavatel slouží ke správě aplikace jménem zákazníka.
* **PrincipalId**: Tato vlastnost je Azure Active Directory (Azure AD) identifikátor uživatele, skupiny uživatelů nebo aplikací, který má oprávnění určité s prostředky v rámci předplatného zákazníka. Definice Role jsou popsána oprávnění. 
* **Definice role**: Tato vlastnost je seznam všech uvedené vestavěné role řízení přístupu na základě Role (RBAC) poskytované Azure AD. Můžete vybrat roli, která je nejvhodnější používat ke správě prostředků jménem zákazníka.

Můžete přidat více oprávnění. Doporučujeme vytvořit skupinu uživatele AD a zadejte své ID v **PrincipalId**. Tímto způsobem můžete přidat další uživatele do skupiny uživatelů, aniž by bylo nutné aktualizovat verze SKU.

Další informace o RBAC najdete v tématu [začít pracovat s RBAC na portálu Azure](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Formulář Marketplace.

Ve formuláři Marketplace je dotaz pro pole, které se zobrazí na [Azure Marketplace](https://azuremarketplace.microsoft.com) a na [portál Azure](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>ID odběru Preview

Zadejte seznam předplatného Azure ID, kteří mohou přistupovat k nabídku po publikování. Tyto odběry uvedené prázdné můžete použít k testování nabídku náhled před jeho provedením za provozu. Seznam povolených až 100 předplatná v portálu pro partnery můžete zkompilovat.

### <a name="suggested-categories"></a>Navrhované kategorií

Vyberte až pěti kategorií ze seznamu, který může být nejlépe přidružený vaši nabídku. Tyto kategorie se používají pro mapování vaši nabídku do kategorie produktů, které jsou k dispozici v [Azure Marketplace](https://azuremarketplace.microsoft.com) a [portál Azure](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Marketplace

Souhrn spravované aplikace zobrazí následující pole:

![Souhrn Marketplace](./media/managed-application-author-marketplace/publishvm10.png)

**Přehled** kartě pro spravované aplikace zobrazí následující pole:

![Přehled webu Marketplace](./media/managed-application-author-marketplace/publishvm11.png)

**Plány + cenová** kartě pro spravované aplikace zobrazí následující pole:

![Plány Marketplace.](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>portál Azure

Souhrn spravované aplikace zobrazí následující pole:

![Portál souhrn](./media/managed-application-author-marketplace/publishvm12.png)

Přehled pro spravované aplikace zobrazí následující pole:

![Přehled portálu](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Logu

Postupujte podle těchto pokynů pro všechny logo, který nahrajete na portálu Cloud Partner:

*   Azure návrh obsahuje paletu barev jednoduché. Omezte počet primární a sekundární barev v loga.
*   Barev motivů portálu jsou bílé a černé. Nepoužívejte tyto barvy jako barvu pozadí pro loga. Použijte barvu, která umožňuje loga viditelného na portálu. Doporučujeme, abyste jednoduché primární barvy. *Pokud používáte průhledné pozadí, ujistěte se, že nejsou bílé, logem a text černé nebo blue.*
*   Nepoužívejte barevného přechodu pozadí na logo.
*   Neukládejte text do logo, ani vaše společnost nebo název značky. Vzhled a chování vaše logo musí být plochý a vyhnout se přechody.
*   Zajistěte, aby že logo není roztažená.

#### <a name="hero-logo"></a>Logo nejdůležitější

Logo nejdůležitější je volitelný. Vydavatele můžete vypnout nahrávat logo nejdůležitější. Po odeslání ikonu hrdina nelze odstranit. V ten moment se musí partnerovi postupujte podle pokynů Marketplace pro nejdůležitější ikony.

Postupujte podle těchto pokynů pro ikonu logo nejdůležitější:

*   Zobrazovaný název vydavatele, název plánu a nabídky dlouhé shrnutí jsou zobrazeny v prázdné. Proto nepoužívejte světla barvu pozadí ikonu nejdůležitější. Černé, bílé nebo průhledné pozadí není povolena pro nejdůležitější ikony.
*   Po nabídka je uvedena, vydavatele zobrazí název, plán title, nabídku dlouhé shrnutí a **vytvořit** tlačítko jsou uvnitř logo hrdina vložených prostřednictvím kódu programu. V důsledku toho není zadat libovolný text při návrhu logo nejdůležitější. Vzhledem k tomu, že je text do tohoto místa zahrnuto prostřednictvím kódu programu, ponechte prázdné místo na pravé straně. Prázdné místo pro text by mělo být 415 × 100 pixelů na pravé straně. Posunut 370 pixelů od levého okraje.

    ![Příklad logo nejdůležitější](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Podpora formuláře

Vyplňte **podporu** formuláře s podporou kontaktuje z vaší společnosti. Tyto informace může technici, kontakty a kontakty podpory zákazníků.

## <a name="publish-an-offer"></a>Publikování nabídky

Po vyplnění všech oddílů, vyberte **publikovat** ke spuštění procesu, který zpřístupní vaši nabídku pro zákazníky.

## <a name="next-steps"></a>Další kroky

* Úvod do spravovaných aplikací, najdete v části [spravované aplikace přehled](managed-application-overview.md).
* Informace o využívání spravované aplikace z webu Marketplace najdete v tématu [využívat Azure spravované aplikace na webu Marketplace](managed-application-consume-marketplace.md).
* Informace o publikování aplikace spravované katalogu služeb najdete v tématu [vytvoření a publikování aplikace spravované katalogu služeb](managed-application-publishing.md).
* Informace o použití katalogu služeb spravovaných aplikací najdete v tématu [využívat katalogu služeb, které jsou spravované aplikace](managed-application-consumption.md).
