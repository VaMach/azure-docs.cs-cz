---
title: "Vytvoření první funkce v Azure pomocí sady Visual Studio | Dokumentace Microsoftu"
description: "Vytvořte a publikujte do Azure jednoduchou funkci aktivovanou protokolem HTTP pomocí Azure Functions Tools for Visual Studio."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: azure functions, functions, event processing, compute, serverless architecture
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/25/2017
ms.author: rachelap, glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 64769458ad90f14c2f7a87b9a405b80616a478be
ms.contentlocale: cs-cz
ms.lasthandoff: 06/28/2017


---
<a id="create-your-first-function-using-visual-studio" class="xliff"></a>

# Vytvoření první funkce pomocí sady Visual Studio 

Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci. 

V tomto tématu zjistíte, jak pomocí Azure Functions Tools for Visual Studio 2017 místně vytvořit a otestovat funkci Hello World. Kód funkce potom budete publikovat do Azure.

![Kód služby Azure Functions v projektu sady Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

<a id="prerequisites" class="xliff"></a>

## Požadavky

Pro absolvování tohoto kurzu nainstalujte:

* [Visual Studio 2017 Preview verze 15.3](https://www.visualstudio.com/vs/preview/), včetně sady funkcí **Vývoj pro Azure**.
    
    ![Instalace sady Visual Studio 2017 se sadou funkcí Vývoj pro Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="install-azure-functions-tools-for-visual-studio-2017" class="xliff"></a>

## Instalace Azure Functions Tools for Visual Studio 2017

Než začnete, musíte stáhnout a nainstalovat Azure Functions Tools for Visual Studio 2017. Tyto nástroje je možné použít pouze se sadou Visual Studio 2017 Preview verze 15.3 nebo novější. Pokud jste už Azure Functions Tools nainstalovali, můžete tuto část přeskočit.

[!INCLUDE [Install the Azure Functions Tools for Visual Studio](../../includes/functions-install-vstools.md)]   

<a id="create-an-azure-functions-project-in-visual-studio" class="xliff"></a>

## Vytvoření projektu Azure Functions v sadě Visual Studio

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Teď, když jste vytvořili projekt, můžete vytvořit svou první funkci.

<a id="create-the-function" class="xliff"></a>

## Vytvoření funkce

V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**. Vyberte **Funkce Azure Functions** a klikněte na **Přidat**.

Vyberte **HttpTrigger**, zadejte **Název funkce**, jako **Přístupová práva** vyberte **Anonymní** a klikněte na **Vytvořit**. Vytvořená funkce je přístupná prostřednictvím požadavku HTTP z jakéhokoli klienta. 

![Vytvoření nové funkce Azure Functions](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

Teď máte vytvořenou funkci aktivovanou protokolem HTTP a můžete ji otestovat na místním počítači.

<a id="test-the-function-locally" class="xliff"></a>

## Místní testování funkce

[!INCLUDE [Test the function locally](../../includes/functions-vstools-test.md)]

Zkopírujte adresu URL vaší funkce z výstupu modulu runtime služby Azure Functions.  

![Místní modul runtime Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

 Vložte adresu URL pro požadavek HTTP do panelu adresy prohlížeče. K této adrese URL připojte řetězec dotazu `&name=<yourname>` a proveďte požadavek. Následuje ukázka odezvy na místní požadavek GET vrácené funkcí v prohlížeči: 

![Odezva místního hostitele funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

Pokud chcete zastavit ladění, klikněte na tlačítko **Zastavit** na panelu nástrojů sady Visual Studio.

Po ověření správného fungování funkce na místním počítači je na čase publikovat projekt do Azure.

<a id="publish-the-project-to-azure" class="xliff"></a>

## Publikování projektu do Azure

Před publikováním projektu musíte mít v předplatném Azure aplikaci funkcí. Aplikaci funkcí můžete vytvořit přímo v sadě Visual Studio.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

<a id="test-your-function-in-azure" class="xliff"></a>

## Testování funkce v Azure

Zkopírujte základní adresu URL aplikace funkcí ze stránky Publikovat profil. Nahraďte část adresy URL použité při místním testování funkce, která obsahuje `localhost:port`, novou základní adresou URL. Stejně jako dříve nezapomeňte k této adrese URL připojit řetězec dotazu `&name=<yourname>` a provést požadavek.

Adresa URL, která volá vaši funkci aktivovanou protokolem HTTP, vypadá nějak takto:

    http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

Vložte tuto novou adresu URL pro požadavek HTTP do panelu Adresa prohlížeče. Následuje ukázka odezvy na vzdálený požadavek GET vrácené funkcí v prohlížeči: 

![Odezva funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
<a id="next-steps" class="xliff"></a>

## Další kroky

Pomocí sady Visual Studio jste vytvořili aplikaci funkcí jazyka C# s jednoduchou funkcí aktivovanou protokolem HTTP. 

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

Další informace o místním testování a ladění pomocí základních funkcí služby Azure Functions najdete v tématu [Místní kódování a testování služby Azure Functions](functions-run-local.md). 


