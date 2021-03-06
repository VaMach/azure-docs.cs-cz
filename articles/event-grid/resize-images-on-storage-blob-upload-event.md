---
title: "Automatizace změny velikosti nahraných obrázků pomocí Azure Event Gridu | Microsoft Docs"
description: "Při každém nahrání objektu blob v Azure Storage se může aktivovat Azure Event Grid. Díky tomu můžete odesílat soubory obrázků nahrané do Azure Storage do jiných služeb, třeba Azure Functions, ke změně velikosti a dalším vylepšením."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: b0fccd058620537f6dcfaf37ee14c1ff0cb8857a
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatizace změny velikosti nahraných obrázků s využitím služby Event Grid

[Azure Event Grid](overview.md) je služba zpracování událostí pro cloud. Event Grid umožňuje vytvářet odběry událostí vyvolaných službami Azure nebo prostředky třetích stran.  

Tento kurz je druhou částí série kurzů o službě Storage. Navazuje na [předchozí kurz o službě Storage][previous-tutorial] a přidává automatické vytváření miniatur bez serveru s využitím služeb Azure Event Grid a Azure Functions. Event Grid umožňuje službě [Azure Functions](..\azure-functions\functions-overview.md) reagovat na události služby [Azure Blob Storage](..\storage\blobs\storage-blobs-introduction.md) a vytvářet miniatury nahraných obrázků. K události vytvoření ve službě Blob Storage se vytvoří odběr události. Při přidání objektu blob do určitého kontejneru služby Blob Storage dojde k volání koncového bodu funkce. K přístupu k objektu blob se použití data předaná do vazby funkce ze služby Event Grid a vygeneruje se obrázek miniatury. 

Funkce změny velikosti se do existující aplikace pro nahrávání obrázků přidává pomocí rozhraní Azure CLI a webu Azure Portal.

![Publikovaná webová aplikace v prohlížeči Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořit obecný účet služby Azure Storage
> * Nasadit kód bez serveru pomocí služby Azure Functions
> * Vytvořit odběr události služby Blob Storage ve službě Event Grid

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

+ Nejdřív je potřeba dokončit předchozí kurz o službě Blob Storage: [Odeslání dat obrázků do cloudu v Azure Storage][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít verzi Azure CLI 2.0.14 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud nepoužíváte cloudové prostředí, musíte se nejdřív přihlásit pomocí `az login`.

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

Azure Functions vyžaduje obecný účet úložiště. V rámci skupiny prostředků vytvořte samostatný obecný účet úložiště pomocí příkazu [az storage account create](/cli/azure/storage/account#az_storage_account_create).

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. 

V následujícím příkazu nahraďte zástupný symbol `<general_storage_account>` vlastním globálně jedinečným názvem obecného účtu úložiště. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Vytvoření Function App  

K hostování provádění funkcí potřebujete aplikaci Function App. Function App poskytuje prostředí pro provádění kódu funkce bez serveru. Aplikaci Function App vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#az_functionapp_create). 

V následujícím příkazu nahraďte zástupný symbol `<function_app>` vlastním jedinečným názvem aplikace Function App. Název aplikace funkcí se použije jako výchozí doména DNS pro příslušnou aplikaci funkcí, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. Hodnotu `<general_storage_account>` nahraďte názvem obecného účtu úložiště, který jste vytvořili.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Teď je potřeba nakonfigurovat připojení aplikace funkcí k účtu služby Blob Storage, který jste vytvořili v [předchozím kurzu][previous-tutorial].

## <a name="configure-the-function-app"></a>Konfigurace aplikace Function App

Funkce potřebuje k připojení k účtu služby Blob Storage připojovací řetězec. Kód funkce, který v následujícím kroku nasadíte do Azure, hledá připojovací řetězec v nastavení aplikace myblobstorage_STORAGE a název kontejneru obrázků miniatur v nastavení aplikace myContainerName. Připojovací řetězec zobrazíte pomocí příkazu [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Pomocí příkazu [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) nastavte nastavení aplikace.

V následujících příkazech rozhraní příkazového řádku je `<blob_storage_account>` název účtu služby Blob Storage, který jste vytvořili v předchozím kurzu.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Teď můžete nasadit do této aplikace Function App nasadit projekt projektu kódu funkce.

## <a name="deploy-the-function-code"></a>Nasazení kódu funkce 

Funkce jazyka C#, která provádí změnu velikosti obrázků, je k dispozici v [tomto úložišti GitHub](https://github.com/Azure-Samples/function-image-upload-resize). Pomocí příkazu [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config) nasaďte tento projekt kódu funkce do aplikace Function App. 

V následujícím příkazu je `<function_app>` název aplikace funkcí, kterou jste vytvořili dříve.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Funkce změny velikosti obrázků se aktivuje požadavky HTTP, které se do ní odesílají ze služby Event Grid. Službě Event Grid můžete sdělit, že chcete přijímat tato oznámení na adrese URL vaší funkce, vytvořením odběru událostí. Pro účely tohoto kurzu se přihlásíte k odběru událostí vytvářených objekty blob.

Data předávaná do funkce z oznámení služby Event Grid zahrnují adresu URL objektu blob. Tato adresa URL se pak předá do vstupní vazby za účelem získání nahraného obrázku ze služby Blob Storage. Funkce vygeneruje obrázek miniatury a zapíše výsledný datový proud do samostatného kontejneru ve službě Blob Storage. 

Tento projekt používá aktivační události typu `EventGridTrigger`. Použití aktivační události Event Grid je vhodnější než obecné aktivační události HTTP. Event Grid automaticky ověřuje aktivační události funkcí Event Grid. U obecných aktivačních událostí HTTP je potřeba implementovat [odpověď ověření](security-authentication.md#webhook-event-delivery).

Další informace o této funkci najdete v [souborech function.json a run.csx](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Kód projektu funkce se nasadí přímo z veřejného úložiště ukázek. Další informace o možnostech nasazení pro Azure Functions najdete v článku[Průběžné nasazování se službou Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-an-event-subscription"></a>Vytvoření odběru událostí

Odběr událostí udává, které události vygenerované zprostředkovatelem chcete odeslat do určitého koncového bodu. V tomto případě zveřejňuje koncový bod vaše funkce. Pomocí následujících kroků vytvořte na webu Azure Portal odběr událostí, který odesílá oznámení do vaší funkce: 

1. Na webu [Azure Portal](https://portal.azure.com) kliknutím na šipku vlevo dole rozbalte všechny služby, do pole **Filtr** zadejte *funkce* a pak zvolte **Aplikace funkcí**. 

    ![Přechod na aplikace Function App na webu Azure Portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Rozbalte svoji aplikaci Function App, vyberte funkci **imageresizerfunc** a potom vyberte **Přidat předplatné Event Gridu**.

    ![Přechod na aplikace Function App na webu Azure Portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Použijte nastavení odběru událostí uvedená v tabulce.
    
    ![Vytvoření odběru událostí z funkce na webu Azure Portal](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název** | imageresizersub | Název identifikující nový odběr událostí. | 
    | **Typ tématu** |  Účty úložiště | Vyberte zprostředkovatele událostí Účty úložiště. | 
    | **Předplatné** | Vaše předplatné Azure | Ve výchozím nastavení je vybrané vaše aktuální předplatné Azure.   |
    | **Skupina prostředků** | myResourceGroup | Vyberte **Použít existující** a zvolte skupinu prostředků, které jste už používali v tomto kurzu.  |
    | **Instance** |  Váš účet služby Blob Storage |  Vyberte účet služby Blob Storage, který jste vytvořili. |
    | **Typy událostí** | Vytvoření objektu blob | Zrušte zaškrtnutí všech typů komě **Vytvoření objektu blob**. Do funkce se předají jenom události typu `Microsoft.Storage.BlobCreated`.| 
    | **Typ odběratele** |  Webhook |  Možné hodnoty jsou Webhook nebo Event Hubs. |
    | **Koncový bod odběratele** | automaticky generovaný | Použijte adresu URL koncového bodu, která se vygeneruje. | 
    | **Filtr předpon** | /blobServices/default/containers/images/blobs/ | Vyfiltruje události úložiště jenom na ty, které jsou v kontejneru **images**.| 

4. Přidejte odběr událostí kliknutím na **Vytvořit**. Vytvoří se odběr událostí, který při přidání objektu blob do kontejneru *images* aktivuje funkci `imageresizerfunc`. Tato funkce změní velikost obrázků a přidá je do kontejneru *thumbs*.

Teď máte nakonfigurované back-endové služby a můžete funkci změny velikosti obrázků otestovat v ukázkové webové aplikaci. 

## <a name="test-the-sample-app"></a>Testování ukázkové aplikace

Pokud chcete ve webové aplikaci otestovat změnu velikosti obrázků, přejděte na adresu URL publikované aplikace. Výchozí adresa URL webové aplikace je `https://<web_app>.azurewebsites.net`.

Klikněte na oblast **Nahrát fotografie** a vyberte a nahrajte soubor. Do této oblasti také můžete fotografii přetáhnout. 

Všimněte si, že po zmizení nahraného obrázku se na karuselu **Vygenerované miniatury** zobrazí kopie nahraného obrázku. Funkce změnila velikost tohoto obrázku, přidala miniaturu do kontejneru *thumbs* a webový klient ji stáhl.

![Publikovaná webová aplikace v prohlížeči Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvořit obecný účet služby Azure Storage
> * Nasadit kód bez serveru pomocí služby Azure Functions
> * Vytvořit odběr události služby Blob Storage ve službě Event Grid

Ve třetí části série kurzů o službě Storage se dozvíte, jak zabezpečit přístup k účtu úložiště.

> [!div class="nextstepaction"]
> [Zabezpečený přístup k datům aplikací v cloudu](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Další informace o službě Event Grid najdete v článku [Úvod do služby Azure Event Grid](overview.md). 
+ Pokud chcete vyzkoušet jiný kurz, který se týká služby Azure Functions, podívejte se na článek [Vytvoření funkce pro integraci s Azure Logic Apps](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
