---
title: "Nahrát mřížky událostí Azure použijte k automatizaci Změna velikosti obrázků | Microsoft Docs"
description: "Azure mřížky událostí můžete aktivovat pro nahrávání objektů blob v Azure Storage. Můžete použít k odeslání souborů image nahrané do úložiště Azure do jiných služeb, jako je například Azure Functions pro změnu velikosti a dalších vylepšení."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 709d23ab590c06d5da9b03e2767bc0be5905355b
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/25/2017
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Změna velikosti nahrané obrázků pomocí mřížky událostí automatizace

[Azure mřížky událostí](overview.md) je služba eventing pro cloud. Událost mřížky umožňuje vytvářet odběry události vyvolané službou Azure služby nebo prostředky třetích stran.  

V tomto kurzu je součástí dva ze série kurzů úložiště. Ji rozšiřuje [předchozí kurzu úložiště] [ previous-tutorial] přidat bez serveru automatické generování miniatur pomocí mřížky událostí Azure a Azure Functions. Umožňuje událostí mřížky [Azure Functions](..\azure-functions\functions-overview.md) reagovat na [úložiště objektů Azure Blob](..\storage\blobs\storage-blobs-introduction.md) události a vytváření miniatur nahrané bitových kopií. Odběr událostí je vytvořen u objektu Blob úložiště vytvoření události. Pokud objekt blob je přidán do konkrétní kontejner úložiště objektů Blob, se nazývá koncový bod funkce. Data předaná vazba funkce z mřížky událostí se používá pro přístup k objektu blob a generovat na miniaturu. 

Pomocí rozhraní příkazového řádku Azure a portálu Azure přidejte funkcí změny velikosti do existující aplikace odesílání bitové kopie.

![Publikované webové aplikace v prohlížeči Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření obecných účtu úložiště Azure
> * Nasazení bez serveru kódu pomocí Azure Functions
> * Vytvoření odběru událostí úložiště objektů Blob v mřížce událostí

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

+ Je nutné provést předchozí kurzu úložiště objektů Blob: [nahrát bitové kopie dat v cloudu s Azure Storage][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, v tomto tématu vyžaduje, že používáte Azure CLI verze 2.0.14 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud nepoužíváte cloudové prostředí, musíte se nejdřív přihlásit pomocí `az login`.

## <a name="create-an-azure-storage-account"></a>Vytvoření účtu služby Azure Storage

Azure Functions vyžaduje účet obecné úložiště. Vytvoření účtu samostatné obecné úložiště ve skupině prostředků s použitím [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz.

Názvy účtů úložiště musí mít od 3 do 24 znaků a můžou obsahovat jenom číslice a malá písmena. 

V následujícím příkazu nahraďte vlastní globálně jedinečný název pro účet obecné úložiště, kde uvidíte `<general_storage_account>` zástupný symbol. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Vytvoření Function App  

Musíte mít aplikaci funkce pro hostování provádění funkce. Function App poskytuje prostředí pro provádění kódu funkce bez serveru. Aplikaci Function App vytvoříte pomocí příkazu [az functionapp create](/cli/azure/functionapp#create). 

V následujícím příkazu nahraďte název vlastní jedinečné funkce aplikace, kde uvidíte `<function_app>` zástupný symbol. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<function_app>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. V takovém případě `<general_storage_account>` je název účtu obecné úložiště, který jste vytvořili.  

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Nyní je nutné nakonfigurovat funkce aplikace pro připojení k úložišti objektů blob. 

## <a name="configure-the-function-app"></a>Konfigurace aplikace – funkce

Funkce musí připojovacího řetězce pro připojení k účtu úložiště objektů blob. V takovém případě `<blob_storage_account>` je název účtu úložiště Blob, který jste vytvořili v předchozí kurzu. Získání připojovacího řetězce s [az úložiště účet zobrazit. připojovací řetězec](/cli/azure/storage/account#show-connection-string) příkaz. Název kontejneru miniaturu musí být také nastaven na `thumbs`. Přidání těchto nastavení aplikace v aplikaci funkce pomocí [az functionapp konfigurace appsettings sadu](/cli/azure/functionapp/config/appsettings#set) příkaz.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

Teď můžete nasadit projektu kódu funkce k této aplikaci funkce.

## <a name="deploy-the-function-code"></a>Nasazení kódu – funkce 

Funkce jazyka C#, která provede změna velikosti obrázků je k dispozici v tomto [úložiště GitHub ukázka](https://github.com/Azure-Samples/function-image-upload-resize). Nasazení projektu kódu této funkce do aplikaci funkce pomocí [az functionapp nasazení zdroj konfigurace](/cli/azure/functionapp/deployment/source#config) příkaz. 

V následujícím příkazu `<function_app>` je stejná funkce aplikace, které jste vytvořili v předchozí skript.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

Funkce změny velikosti obrázku se aktivuje odběr událostí pro událost vytvořit objekt Blob. Data předaná aktivační události obsahuje adresu URL objektu blob, který naopak předaný vstupní vazby získat nahraný obrázek z úložiště objektů Blob. Funkce generuje miniatury a zapíše výsledný datový proud do samostatné kontejneru v úložiště objektů Blob. Další informace o této funkci najdete v tématu [souboru readme v úložišti ukázka](https://github.com/Azure-Samples/function-image-upload-resize/blob/master/README.md).
 
Kód projektu funkce je nasadit přímo z veřejné vzorové úložiště. Další informace o možnostech nasazení pro Azure Functions najdete v tématu [průběžné nasazování pro Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-your-event-subscription"></a>Vytvoření vašeho odběru událostí

Odběr událostí označuje generované zprostředkovatele události, které chcete odeslat na konkrétní. V takovém případě koncový bod je zveřejněný prostřednictvím funkce. Pomocí následujících kroků můžete vytvořit odběr událostí z funkce na portálu Azure: 

1. V [portál Azure](https://portal.azure.com), klikněte na šipku vlevo dole rozbalit všechny služby, zadejte `functions` v **filtru** pole a potom vyberte **funkce aplikace**. 

    ![Přejděte do funkce aplikace na portálu Azure](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Rozbalte funkce aplikace, vyberte **imageresizerfunc** fungovat a potom vyberte **předplatné přidat mřížky událostí**.

    ![Přejděte do funkce aplikace na portálu Azure](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Použijte nastavení přihlášení k odběru událostí specifikovaný v tabulce.

    ![Vytvoření odběru událostí z funkce na portálu Azure](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Nastavení      | Navrhovaná hodnota  | Popis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Název** | imageresizersub | Název, který identifikuje nového předplatného událostí. | 
    | **Typ tématu** |  Účty úložiště | Vyberte zprostředkovatele událostí účet úložiště. | 
    | **Předplatné** | Vaše předplatné | Ve výchozím nastavení měla by být vybrána vaším aktuálním předplatným.   |
    | **Skupina prostředků** | myResourceGroup | Vyberte **použít existující** a zvolte skupinu prostředků, které jste použili v tomto tématu.  |
    | **Instance** |  `<blob_storage_account>` |  Zvolte účet úložiště objektů Blob, kterou jste vytvořili. |
    | **Typy událostí** | Vytvoření objektu BLOB | Zrušte zaškrtnutí všech typů než **Blob vytvořit**. Jenom událost typy `Microsoft.Storage.BlobCreated` jsou předaný funkci.| 
    | **Koncový bod odběratele** | automaticky generovaný | Použijte adresu URL koncového bodu, který je pro vás vygeneroval. | 
    | **Předpona filtru** | / blobServices nebo výchozí/kontejnery nebo bitové kopie nebo objekty BLOB nebo | Vyfiltruje události úložiště jenom na ty na **bitové kopie** kontejneru.| 

4. Klikněte na tlačítko **vytvořit** přidat odběr událostí. Tím se vytvoří odběr událostí, která aktivuje **imageresizerfunc** po přidání objektu blob do **bitové kopie** kontejneru. Změněnou bitové kopie jsou přidány do **palec** kontejneru.

Teď, když jsou nakonfigurované služby back-end, otestovat funkci změny velikosti obrázku v ukázkovou webovou aplikaci. 

## <a name="test-the-sample-app"></a>Testování ukázkové aplikace

K otestování image změnu velikosti ve webové aplikaci, přejděte na adresu URL publikované aplikace. Výchozí adresa URL webové aplikace je `https://<web_app>.azurewebsites.net`.

Klikněte na tlačítko **nahrát fotografie** oblasti a vyberte nahrát soubor. Můžete také přetáhnout fotografie do této oblasti. 

Všimněte si, že po nahraný obrázek zmizí, kopie nahraný obrázek se zobrazí v **generované miniatur** karuselu. Tuto bitovou kopii byl po změně velikosti funkcí, přidat do kontejneru palec a stažen klientem web. 

![Publikované webové aplikace v prohlížeči Edge](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Vytvoření obecných účtu úložiště Azure
> * Nasazení bez serveru kódu pomocí Azure Functions
> * Vytvoření odběru událostí úložiště objektů Blob v mřížce událostí

Přechodu na tři součástí řady úložiště kurzu se dozvíte, jak zabezpečit přístup k účtu úložiště.

> [!div class="nextstepaction"]
> [Zabezpečený přístup k datům aplikací v cloudu](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)


+ Další informace o události mřížky, najdete v části [Úvod do Azure událostí mřížky](overview.md). 
+ Vyzkoušejte jiné kurz, který funkce Azure Functions, najdete v tématu [vytvoří funkci, která se integruje se službou Azure Logic Apps](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md