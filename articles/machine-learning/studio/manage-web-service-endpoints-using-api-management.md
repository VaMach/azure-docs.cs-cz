---
title: "Naučte se spravovat AzureML webové služby pomocí rozhraní API Management | Microsoft Docs"
description: "Příručka znázorňující postup správy AzureML webové služby pomocí rozhraní API správy."
keywords: "strojového učení, rozhraní api management"
services: machine-learning
documentationcenter: 
author: roalexan
manager: jhubbard
editor: 
ms.assetid: 05150ae1-5b6a-4d25-ac67-fb2f24a68e8d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: roalexan
ms.openlocfilehash: 53a6b18fb74db46ccb66c7c70851a9bf364e927c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="learn-how-to-manage-azureml-web-services-using-api-management"></a>Zjistěte, jak spravovat webové služby Azure ML pomocí služby API Management
## <a name="overview"></a>Přehled
Tento průvodce vám ukáže, jak rychle začít spravovat vaše AzureML webové služby pomocí rozhraní API Management.

## <a name="what-is-azure-api-management"></a>Co je Azure API Management?
Azure API Management je služba Azure, která umožňuje spravovat koncové body REST API definováním přístupu uživatele, omezení využití a řídicí panel monitorování. Klikněte na tlačítko [sem](https://azure.microsoft.com/services/api-management/) podrobnosti o službě Azure API Management. Klikněte na tlačítko [sem](../../api-management/api-management-get-started.md) informace o tom, jak začít pracovat s Azure API Management. Tento další průvodce, která tato příručka je založena na, popisuje další témata, včetně konfigurace oznámení, cenovou úroveň, zpracování odpovědi, ověřování uživatelů, vytvoření produktů, developer odběry a dashboarding využití.

## <a name="what-is-azureml"></a>Co je AzureML?
AzureML je služba Azure pro strojové učení, která umožňuje snadno vytvářet, nasazovat a sdílet pokročilou analýzu řešení. Klikněte na tlačítko [sem](https://azure.microsoft.com/services/machine-learning/) podrobnosti o AzureML.

## <a name="prerequisites"></a>Požadavky
K dokončení tohoto průvodce, budete potřebovat:

* Účet Azure. Pokud nemáte účet Azure, klikněte na tlačítko [sem](https://azure.microsoft.com/pricing/free-trial/) podrobnosti o tom, jak vytvořit Bezplatný zkušební účet.
* Účet AzureML. Pokud nemáte účet AzureML, klikněte na tlačítko [sem](https://studio.azureml.net/) podrobnosti o tom, jak vytvořit Bezplatný zkušební účet.
* Pracovní prostor, služby a api_key pro experimentu AzureML nasadit jako webovou službu. Klikněte na tlačítko [sem](create-experiment.md) podrobnosti o tom, jak vytvořit nový experiment AzureML. Klikněte na tlačítko [sem](publish-a-machine-learning-web-service.md) pro informace o tom, jak nasadit AzureML experimentovat jako webovou službu. Příloha A případně obsahuje pokyny, jak vytvořit a otestovat jednoduchý experiment AzureML a nasadit jako webovou službu.

## <a name="create-an-api-management-instance"></a>Vytvoření instance služby API Management
Níže jsou uvedeny kroky pro použití ke správě AzureML webová služba API Management. Nejprve vytvořte instanci služby. Přihlaste se k [portálu Classic](https://manage.windowsazure.com/) a klikněte na tlačítko **nový** > **App Services** > **API Management** > **vytvořit**.

![Vytvoření instance](./media/manage-web-service-endpoints-using-api-management/create-instance.png)

Zadejte jedinečný **URL**. Tato příručka používá **demoazureml** – budete muset zvolit něco jiného. Vyberte požadované **Předplatné** a **Oblast** pro instanci služby. Po provedení výběru klikněte na tlačítko Další.

![Vytvoření service-1](./media/manage-web-service-endpoints-using-api-management/create-service-1.png)

Zadejte hodnotu **název organizace**. Tato příručka používá **demoazureml** – budete muset zvolit něco jiného. Zadejte e-mailovou adresu v **e-mailu správce** pole. Tato e-mailová adresa se bude používat pro zasílání oznámení ze systému API Management.

![Vytvoření service-2](./media/manage-web-service-endpoints-using-api-management/create-service-2.png)

Kliknutím na zaškrtávací políčko vytvoříte instanci služby. *Jak dlouho trvá až 30 minut pro vytvoření nové služby*.

## <a name="create-the-api"></a>Vytvoření rozhraní API
Po vytvoření instance služby je dalším krokem je vytvoření rozhraní API. Rozhraní API se skládá ze sady operací, které můžete vyvolat z klientské aplikace. Operace rozhraní API se odesílají přes proxy servery do existujících webových služeb. Tento průvodce vytvoří rozhraní API, zda proxy serveru do existujících webových služeb AzureML RRS a BES.

Rozhraní API vytvoříte a nakonfigurujete z rozhraní API portálu vydavatele, který je přístupný prostřednictvím portálu Azure Classic. Pokud chcete dostat na portál vydavatele, vyberte instanci služby.

![Vyberte instanci služby](./media/manage-web-service-endpoints-using-api-management/select-service-instance.png)

Klikněte na tlačítko **spravovat** na portálu Azure Classic služby API Management.

![Spravovat službu](./media/manage-web-service-endpoints-using-api-management/manage-service.png)

Klikněte na tlačítko **rozhraní API** z **API Management** nabídky na levé straně a pak klikněte na **přidat rozhraní API**.

![rozhraní API. správu nabídky](./media/manage-web-service-endpoints-using-api-management/api-management-menu.png)

Typ **API ukázkový AzureML** jako **název webového rozhraní API**. Typ **https://ussouthcentral.services.azureml.net** jako **adresu URL webové služby**. Typ **azureml ukázku** jako **přípona adresy URL webového rozhraní API**. Zkontrolujte **HTTPS** jako **adresy URL webového rozhraní API** schéma. Vyberte **Starter** jako **produkty**. Po dokončení klikněte na tlačítko **Uložit** k vytvoření rozhraní API.

![Přidat – nový – rozhraní api](./media/manage-web-service-endpoints-using-api-management/add-new-api.png)

## <a name="add-the-operations"></a>Přidání operace
Klikněte na tlačítko **operace přidání** přidání operací do toto rozhraní API.

![Přidání operace](./media/manage-web-service-endpoints-using-api-management/add-operation.png)

**Operaci nového** se zobrazí a **podpis** ve výchozím nastavení bude vybraná karta.

## <a name="add-rrs-operation"></a>Záznamy o prostředku operace přidání
Nejprve vytvořte operace pro službu AzureML RRS. Vyberte **POST** jako **příkaz HTTP**. Typ **/services/ /workspaces/ {prostoru} {služby} / execute? api-version = {apiversion} & Podrobnosti = {Podrobnosti}** jako **adresa URL šablony**. Typ **RRS provést** jako **zobrazovaný název**.

![přidat záznamy o prostředku operace podpisu](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-signature.png)

Klikněte na tlačítko **odpovědí** > **přidat** na levé straně a vyberte **200 OK**. Klikněte na tlačítko **Uložit** uložte tuto operaci.

![přidat záznamy o prostředku operace odpověď](./media/manage-web-service-endpoints-using-api-management/add-rrs-operation-response.png)

## <a name="add-bes-operations"></a>Přidání BES operací
Snímky obrazovky nejsou zahrnuté pro BES operace, jako jsou velmi podobné jako u přidání RRS operaci.

### <a name="submit-but-not-start-a-batch-execution-job"></a>Odeslání (ale nespustí) úlohy Batch Execution
Klikněte na tlačítko **operace přidání** přidání operaci AzureML BES do rozhraní API. Vyberte **POST** pro **příkaz HTTP**. Typ **/services/ /workspaces/ {prostoru} {služby} / úloh? api-version = {apiversion}** pro **adresa URL šablony**. Typ **BES odeslání** pro **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na levé straně a vyberte **200 OK**. Klikněte na tlačítko **Uložit** uložte tuto operaci.

### <a name="start-a-batch-execution-job"></a>Spuštění úlohy Batch Execution
Klikněte na tlačítko **operace přidání** přidání operaci AzureML BES do rozhraní API. Vyberte **POST** pro **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid} / start? api-version = {apiversion}** pro **adresa URL šablony**. Typ **BES spustit** pro **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na levé straně a vyberte **200 OK**. Klikněte na tlačítko **Uložit** uložte tuto operaci.

### <a name="get-the-status-or-result-of-a-batch-execution-job"></a>Získat stav nebo výsledek úlohy Batch Execution
Klikněte na tlačítko **operace přidání** přidání operaci AzureML BES do rozhraní API. Vyberte **získat** pro **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro **adresa URL šablony**. Typ **BES stav** pro **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na levé straně a vyberte **200 OK**. Klikněte na tlačítko **Uložit** uložte tuto operaci.

### <a name="delete-a-batch-execution-job"></a>Odstranit úlohu Batch Execution
Klikněte na tlačítko **operace přidání** přidání operaci AzureML BES do rozhraní API. Vyberte **odstranit** pro **příkaz HTTP**. Typ **/jobs/ /services/ {služby} /workspaces/ {prostoru} {jobid}? api-version = {apiversion}** pro **adresa URL šablony**. Typ **BES odstranit** pro **zobrazovaný název**. Klikněte na tlačítko **odpovědí** > **přidat** na levé straně a vyberte **200 OK**. Klikněte na tlačítko **Uložit** uložte tuto operaci.

## <a name="call-an-operation-from-the-developer-portal"></a>Volání operace z portálu pro vývojáře
Operace lze volat přímo z portálu pro vývojáře, která představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API. V tomto kroku průvodce budete volat **RRS provést** metoda, která byla přidána do **API ukázkový AzureML**. Klikněte na tlačítko **portál pro vývojáře** v nabídce v horní části napravo od portálu Classic.

![portál pro vývojáře](./media/manage-web-service-endpoints-using-api-management/developer-portal.png)

Klikněte na tlačítko **rozhraní API** z horní nabídce a pak klikněte na tlačítko **API ukázkový AzureML** chcete zobrazit dostupné operace.

![demoazureml-api](./media/manage-web-service-endpoints-using-api-management/demoazureml-api.png)

Vyberte **RRS provést** pro operaci. Klikněte na tlačítko **vyzkoušet**.

![Try it](./media/manage-web-service-endpoints-using-api-management/try-it.png)

Žádost o parametry, zadejte vaše **prostoru**, **služby**, **2.0** pro **apiversion**, a **true** pro **podrobnosti**. Můžete najít váš **prostoru** a **služby** v řídicím panelu AzureML webové služby (najdete v části **testování webovou službu** v příloze A).

Hlavičky žádosti, klikněte na tlačítko **přidat hlavičku** a typ **Content-Type** a **application/json**, pak klikněte na tlačítko **přidat hlavičku** a typ **autorizace** a **nosiče <YOUR AZUREML SERVICE API-KEY>** . Můžete najít váš **klíč rozhraní api** v řídicím panelu AzureML webové služby (najdete v části **testování webovou službu** v příloze A).

Typ **{"Vstupy": {"input1": {"ColumnNames": ["Col2"], "hodnoty": [["Toto je dobrý den"]]}}, "GlobalParameters": {}}** tělo žádosti.

![rozhraní api azureml demo](./media/manage-web-service-endpoints-using-api-management/azureml-demo-api.png)

Klikněte na tlačítko **odeslat**.

![Odeslat](./media/manage-web-service-endpoints-using-api-management/send.png)

Po vyvolání operace portál pro vývojáře zobrazí **požadovaná adresa URL** ze služby back-end **stav odpovědi**, **hlavičky odpovědi**a jakýkoli **obsah odpovědi**.

![Stav odpovědi](./media/manage-web-service-endpoints-using-api-management/response-status.png)

## <a name="appendix-a---creating-and-testing-a-simple-azureml-web-service"></a>Příloha A - vytváření a testování jednoduché AzureML webové služby
### <a name="creating-the-experiment"></a>Vytvoření experimentu
V následující tabulce jsou kroky pro vytvoření jednoduchého experimentu AzureML a jeho nasazení jako webové služby. Webové služby trvá, protože vstupní sloupec libovolný text a vrátí sadu funkcí vyjádřena jako celá čísla. Například:

| Text | Hash textu |
| --- | --- |
| To je dobrý den |1 1 2 2 0 2 0 1 |

Nejdřív pomocí prohlížeče podle vaší volby, přejděte na: [https://studio.azureml.net/](https://studio.azureml.net/) a zadejte své přihlašovací údaje pro přihlášení. Dále vytvořte nový prázdný experiment.

![Hledat experimentu – šablony](./media/manage-web-service-endpoints-using-api-management/search-experiment-templates.png)

Přejmenujte jej na **SimpleFeatureHashingExperiment**. Rozbalte položku **uložit datové sady** a přetáhněte ji **kniha recenze z Amazon** do experimentu.

![jednoduché – funkce-algoritmu hash experimentu](./media/manage-web-service-endpoints-using-api-management/simple-feature-hashing-experiment.png)

Rozbalte položku **transformaci dat** a **manipulaci s** a přetáhněte ji **výběr sloupců v datové sadě** do experimentu. Připojit **sešit recenze z Amazon** k **výběr sloupců v datové sadě**.

![select-columns](./media/manage-web-service-endpoints-using-api-management/project-columns.png)

Klikněte na tlačítko **výběr sloupců v datové sadě** a pak klikněte na **spustit selektor sloupců** a vyberte **Col2**. Kliknutím na značku zaškrtnutí použití těchto změn.

![select-columns](./media/manage-web-service-endpoints-using-api-management/select-columns.png)

Rozbalte položku **Analýza textu** a přetáhněte ji **Hashování** do experimentu. Připojit **výběr sloupců v datové sadě** k **Hashování**.

![připojit sloupce projektu](./media/manage-web-service-endpoints-using-api-management/connect-project-columns.png)

Typ **3** pro **algoritmu hash bitsize**. Tím se vytvoří 8 (23) sloupce.

![použití algoritmu hash bitsize](./media/manage-web-service-endpoints-using-api-management/hashing-bitsize.png)

V tomto okamžiku můžete chtít klikněte na tlačítko **spustit** k testování experimentu.

![Spustit](./media/manage-web-service-endpoints-using-api-management/run.png)

### <a name="create-a-web-service"></a>Vytvoření webové služby
Teď vytvořte webovou službu. Rozbalte položku **webové služby** a přetáhněte ji **vstup** do experimentu. Připojit **vstup** k **Hashování**. Také přetáhněte **výstup** do experimentu. Připojit **výstup** k **Hashování**.

![výstup na--hashování](./media/manage-web-service-endpoints-using-api-management/output-to-feature-hashing.png)

Klikněte na tlačítko **publikování webové služby**.

![publikování – webové služby](./media/manage-web-service-endpoints-using-api-management/publish-web-service.png)

Klikněte na tlačítko **Ano** publikovat experimentu.

![Ano publikování](./media/manage-web-service-endpoints-using-api-management/yes-to-publish.png)

### <a name="test-the-web-service"></a>Test webové služby
Webové služby AzureML se skládá z RSS (požadavků a odpovědí služby) a koncové body BES (dávky spuštění služby). RSS je pro synchronní zpracování. BES je pro provádění asynchronní úlohy. Testování webové služby s ukázkový zdroj Python níže, budete muset stáhnout a nainstalovat sadu Azure SDK pro jazyk Python (viz: [postup instalace Python](../../python-how-to-install.md)).

Budete také potřebovat **prostoru**, **služby**, a **api_key** experimentu pro následující ukázkový zdroj. Pracovní prostor a služby zjistíte kliknutím na možnost **požadavků a odpovědí** nebo **Batch Execution** svého experimentu v řídicím panelu webové služby.

![Najít prostoru a service](./media/manage-web-service-endpoints-using-api-management/find-workspace-and-service.png)

Můžete najít **api_key** kliknutím experimentu v řídicím panelu webové služby.

![najít klíč rozhraní api](./media/manage-web-service-endpoints-using-api-management/find-api-key.png)

#### <a name="test-rrs-endpoint"></a>Koncový bod RRS testu
##### <a name="test-button"></a>Tlačítko Test
Snadný způsob, jak otestovat záznamy o prostředku koncového bodu je kliknout na **testování** na řídicím panelu webové služby.

![Test](./media/manage-web-service-endpoints-using-api-management/test.png)

Typ **to je dobrý den** pro **col2**. Kliknutím na značku zaškrtnutí.

![zadávání dat](./media/manage-web-service-endpoints-using-api-management/enter-data.png)

Zobrazí se něco podobného jako

![Ukázkový výstup](./media/manage-web-service-endpoints-using-api-management/sample-output.png)

##### <a name="sample-code"></a>Ukázkový kód
Jiný způsob, jak otestovat váš RRS je z vašeho kódu klienta. Pokud kliknete na tlačítko **požadavků a odpovědí** na řídicí panel a přejděte do dolní, uvidíte ukázkový kód pro C#, Python a R. Zobrazí se také syntaxe RRS požadavku, včetně identifikátoru URI žádosti, hlavičky a text.

Tato příručka ukazuje příklad Python funkční. Budete muset upravit její **prostoru**, **služby**, a **api_key** experimentu.

    import urllib2
    import json
    workspace = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE WORKSPACE ID>"
    service = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE SERVICE ID>"
    api_key = "<REPLACE WITH YOUR EXPERIMENT’S WEB SERVICE API KEY>"
    data = {
    "Inputs": {
        "input1": {
            "ColumnNames": ["Col2"],
            "Values": [ [ "This is a good day" ] ]
        },
    },
    "GlobalParameters": { }
    }
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace + "/services/" + service + "/execute?api-version=2.0&details=true"
    headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
    body = str.encode(json.dumps(data))
    try:
        req = urllib2.Request(url, body, headers)
        response = urllib2.urlopen(req)
        result = response.read()
        print "result:" + result
            except urllib2.HTTPError, error:
        print("The request failed with status code: " + str(error.code))
        print(error.info())
        print(json.loads(error.read()))

#### <a name="test-bes-endpoint"></a>Koncový bod BES testu
Klikněte na tlačítko **spuštění dávky** na řídicí panel a přejděte do dolní. Zobrazí se ukázkový kód pro C#, Python a R. Zobrazí se také syntaxe BES žádosti, které chcete odeslat úlohu, spustit úlohu, získání stavu nebo výsledků úlohy a odstranit úlohu.

Tato příručka ukazuje příklad Python funkční. Budete muset upravit její **prostoru**, **služby**, a **api_key** experimentu. Kromě toho budete muset upravit **název účtu úložiště**, **klíč účtu úložiště**, a **název kontejneru úložiště**. Nakonec budete muset změnit umístění **vstupní soubor** a umístění **výstupní soubor**.

    import urllib2
    import json
    import time
    from azure.storage import *
    workspace = "<REPLACE WITH YOUR WORKSPACE ID>"
    service = "<REPLACE WITH YOUR SERVICE ID>"
    api_key = "<REPLACE WITH THE API KEY FOR YOUR WEB SERVICE>"
    storage_account_name = "<REPLACE WITH YOUR AZURE STORAGE ACCOUNT NAME>"
    storage_account_key = "<REPLACE WITH YOUR AZURE STORAGE KEY>"
    storage_container_name = "<REPLACE WITH YOUR AZURE STORAGE CONTAINER NAME>"
    input_file = "<REPLACE WITH THE LOCATION OF YOUR INPUT FILE>" # Example: C:\\mydata.csv
    output_file = "<REPLACE WITH THE LOCATION OF YOUR OUTPUT FILE>" # Example: C:\\myresults.csv
    input_blob_name = "mydatablob.csv"
    output_blob_name = "myresultsblob.csv"
    def printHttpError(httpError):
    print("The request failed with status code: " + str(httpError.code))
    print(httpError.info())
    print(json.loads(httpError.read()))
    return
    def saveBlobToFile(blobUrl, resultsLabel):
    print("Reading the result from " + blobUrl)
    try:
        response = urllib2.urlopen(blobUrl)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    with open(output_file, "w+") as f:
        f.write(response.read())
    print(resultsLabel + " have been written to the file " + output_file)
    return
    def processResults(result):
    first = True
    results = result["Results"]
    for outputName in results:
        result_blob_location = results[outputName]
        sas_token = result_blob_location["SasBlobToken"]
        base_url = result_blob_location["BaseLocation"]
        relative_url = result_blob_location["RelativeLocation"]
        print("The results for " + outputName + " are available at the following Azure Storage location:")
        print("BaseLocation: " + base_url)
        print("RelativeLocation: " + relative_url)
        print("SasBlobToken: " + sas_token)
        if (first):
            first = False
            url3 = base_url + relative_url + sas_token
            saveBlobToFile(url3, "The results for " + outputName)
    return

    def invokeBatchExecutionService():
    url = "https://ussouthcentral.services.azureml.net/workspaces/" + workspace +"/services/" + service +"/jobs"
    blob_service = BlobService(account_name=storage_account_name, account_key=storage_account_key)
    print("Uploading the input to blob storage...")
    data_to_upload = open(input_file, "r").read()
    blob_service.put_blob(storage_container_name, input_blob_name, data_to_upload, x_ms_blob_type="BlockBlob")
    print "Uploaded the input to blob storage"
    input_blob_path = "/" + storage_container_name + "/" + input_blob_name
    connection_string = "DefaultEndpointsProtocol=https;AccountName=" + storage_account_name + ";AccountKey=" + storage_account_key
    payload =  {
        "Input": {
            "ConnectionString": connection_string,
            "RelativeLocation": input_blob_path
        },
        "Outputs": {
            "output1": { "ConnectionString": connection_string, "RelativeLocation": "/" + storage_container_name + "/" + output_blob_name },
        },
        "GlobalParameters": {
        }
    }
        body = str.encode(json.dumps(payload))
    headers = { "Content-Type":"application/json", "Authorization":("Bearer " + api_key)}
    print("Submitting the job...")
    # submit the job
    req = urllib2.Request(url + "?api-version=2.0", body, headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    result = response.read()
    job_id = result[1:-1] # remove the enclosing double-quotes
    print("Job ID: " + job_id)
    # start the job
    print("Starting the job...")
    req = urllib2.Request(url + "/" + job_id + "/start?api-version=2.0", "", headers)
    try:
        response = urllib2.urlopen(req)
    except urllib2.HTTPError, error:
        printHttpError(error)
        return
    url2 = url + "/" + job_id + "?api-version=2.0"

    while True:
        print("Checking the job status...")
        # If you are using Python 3+, replace urllib2 with urllib.request in the follwing code
        req = urllib2.Request(url2, headers = { "Authorization":("Bearer " + api_key) })
        try:
            response = urllib2.urlopen(req)
        except urllib2.HTTPError, error:
            printHttpError(error)
            return
        result = json.loads(response.read())
        status = result["StatusCode"]
        print "status:" + status
        if (status == 0 or status == "NotStarted"):
            print("Job " + job_id + " not yet started...")
        elif (status == 1 or status == "Running"):
            print("Job " + job_id + " running...")
        elif (status == 2 or status == "Failed"):
            print("Job " + job_id + " failed!")
            print("Error details: " + result["Details"])
            break
        elif (status == 3 or status == "Cancelled"):
            print("Job " + job_id + " cancelled!")
            break
        elif (status == 4 or status == "Finished"):
            print("Job " + job_id + " finished!")
            processResults(result)
            break
        time.sleep(1) # wait one second
    return
    invokeBatchExecutionService()
