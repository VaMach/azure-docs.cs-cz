---
title: "Jak používat poznámkové bloky Jupyter v Azure Machine Learning Workbench | Microsoft Docs"
description: "Příručka pro poznámkové bloky Jupyter funkci Azure Machine Learning Workbench pomocí"
services: machine-learning
author: jopela
ms.author: jopela
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 93850a7c9e3d9d69b0da22ebd0656ae40cee2e63
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="how-to-use-jupyter-notebook-in-azure-machine-learning-workbench"></a>Jak používat Poznámkový blok Jupyter v Azure Machine Learning Workbench

Azure Machine Learning Workbench podporuje interaktivní data vědecké účely experimentování prostřednictvím její integrace poznámkového bloku Jupyter. Tento článek popisuje, jak provádět efektivní použití této funkce lze zvýšit rychlost a kvalitu vaší vědecké účely experimentování interaktivní data.

## <a name="prerequisites"></a>Požadavky
- [Instalace a vytvoření Azure Machine Learning](/machine-learning/preview/quickstart-installation.md).
- Se seznamte s [Poznámkový blok Jupyter](http://jupyter.org/), protože není v tomto článku o výukových použití Jupyter.

## <a name="jupyter-notebook-architecture"></a>Architektura poznámkového bloku Jupyter
Na vysoké úrovni architektura poznámkového bloku Jupyter obsahuje tři součásti, každý můžou běžet v různých výpočetních prostředí:

- **Klient**: vstup uživatele a zobrazí výstup se vykresluje obdrží
- **Server**: webovém serveru hostícím poznámkového bloku soubory (.ipynb soubory)
- **Jádra**: běhové prostředí, kde se stane vlastního výkonu Poznámkový blok buněk

Další podrobnosti najdete v oficiální prosím odkazovat [Jupyter dokumentaci](http://jupyter.readthedocs.io/en/latest/architecture/how_jupyter_ipython_work.html). Fowllowing je diagram znázorňující, jak mapovány tohoto klienta, serveru a architektura jádra součásti v Azure ML.

![Architektura poznámkového bloku](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-architecture.png)

## <a name="kernels-in-azure-ml-workbench-notebook"></a>V poznámkovém bloku Azure ML Workbench jádra
Dostanete mnoho různých jádra v Azure ML Workbench pomocí jednoduše nakonfigurovat spuštění konfigurace a výpočetní cílů v `aml_config` složku ve vašem projektu. Přidání nového cíle výpočetní vydáním `az ml computetarget attach` příkaz je ekvivalentem přidání nové jádra.

>[!NOTE]
>Zkontrolujte [nakonfigurovat spuštění](experimentation-service-configuration.md) další podrobnosti o spuštění konfigurace a výpočetní cíle.

### <a name="kernel-naming-convention"></a>Zásady vytváření názvů jádra
Jádrech mají obvykle název ve formátu "\<název projektu > \<spustit název konfigurace >". Například, pokud máte spuštění konfigurace s názvem _docker python_ v projektu s názvem _myIris_, můžete najít jádra, při otevření poznámkového bloku Jupyter s názvem "myIris docker-python" v seznamu jádra.

V současné době nástroje Workbench podporuje následující typy jádra.

### <a name="local-python-kernel"></a>Místní jádra Python
Tato Python jádra podporuje spuštění na místním počítači. Je integrován s podporou historii běhů Azure Machine Learning. Název jádra je obvykle "my_project_name místní".

### <a name="python-kernel-in-docker-local-or-remote"></a>Python jádra v Docker (místní nebo vzdálené)
Tato Python jádra běží v kontejner Docker na místním počítači nebo ve vzdáleném virtuálního počítače s Linuxem. Název jádra je obvykle "my_project docker". Přidruženého `docker.runconfig` soubor má `Framework` pole nastavené na `Python`.

### <a name="pyspark-kernel-in-docker-local-or-remote"></a>Jádra PySpark v Docker (místní nebo vzdálené)
Tato jádra PySpark spustí skripty v kontextu Spark systémem uvnitř kontejner Docker, na místním počítači nebo na vzdálený virtuální počítač s Linuxem. Název jádra je obvykle "my_project docker". Přidruženého `docker.runconfig` soubor má `Framework` pole nastavené na `PySpark`.

### <a name="pyspark-kernel-on-hdinsight-cluster"></a>Jádra PySpark na clusteru HDInsight
Tato jádra běží v vzdáleného clusteru HDInsight, který jste připojili jako cíl výpočetní pro váš projekt. Název jádra je obvykle "my_project my_hdi". 

>[!IMPORTANT]
>V `.compute` souboru HDI výpočetní cíl, musíte změnit `yarnDeployMode` do `client` (výchozí hodnota je `cluster`) Chcete-li použít tento jádra. 

## <a name="start-jupyter-server-from-the-workbench"></a>Spuštění nástroje Workbench Jupyter serveru
Z Azure Machine Learning Workbench, poznámkových bloků je přístupná prostřednictvím nástroje Workbench **poznámkových bloků** kartě. _Klasifikace Iris_ obsahuje ukázkový projekt `iris.ipynb` ukázkový poznámkový blok.

![Karta poznámkových bloků](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-01.png)

Po otevření Poznámkový blok v nástroji Azure Machine Learning Workbench se zobrazí na kartě svůj vlastní dokument v **režim náhledu**. Toto je jen pro čtení zobrazení, která nevyžaduje používají server Jupyter a jádra.

![Poznámkový blok preview](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-02.png)

Kliknutím na tlačítko **spuštění serveru poznámkového bloku** tlačítko spustí Jupyter serveru a přepínače do poznámkového bloku **režimu úprav**. Známé uživatelské rozhraní poznámkového bloku Jupyter se zobrazí vložený v nástroje Workbench. Teď můžete nastavit jádra z **jádra** nabídky a spustit relaci interaktivní poznámkového bloku. 

>[!NOTE]
>Poznámka: pro jiné než místní jádra, může trvat minutu nebo dvě spustit, pokud ji používáte poprvé. Můžete provést `az ml experiment prepare` příkazu z okna příkazového řádku k přípravě cílového výpočetní tak jádra můžete spustit mnohem rychlejší, až se připraví cílový výpočetní.

![Režim úprav](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-04.png)

Toto je plně interaktivní možnosti poznámkového bloku Jupyter. Všechny operace regulární Poznámkový blok a klávesové zkratky jsou podporovány v tomto okně s výjimkou některých operací se soubory, vzhledem k tomu, že lze provést pomocí nástroje Workbench **poznámkových bloků** kartě a **souboru** kartě.

## <a name="start-jupyter-server-from-command-line"></a>Spuštění Jupyter serveru z příkazového řádku
Můžete také spustit relaci poznámkového bloku vydáním `az ml notebook start` z okna příkazového řádku:
```
$ az ml notebook start
[I 10:14:25.455 NotebookApp] The port 8888 is already in use, trying another port.
[I 10:14:25.464 NotebookApp] Serving notebooks from local directory: /Users/johnpelak/Desktop/IrisDemo
[I 10:14:25.465 NotebookApp] 0 active kernels 
[I 10:14:25.465 NotebookApp] The Jupyter Notebook is running at: http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.465 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:14:25.466 NotebookApp] 
    
    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:8889/?token=1f0161ab88b22fc83f2083a93879ec5e8d0ec18490f0b953
[I 10:14:25.759 NotebookApp] Accepting one-time-token-authenticated connection from ::1
[I 10:16:52.970 NotebookApp] Kernel started: 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
[I 10:16:53.854 NotebookApp] Adapting to protocol v5.1 for kernel 7f8932e0-89b9-48b4-b5d0-e8f48d1da159
```
Výchozí prohlížeč se automaticky spustí s Jupyter serveru odkazující na domovský adresář projektu. Také můžete adresu URL a tokenu zobrazí v okně příkazového řádku spouštět místně ostatní okna prohlížeče. 

![řídicího panelu Projekt](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-07.png)

Nyní můžete kliknutím na `.ipynb` soubor poznámkového bloku, otevřete a nastavte jádra (pokud nebylo nastaveno) a spusťte interaktivní relace.

![řídicího panelu Projekt](media/how-to-use-jupyter-notebooks/how-to-use-jupyter-notebooks-08.png)

## <a name="next-steps"></a>Další kroky
- Naučte se používat Poznámkový blok Jupyter, najdete [Jupyter oficiální dokumentaci](http://jupyter-notebook.readthedocs.io/en/latest/).    
- Abyste získali lepší představu o prostředí pro spuštění experimenty Azure ML, zkontrolujte [službu experimentování Přehled služby Azure Machine Learning](experimentation-service-configuration.md)

