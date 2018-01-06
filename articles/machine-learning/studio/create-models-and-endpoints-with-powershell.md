---
title: "Vytvoření více modelů z jednoho experimentu | Microsoft Docs"
description: "Pomocí prostředí PowerShell vytvořit více modely Machine Learning a webové koncové body služby se stejným algoritmus, ale jiné školení datové sady."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 3386e3bab7a92b080276c4f03f0b006bd5f68e98
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/05/2018
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Vytvoření mnoha modelů Machine Learning a koncových bodů webové služby z jednoho experimentu pomocí prostředí PowerShell
Zde problém je běžný, machine learning: Chcete vytvořit mnoho modely, které mají stejný pracovní postup školení a použít stejný algoritmus, ale mít jiný školení datové sady jako vstup. Tento článek ukazuje, jak to udělat ve velkém měřítku v Azure Machine Learning Studio pomocí právě jeden experimentu.

Řekněme například, že vlastníte firma franšíza pronájem globální kolo. Budete chtít vytvořit regresní model k vyžádání pronájem na základě historických dat předpovědi. Máte 1000 pronájem umístění po celém světě. Jste shromažďují datovou sadu pro každé umístění, která obsahuje důležité funkce, jako je například datum, čas, počasí a provoz, které jsou specifické pro každé umístění.

Může natrénování modelu jednou pomocí sloučené verzi všechny datové sady ve všech umístěních. Ale protože každé z vašich lokalit má jedinečné prostředí, je vhodnější by k natrénování modelu regrese samostatně pomocí datovou sadu pro každé umístění. Tímto způsobem, každý trained model může vzít v úvahu velikosti jiné úložiště, svazek, geography, plnění, provoz kolo friendly prostředí a další.

Který může být nejlepším přístupem, ale nechcete vytvořit 1000 experimenty školení v Azure Machine Learning s každé z nich představující jedinečné umístění. Kromě toho se čtenáře úloh, je také zdá se, že neefektivní vzhledem k tomu, že každý experiment by měla mít stejné komponenty s výjimkou školení datovou sadu.

Naštěstí toho lze dosáhnout pomocí [Azure Machine Learning retraining API](retrain-models-programmatically.md) a automatizaci úloh s [Azure Machine Learning PowerShell](powershell-module.md).

> [!NOTE]
> Chcete-li ukázky rychleji probíhají rychleji, snižte počet umístění z 1000 až 10. Ale stejné zásady a postupy platí pro 1 000 umístění. Jediným rozdílem je, pokud chcete cvičení z datových sad, 1 000 pravděpodobně chcete zamyslet nad paralelním spuštěním následujících skriptů prostředí PowerShell. Jak to provést, je nad rámec tohoto článku, ale můžete najít příklady prostředí PowerShell více vláken na Internetu.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Nastavit výukový experiment
Použijte tento příklad [výukový experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) který se v [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Otevřete tento experiment v vaše [Azure Machine Learning Studio](https://studio.azureml.net) pracovního prostoru.

> [!NOTE]
> Aby bylo možné postupovat podle spolu se v tomto příkladu, můžete použít standardní pracovní prostor než volného prostoru. Můžete vytvořit jeden koncový bod pro každého zákazníka – celkem 10 koncové body – a který vyžaduje standardní prostoru vzhledem k tomu, že je omezený na 3 koncové body volného prostoru. Pokud máte pouze volného prostoru, změňte jenom skripty umožňující jenom 3 umístění.
> 
> 

Experiment používá **Import dat** modulu k importu datovou sadu školení *customer001.csv* z účtu úložiště Azure. Předpokládejme, jste neshromáždili datové sady školení ze všech kolo pronájem umístění a je uložen ve stejném umístění úložiště objektů blob s názvy souborů od *rentalloc001.csv* k *rentalloc10.csv*.

![Bitové kopie](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Všimněte si, že **výstup webové služby** modul byl přidán do **Train Model** modulu.
Při nasazení tohoto experimentu jako webové služby, koncový bod přidružené, výstup ve formátu souboru .ilearner vrátí naučeného modelu.

Také Upozorňujeme, že nastavíte parametr webové služby pro adresu URL, **importovat Data** používá modul. Tímto způsobem lze jednotlivé školení datové sady pro trénování modelu pro každé umístění zadejte pomocí parametru.
Existují další způsoby, může to uděláte. Dotaz SQL s parametrem webové služby můžete použít k získání dat z databáze SQL Azure, nebo použijte **vstup webové služby** modulu v datové sadě předat webovou službu.

![Bitové kopie](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Teď umožňuje spustit tento výukový experiment pomocí výchozí hodnota *rental001.csv* jako školení datovou sadu. Je-li zobrazit výstup **Evaluate** modulu (klikněte na výstup a vyberte **vizualizovat**), zobrazí se vám dostatečnou výkon *AUC* = 0.91. V tomto okamžiku jste připravení nasadit webovou službu mimo tento výukový experiment.

## <a name="deploy-the-training-and-scoring-web-services"></a>Nasazení školení a vyhodnocování webové služby
Chcete-li nasadit webovou službu školení, klikněte na tlačítko **nastavit webové služby** tlačítko níže na plátno experimentu a vyberte **nasazení webové služby**. Volání této webové služby "" kolo pronájem školení".

Teď je potřeba nasadit webovou službu vyhodnocování.
Chcete-li to provést, klikněte na tlačítko **nastavit webové služby** níže na plátno a vyberte **webové služby prediktivní**. Tím se vytvoří vyhodnocování experimentu.
Budete muset provést pouze několik dílčí úpravy, aby fungoval jako webovou službu. Popisek sloupce "cnt" odebrání vstupních dat a omezit výstup pouze id instance a odpovídající předpovězené hodnoty.

Sami práci uložit, můžete jednoduše otevřít [prediktivní experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) v galerii, který je již připravena.

Pokud chcete nasadit webovou službu, spustit prediktivní experiment a pak klikněte na **nasazení webové služby** tlačítko níže na plátno. Název vyhodnocování webové služby "Kolo pronájem vyhodnocování" ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Vytvoření 10 koncových bodů identické webové služby pomocí prostředí PowerShell
Této webové služby se dodává s výchozí koncový bod. Ale nejste zajímat výchozí koncový bod, protože jej nelze aktualizovat. Co musíte udělat je vytvoření 10 další koncové body, jeden pro každé umístění. Můžete provést pomocí prostředí PowerShell.

První můžete nastavit prostředí PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Potom spusťte následující příkaz Powershellu:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nyní jste vytvořili 10 koncových bodů a všechny obsahují stejné trénink na cvičení modelu *customer001.csv*. Můžete je zobrazit na portálu Azure.

![Bitové kopie](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Aktualizovat koncové body používat samostatný školení datové sady pomocí prostředí PowerShell
Dalším krokem je aktualizovat koncové body s modely, které jednoznačně trénink na jednotlivé data každého zákazníka. Ale nejdřív je potřeba vytvořit tyto modely z **kolo pronájem školení** webové služby. Přejděte zpět do **kolo pronájem školení** webové služby. je třeba volat svůj koncový bod BES 10krát s 10 různých školení datové sady, chcete-li vytvořit 10 odlišnými modely. Použijeme **InovkeAmlWebServiceBESEndpoint** rutiny prostředí PowerShell k tomu.

Budete taky muset zadat přihlašovací údaje pro účet úložiště objektů blob do `$configContent`, a to, v polích `AccountName`, `AccountKey` a `RelativeLocation`. `AccountName` Může být jedna z vaší názvy účtů, jak je vidět **portál Azure** (*úložiště* karta). Když kliknete na účet úložiště jeho `AccountKey` naleznete stisknutím **spravovat přístupové klíče** tlačítko dole a kopírování *primární přístupový klíč*. `RelativeLocation` Je relativní úložiště cestu, kde bude uložena nový model. Například cesta `hai/retrain/bike_rental/` ve skriptu níže odkazuje na kontejner s názvem `hai`, a `/retrain/bike_rental/` obsahuje podsložky. V současné době nelze vytvořit podsložky prostřednictvím portálu uživatelského rozhraní, ale existují [několik Průzkumníci úložiště Azure](../../storage/common/storage-explorers.md) které umožňují učinit. Doporučuje se vytvořit nový kontejner v úložišti pro uložení nové trénované modely (soubory .ilearner) následujícím způsobem: ze stránky úložiště, klikněte na **přidat** tlačítko dole a pojmenujte ji `retrain`. Souhrn potřebné změny, které níže uvedeném skriptu týkají `AccountName`, `AccountKey` a `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> Koncový bod BES je jediný podporovaný režim pro tuto operaci. Záznamy o prostředku nelze použít pro vytvoření trénované modely.
> 
> 

Jak je uvedeno výše, namísto vytváření 10 různých BES úlohy konfigurace soubory json, dynamicky místo toho vytvoříte konfigurační řetězec a kanál, aby *jobConfigString* parametr  **InvokeAmlWebServceBESEndpoint** rutiny, protože je ve skutečnosti není potřeba zachovat kopii na disku.

Pokud všechno proběhne správně, po chvíli se měl zobrazit 10 .ilearner souborů z *model001.ilearner* k *model010.ilearner*, v účtu úložiště Azure. Nyní je vše připraveno k aktualizaci naše 10 vyhodnocování koncových bodů webové služby pomocí těchto modelů pomocí **oprava AmlWebServiceEndpoint** rutiny prostředí PowerShell. Znovu si pamatujte, že můžete pouze oprava jiné než výchozí koncové body prostřednictvím kódu programu dříve vytvořené.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

To měly být spuštěny docela rychle. Po dokončení provádění jsme budete úspěšně jste vytvořili 10 prediktivní koncových bodů webové služby, každý obsahující modulu trained model jednoznačně trénink na konkrétní datové sady do umístění pronájem, vše z jedné výukový experiment. Chcete-li to ověřit, můžete zkusit volání tyto koncové body pomocí **InvokeAmlWebServiceRRSEndpoint** rutinu a poskytovat jim se stejným vstupní data a byste měli očekávat zobrazte výsledky různých předpovědi vzhledem k tomu, že probíhá Trénink modely s jinou školení sad.

## <a name="full-powershell-script"></a>Úplné skript prostředí PowerShell
Tady je seznam úplný zdrojový kód:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
