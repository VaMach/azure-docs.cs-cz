<properties 
    pageTitle="Co je Azure Machine Learning Studio? | Microsoft Azure"
    description="Přehled nástroje Azure ML Studio, který umožňuje přetahováním rychle vytvářet modely z předpřipravených knihoven algoritmů a modulů"
    keywords="azure machine learning,azure ml,ml studio"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/10/2016"
    ms.author="garye"/>

# Co je Azure Machine Learning Studio?

Microsoft Azure Machine Learning Studio je nástroj pro spolupráci, ve kterém je možné přetahováním vytvářet, testovat a nasazovat řešení prediktivní analýzy dat. Machine Learning Studio publikuje modely jako webové služby, které je možné snadno využívat ve vlastních aplikacích nebo nástrojích BI, například v Excelu.

Machine Learning Studio je místo, kde se setkává datová věda, prediktivní analýza, cloudové prostředky a vaše data.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Interaktivní pracovní prostor Machine Learning Studio

K vývoji modelu prediktivní analýzy zpravidla použijete data z jednoho nebo více zdrojů, transformujete a analyzujete je prostřednictvím různých statistických funkcí a funkcí pro manipulaci s daty a vygenerujete sadu výsledků. Vývoj takového modelu je iterativní proces. Úpravou různých funkcí a jejich parametrů se výsledky zpřesňují, dokud nebudete přesvědčeni, že máte natrénován efektivní model.

**Azure Machine Learning Studio** nabízí interaktivní vizuální pracovní prostor, abyste mohli snadno vytvářet, testovat a iterovat model prediktivní analýzy. Při práci přetahujete ***datové sady*** a analytické ***moduly*** na interaktivní ***plátno***, kde je vzájemně propojujete. Tím vzniká ***experiment***, který pak ***spustíte*** v nástroji Machine Learning Studio. Iterace návrhu modelu probíhá tak, že experiment ***upravujete***, v případě potřeby ***uložíte*** kopii a spustíte jej znovu. Až budete připraveni, můžete ***výukový experiment*** převést na ***prediktivní experiment*** a ***publikovat*** jej jako ***webovou službu***, aby k vašemu modelu měli přístup i jiní uživatelé.

>[AZURE.TIP] Pokud si chcete stáhnout a vytisknout diagram s přehledem funkcí nástroje Machine Learning Studio, nahlédněte do tématu [Diagram s přehledem možností nástroje Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Nevyžaduje se žádné programování – model prediktivní analýzy se konstruuje vizuálním propojováním datových sad a modulů.

![Diagram nástroje Azure ML Studio: Vytváření experimentů, čtení dat z mnoha zdrojů, zápis dat se stanoveným skóre, zápis modelů][ml-studio-overview]

## Začínáme s nástrojem Machine Learning Studio

Když poprvé vstoupíte do nástroje [Machine Learning Studio](https://studio.azureml.net), zobrazí se stránka **Domů**. Zde si můžete zobrazit dokumentaci, videa a webináře a najdete tu i další přínosné materiály.

V horní části jsou tři karty: **Domů** (zde začínáte), **Studio** a **Galerie**.

### Studio

Po kliknutí na kartu **Studio** se zobrazí výzva, abyste se přihlásili účtem Microsoft či školním nebo pracovním účtem. Jakmile se přihlásíte, na levé straně uvidíte následující karty:

- **PROJEKTY** – Kolekce experimentů, datových sad, poznámkových bloků a jiných prostředků představující jeden objekt
- **EXPERIMENTY** – Experimenty, které byly vytvořeny, spuštěny a uloženy jako koncepty
- **WEBOVÉ SLUŽBY** – Webové služby, které jste nasadili z experimentů
- **POZNÁMKOVÉ BLOKY** – Jupyter Notebooks, které jste vytvořili
- **DATOVÉ SADY** – Datové sady, které jste nahráli do nástroje Studio
- **TRÉNOVANÉ MODELY** – Modely, které jste v experimentech natrénovali a uložili je v nástroji Studio
- **NASTAVENÍ** – Kolekce nastavení, kterou můžete použít ke konfiguraci účtu a prostředků

### Galerie

Po kliknutí na kartu **Galerie** přejdete na web Cortana Intelligence Gallery. Tato galerie je místo, kde komunita datových vědců a vývojářů může sdílet řešení vytvořená pomocí komponent sady Cortana Intelligence Suite.

Další informace o galerii najdete v tématu [Sdílení a hledání řešení na webu Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).

## Komponenty experimentu

Experiment sestává z datových sad, které poskytují data analytickým modulům. Ty pak lze vzájemně propojit a vytvořit tak model prediktivní analýzy. Platný experiment má konkrétně tyto charakteristiky:

- Experiment obsahuje alespoň jednu datovou sadu a jeden modul.
- Datová sady je možné připojit jen k modulům.
- Moduly je možné připojovat jak k datovým sadám, tak k jiným modulům.
- Všechny vstupní porty modulů musí určitým způsobem souviset s tokem dat.
- Musí být nastaveny všechny povinné parametry jednotlivých modulů.

Experiment můžete vytvořit zcela od začátku, ale také můžete využít existující ukázku jako šablonu. Další informace najdete v tématu o [využití ukázkových experimentů k vytvoření nových experimentů](machine-learning-sample-experiments.md).

Příklad vytvoření jednoduchého experimentu najdete v tématu [Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning Studio](machine-learning-create-experiment.md).

Obsáhlejší návod, jak vytvořit řešení prediktivní analýzy, najdete v tématu o [vývoji prediktivního řešení s Azure Machine Learningem](machine-learning-walkthrough-develop-predictive-solution.md).

### Datové sady

Datová sada obsahuje data, která byla nahrána do nástroje Machine Learning Studio, aby je bylo možné použít v procesu modelování. Součástí nástroje Machine Learning Studio je několik ukázkových datových sad, se kterými můžete experimentovat. Dle potřeby můžete nahrávat další datové sady. Zde jsou některé příklady dodávaných datových sad:

- **Data o spotřebě paliv u různých automobilů** – Hodnoty spotřeby paliva u automobilů na jednotku vzdálenosti (MPG) stanovené podle počtu válců, koňských sil apod.
- **Data o rakovině prsu** – Diagnostická data rakoviny prsu
- **Data o lesních požárech** – Rozsahy lesních požárů v severovýchodním Portugalsku

Během vytváření experimentu je možné si vybírat ze seznamu datových sad, které jsou k dispozici v nabídce nalevo od plátna.

Seznam ukázkových datových sad obsažených v nástroji Machine Learning Studio, najdete v tématu o [využití ukázkových datových sad v nástroji Azure Machine Learning Studio](machine-learning-use-sample-datasets.md).

### Moduly

Modul je algoritmus, který je možné provést na datech. Machine Learning Studio obsahuje několik modulů od funkcí pro příjem dat po procesy trénování, stanovení skóre a ověřování. Zde jsou některé příklady dodávaných modulů:

- [Převod na ARFF][convert-to-arff] – Převede serializovanou datovou sadu .NET do formátu ARFF (Attribute-Relation File Format).
- [Výpočet základních statistik][elementary-statistics] – Vypočítá základní statistiky, jako je střední hodnota, směrodatná odchylka atd.
- [Lineární regrese][linear-regression] – Vytvoří online model lineární regrese na základě klesání gradientu.
- [Určení skóre modelu][score-model] – Stanoví skóre pro trénovaný klasifikační nebo regresní model.

Během vytváření experimentu je možné si vybírat ze seznamu modulů, které jsou k dispozici v nabídce nalevo od plátna.  

Modul může obsahovat sadu parametrů, pomocí kterých je možné konfigurovat jeho vnitřní algoritmy. Když na plátnu vyberete modul, parametry modulu se zobrazí v podokně **Vlastnosti** napravo od plátna. Úpravou parametrů v tomto podokně můžete model optimalizovat.

S procházením rozsáhlé knihovny dostupných algoritmů strojového učení vám pomůže téma [Jak zvolit algoritmy pro Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

## Nasazení webové služby prediktivní analýzy

Jakmile je váš model prediktivní analýzy připraven, můžete jej přímo v nástroji Machine Learning Studio nasadit jako webovou službu. Další podrobnosti k tomuto procesu najdete v tématu [Nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/



<!---HONumber=Aug16_HO4-->


