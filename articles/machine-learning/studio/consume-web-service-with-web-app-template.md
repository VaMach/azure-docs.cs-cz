---
title: "Využívat pomocí šablony webové aplikace webové služby Machine Learning | Microsoft Docs"
description: "Použití šablony webové aplikace v Azure Marketplace využívat prediktivní webové služby v Azure Machine Learning."
keywords: "Webová služba, operationalization, REST API strojového učení"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1c182403409966923440f359cb2514af7b7df9f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Využívání webové služby Azure Machine Learning pomocí šablony webové aplikace

Jednou jste vyvinuté prediktivního modelu a nasadit jako Azure webové služby pomocí Machine Learning Studio, nebo pomocí nástrojů, jako je R nebo Python, můžete přístup operationalized modelu s použitím rozhraní REST API.

Existuje několik způsobů, jak používat rozhraní REST API a přístup k webové službě. Můžete například napsat aplikaci v jazyce C#, R nebo Python pomocí ukázkový kód pro vás vygeneroval při nasazení webové služby (k dispozici v [Machine Learning Web Services portálu](https://services.azureml.net/quickstart) nebo v řídicím panelu webové služby v nástroji Machine Learning Studio). Nebo můžete použít sešit aplikace Excel ukázka vytvořili pro vás ve stejnou dobu.

Ale nejrychlejší a nejsnazší způsob pro přístup k webové služby je prostřednictvím šablony webové aplikace k dispozici v [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure Machine Learning šablony webové aplikace
Webové aplikace šablony dostupné v Azure Marketplace můžete vytvořit vlastní webové aplikace, kterou zná vstupní data webové služby a očekávané výsledky. Jediné, co musíte udělat je poskytnout přístup k webové aplikaci pro webové služby a data a šablona nemá rest.

Dvě šablony jsou k dispozici:

* [Šablony aplikace webové služby Azure ML požadavků a odpovědí](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Šablony aplikace webové služby provedení dávky Azure ML](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Každá šablona vytvoří ukázkovou aplikaci ASP.NET, pomocí rozhraní API URI a klíč pro webovou službu a nasadí jej jako webového serveru do Azure. Šablona služby požadavků a odpovědí (záznamy RR) vytvoří webovou aplikaci, která umožňuje odesílání jednoho řádku dat k webové službě získat jeden výsledek. Spuštění služby Batch (BES) šablona vytvoří webovou aplikaci, která umožňuje odeslat mnoho řádků dat. Chcete-li získat více výsledků.

Žádné kódování je potřeba použít tyto šablony. Stačí zadat klíč rozhraní API a identifikátor URI a šablona vytvoří aplikaci za vás.

Pokud chcete získat klíč rozhraní API a URI požadavku pro webovou službu:

1. V [webový portál služby](https://services.azureml.net/quickstart), novou webovou službu, klikněte na tlačítko **webové služby** v horní části. Nebo pro web Classic, klikněte na tlačítko služby **Classic webové služby**.
2. Klikněte na tlačítko webové služby, kterou chcete získat přístup.
3. Pro webovou službu Classic klikněte na koncový bod, které chcete získat přístup.
4. Klikněte na tlačítko **spotřebě** v horní části.
5. Kopírování **primární** nebo **sekundární klíč** a uložte ho.
6. Pokud vytváříte šablonu služby požadavků a odpovědí (záznamy RR), zkopírovat **požadavků a odpovědí** URI a uložte ho. Pokud vytváříte šablonu spuštění služby Batch (BES), zkopírovat **dávkových žádostí** URI a uložte ho.


## <a name="how-to-use-the-request-response-service-rrs-template"></a>Použití šablony služby požadavků a odpovědí (záznamy RR)
Použijte následující postup použijte šablony webové aplikace RRS, jak je znázorněno v následujícím diagramu.

![Postup použití RRS webové šablony][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Přejděte na [portál Azure](https://portal.azure.com), **přihlášení**, klikněte na tlačítko **nový**, vyhledejte a vyberte **webové aplikace Azure ML požadavků a odpovědí Service**, pak klikněte na tlačítko **vytvořit**. 
   
   * Zadejte jedinečný název webové aplikace. Adresa URL webové aplikace bude tento název, za nímž následuje `.azurewebsites.net.` například`http://carprediction.azurewebsites.net.`
   * Vyberte předplatné Azure a služby, pod kterým běží webové služby.
   * Klikněte na možnost **Vytvořit**.
     
     ![Vytvoření webové aplikace][image5]

4. Po dokončení nasazení webové aplikace Azure klikněte **URL** v nastavení aplikace webové stránky v Azure nebo ve webovém prohlížeči zadejte adresu URL. Například `http://carprediction.azurewebsites.net.`.
5. Při prvním spuštění webové aplikace se zobrazí dotaz **Post URL rozhraní API** a **klíč rozhraní API**.
   Zadejte hodnoty, které jste předtím uložili (**URI požadavku** a **klíč rozhraní API**, v uvedeném pořadí).
     
     Klikněte na tlačítko **odeslání**.
     
     ![Zadejte Post URI a klíč rozhraní API][image6]

6. Zobrazí aplikace webové jeho **konfiguraci webové aplikace** stránka s aktuální nastavení webové služby. Sem můžete provést změny nastavení používaná při webové aplikace.
   
   > [!NOTE]
   > Nastavení zde pouze změna pro tuto webovou aplikaci. Výchozí nastavení webové služby neprojeví. Například, pokud změníte **popis** zde nemění Popis zobrazený na řídicím panelu webové služby v nástroji Machine Learning Studio.
   > 
   > 
   
    Když jste hotovi, klikněte na tlačítko **uložit změny**a potom klikněte na **přejít na domovskou stránku**.

7. Na domovské stránce můžete zadat hodnoty k odeslání do webové služby. Klikněte na tlačítko **odeslání** když jste hotovi, a vrátí výsledek.

Pokud chcete vrátit **konfigurace** stránky, přejděte na `setting.aspx` stránku webové aplikace. Příklad: `http://carprediction.azurewebsites.net/setting.aspx.` zobrazí se výzva k opakovanému zadání klíč rozhraní API – budete potřebovat, který pro přístup k stránce a aktualizujte nastavení.

Můžete zastavit, restartovat nebo odstranit webovou aplikaci na portálu Azure stejně jako jiná webové aplikace. Tak dlouho, dokud běží můžete procházet domácí webovou adresu a zadejte nové hodnoty.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Použití šablony spuštění služby Batch (BES)
Šablona BES webové aplikace můžete použít stejným způsobem jako šablonu RRS, s tím rozdílem, že webová aplikace, která je vytvořena vám umožní odeslat více řádků dat a příjem více výsledků.

Vstupní hodnoty pro spuštění webové služby batch mohou pocházet z úložiště Azure nebo místního souboru; výsledky jsou uloženy v kontejneru úložiště Azure.
Ano budete potřebovat kontejner úložiště Azure pro uložení výsledků vrácených webové aplikace a budete muset připravit vstupní data.

![Postup použití BES webové šablony][image2]

1. Postupujte stejným způsobem, k vytvoření webové aplikace BES jako záznamy o prostředku šablony, s výjimkou, přejděte na [Azure ML dávky spuštění služby webové aplikace šablona](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) BES šablony na webu Azure Marketplace otevřete a kliknete na **vytvořit webovou aplikaci**.

2. K určení, kde chcete výsledky uložené, zadejte informace o cílovém kontejneru na domovskou stránku webové aplikace. Také určete, kde webové aplikace můžete získat vstupní hodnoty, buď v případě místního souboru nebo kontejner úložiště Azure.
   Klikněte na tlačítko **odeslání**.
   
    ![Informace o úložiště][image7]

Webové aplikace se zobrazí stránka s stav úlohy.
Po dokončení úlohy budete mít k dispozici umístění výsledky v úložišti objektů blob Azure. Máte také možnost stahování výsledky do místního souboru.

## <a name="for-more-information"></a>Další informace
Další informace o...

* Vytvoření experimentu machine learning s Machine Learning Studio najdete v části [vytvoření prvního experimentu v nástroji Azure Machine Learning Studio](create-experiment.md)
* tom, jak nasadit experimentu machine learning jako webovou službu, najdete v části [nasazení webové služby Azure Machine Learning](publish-a-machine-learning-web-service.md)
* Další způsoby pro přístup k vaší webové služby, najdete v části [využívání Azure Machine Learning webové služby](consume-web-services.md)

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
