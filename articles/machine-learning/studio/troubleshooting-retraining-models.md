---
title: "Řešení potíží s retraining webové služby Azure Machine Learning Classic | Microsoft Docs"
description: "Identifikujte a opravte narazil běžné problémy, když jsou retraining modelu pro webové služby Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 85cf9175bb4a5f253c7b47b2edc3ac8b00616ba2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Řešení potíží s retraining Azure Machine Learning Classic webové služby
## <a name="retraining-overview"></a>Retraining – přehled
Když nasadíte prediktivní experiment jako vyhodnocování webové služby je statický model. Jakmile nová data k dispozici nebo pokud příjemce rozhraní API má svá vlastní data, musí být retrained modelu. 

Kompletní a podrobný postup retraining procesu Classic webové služby, najdete v části [Přeučování Machine Learning modely prostřednictvím kódu programu](retrain-models-programmatically.md).

## <a name="retraining-process"></a>Retraining procesu
Když potřebujete přeučování webovou službu, musíte přidat další některé jeho součásti:

* Webovou službu nasazenou z výukový Experiment. Musí mít experiment **výstup webové služby** modulu připojené k výstupu **Train Model** modulu.  
  
    ![Připojte k train model výstup webové služby.][image1]
* Nový koncový bod přidán do vyhodnocování webovou službu.  Můžete přidat koncový bod programově pomocí ukázkový kód, kterou se odkazuje v Machine Learning Přeučování modelů prostřednictvím kódu programu tématu nebo prostřednictvím portálu Azure classic.

Ukázka kódu C# ze stránky nápovědy školení webové rozhraní API pak můžete přeučování modelu. Po vyhodnocení výsledky a spokojeni s nimi, je třeba aktualizovat pro cvičný model vyhodnocování webové službě pomocí nového koncového bodu, který jste přidali.

Hlavní kroky, které je nutné provést při programovém modelu s všechny části na místě, jsou následující:

1. Volání webové služby školení: volání je do dávky spuštění služby (BES), není žádosti o odpověď služby (záznamy RR). Ukázkový kód jazyka C# můžete na stránce nápovědy rozhraní API pro volání. 
2. Najít hodnoty *BaseLocation*, *RelativeLocation*, a *SasBlobToken*: tyto hodnoty jsou vráceny ve výstupu z volání k webové službě školení. 
   ![zobrazuje výstup retraining ukázka a BaseLocation, RelativeLocation a SasBlobToken hodnoty.][image6]
3. Aktualizace přidané koncového bodu z vyhodnocování webové služby s novou trénovaného modelu: pomocí ukázkový kód, který je součástí Machine Learning Přeučování modelů prostřednictvím kódu programu, aktualizaci nový koncový bod, které jste přidali do vyhodnocování model s nově trénovaného modelu z Školení webové služby.

## <a name="common-obstacles"></a>Běžné překážek
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Zkontrolujte, jestli máte správnou adresu URL oprava
Adresa URL opravy, kterou používáte musí být přidružený nový vyhodnocovací koncový bod, které jste přidali k webové službě vyhodnocování. Existuje několik způsobů, jak získat adresu URL opravy:

**Možnost 1: programově**

Pokud chcete získat správnou adresu URL opravy:

1. Spustit [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) ukázkový kód.
2. Z výstupu AddEndpoint, Najít *HelpLocation* hodnotu a zkopírujte adresu URL.
   
   ![HelpLocation ve výstupu addEndpoint vzorku.][image2]
3. Vložte adresu URL do prohlížeče, přejděte na stránku, která poskytuje odkazy na nápovědu pro webovou službu.
4. Klikněte **aktualizace prostředků** odkazu k otevření stránky nápovědy opravy.

**Možnost 2: Pomocí portálu Azure classic**

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Otevřete kartu Machine Learning. ![Karta opřené počítače.][image4]
3. Pak klikněte na název pracovního prostoru, **webové služby**.
4. Klikněte na tlačítko vyhodnocování webové služby, kterou pracujete. (Pokud jste nezměnila výchozí název webové služby, se bude končit [vyhodnocování Exp.].)
5. Klikněte na tlačítko **přidání koncového bodu**.
6. Po přidání koncového bodu, klikněte na název koncového bodu. Pak klikněte na tlačítko **aktualizace prostředků** chcete otevřít stránku nápovědy oprav.

> [!NOTE]
> Pokud jste přidali koncový bod k webové službě školení místo prediktivní webové služby, zobrazí se chybová zpráva po kliknutí na tlačítko **aktualizace prostředků** odkaz: je nám líto, ale tato funkce není podporována nebo k dispozici v tomto kontext. Tato webová služba nemá žádné aktualizovat prostředky. Omlouváme se za nepříjemnosti a práce na zlepšení tento pracovní postup.
> 
> 

![Nový koncový bod řídicí panel.][image3]

Na stránce nápovědy oprava obsahuje adresu URL opravy, je nutné použít a poskytuje ukázkový kód, které můžete použít k volání.

![Adresa URL opravy.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Zkontrolujte aktualizaci správný vyhodnocování koncový bod
* Není oprava webovou službu školení: operace opravy se musí provést na vyhodnocování webové službě.
* Výchozí koncový bod webové služby není oprava: operace opravy se musí provést na nové vyhodnocování webové služby koncového bodu, který jste přidali.

Můžete ověřit, které webové služby, koncový bod je na pomocí portálu Azure classic. 

> [!NOTE]
> Ujistěte se, že přidáváte koncový bod do prediktivní webové služby není školení webovou službu. Pokud jste nasadili správně školení a prediktivní webové služby, měli byste vidět dvě samostatné webové služby uvedené. Prediktivní webové služby musí končit "[prediktivní exp.]".
> 
> 

1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Otevřete kartu Machine Learning. ![Machine learning prostoru uživatelského rozhraní.][image4]
3. Vyberte pracovní prostor.
4. Klikněte na tlačítko **webové služby**.
5. Vyberte prediktivní webové služby.
6. Ověřte, že váš nový koncový bod byl přidán k webové službě.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Zkontrolujte pracovní prostor, který webová služba je v zajistit, že je ve správném oblasti
1. Přihlaste se do [portál Azure Classic](https://manage.windowsazure.com).
2. Z nabídky vyberte Machine Learning.
   ![Machine learning oblast uživatelského rozhraní.][image4]
3. Zkontrolujte umístění pracovního prostoru.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
