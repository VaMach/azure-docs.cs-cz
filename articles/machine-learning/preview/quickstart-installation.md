---
title: "Stručný průvodce instalací služeb Azure Machine Learning | Dokumentace Microsoftu"
description: "V tomto stručném průvodci se dozvíte, jak vytvořit prostředky služby Azure Machine Learning a nainstalovat aplikaci Azure Machine Learning Workbench."
services: machine-learning
author: hning86
ms.author: haining, raymondl, chhavib
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: hero-article
ms.date: 10/13/2017
ms.openlocfilehash: 7c74a9ebaae0b027277fe282b958a653ab498fc5
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Vytvoření účtů služby Azure Machine Learning verze Preview a instalace aplikace Azure Machine Learning Workbench
Azure Machine Learning je integrované, komplexní řešení datové vědy a pokročilé analýzy. Pomáhá profesionálním datovým vědcům připravovat data, vyvíjet experimenty a nasazovat modely na úrovni cloudu.

V tomto kurzu Rychlý start se dozvíte, jak ve službě Azure Machine Learning verze Preview vytvořit účty služeb Experimentování a Správa modelů. Také se dozvíte, jak nainstalovat desktopovou aplikaci Azure Machine Learning Workbench a nástroje příkazového řádku. Potom přejdete na stručný přehled funkcí služby Azure Machine Learning verze Preview, ve kterém pomocí nadčasové [datové sady Iris](https://en.wikipedia.org/wiki/iris_flower_data_set) vytvoříte model pro předpověď typu dat Iris na základě některých fyzických charakteristik.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

V současné době můžete desktopovou aplikaci Azure Machine Learning Workbench nainstalovat jenom v následujících operačních systémech: 
- Windows 10
- Windows Server 2016
- macOS Sierra
- macOS High Sierra

## <a name="sign-in-to-the-azure-portal"></a>Přihlášení k webu Azure Portal
Přihlaste se k webu [Azure Portal](https://portal.azure.com/).

## <a name="create-azure-machine-learning-accounts"></a>Vytvoření účtů služby Azure Machine Learning
Ke zřízení účtů služby Azure Machine Learning použijte web Azure Portal: 
1. Vyberte tlačítko **Nový** (+) v levém horním rohu portálu.

2. Do panelu vyhledávání zadejte **Machine Learning**. Vyberte výsledek hledání s názvem **Experimentování ve službě Machine Learning (Preview)**.  Kliknutím na ikonu hvězdičky zařaďte tuto volbu mezi oblíbené položky na webu Azure Portal.

   ![Vyhledání služby Azure Machine Learning](media/quickstart-installation/portal-more-services.png)

3. Vyberte **+ Přidat** a nakonfigurujte nový účet služby Experimentování ve službě Machine Learning. Otevře se podrobný formulář.

   ![Účet služby Experimentování ve službě Machine Learning](media/quickstart-installation/portal-create-experimentation.png)

4. Do formuláře služby Experimentování ve službě Azure Machine Learning zadejte následující informace:

   Nastavení|Navrhovaná hodnota|Popis
   ---|---|---
   Název účtu služby Experimentování | _Jedinečný název_ |Vyberte jedinečný název, který identifikuje váš účet. Můžete použít své jméno nebo třeba název oddělení nebo projektu, který experiment nejlépe identifikuje. Název musí mít délku od 2 do 32 znaků. Může obsahovat jenom alfanumerické znaky a znak spojovníku (-). 
   Předplatné | _Vaše předplatné_ |Zvolte předplatné Azure, které chcete pro experiment použít. Pokud máte více předplatných, zvolte odpovídající předplatné, ve kterém se prostředek účtuje.
   Skupina prostředků | _Vaše skupina prostředků_ | Můžete vytvořit nový název skupiny prostředků nebo použít název některé ze stávajících skupin prostředků ve vašem předplatném.
   Umístění | _Oblast nejbližší vašim uživatelům_ | Zvolte umístění co nejblíže vašim uživatelům a datovým prostředkům.
   Počet licencí | 2 | Zadejte počet licencí. Tato volba ovlivňuje [ceny](https://azure.microsoft.com/pricing/details/machine-learning/). První dvě licence jsou zdarma. Pro účely tohoto stručného průvodce jsou potřebné dvě licence. Počet licencí můžete později podle potřeby aktualizovat na portálu Azure.
   Účet úložiště | _Jedinečný název_ | Vyberte **Vytvořit nový** a zadejte název pro vytvoření účtu úložiště Azure. Nebo vyberte **Použít existující** a z rozevíracího seznamu vyberte existující účet úložiště. Účet úložiště je povinný a slouží k ukládání artefaktů projektu a dat historie spuštění. 
   Účet aplikace Workspace pro experimentování | _Jedinečný název_ | Zadejte název nového pracovního prostoru. Název musí mít délku od 2 do 32 znaků. Může obsahovat jenom alfanumerické znaky a znak spojovníku (-).
   Přiřazení vlastníka pracovního prostoru | _Váš účet_ | Jako vlastníka pracovního prostoru vyberte svůj vlastní účet.
   Vytvoření účtu služby Správa modelů | *zaškrtnutí* | V rámci uživatelského prostředí při vytváření účtu služby Experimentování máte možnost vytvořit také účet služby Správa modelů ve službě Machine Learning. Tento prostředek využijete, až budete připraveni nasadit a spravovat své modely jako webové služby v reálném čase. Doporučujeme vytvořit služby Správa modelů zároveň s účtem služby Experimentování.
   Název účtu | _Jedinečný název_ | Vyberte jedinečný název, který identifikuje váš účet služby Správa modelů. Můžete použít své jméno nebo třeba název oddělení nebo projektu, který experiment nejlépe identifikuje. Název musí mít délku od 2 do 32 znaků. Může obsahovat jenom alfanumerické znaky a znak spojovníku (-). 
   Cenová úroveň služby Správa modelů | **DEVTEST** | Vyberte **Není vybraná žádná cenová úroveň** a určete cenovou úroveň svého nového účtu služby Správa modelů. Z důvodu úspory nákladů vyberte cenovou úroveň **DEVTEST**, pokud je ve vašem předplatném dostupná (omezená dostupnost). Jinak kvůli úspoře nákladů vyberte cenovou úroveň S1. Kliknutím na možnost **Vybrat** uložte volbu cenové úrovně. 
   Připnutí na řídicí panel | _zaškrtnutí_ | Výběrem možnosti **Připnout na řídicí panel** povolíte snadné sledování vašeho účtu služby Experimentování ve službě Machine Learning na úvodní stránce řídicího panelu na webu Azure Portal.

5. Vyberte **Vytvořit** a spusťte proces vytváření.

6. Pokud chcete monitorovat proces nasazení, klikněte na panelu nástrojů webu Azure Portal na **Oznámení** (ikona zvonku). 

   Zobrazí se oznámení **Probíhá nasazení**. Po dokončení se stav změní na **Nasazení bylo úspěšné**. Po úspěšném nasazení se otevře stránka účtu služby Experimentování ve službě Machine Learning.
   
   ![Oznámení portálu Azure Portal](media/quickstart-installation/portal-notification.png)

Teď přejděte do jedné ze dvou částí v závislosti na tom, jaký operační systém používáte v místním počítači, a nainstalujte aplikaci Azure Machine Learning Workbench. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Instalace aplikace Azure Machine Learning Workbench ve Windows
Aplikaci Azure Machine Learning Workbench můžete nainstalovat do počítače se systémem Windows 10, Windows Server 2016 nebo novějším.

1. Stáhněte si nejnovější verzi instalačního programu aplikace Azure Machine Learning Workbench [AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi).

2. V Průzkumníku souborů dvakrát klikněte na stažený instalační program **AmlWorkbenchSetup.msi**.

   >[!IMPORTANT]
   >Stáhněte celý instalační program na disk a spusťte jej. Nespouštějte jej přímo z widgetu stahování v prohlížeči.

3. Dokončete instalaci podle pokynů na obrazovce.

   Instalační program stáhne všechny potřebné závislé komponenty, jako je Python, Miniconda a další související knihovny. Instalace všech komponent může trvat přibližně půl hodiny. 

4. Aplikace Azure Machine Learning Workbench je teď nainstalovaná v tomto adresáři:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>Instalace aplikace Azure Machine Learning Workbench v systému macOS
Nainstalujte aplikaci Azure Machine Learning Workbench na svém počítači se systémem macOS Sierra nebo novějším.

1. Stáhněte si nejnovější verzi instalačního programu aplikace Azure Machine Learning Workbench [AmlWorkbench.dmg](https://aka.ms/azureml-wb-dmg).

   >[!IMPORTANT]
   >Stáhněte celý instalační program na disk a spusťte jej. Nespouštějte jej přímo z widgetu stahování v prohlížeči.

2. V aplikaci Finder dvakrát klikněte na stažený instalační program **AmlWorkbench.dmg**.

3. Dokončete instalaci podle pokynů na obrazovce.

   Instalační program stáhne všechny potřebné závislé komponenty, jako je Python, Miniconda a další související knihovny. Instalace všech komponent může trvat přibližně půl hodiny. 

4. Aplikace Azure Machine Learning Workbench je teď nainstalovaná v tomto adresáři: 

   `/Applications/AmlWorkbench.app`

## <a name="run-azure-machine-learning-workbench-to-sign-in-for-the-first-time"></a>Spuštění aplikace Azure Machine Learning Workbench a první přihlášení
1. Po dokončení procesu instalace vyberte na poslední obrazovce instalačního programu tlačítko pro **spuštění aplikace Workbench**. Pokud jste instalační program zavřeli, na ploše nebo v nabídce **Start** najdete zástupce aplikace Machine Learning Workbench s názvem **Azure Machine Learning Workbench**, pomocí kterého můžete aplikaci spustit.

2. Přihlaste se do aplikace Workbench pomocí stejného účtu, který jste předtím použili ke zřízení prostředků Azure. 

3. Po úspěšném procesu přihlášení se aplikace Workbench pokusí najít účty služby Experimentování ve službě Machine Learning, které jste vytvořili dříve. Prohledá všechna předplatná Azure, ke kterým mají vaše přihlašovací údaje přístup. Po nalezení alespoň jednoho účtu služby Experimentování se aplikace Workbench otevře s tímto účtem. Pak zobrazí seznam pracovních prostorů a projektů, které v tomto účtu našla. 

   >[!TIP]
   > Pokud máte přístup k více účtům služby Experimentování, můžete výběrem ikony avataru v levém spodním rohu aplikace Workbench přepnout na jiný účet.

Informace o vytvoření prostředí pro nasazení vašich webových služeb najdete v tématu [Nastavení prostředí nasazení](deployment-setup-configuration.md).

## <a name="create-a-new-project"></a>Vytvoření nového projektu
1. Spusťte aplikaci Azure Machine Learning Workbench a přihlaste se. 

2. Vyberte **Soubor** > **Nový projekt** (nebo vyberte znaménko **+** v podokně **PROJEKTY**). 

3. Vyplňte pole **Název projektu** a **Adresář projektu**. Pole **Popis projektu** je volitelné, ale užitečné. Pole **adresy URL úložiště GIT Visualstudio.com** nechte zatím prázdné. Vyberte si pracovní prostor a vyberte šablonu projektu **Klasifikace Iris**.

   >[!TIP]
   >Volitelně můžete do pole úložiště Git zadat adresu URL úložiště Git hostovaného v projektu [Visual Studio Team Services](https://www.visualstudio.com). Toto úložiště Git už musí existovat, musí být prázdné a nesmí obsahovat žádnou hlavní větev. Také pro něj musíte mít oprávnění k zápisu. Pokud teď přidáte úložiště Git, můžete později povolit scénáře roamingu a sdílení. [Přečtěte si další informace](using-git-ml-project.md).

4. Vyberte tlačítko **Vytvořit** a vytvořte nový projekt. Vytvoří se nový projekt a otevře se. Teď můžete prozkoumat domovskou stránku projektu, zdroje dat, poznámkové bloky a soubory zdrojového kódu. 

    >[!TIP]
    >Také můžete projekt otevřít v nástroji Visual Studio Code nebo jiném editoru, a to jednoduše tak, že nakonfigurujete odkaz na integrované vývojové prostředí (IDE) a potom v něm otevřete adresář projektu. [Přečtěte si další informace](how-to-configure-your-IDE.md). 

## <a name="run-a-python-script"></a>Spuštění skriptu v jazyce Python
Teď spustíme skript na vašem místním počítači. 

1. Každý projekt se otevře na vlastní stránce **Řídicí panel Projekt**. Na příkazovém řádku v horní části aplikace vyberte cíl spuštění **local** (místní) a skript ke spuštění **iris_sklearn.py**. Ukázka obsahuje další soubory, které si můžete prohlédnout později. 

   ![Panel příkazů](media/quickstart-installation/run_control.png)

2. Do textového pole **Argumenty** zadejte **0.01**. Toto číslo se používá v kódu k nastavení míry regularizace. Tato hodnota se používá ke konfiguraci způsobu trénování modelu lineární regrese. 

3. Vyberte tlačítko **Spustit** a zahajte spouštění souboru **iris_sklearn.py** na vašem počítači. 

   Tento kód používá k sestavení modelu algoritmus [logistické regrese](https://en.wikipedia.org/wiki/logistic_regression) z oblíbené knihovny [scikit-learn](http://scikit-learn.org/stable/index.html) jazyka Python.

4. Zprava se vysune panel **Úlohy**, pokud ještě nebyl viditelný, a přidá se na něj úloha **iris_sklearn**. Jakmile se úloha spustí, její stav se během několika sekund změní z hodnoty **Odesílání** na **Spuštěno** a potom na **Dokončeno**. 

   Blahopřejeme. Úspěšně jste spustili skript Pythonu v aplikaci Azure Machine Learning Workbench.

6. Kroky 2–4 několikrát zopakujte. Pokaždé při tom použijte jiné hodnoty argumentu v rozsahu od **10** do **0.001**.

## <a name="view-run-history"></a>Zobrazení historie spuštění
1. Přejděte do zobrazení **Spuštění** a v seznamu spuštění vyberte **iris_sklearn.py**. Otevře se řídicí panel historie spuštění souboru **iris_sklearn.py**. Ten obsahuje každé spuštění souboru **iris_sklearn.py**. 

   ![Řídicí panel historie spuštění skriptu](media/quickstart-installation/run_view.png)

2. Řídicí panel historie spuštění také obsahuje hlavní metriky, sadu výchozích grafů a seznam metrik pro každé spuštění. Toto zobrazení si můžete přizpůsobit řazením, filtrováním a úpravou konfigurací. Stačí vybrat ikonu konfigurace nebo filtru.

   ![Metriky a grafy](media/quickstart-installation/run_dashboard.png)

3. Výběrem dokončeného spuštění můžete zobrazit podrobný přehled tohoto konkrétního spuštění. Podrobnosti zahrnují další metriky, vytvořené soubory a další potenciálně užitečné protokoly.

## <a name="next-steps"></a>Další kroky
Teď jste úspěšně vytvořili účet služby Experimentování ve službě Azure Machine Learning a účet služby Správa modelů ve službě Azure Machine Learning. Nainstalovali jste desktopovou aplikaci Azure Machine Learning Workbench a rozhraní příkazového řádku. Vytvořili jste nový projekt, vytvořili model spuštěním skriptu a prozkoumali historii spuštění tohoto skriptu.

Podrobnější poznatky o tomto pracovním postupu, včetně postupu nasazení modelu Iris jako webové služby, najdete v kurzu *Klasifikace Iris* na pokračování. Tento kurz obsahuje podrobné postupy pro [přípravu dat](tutorial-classifying-iris-part-1.md), [experimentování](tutorial-classifying-iris-part-2.md) a [správu modelů](tutorial-classifying-iris-part-3.md). 

> [!div class="nextstepaction"]
> [Kurz Klasifikace Iris](tutorial-classifying-iris-part-1.md)
