<properties
   pageTitle="Použití VS Code se šablonami Resource Manageru | Microsoft Azure"
   description="Zobrazuje, jak nastavit Visual Studio Code pro vytvoření šablony Azure Resource Manageru."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="cmatskas"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="chmatsk;tomfitz"/>


# Práce s šablonami Azure Resource Manageru ve Visual Studio Code

Šablony Azure Resource Manageru jsou soubory JSON, které popisují prostředek a související závislosti. Tyto soubory mohou být někdy velké a složité, proto jsou důležité podpůrné nástroje. Visual Studio Code je nový, jednoduchý, open source editor kódu pro různé platformy. Podporuje vytváření a úpravy šablon Resource Manageru prostřednictvím [nového rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools). VS Code běží všude a nevyžaduje přístup k internetu, pokud nechcete své šablony Resource Manageru také rovnou nasadit.

Pokud VS Code ještě nemáte, můžete si jej nainstalovat z adresy [https://code.visualstudio.com/](https://code.visualstudio.com/).

## Instalace rozšíření Resource Manageru

Pokud chcete ve VS Code pracovat s šablonami JSON, musíte nainstalovat rozšíření. Následujícím postupem si stáhněte a nainstalujte jazykovou podporu pro šablony JSON Resource Manageru:

1. Spusťte VS Code 
2. Otevřete okno rychlého otevření (Ctrl + P) 
3. Spusťte následující příkaz: 

        ext install azurerm-vscode-tools

4. Až budete vyzváni, ukončete a znovu spusťte VS Code, aby se rozšíření povolilo. 

 Hotovo!

## Nastavení fragmentů Resource Manageru

V předchozích krocích byla nainstalována podpora nástrojů, nyní je však potřeba nakonfigurovat VS Code pro použití fragmentů šablony JSON.

1. Zkopírujte obsah souboru z úložiště [azure-xplat-arm-tooling](https://raw.githubusercontent.com/Azure/azure-xplat-arm-tooling/master/VSCode/armsnippets.json) do schránky.
2. Spusťte VS Code 
3. Ve VS Code můžete otevřít soubor fragmentů JSON buď tak, že přejdete do **File** (Soubor)  -> **Preferences** (předvolby)  -> **User Snippets** (uživatelské úryvky)  -> **JSON**, nebo vyberte **F1** a začnete zadávat **preferences**, abyste mohli vybrat **Preferences: Snippets**.

    ![předvolby fragmentů](./media/resource-manager-vs-code/preferences-snippets.png)

    Z nabízených možností vyberte **JSON**.

    ![výběr json](./media/resource-manager-vs-code/select-json.png)

4. Vložte obsah souboru z kroku 1 do souboru uživatelských fragmentů před koncovou složenou závorku „}“ 
5. Ujistěte se, JSON vypadá v pořádku a nikde nejsou žádná podtržení vlnovkou. 
6. Uložte a zavřete soubor uživatelských fragmentů.

To je vše, co je potřeba k zahájení práce s fragmenty Resource Manageru. Dále toto nastavení otestujeme.

## Práce s šablonami ve VS Code

Nejjednodušší způsob, jak začít pracovat s šablonou, je buď stáhnout si jednu ze šablon pro rychlý start, které jsou dostupné na [Githubu](https://github.com/Azure/azure-quickstart-templates), nebo použít vlastní. Prostřednictvím portálu můžete snadno [exportovat šablonu](resource-manager-export-template.md) pro jakoukoli ze svých skupin prostředků. 

1. Pokud jste exportovali šablonu ze skupiny prostředků, otevřete extrahované soubory ve VS Code.

    ![zobrazení souborů](./media/resource-manager-vs-code/show-files.png)

2. Otevřete soubor template.json, abyste jej mohli upravit a přidat některé další prostředky. Za **"resources": [** přidejte stisknutím klávesy enter nový řádek.. Pokud zadáte **arm**, zobrazí se seznam možností. Tyto možnosti jsou fragmenty šablony, které jste nainstalovali. Mělo by to vypadat takto: 

    ![zobrazení fragmentů](./media/resource-manager-vs-code/type-snippets.png)

3. Zvolte požadovaný fragment. V tomto článku volíme **arm ip**, abychom vytvořili novou veřejnou IP adresu. Za uzavírací závorku „}“ nově vytvořeného prostředku vložte čárku, abyste byla syntaxe šablony platná.

     ![přidání čárky](./media/resource-manager-vs-code/add-comma.png)

4. VS Code má integrovanou technologii IntelliSense. Při úpravách šablony VS Code navrhuje dostupné hodnoty. Pokud například chcete přidat do šablony část proměnné, přidejte **""** (dvě dvojité uvozovky) a mezi těmito uvozovkami vyberte **Ctrl + mezerník**. Zobrazí se možnosti, mezi nimi **variables** (proměnné).

    ![přidání proměnných](./media/resource-manager-vs-code/add-variables.png)

5. Technologie IntelliSense může také navrhovat dostupné hodnoty nebo funkce. Pokud chcete nastavit vlastnost na hodnotu parametru, vytvořte výraz s **"[]"** a **Ctrl + mezerník**. Můžete začít zadávat název funkce. Po nalezení požadované funkce stiskněte klávesu **Tab**.

    ![přidání parametru](./media/resource-manager-vs-code/select-parameters.png)

6. Uvnitř funkce znovu vyberte **Ctrl + mezerník**, tím zobrazíte seznam dostupných parametrů ve vaší šabloně.

    ![přidání parametru](./media/resource-manager-vs-code/select-avail-parameters.png)

7. Pokud jsou v šabloně jakékoli problémy s ověřením schématu, zobrazí se v editoru známé podtržení vlnovkou. Seznam chyb a upozornění můžete zobrazit zadáním **Ctrl + Shift + M** nebo výběrem piktogramů v levém dolním stavovém řádku.

    ![chyby](./media/resource-manager-vs-code/errors.png)

    Ověření šablony vám může pomoci odhalit problémy syntaxe. Mohou se nicméně objevit i chyby, které můžete ignorovat. V některých případech editor porovnává vaši šablonu se schématem, které není aktuální, a proto hlásí chybu, i když víte, že daná věc je v pořádku. Předpokládejme například, že do Resource Manageru byla nedávno přidána funkce, ale schéma nebylo aktualizováno. Editor hlásí chybu navzdory tomu, že funkce při nasazení bude fungovat správně.

    ![chybová zpráva](./media/resource-manager-vs-code/unrecognized-function.png)

## Nasazení nových prostředků

Když je šablona připravena, můžete nasadit nové prostředky podle následujících pokynů: 

### Windows

1. Otevřete příkazový řádek prostředí PowerShell. 
2. Přihlaste se zadáním příkazu: 

        Login-AzureRmAccount 

3. Pokud máte více předplatných, získejte seznam předplatných příkazem:

        Get-AzureRmSubscription

    A vyberte předplatné, které chcete použít.
   
        Select-AzureRmSubscription -SubscriptionId <Subscription Id>

4. Aktualizujte parametry v souboru parameters.json.
5. Spuštěním Deploy.ps1 nasaďte svou šablonu do služby Azure.

### OSX/Linux

1. Otevřete okno terminálu. 
2. Přihlaste se zadáním příkazu:

        azure login 

3. Pokud máte více předplatných, vyberte správné předplatné příkazem:

        azure account set <subscriptionNameOrId> 

4. Aktualizujte parametry v souboru parameters.json.
5. Nasaďte šablonu (pokud chcete) příkazem:

        azure group deployment create -f <PathToTemplate> 

## Další kroky

- Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).
- Další informace o funkcích šablon najdete v článku [Funkce šablon Azure Resource Manageru](resource-group-template-functions.md).
- Další příklady práce s nástrojem Visual Studio Code najdete v článku o [sestavení cloudových aplikací v nástroji Visual Studio Code](https://github.com/Microsoft/HealthClinic.biz/wiki/Build-cloud-apps-with-Visual-Studio-Code) z materiálu [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [(ukázka)](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/). Další rychlé starty z ukázky HealthClinic.biz najdete v článku [Rychlé starty nástrojů pro vývojáře Azure](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).



<!--HONumber=Sep16_HO4-->


