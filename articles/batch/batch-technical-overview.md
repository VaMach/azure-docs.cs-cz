<properties
    pageTitle="Základy služby Azure Batch | Microsoft Azure"
    description="Další informace o používání služby Azure Batch pro rozsáhlé paralelní úlohy a úlohy v prostředí HPC."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# Základy služby Azure Batch

Služba Azure Batch umožňuje efektivně spouštět rozsáhlé paralelní aplikace a aplikace vysokovýkonného výpočetního prostředí (HPC) v cloudu. Je to služba platformy, která plánuje spouštění výpočetně náročných úloh ve spravované kolekci virtuálních počítačů a která dokáže automaticky škálovat výpočetní prostředky tak, aby splňovaly potřeby vašich úloh.

Pomocí služby Batch definujete výpočetní prostředky, které vaše aplikace spustí paralelně a škálovaně. Můžete spouštět úlohy na vyžádání nebo naplánované úlohy a není nutné ručně vytvářet, konfigurovat a spravovat cluster prostředí HPC, jednotlivé virtuální počítače, virtuální sítě ani infrastrukturu komplexních úloh nebo plánování úkolů.

## Případy použití služby Batch

Batch je spravovaná služba Azure, která slouží k *dávkovému zpracování* a *dávkovým výpočtům* – spouštění velkého objemu podobných úloh s cílem dosáhnout požadovaného výsledku. Dávkové zpracování nejčastěji používají organizace, které pravidelně zpracovávají, transformují a analyzují velké objemy dat.

Služba Batch pracuje s vnitřně paralelními aplikacemi a úlohami (také známé jako „jednoduše paralelně zpracovatelné“). Vnitřně paralelní úlohy se snadno rozdělují na více úkolů, které provádějí práci současně na mnoha počítačích.

![Paralelní úkoly][1]<br/>

Mezi příklady úloh, které se běžně zpracovávají touto technikou, patří:

* Modelování finančních rizik
* Analýza dat klimatu a hydrologie
* Vykreslování, analýza a zpracování obrázků
* Kódování a překódování multimédií
* Genetická sekvenční analýza
* Analýza zátěže v inženýrství
* Testování softwaru

Služba Batch dokáže rovněž provádět paralelní výpočty s fází Reduce na konci a spouštět složitější úlohy v prostředí HPC, například aplikace [Message Passing Interface (MPI)](batch-mpi.md).

Porovnání mezi službou Batch a dalšími možnostmi řešení prostředí HCP najdete v části [Řešení Batch a HPC](batch-hpc-solutions.md).

## Vývoj se službou Batch

Zpracování paralelních úloh službou Batch se obvykle provádí programově pomocí jednoho z [rozhraní API služby Batch](#batch-development-apis). Pomocí rozhraní API služby Batch vytváříte a spravujete fondy počítačových uzlů (virtuální počítače) a plánujete úlohy a úkoly, které se budou na těchto uzlech spouštět. Klientská aplikace nebo služba, kterou vytvoříte, používá rozhraní API služby Batch ke komunikaci se službou Batch.

Umožní vám to efektivně zpracovávat rozsáhlé úlohy pro vaši organizaci nebo poskytovat front-end služby zákazníkům, aby mohli spouštět úlohy a úkoly – na vyžádání nebo naplánované – na jednom, stovkách nebo tisících uzlů. Službu Batch můžete také používat jako součást rozsáhlejšího pracovního postupu spravovaného nástroji, například [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md).

> [AZURE.TIP] Až budete připraveni rozhraní API služby Batch prozkoumat podrobněji, abyste hlouběji porozuměli jeho poskytovaným funkcím, přečtěte si [Přehled funkcí služby Batch pro vývojáře](batch-api-basics.md).

### Účty Azure, které budete potřebovat

Při vývoji řešení Batch budete ve službě Microsoft Azure používat následující účty.

- **Účet a předplatné služby Azure** – Pokud zatím předplatné služby Azure nemáte, můžete si aktivovat [výhodu předplatitele MSDN][msdn_benefits] nebo si zaregistrovat [bezplatný účet Azure][free_account]. Při vytvoření účtu pro vás bude vytvořeno výchozí předplatné.

- **Účet Batch** – Při komunikaci vašich aplikací se službou Batch se jako přihlašovací údaje používají název účtu, adresa URL účtu a přístupový klíč. Všechny prostředky služby Batch, například fondy, výpočetní uzly, úlohy a úkoly, jsou přidruženy k účtu Batch. [Účet Batch můžete vytvořit a spravovat](batch-account-create-portal.md) na portálu Azure.

- **Účet Storage** – služba Batch zahrnuje integrovanou podporu pro práci se soubory ve [službě Azure Storage][azure_storage]. Téměř každý scénář služby Batch používá službu Azure Storage – pro přípravu programů, které budou vaše úkoly spouštět, a dat, která budou zpracovávat, a také pro ukládání generovaných výstupních dat. Informace o vytvoření účtu služby Storage najdete v článku [Informace o účtech Azure Storage](./../storage/storage-create-storage-account.md).

### Rozhraní API pro vývoj služby Batch

Aplikace a služby mohou pomocí služby Batch vydávat přímá volání rozhraní REST API, používat jednu nebo více následujících klientských knihoven nebo kombinaci obou, a spravovat tak výpočetní prostředky či spouštět paralelní, škálované úlohy.

| Rozhraní API    | API – referenční informace | Stáhnout | Ukázky kódů |
| ----------------- | ------------- | -------- | ------------ |
| **Batch REST** | [MSDN][batch_rest] | Není k dispozici | [MSDN][batch_rest] |
| **Batch .NET**    | [MSDN][api_net] | [NuGet ][api_net_nuget] | [GitHub][api_sample_net] |
| **Batch Python**  | [readthedocs.io][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Batch Node.js** | [github.io][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Batch Java** (preview) | [github.io][api_java] | [Maven][api_java_jar] | [Github][api_sample_java] |

### Správa prostředků Batch

Kromě klientských rozhraní API můžete v rámci účtu Batch ke správě prostředků používat následující funkce.

- [Rutiny prostředí PowerShell služby Batch][batch_ps]: Rutiny služby Azure Batch vám v modulu [Azure PowerShell](../powershell-install-configure.md) umožňují spravovat prostředky Batch v prostředí PowerShell.

- [Azure CLI](../xplat-cli-install.md): Rozhraní příkazového řádku služby Azure (Azure CLI) je sada nástrojů pro různé platformy, která poskytuje příkazy prostředí pro komunikaci s řadou služeb Azure, včetně služby Batch.

- Klientská knihovna [Batch Management .NET](batch-management-dotnet.md): Dostupná také prostřednictvím balíčku [NuGet][api_net_mgmt_nuget]. Pomocí klientské knihovny Batch Management .NET můžete programově spravovat účty Batch, kvóty a balíčky aplikací. Reference pro knihovnu správy naleznete na [MSDN][api_net_mgmt].

### Nástroje služby Batch

Ačkoli následující nástroje nejsou k vytváření řešení pomocí služby Batch vyžadovány, mohou být při sestavování a ladění aplikací a služeb Batch velmi přínosné.

 - [Azure Portal][portal]: Na webu Azure Portal můžete v oknech Batch vytvářet, sledovat a odstraňovat fondy, úlohy a úkoly služby Batch. Můžete zobrazit informace o stavu pro tyto a další prostředky, zatímco spouštíte úlohy, a dokonce i stahovat soubory z výpočetních uzlů ve fondech (při řešení potíží si můžete například stáhnout soubor `stderr.txt` neúspěšného úkolu). Můžete si také stáhnout soubory vzdálené plochy (RDP), které lze použít k přihlášení do výpočetních uzlů.

 - [Azure Batch Explorer][batch_explorer]: Batch Explorer nabízí podobné funkce správy prostředků Batch jako web Azure Portal, ale v samostatné klientské aplikaci Windows Presentation Foundation (WPF). Jednu z ukázkových aplikací Batch .NET dostupnou na [Githubu][github_samples] lze sestavit pomocí sady Visual Studio 2015 nebo novější a použít k procházení a správě prostředků v účtu Batch při vývoji a ladění řešení Batch. Prohlížejte si podrobnosti úloh, fondů a úkolů, stahujte soubory z výpočetních uzlů a připojujte se k těmto uzlům vzdáleně pomocí souborů vzdálené plochy (RDP), které lze stáhnout v nástroji Batch Explorer.

 - [Microsoft Azure Storage Explorer][storage_explorer]: Ačkoliv se nejedná striktně o nástroj služby Azure Batch, představuje program Storage Explorer při vývoji a ladění řešení Batch další hodnotný nástroj.

## Scénář: Horizontální navýšení kapacity paralelní úlohy

Běžné řešení, které rozhraní API služby Batch používají ke komunikaci se službami Batch, zahrnuje škálování vnitřně paralelní práce – například vykreslování obrázků pro 3D scény – na fond výpočetních uzlů. Tento fond výpočetních uzlů může být například „vykreslovací farma“, která bude zajišťovat desítky, stovky nebo i tisíce jader pro vaši úlohu vykreslování.

Následující diagram znázorňuje běžný pracovní postup služby Batch s klientskou aplikací nebo hostovanou službou, která službu Batch používá ke spouštění paralelních úloh.

![Pracovní postup řešení Batch][2]

V tomto běžném scénáři vaše aplikace nebo služba zpracovává výpočetní úlohu ve službě Azure Batch následujícím postupem:

1. **Vstupní soubory** a **aplikaci**, která je bude zpracovávat, odešlete do účtu Azure Storage. Vstupní soubory mohou být jakákoliv data, která vaše aplikace zpracuje, například data finančního modelování nebo video soubory k převodu. Soubory aplikace mohou být z jakékoliv aplikace, která slouží ke zpracování dat, například aplikace pro 3D vykreslování nebo převaděč médií.

2. V účtu Batch vytvořte **fond** výpočetních uzlů služby Batch – tyto uzly jsou virtuální počítače, na kterých se budou vaše úkoly provádět. Zadejte vlastnosti aplikace, jako [velikost uzlu](./../cloud-services/cloud-services-sizes-specs.md), operační systém a umístění ve službě Azure Storage. Tato aplikace se nainstaluje, když se uzly připojí k fondu (aplikace, kterou jste odeslali v kroku č. 1). Fond můžete také nakonfigurovat tak, aby [automaticky škáloval](batch-automatic-scaling.md) – dynamicky upravoval počet výpočetních uzlů ve fondu – v reakci na zatížení, které vaše úlohy generují.

3. Vytvořte **úlohu** služby Batch ke spouštění úloh ve fondu výpočetních uzlů. Když vytvoříte úlohu, přiřaďte ji k fondu služby Batch.

4. Přidejte do úlohy **úkoly**. Když do úlohy přidáte úkoly, služba Batch automaticky naplánuje úkoly k provedení ve výpočetních uzlech ve fondu. Každý úkol používá aplikaci, kterou jste nahráli, ke zpracování vstupních souborů.

    - 4a. Úkol může před spuštěním stáhnout data (vstupní soubory), která bude zpracovávat ve výpočetním uzlu, ke kterému je přiřazen. Pokud zatím aplikace ve daném uzlu není nainstalována (viz krok č. 2), může být v tomto kroku stažena. Po dokončení stahování se úkoly spustí v jejich přiřazených uzlech.

5. Když se úkoly spouštějí, můžete ve službě Batch zadat dotaz na monitorování postupu úloh a jejich úkolů. Klientská aplikace nebo služba komunikuje se službou Batch pomocí protokolu HTTPS, a protože se může stát, že budete monitorovat tisíce úkolů spuštěných v tisících výpočetních uzlů, zadávejte [dotazy do služby Batch efektivně](batch-efficient-list-queries.md).

6. Úkoly mohou po dokončení nahrát data svých výsledků do služby Azure Storage. Soubory můžete také načíst přímo z výpočetních uzlů.

7. Když funkce monitorování zjistí, že jsou úkoly ve vaší úloze dokončené, klientská aplikace nebo služba může stáhnout výstupní data pro další zpracování nebo vyhodnocení.

Mějte na paměti, že toto je pouze jeden ze způsobů použití služby Batch a že tento scénář popisuje pouze několik z dostupných funkcí. Na každém výpočetním uzlu můžete například spustit [několik paralelních úkolů](batch-parallel-node-tasks.md) Nebo můžete použít [úkoly pro přípravu a dokončení úlohy](batch-job-prep-release.md) k přípravě uzlů pro vaše úlohy a následnému vyčištění po skončení úloh.

## Další kroky

Teď máte obecný přehled o použití služby Batch a je na čase službu prozkoumat podrobněji. Zjistěte, jak můžete s její pomocí zpracovat paralelní úlohy náročné na výkon.

- Přečtěte si kapitolu [Přehled funkcí Batch pro vývojáře](batch-api-basics.md), kde získáte podrobnější informace o funkcích rozhraní API, které služba Batch používá ke zpracování vašich úloh. Informace v tomto článku jsou nezbytné pro každého, kdo se chystá používat službu Batch.

- V kapitole [Začínáme s knihovnou Azure Batch pro .NET](batch-dotnet-get-started.md) zjistíte, jak použít C# a knihovnu Batch .NET ke spuštění jednoduché úlohy s použitím běžného pracovního postupu služby Batch. Při studiu používání služby Batch by měl být tento článek jednou z vašich prvních zastávek. Tento kurz je také k dispozici ve [verzi pro Python](batch-python-tutorial.md).

- Stáhněte si [ukázky kódu na Githubu][github_samples] a podívejte se, jak mohou C# i Python komunikovat přes rozhraní se službou Batch při plánování a zpracování ukázkových úloh.

- Přečtěte si kapitolu [Studijní program služby Batch][learning_path], kde získáte představu o zdrojích, které jsou pro vás při studiu práce ve službě Batch dostupné.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
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

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png



<!--HONumber=Aug16_HO4-->


