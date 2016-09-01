<properties
    pageTitle="Modul PowerShell pro Machine Learning | Microsoft Azure"
    description="Modul PowerShell pro Azure Machine Learning je dostupný v režimu veřejné zkušební verze. Pomocí modulu PowerShell se dají vytvářet a spravovat pracovní prostory, experimenty, webové služby a další."
    keywords="experiment, lineární regrese, algoritmy strojového učení, kurz strojového učení, techniky prediktivního modelování, experiment datové vědy"
    services="machine-learning"
    documentationCenter=""
    authors="hning86"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="garye;haining"/>

# Modul PowerShell pro Microsoft Azure Machine Learning

Modul PowerShell pro Azure Machine Learning je výkonný nástroj, který umožňuje spravovat pracovní prostory, experimenty, datové sady či webové služby v prostředí Windows PowerShell.

Projít si dokumentaci a stáhnout modul včetně celého zdrojového kódu můžete na adrese [https://aka.ms/amlps](https://aka.ms/amlps). 

## Co je modul Machine Learning PowerShell?

Modul Machine Learning PowerShell je modul DLL založený na rozhraní .NET, který umožňuje plně spravovat pracovní prostory Azure Machine Learning, experimenty, datové sady, webové služby a koncové body webové služby z prostředí Windows PowerShell. Kromě modulu si můžete stáhnout i celý zdrojový kód zahrnující jasně oddělenou [vrstvu API v jazyce C#](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Na modul tak můžete odkazovat z vlastního .NET projektu a spravovat Azure Machine Learning prostřednictvím kódu rozhraní .NET. Modul také závisí na dalších rozhraních REST API, která můžete využívat přímo ze svého oblíbeného klienta.

## Co můžu s modulem PowerShell dělat?

S modulem PowerShell můžete provádět například některé z těchto úloh. Tyto a mnoho dalších funkcí najdete také v [úplné dokumentaci](https://aka.ms/amlps).

- Zřiďte nový pracovní prostor s pomocí certifikátu pro správu ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace)).
- Exportujte a importujte soubor JSON s grafem experimentu ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) a [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph)).
- Spusťte experiment ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment)).
- Z prediktivního experimentu vytvořte webovou službu ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice)).
- Vytvořte nový koncový bod v publikované webové službě ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint)).
- Zavolejte koncový bod webové služby RRS či BES ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) a [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint)).

Na jednoduchém příkladu si můžeme ukázat, jak se pomocí PowerShell spustí existující experiment:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Složitější příklad užití najdete v tomto článku, kde se píše, jak modul PowerShell použít k automatizaci velmi často požadované úlohy: [Jak pomocí PowerShell z jednoho experimentu vytvořit mnoho modelů Machine Learning a koncových bodů webové služby](machine-learning-create-models-and-endpoints-with-powershell.md).

## Jak mám začít?

Pokud chcete začít s Machine Learning PowerShell, stáhněte si [vydaný balíček](https://github.com/hning86/azuremlps/releases) z GitHubu a postupujte podle [instalačních pokynů](https://github.com/hning86/azuremlps/blob/master/README.md). Stažený a rozbalený modul DLL odblokujte a potom ho importujte do prostředí PowerShell. Většina rutin vyžaduje, abyste zadali ID pracovního prostoru, jeho autorizační token a oblast Azure, ve které se pracovní prostor nachází. Nejsnáze tyto údaje předáte prostřednictvím výchozího souboru config.json (jak na to, se dočtete v instalačních pokynech). Samozřejmě můžete také naklonovat strom systému Git a kód upravit a zkompilovat v nástroji Visual Studio na místním počítači.

## Další kroky

Modul PowerShell bude dál vylepšován a rozšiřován i v současném období zkušebního provozu. Novinky a další informace se dočtete na [blogu o Cortana Intelligence a Machine Learning](https://blogs.technet.microsoft.com/machinelearning/).


<!---HONumber=Aug16_HO4-->


