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
ms.date: 09/20/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 91d2f47a528050f644973044f96c0354b91dba25
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---

# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Vytvoření účtů služby Azure Machine Learning verze Preview a instalace aplikace Azure Machine Learning Workbench
Služby Azure Machine Learning představují integrované, komplexní řešení datové vědy a pokročilé analýzy pro profesionální datové vědce, které slouží k přípravě dat, vývoji experimentů a nasazování modelů na úrovni cloudu.

V tomto stručném průvodci se dozvíte, jak ve službě Azure Machine Learning verze Preview vytvořit účty služeb Experimentování a Správa modelů. Také se dozvíte, jak nainstalovat desktopovou aplikaci Azure Machine Learning Workbench a nástroje příkazového řádku. Potom přejdete na stručný přehled funkcí služby Azure Machine Learning verze Preview, ve kterém pomocí nadčasové [datové sady Iris](https://en.wikipedia.org/wiki/iris_flower_data_set) vytvoříte model pro předpověď typu dat Iris na základě některých jeho fyzických charakteristik.  

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky
V současné době se dá aplikace Azure Machine Learning Workbench instalovat jenom do těchto operačních systémů: Windows 10, Windows Server 2016 a macOS Sierra.

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal
Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="create-azure-machine-learning-accounts"></a>Vytvoření účtů služby Azure Machine Learning
Ke zřízení účtů služby Azure Machine Learning použijte portál Azure. 
1. Vyberte tlačítko **Nový** (+) v levém horním rohu portálu.

2. Do panelu vyhledávání zadejte „Machine Learning“. Vyberte výsledek hledání s názvem **Experimentování ve službě Machine Learning (Preview)**.  Kliknutím na ikonu hvězdičky zařaďte tuto volbu mezi oblíbené položky na portálu Azure.

   ![Vyhledání služby Azure Machine Learning](media/quickstart-installation/portal-more-services.png)

3. Klikněte na tlačítko **+ Přidat** a nakonfigurujte nový účet služby Experimentování ve službě Azure Machine Learning. Otevře se podrobný formulář.

   ![Účet služby Experimentování ve službě Machine Learning](media/quickstart-installation/portal-create-experimentation.png)

4. Do formuláře služby Experimentování ve službě Azure Machine Learning zadejte následující informace:

   Nastavení|Navrhovaná hodnota|Popis
   ---|---|---
   Název účtu služby Experimentování | _Jedinečný název_ |Vyberte jedinečný název, který identifikuje váš účet. Můžete použít své jméno nebo třeba název oddělení nebo projektu, který experiment nejlépe identifikuje. Název musí mít délku od 2 do 32 znaků a smí obsahovat jenom alfanumerické znaky a znak spojovníku „-“. 
   Předplatné | _Vaše předplatné_ |Předplatné Azure, které chcete pro experiment použít. Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém se prostředek účtuje.
   Skupina prostředků | _Vaše skupina prostředků_ | Můžete vytvořit nový název skupiny prostředků nebo použít některý ze stávajících ve vašem předplatném.
   Umístění | _Oblast nejbližší vašim uživatelům_ | Zvolte umístění co nejblíže vašim uživatelům a datovým prostředkům.
   Počet licencí | 2 | Zadejte počet licencí. Tato volba má dopad na [ceny](https://azure.microsoft.com/pricing/details/machine-learning/). První dvě licence jsou zdarma. Pro účely tohoto stručného průvodce jsou potřebné dvě licence. Počet licencí můžete později podle potřeby aktualizovat na portálu Azure.
   Účet úložiště | _Jedinečný název_ | Zvolte **Vytvořit nový** a zadejte název, pokud chcete vytvořit nový účet úložiště Azure, nebo zvolte **Použít existující** a v rozevíracím seznamu vyberte existující účet úložiště. Účet úložiště je povinný a slouží k ukládání artefaktů projektu a dat historie spuštění. 
   Účet aplikace Workspace pro experimentování | _Jedinečný název_ | Zadejte název nového pracovního prostoru. Název musí mít délku od 2 do 32 znaků a smí obsahovat jenom alfanumerické znaky a znak spojovníku (-).
   Přiřazení vlastníka pracovního prostoru | _Váš účet_ | Jako vlastníka pracovního prostoru vyberte svůj vlastní účet.
   Vytvoření účtu služby Správa modelů | *zaškrtnutí* | V rámci uživatelského prostředí při vytváření účtu služby Experimentování máte možnost vytvořit také účet služby Správa modelů ve službě Machine Learning. Tento prostředek využijete, až budete připravení nasadit a spravovat své modely jako webové služby v reálném čase. Doporučujeme vytvořit služby Správa modelů zároveň s účtem služby Experimentování.
   Název účtu | _Jedinečný název_ | Vyberte jedinečný název, který identifikuje váš účet služby Správa modelů. Můžete použít své jméno nebo třeba název oddělení nebo projektu, který experiment nejlépe identifikuje. Název musí mít délku od 2 do 32 znaků a smí obsahovat jenom alfanumerické znaky a znak spojovníku (-). 
   Cenová úroveň služby Správa modelů | **DEVTEST** | Klikněte na **Není vybraná žádná cenová úroveň** a určete cenovou úroveň svého nového účtu služby Správa modelů. Z důvodu úspory nákladů vyberte cenovou úroveň **DEVTEST**, pokud je ve vašem předplatném dostupná (omezená dostupnost), jinak kvůli úspoře nákladů vyberte cenovou úroveň S1. Kliknutím na možnost **Vybrat** uložte volbu cenové úrovně. 
   Připnutí na řídicí panel | _zaškrtnutí_ | Zaškrtnutím možnosti **Připnout na řídicí panel** povolíte snadné sledování vašeho účtu služby Experimentování ve službě Machine Learning na přední stránce řídicího panelu na vašem portálu Azure.

5. Kliknutím na tlačítko **Vytvořit** zahájíte proces vytváření.

6. Pokud chcete monitorovat proces nasazení, v pravém horním rohu panelu nástrojů portálu Azure klikněte na **Oznámení** (ikona zvonku). 

   Zobrazí se oznámení „Probíhá nasazení…“. Po dokončení se stav změní na „Nasazení bylo úspěšné“. Po úspěšném nasazení se otevře stránka účtu služby Experimentování ve službě Machine Learning.
   
   ![Oznámení portálu Azure Portal](media/quickstart-installation/portal-notification.png)

Teď přejděte do jedné ze dvou částí v závislosti na tom, jaký operační systém používáte v místním počítači, a nainstalujte do počítače aplikaci Azure Machine Learning Workbench. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Instalace aplikace Azure Machine Learning Workbench ve Windows
Aplikaci Azure Machine Learning Workbench můžete nainstalovat do počítače se systémem Windows 10, Windows Server 2016 nebo novějším.

1. Stáhněte si nejnovější verzi instalačního programu aplikace Azure Machine Learning Workbench **[AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi)**.

2. V Průzkumníku souborů dvakrát klikněte na stažený instalační program _AmlWorkbenchSetup.msi_.

   >[!IMPORTANT]
   >Stáhněte celý instalační program na disk a spusťte jej. Nespouštějte jej přímo z widgetu stahování v prohlížeči.

3. Dokončete instalaci podle pokynů na obrazovce.

   Instalační program stáhne všechny potřebné závislé komponenty, jako je Python, Miniconda a další související knihovny. Instalace všech komponent může trvat přibližně půl hodiny. 

4. Aplikace Azure Machine Learning Workbench je teď nainstalovaná v tomto adresáři:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>Instalace aplikace Azure Machine Learning Workbench v systému macOS
Aplikaci Azure Machine Learning Workbench můžete nainstalovat do počítače se systémem macOS Sierra.

1. Pomocí nástroje [Homebrew](http://brew.sh) nainstalujte knihovnu openssl. Podrobnější informace najdete v tématu [Požadavky rozhraní .NET Core v počítačích Mac](https://docs.microsoft.com/dotnet/core/macos-prerequisites).
   ```
   # install Homebrew first if you don't have it already
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # install latest openssl needed for .NET Core 1.x
   brew update
   brew install openssl
   mkdir -p /usr/local/lib
   ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
   ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
   ```

2. Stáhněte si nejnovější verzi instalačního programu aplikace Azure Machine Learning Workbench **[AmlWorkbench.dmg](https://aka.ms/azureml-wb-dmg)**.

   >[!IMPORTANT]
   >Stáhněte celý instalační program na disk a spusťte jej. Nespouštějte jej přímo z widgetu stahování v prohlížeči.

3. V aplikaci Finder dvakrát klikněte na stažený instalační program _AmlWorkbench.dmg_.

4. Dokončete instalaci podle pokynů na obrazovce.

   Instalační program stáhne všechny potřebné závislé komponenty, jako je Python, Miniconda a další související knihovny. Instalace všech komponent může trvat přibližně půl hodiny. 

5. Aplikace Azure Machine Learning Workbench je teď nainstalovaná v tomto adresáři: 

   _/Applications/AmlWorkbench.app_

## <a name="run-azure-machine-learning-workbench-to-log-in-the-first-time"></a>Spuštění aplikace Azure Machine Learning Workbench a první přihlášení
1. Po dokončení procesu instalace klikněte na poslední obrazovce instalačního programu na tlačítko pro **spuštění aplikace Workbench**. Pokud jste instalační program zavřeli, na ploše nebo v nabídce Start najdete zástupce aplikace Machine Learning Workbench s názvem **Azure Machine Learning Workbench**, pomocí kterého můžete aplikaci spustit.

2. Přihlaste se do aplikace Workbench pomocí stejného účtu, který jste předtím použili ke zřízení prostředků Azure. 

3. Po úspěšném procesu přihlášení se aplikace Workbench pokusí najít experimenty služby Experimentování ve službě Machine Learning, které jste předtím vytvořili. Prohledá všechna předplatná Azure, ke kterým mají vaše přihlašovací údaje přístup. Po nalezení aspoň jednoho účtu služby Experimentování se aplikace Workbench otevře s tímto účtem. Pak zobrazí seznam pracovních prostorů a projektů, které v tomto účtu našla. 

   >[!TIP]
   > Pokud máte přístup k více účtům služby Experimentování, můžete kliknutím na ikonu avataru v levém spodním rohu aplikace Workbench přepnout na jiný účet.

Informace o vytvoření prostředí pro nasazení vašich webových služeb najdete v tématu [Nastavení prostředí nasazení](deployment-setup-configuration.md).

## <a name="create-a-new-project"></a>Vytvoření nového projektu
1. Spusťte aplikaci Azure ML Workbench a přihlaste se. 

2. Klikněte na **Soubor** --> **Nový projekt** (nebo klikněte na znaménko **+** v podokně **PROJEKTY**). 

3. Zadejte údaje do polí **Název projektu** a **Adresář projektu**. Pole **Projektu popis** je volitelné, ale užitečné. Pole **adresy URL úložiště GIT Visualstudio.com** nechte zatím prázdné. Vyberte si pracovní prostor a vyberte šablonu projektu **Klasifikace Iris**.

   >[!TIP]
   >Volitelně můžete do pole úložiště Git zadat adresu URL úložiště Git hostovaného v projektu služby [VSTS (Visual Studio Team Service)](https://www.visualstudio.com). Toto úložiště Git už musí existovat, musí být prázdné a nesmí obsahovat žádnou hlavní větev. Také pro něj musíte mít oprávnění k zápisu. Pokud teď přidáte úložiště Git, můžete později povolit scénáře roamingu a sdílení. [Přečtěte si další informace](using-git-ml-project.md).

4. Kliknutím na **Vytvořit** vytvořte nový projekt. Vytvoří se nový projekt a otevře se. Teď můžete prozkoumat domovskou stránku projektu, zdroje dat, poznámkové bloky a soubory zdrojového kódu. 

    >[!TIP]
    >Také můžete projekt otevřít v nástroji VS Code nebo jiném editoru, a to jednoduše tak, že nakonfigurujete odkaz na integrované vývojové prostředí (IDE) a potom v něm otevřete adresář projektu. [Přečtěte si další informace](how-to-configure-your-IDE.md). 

## <a name="run-a-python-script"></a>Spuštění skriptu v jazyce Python
Teď spustíme skript ve vašem místním počítači. 

1. Každý projekt se otevře na vlastní stránce **Řídicí panel Projekt**. Na příkazovém řádku v horní části aplikace vlevo vedle tlačítka Spustit vyberte cíl spuštění `local` a skript ke spuštění `iris_sklearn.py`.  Ukázka obsahuje řadu dalších souborů, které si můžete prohlédnout později. 

   ![obrázek](media/quickstart-installation/run_control.png)

2. Do textového pole **Argumenty** zadejte hodnotu `0.01`. Toto číslo se v kódu použije k nastavení míry regularizace, která určuje způsob trénování modelu lineární regrese. 

3. Kliknutím na **Spustit** spusťte skript `iris_sklearn.py` ve svém počítači. 

   Tento kód používá k sestavení modelu algoritmus [logistické regrese](https://en.wikipedia.org/wiki/logistic_regression) z oblíbené knihovny [scikit-learn](http://scikit-learn.org/stable/index.html) jazyka Python.

4. Zprava se vysune panel **Úlohy**, pokud ještě nebyl viditelný, a přidá se na něj úloha `iris_sklearn`. Jakmile se úloha spustí, její stav se během několika sekund změní z hodnoty **Odesílání** na **Spuštěno** a potom na **Dokončeno**. 

5. Blahopřejeme. Úspěšně jste spustili skript v jazyce Python v aplikaci Azure ML Workbench.

6. Kroky 2–4 několikrát zopakujte. Pokaždé při tom použijte jiné hodnoty argumentu v rozsahu od `10` do `0.001`.

## <a name="view-run-history"></a>Zobrazení historie spuštění
1. Přejděte do zobrazení **Spuštění** a v seznamu spuštění klikněte na skript **iris_sklearn.py**. Otevře se řídicí panel historie spuštění skriptu `iris_sklearn.py`. Ten obsahuje každé spuštění skriptu `iris_sklearn.py`. 

   ![obrázek](media/quickstart-installation/run_view.png)

2. Řídicí panel historie spuštění také obsahuje hlavní metriky, sadu výchozích grafů a seznam metrik pro každé spuštění. Toto zobrazení můžete po kliknutí na ikonu konfigurace nebo ikonu filtru přizpůsobit pomocí řazení, filtrování a úpravy konfigurací.

   ![obrázek](media/quickstart-installation/run_dashboard.png)

3. Kliknutím na dokončenou úlohu zobrazíte podrobnosti o daném spuštění, včetně dalších metrik, vytvořených souborů a dalších protokolů, které by mohly být užitečné.

## <a name="next-steps"></a>Další kroky
Teď jste úspěšně vytvořili účet služby Experimentování ve službě Azure Machine Learning a účet služby Správa modelů ve službě Azure Machine Learning. Nainstalovali jste desktopovou aplikaci Azure Machine Learning Workbench a rozhraní příkazového řádku. Vytvořili jste nový projekt, vytvořili model spuštěním skriptu a prozkoumali historii spuštění tohoto skriptu.

Podrobnější poznatky o tomto pracovním postupu, včetně postupu nasazení modelu Iris jako webové služby, najdete v kurzu o klasifikaci Iris na pokračování, který obsahuje podrobné návody k [přípravě dat](tutorial-classifying-iris-part-1.md), [ experimentování](tutorial-classifying-iris-part-2.md) a [správě modelů](tutorial-classifying-iris-part-3.md). 

> [!div class="nextstepaction"]
> [Kurz Klasifikace Iris](tutorial-classifying-iris-part-1.md)

