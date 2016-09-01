<properties
   pageTitle="Projekty skupiny prostředků Azure v sadě Visual Studio | Microsoft Azure"
   description="Pomocí sady Visual Studio můžete vytvořit projekt skupiny prostředků Azure a nasadit tyto prostředky do Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="tomfitz" />

# Vytvoření a nasazení skupiny prostředků Azure pomocí sady Visual Studio

Pomocí sady Visual Studio a [Azure SDK](https://azure.microsoft.com/downloads/) můžete vytvořit projekt, který nasadí vaši infrastrukturu a kód do Azure. Můžete například definovat webového hostitele, webový server a databázi pro vaši aplikaci a nasadit tuto infrastrukturu spolu s kódem. Nebo můžete definovat virtuální počítač, virtuální síť a účet úložiště a nasadit tuto infrastrukturu spolu se skriptem, který se spouští na virtuálním počítači. Projekt nasazení **skupiny prostředků Azure** umožňuje nasadit všechny potřebné prostředky v rámci jedné opakovatelné operace. Další informace o nasazení a správě prostředků najdete v tématu [Přehled Azure Resource Manageru](resource-group-overview.md).

Projekty skupiny prostředků Azure obsahují šablony JSON pro Azure Resource Manager, které definují prostředky, nasazované do Azure. Další informace o jednotlivých prvcích šablony Resource Manageru najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md). Visual Studio umožňuje upravit tyto šablony a poskytuje nástroje, které práci s nimi usnadňují.

V tomto tématu nasadíte webovou aplikaci a databázi SQL. Postup je však téměř stejný pro libovolný typ prostředku. Stejně snadno můžete nasadit virtuální počítač a prostředky, které s ním souvisejí. Visual Studio poskytuje řadu různých předem připravených šablon pro běžné scénáře nasazení.

V tomto článku je použita sada Visual Studio 2015 Update 2 a Microsoft Azure SDK pro .NET 2.9. Pokud použijete Visual Studio 2013 a Azure SDK 2.9, bude vaše práce v podstatě stejná. Sadu Azure SDK můžete využít ve verzi 2.6 nebo novější. Podoba uživatelského rozhraní se ale může lišit od příkladů uvedených v tomto článku. Důrazně doporučujeme, abyste si před zahájením těchto kroků nainstalovali nejnovější verzi sady [Azure SDK](https://azure.microsoft.com/downloads/). 

## Vytvoření projektu skupiny prostředků Azure

V tomto postupu vytvoříte projekt skupiny prostředků Azure pomocí šablony **Web app + SQL** (Webová aplikace a SQL).

1. V sadě Visual Studio zvolte **Soubor**, **Nový projekt** a potom zvolte **C#** nebo **Visual Basic**. Potom vyberte **Cloud** a pak zvolte projekt **Azure Resource Group** (Skupina prostředků Azure).

    ![Projekt nasazení v cloudu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Zvolte šablonu, kterou chcete nasadit do Azure Resource Manageru. Všimněte si, že máte spoustu různých možností v závislosti na typu projektu, který chcete nasadit. Pro toto téma si vyberte šablonu **Web app + SQL** (Webová aplikace a SQL).

    ![Volba šablony](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Šablona, kterou vyberete, je jenom výchozí bod, podle potřeb vašeho scénáře můžete prostředky přidat nebo odebrat.

    >[AZURE.NOTE] Visual Studio načte seznam dostupných šablon online. Seznam se může změnit.

    Visual Studio vytvoří projekt nasazení skupiny prostředků pro webovou aplikaci a SQL Database.

1. Pokud si chcete vytvořený projekt prohlédnout, rozbalte uzly v projektu nasazení.

    ![zobrazení uzlů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Vzhledem k tomu, žen jsme pro tento příklad vybrali šablonu Web app + SQL, zobrazí se následující soubory. 

  	|Název souboru|Popis|
  	|---|---|
  	|Deploy-AzureResourceGroup.ps1|Skript PowerShellu, který vyvolá příkazy PowerShellu pro nasazení Azure Resource Manageru.<br />**Poznámka** Sada Visual Studio tento skript PowerShellu využívá k nasazení vaší šablony. Libovolné změny tohoto skriptu ovlivní také nasazení v sadě Visual Studio, proto buďte opatrní.|
  	|WebSiteSQLDatabase.json|Šablona Resource Manageru, která definuje infrastrukturu, kterou chcete nasadit do Azure, a parametry, které můžete během nasazení zadat. Definuje také závislosti mezi prostředky, takže je Resource Manager nasadí ve správném pořadí.|
  	|WebSiteSQLDatabase.parameters.json|Soubor parametrů, který obsahuje hodnoty požadované šablonou. Předáním hodnot těchto parametrů přizpůsobujete jednotlivá nasazení.|

    Tyto základní soubory obsahují všechny projekty nasazení skupiny prostředků. Ostatní projekty mohou obsahovat i další soubory, které podporují jiné funkce.

## Přizpůsobení šablony Resource Manageru

Projekt nasazení lze přizpůsobit úpravou šablony JSON s popisem prostředků, které chcete nasadit. JSON je zkratka pro JavaScript Object Notation. Jedná se o serializovaný datový formát, se kterým se snadno pracuje. Soubory JSON využívají schéma, na které se odkazujete na začátku každého souboru. Pokud chcete schématu lépe porozumět, můžete si ho stáhnout a prohlédnout. Schéma definuje platné prvky, typy a formáty polí, přípustné výčtové hodnoty a tak dále. Další informace o jednotlivých prvcích šablony Resource Manageru najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).

Pokud chcete se svou šablonou začít pracovat, otevřete **WebSiteSQLDatabase.json**.

Editor sady Visual Studio poskytuje nástroje, které vám s úpravami šablony Resource Manageru pomohou. Okno **JSON Outline** (Osnova JSON) usnadňuje zobrazení prvků, které jsou v šabloně definované.

![zobrazení osnovy JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Výběrem libovolné prvku v osnově přejdete k příslušné části šablony a zvýrazní se odpovídající JSON.

![navigace JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Pokud chcete přidat prostředek, můžete buď použít tlačítko **Přidat prostředek** v horní části okna s osnovou JSON, nebo kliknout pravým tlačítkem na **resources** a vybrat **Přidat nový prostředek**.

![přidání prostředku](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

V tomto kurzu vyberte **účet úložiště** a pojmenujte ho. Název účtu úložiště musí obsahovat jenom čísla a malá písmena a musí být kratší než 24 znaků. Projekt k zadanému názvu přidá jedinečný 13znakový řetězec, proto zkontrolujte, že váš název neobsahuje více než 11 znaků.

![přidání úložiště](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Všimněte si, že se přidal nejenom tento prostředek, ale také parametr pro typ účtu úložiště a proměnná pro název účtu úložiště.

![zobrazení osnovy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametr **storageType** je předdefinovaný a obsahuje povolené typy a také výchozí typ. Tyto hodnoty můžete ponechat beze změny nebo je můžete podle potřeby upravit. Pokud nechcete, aby pomocí této šablony nasazoval účet úložiště **Premium_LRS**, stačí ho odebrat z povolených typů. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio také poskytuje technologii IntelliSense, která vám pomůže pochopit, které vlastnosti jsou při úpravě šablony dostupné. Pokud chcete třeba upravit vlastnosti pro plán služby App Service, přejděte k prostředku **HostingPlan** a přidejte hodnotu **properties**. Všimněte si, že IntelliSense zobrazuj dostupné hodnoty a poskytuje k nim také popis.

![zobrazení technologie IntelliSense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Hodnotu **numberOfWorkers** můžete nastavit na 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## Nasazení projektu skupiny prostředků do Azure

Nyní jste připraveni svůj projekt nasadit. Když nasadíte projekt skupiny prostředků Azure, nasadíte ho do skupiny prostředků Azure. Skupina prostředků je logické seskupení prostředků, které sdílejí společný životní cyklus.

1. V místní nabídce uzlu projektu nasazení zvolte **Nasadit** > **Nové nasazení**.

    ![Položka nabídky Nasadit, Nové nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Zobrazí se dialogové okno **Deploy to Resource Group** (Nasadit do skupiny prostředků).

    ![Dialogové okno nasazení do skupiny prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. V rozevíracím seznamu **Skupina prostředků** vyberte existující skupinu prostředků nebo vytvořte novou. Pokud chcete vytvořit novou skupinu prostředků, otevřete rozevírací seznam **Skupina prostředků** a vyberte **Vytvořit novou**.

    ![Dialogové okno nasazení do skupiny prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Zobrazí se dialogové okno **Vytvořit skupinu prostředků**. Přidělte skupině název a umístění a stiskněte **Vytvořit**.

    ![Dialogové okno vytvoření skupiny prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Kliknutím na **Upravit parametry** můžete upravit parametry pro nasazení. Zadejte hodnoty parametrů a stiskněte **Uložit**.

    ![Dialogové okno Upravit parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
    Možnost **Ukládat hesla do souboru parametrů jako prostý text** není bezpečná.

1. Stisknutím tlačítka **Nasadit** nasadíte projekt do Azure. Průběh nasazení uvidíte v okně **Výstup**. Nasazení může v závislosti na konfiguraci trvat několik minut. V konzole PowerShell po zobrazení výzvy zadejte heslo správce databáze. Pokud se průběh nasazení zastaví, může to být proto, že proces čeká na zadání hesla v konzole PowerShell.

    >[AZURE.NOTE] Sada Visual Studio vás může požádat o instalaci rutin Azure PowerShell. Rutiny Azure PowerShell jsou zapotřebí k úspěšnému nasazení skupin prostředků. Pokud k tomu budete vyzváni, nainstalujte je.
    
1. Po nasazení se zobrazí zpráva v okně **Výstup**, zhruba v tomto tvaru:

        ...
        15:19:19 - DeploymentName     : websitesqldatabase-0212-2318
        15:19:19 - CorrelationId      : 6cb43be5-86b4-478f-9e2c-7e7ce86b26a2
        15:19:19 - ResourceGroupName  : DemoSiteGroup
        15:19:19 - ProvisioningState  : Succeeded
        ...

1. V prohlížeči otevřete [Portál Azure](https://portal.azure.com/) a přihlaste se do svého účtu. Chcete-li skupinu prostředků zobrazit, klikněte na odkaz **Skupiny prostředků** a pak na skupinu, do které jste provedli nasazení.

    ![výběr skupiny](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Zobrazí se všechny nasazené prostředky.

    ![zobrazení prostředků](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Pokud provedete změny a chcete projekt znovu nasadit, vyberte si stávající skupinu prostředků z místní nabídky projektu skupiny prostředků Azure. V místní nabídce zvolte **Nasadit** a pak vyberte skupinu prostředků, kterou jste nasadili.

    ![Nasazená skupina prostředků Azure](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## Nasazení kódu do vaší infrastruktury

V tomto okamžiku jste nasadili infrastrukturu pro vaši aplikaci, ale zatím není v projektu nasazený žádný kód. Toto téma ukazuje, jak nasadit webovou aplikaci a tabulky SQL Database. Pokud nasazujete namísto webové aplikace virtuální počítač, budete v něm asi chtít spouštět nějaký kód. Proces nasazení kódu pro webovou aplikaci nebo pro virtuální počítač je téměř stejný.

1. V řešení sady Visual Studio přidejte **ASP.NET Web Application** (Webová aplikace ASP.NET). 

    ![přidání webové aplikace](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Vyberte **MVC** a zrušte zaškrtnutí políčka **Hostitel v cloudu**, protože tento úkol provede projekt skupiny prostředků.

    ![výběr MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Po tom, co sada Visual Studio vytvoří webovou aplikaci, přidejte v projektu skupiny prostředků odkaz na projekt webové aplikace.

    ![přidání odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Přidáním odkazu propojíte projekt webové aplikace s projektem skupiny prostředků a automaticky nastavíte tři klíčové vlastnosti.  
    
    - Pole **Additional Properties** (Další vlastnosti) obsahuje pracovní umístění balíčku pro nasazení webu, který se vloží do úložiště Azure. 
    - Pole **Include File Path** (Cesta k souboru pro zahrnutí) obsahuje cestu, kde je balíček vytvořen. Pole **Include Targets** (Zahrnout cíle) obsahuje příkaz, který proces nasazení provede. 
    - Výchozí hodnota **Build;Package** umožňuje sestavení a vytvoření balíčku pro nasazení webu (package.zip).  
    
    Profil publikování není potřeba, protože nasazení získá informace potřebné k vytvoření balíčku z nastavených vlastností.
    
      ![zobrazení odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
      
1. Přidejte k šabloně prostředek a tentokrát vyberte **Web Deploy for Web Apps** (Nasazení webu pro webové aplikace). 

    ![přidání nasazení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Znovu nasaďte projekt skupiny prostředků do skupiny prostředků. Tentokrát použijeme několik nových parametrů. Není třeba zadávat hodnoty pro **_artifactsLocation** nebo **_artifactsLocationSasToken**, ty sada Visual Studio vygeneruje automaticky. Nastavte cestu a název souboru balíčku pro nasazení.

    ![přidání nasazení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Pro **Artifact storage account** (Účet úložiště artefaktů) můžete použít účet nasazený s touto skupinou prostředků.
    
Po dokončení nasazení můžete přejít na web a ověřit, že jste úspěšně nasadili výchozí aplikaci ASP.NET.

![zobrazení nasazené aplikace](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## Další kroky

- Informace o správě prostředků prostřednictvím portálu najdete v tématu [Použití portálu Azure ke správě prostředků Azure](./azure-portal/resource-group-portal.md).
- Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](resource-group-authoring-templates.md).



<!--HONumber=Aug16_HO4-->


