---
title: "Použití rozhraní API a nástrojů služby Batch k vývoji řešení rozsáhlých paralelních zpracování | Dokumentace Microsoftu"
description: "Přečtěte si o dostupných rozhraních API a nástrojích pro vývoj řešení pomocí služby Azure Batch."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 93e37d44-7585-495e-8491-312ed584ab79
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: b886bab44f11354ba60ae06c6d1d671e01316d48
ms.lasthandoff: 03/21/2017

---


# <a name="overview-of-batch-apis-and-tools"></a>Přehled rozhraní API a nástrojů služby Batch

Zpracování paralelních úloh službou Azure Batch se obvykle provádí programově pomocí jednoho z [rozhraní API služby Batch](#batch-development-apis). Vaše klientská aplikace nebo služba může používat rozhraní API služby Batch ke komunikaci se službou Batch. Pomocí rozhraní API služby Batch můžete vytvořit a spravovat fondy výpočetních uzlů – virtuální počítače nebo cloudové služby. Pak můžete plánovat úlohy a úkoly, které se mají v těchto uzlech spouštět. 

Umožní vám to efektivně zpracovávat rozsáhlé úlohy pro vaši organizaci nebo poskytovat front-end služby zákazníkům, aby mohli spouštět úlohy a úkoly – na vyžádání nebo naplánované – na jednom, stovkách nebo tisících uzlů. Službu Azure Batch můžete také používat jako součást rozsáhlejšího pracovního postupu spravovaného nástroji, například [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [!TIP]
> Až budete připraveni rozhraní API služby Batch prozkoumat podrobněji, abyste hlouběji porozuměli jeho poskytovaným funkcím, přečtěte si [Přehled funkcí služby Batch pro vývojáře](batch-api-basics.md).
> 
> 

## <a name="azure-accounts-youll-need"></a>Účty Azure, které budete potřebovat
Při vývoji řešení Batch budete ve službě Microsoft Azure používat následující účty.

* **Účet a předplatné Azure** – Pokud zatím předplatné Azure nemáte, můžete si aktivovat [výhodu předplatitele MSDN][msdn_benefits] nebo si zaregistrovat [bezplatný účet Azure][free_account]. Při vytvoření účtu pro vás bude vytvořeno výchozí předplatné.
* **Účet Batch** – Prostředky služby Azure Batch, včetně fondů, výpočetních uzlů, úloh a úkolů, jsou přidružené k účtu Azure Batch. Pokud vaše aplikace odešle požadavek na službu Batch, ověří se tato žádost pomocí názvu účtu Azure Batch, adresy URL účtu a přístupové klávesy. [Účet Batch můžete vytvořit](batch-account-create-portal.md) na webu Azure Portal.
* **Účet Storage** – Služba Batch zahrnuje integrovanou podporu pro práci se soubory ve službě [Azure Storage][azure_storage]. Téměř každý scénář služby Batch používá úložiště objektů blob v Azure – pro přípravu programů, které budou vaše úkoly spouštět, a dat, která budou zpracovávat, a také pro ukládání generovaných výstupních dat. Informace o vytvoření účtu služby Storage najdete v článku [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md).

## <a name="batch-development-apis"></a>Rozhraní API pro vývoj služby Batch
Vaše aplikace a služby mohou provádět přímá volání rozhraní REST API nebo používat jednu nebo více následujících klientských knihoven ke spuštění a správě úloh služby Azure Batch.

| Rozhraní API | API – referenční informace | Stáhnout | Kurz | Ukázky kódů | Další informace |
| --- | --- | --- | --- | --- | --- |
| **Batch REST** |[MSDN][batch_rest] |Není k dispozici |- |- | [Podporované verze](https://docs.microsoft.com/rest/api/batchservice/batch-service-rest-api-versioning) |
| **Batch .NET** |[docs.microsoft.com][api_net] |[NuGet ][api_net_nuget] |[Kurz](batch-dotnet-get-started.md) |[GitHub][api_sample_net] | [Poznámky k verzi](https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/Client/changelog.md) |
| **Batch Python** |[readthedocs.io][api_python] |[PyPI][api_python_pypi] |[Kurz](batch-python-tutorial.md)|[GitHub][api_sample_python] | [Soubor Readme](https://github.com/Azure/azure-sdk-for-python/blob/master/doc/batch.rst) |
| **Batch Node.js** |[github.io][api_nodejs] |[npm][api_nodejs_npm] |- |- | [Soubor Readme](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/batch) |
| **Batch Java** (preview) |[github.io][api_java] |[Maven][api_java_jar] |- |[Soubor Readme][api_sample_java] | [Soubor Readme](https://github.com/Azure/azure-batch-sdk-for-java)|

## <a name="batch-command-line-tools"></a>Nástroje příkazového řádku služby Batch

Funkce poskytované vývojovými rozhraními API jsou dostupné také prostřednictvím nástrojů příkazového řádku: 

* [Rutiny prostředí PowerShell služby Batch][batch_ps]: Rutiny služby Azure Batch vám v modulu [Azure PowerShell](/powershell/azureps-cmdlets-docs) umožňují spravovat prostředky Batch v prostředí PowerShell.
* [Azure CLI](../cli-install-nodejs.md): Rozhraní příkazového řádku služby Azure (Azure CLI) je sada nástrojů pro různé platformy, která poskytuje příkazy prostředí pro komunikaci s řadou služeb Azure, včetně služby Batch.

## <a name="batch-resource-management"></a>Správa prostředků Batch

Rozhraní API Azure Resource Manageru pro službu Batch poskytují programový přístup k účtům Batch. Pomocí těchto rozhraní API můžete programově spravovat účty Batch, kvóty a balíčky aplikací.  

| Rozhraní API | API – referenční informace | Stáhnout | Kurz | Ukázky kódů |
| --- | --- | --- | --- | --- |
| **REST Batch Resource Manageru** |[docs.microsoft.com][api_rest_mgmt] |– |- |[GitHub](https://github.com/Azure-Samples/batch-dotnet-manage-batch-accounts) |
| **.NET Batch Resource Manageru** |[docs.microsoft.com][api_net_mgmt] |[NuGet ][api_net_mgmt_nuget] | [Kurz](batch-management-dotnet.md) |[GitHub][api_sample_net] |


## <a name="batch-tools"></a>Nástroje služby Batch
Ačkoli následující nástroje nejsou k vytváření řešení pomocí služby Batch vyžadovány, mohou být při sestavování a ladění aplikací a služeb Batch velmi přínosné.

* [Azure Portal][portal]: Na webu Azure Portal můžete v oknech Batch vytvářet, monitorovat a odstraňovat fondy, úlohy a úkoly služby Batch. Můžete zobrazit informace o stavu pro tyto a další prostředky, zatímco spouštíte úlohy, a dokonce i stahovat soubory z výpočetních uzlů ve fondech (při řešení potíží si můžete například stáhnout soubor `stderr.txt` neúspěšného úkolu). Můžete si také stáhnout soubory vzdálené plochy (RDP), které lze použít k přihlášení do výpočetních uzlů.
* [Azure Batch Explorer][batch_explorer]: Batch Explorer nabízí podobné funkce správy prostředků Batch jako web Azure Portal, ale v samostatné klientské aplikaci Windows Presentation Foundation (WPF). Jednu z ukázkových aplikací Batch .NET dostupnou na [GitHubu][github_samples] lze sestavit pomocí sady Visual Studio 2015 nebo novější a použít k procházení a správě prostředků v účtu Batch při vývoji a ladění řešení Batch. Prohlížejte si podrobnosti úloh, fondů a úkolů, stahujte soubory z výpočetních uzlů a připojujte se k těmto uzlům vzdáleně pomocí souborů vzdálené plochy (RDP), které lze stáhnout v nástroji Batch Explorer.
* [Microsoft Azure Storage Explorer][storage_explorer]: Ačkoliv se nejedná vyloženě o nástroj služby Azure Batch, Storage Explorer je dalším užitečným nástrojem při vývoji a ladění řešení Batch.

## <a name="next-steps"></a>Další kroky

* Přečtěte si téma [Přehled funkcí Batch pro vývojáře](batch-api-basics.md), kde jsou základní informace pro každého, kdo se připravuje použít Batch. Článek obsahuje podrobné informace o prostředcích služby Batch, jako jsou fondy, uzly a úlohy, a mnoha funkcích rozhraní API, které můžete použít při vytváření aplikace Batch.
* V kapitole [Začínáme s knihovnou Azure Batch pro .NET](batch-dotnet-get-started.md) zjistíte, jak použít C# a knihovnu Batch .NET ke spuštění jednoduché úlohy s použitím běžného pracovního postupu služby Batch. Při studiu používání služby Batch by měl být tento článek jednou z vašich prvních zastávek. Tento kurz je také k dispozici ve [verzi pro Python](batch-python-tutorial.md).
* Stáhněte si [ukázky kódu na GitHubu][github_samples] a podívejte se, jak můžou jazyk C# i Python komunikovat přes rozhraní se službou Batch při plánování a zpracování ukázkových úloh.
* Přečtěte si [Postup výuky pro Batch][learning_path], kde získáte představu o zdrojích, které jsou pro vás při studiu práce ve službě Batch dostupné.


[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_rest_mgmt]: https://docs.microsoft.com/\rest/api/batchmanagement/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

