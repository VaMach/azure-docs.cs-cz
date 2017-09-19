---
title: "Azure Batch spouští rozsáhlé paralelní výpočetní řešení v cloudu | Dokumentace Microsoftu"
description: "Další informace o používání služby Azure Batch pro rozsáhlé paralelní úlohy a úlohy v prostředí HPC."
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
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: a99f96db0c1e8bcd0cf29c564e5badf0eb728e56
ms.contentlocale: cs-cz
ms.lasthandoff: 09/14/2017

---
# <a name="run-intrinsically-parallel-workloads-with-batch"></a>Spuštění vnitřně paralelních úlohy pomocí služby Batch

Azure Batch je služba platformy pro efektivní spouštění rozsáhlých paralelních aplikací a aplikací vysoce výkonného výpočetního prostředí (HPC) v cloudu. Azure Batch plánuje spouštění výpočetně náročných úloh ve spravované kolekci virtuálních počítačů a dokáže automaticky škálovat výpočetní prostředky tak, aby splňovaly potřeby vašich úloh.

Se službou Azure Batch můžete snadno definovat výpočetní prostředky, které vaše aplikace spustí paralelně a škálovaně. Není nutné ručně vytvářet, konfigurovat a spravovat cluster prostředí HPC, jednotlivé virtuální počítače, virtuální sítě ani infrastrukturu komplexních úloh nebo plánování úkolů. Azure Batch pro vás tyto úlohy automatizuje nebo zjednodušuje.

## <a name="use-cases-for-batch"></a>Případy použití služby Batch
Batch je spravovaná služba Azure, která slouží k *dávkovému zpracování* a *dávkovým výpočtům* – spouštění velkého objemu podobných úloh pro požadovaný výsledek. Dávkové zpracování nejčastěji používají organizace, které pravidelně zpracovávají, transformují a analyzují velké objemy dat.

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

Porovnání mezi službou Batch a dalšími možnostmi řešení prostředí HCP v Azure najdete v tématu [Řešení prostředí HPC, Batch a Big Compute](../virtual-machines/linux/high-performance-computing.md).

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="scenario-scale-out-a-parallel-workload"></a>Scénář: Horizontální navýšení kapacity paralelní úlohy
Běžné řešení, které rozhraní API služby Batch používají ke komunikaci se službami Batch, zahrnuje škálování vnitřně paralelní práce – například vykreslování obrázků pro 3D scény – na fond výpočetních uzlů. Tento fond výpočetních uzlů může být například „vykreslovací farma“, která bude zajišťovat desítky, stovky nebo i tisíce jader pro vaši úlohu vykreslování.

Následující diagram znázorňuje běžný pracovní postup služby Batch s klientskou aplikací nebo hostovanou službou, která službu Batch používá ke spouštění paralelních úloh.

![Pracovní postup řešení Batch][2]

V tomto běžném scénáři vaše aplikace nebo služba zpracovává výpočetní úlohu ve službě Azure Batch následujícím postupem:

1. **Vstupní soubory** a **aplikaci**, která je bude zpracovávat, odešlete do účtu Azure Storage. Vstupní soubory mohou být jakákoliv data, která vaše aplikace zpracuje, například data finančního modelování nebo video soubory k převodu. Soubory aplikace mohou být z jakékoliv aplikace, která slouží ke zpracování dat, například aplikace pro 3D vykreslování nebo převaděč médií.
2. V účtu Batch vytvořte **fond** výpočetních uzlů služby Batch – tyto uzly jsou virtuální počítače, na kterých se budou vaše úkoly provádět. Zadejte vlastnosti aplikace, jako [velikost uzlu](../cloud-services/cloud-services-sizes-specs.md), operační systém a umístění ve službě Azure Storage. Tato aplikace se nainstaluje, když se uzly připojí k fondu (aplikace, kterou jste odeslali v kroku č. 1). Fond můžete také nakonfigurovat tak, aby [automaticky škáloval](batch-automatic-scaling.md) v reakci na zatížení, které vaše úlohy generují. Automatické škálování dynamicky přizpůsobí počet výpočetních uzlů ve fondu.
3. Vytvořte **úlohu** služby Batch ke spouštění úloh ve fondu výpočetních uzlů. Když vytvoříte úlohu, přiřaďte ji k fondu služby Batch.
4. Přidejte do úlohy **úkoly**. Když do úlohy přidáte úkoly, služba Batch automaticky naplánuje úkoly k provedení ve výpočetních uzlech ve fondu. Každý úkol používá aplikaci, kterou jste nahráli, ke zpracování vstupních souborů.
   
   * 4a. Úkol může před spuštěním stáhnout data (vstupní soubory), která bude zpracovávat ve výpočetním uzlu, ke kterému je přiřazen. Pokud zatím aplikace ve daném uzlu není nainstalována (viz krok č. 2), může být v tomto kroku stažena. Po dokončení stahování se úkoly spustí v jejich přiřazených uzlech.
5. Když se úkoly spouštějí, můžete ve službě Batch zadat dotaz na monitorování postupu úloh a jejich úkolů. Vaše klientská aplikace nebo služba komunikuje se službou Batch přes HTTPS. Vzhledem k tomu, že můžete monitorovat tisíce úloh spuštěných v tisících výpočetních uzlů, ujistěte se, že [se služby Batch dotazujete efektivně](batch-efficient-list-queries.md).
6. Úkoly mohou po dokončení nahrát data svých výsledků do služby Azure Storage. Soubory můžete také načíst přímo ze systému souborů na výpočetním uzlu.
7. Když funkce monitorování zjistí, že jsou úkoly ve vaší úloze dokončené, klientská aplikace nebo služba může stáhnout výstupní data pro další zpracování nebo vyhodnocení.

Mějte na paměti, že toto je pouze jeden ze způsobů použití služby Batch a že tento scénář popisuje pouze několik z dostupných funkcí. Na každém výpočetním uzlu můžete například spustit [několik paralelních úkolů](batch-parallel-node-tasks.md) Nebo můžete použít [úkoly pro přípravu a dokončení úlohy](batch-job-prep-release.md) k přípravě uzlů pro vaše úlohy a následnému vyčištění po skončení úloh.

## <a name="next-steps"></a>Další kroky
Teď máte obecný přehled o použití služby Batch a je na čase službu prozkoumat podrobněji. Zjistěte, jak můžete s její pomocí zpracovat paralelní úlohy náročné na výkon.

* Přečtěte si téma [Přehled funkcí Batch pro vývojáře](batch-api-basics.md), kde jsou základní informace pro každého, kdo se připravuje použít Batch. Článek obsahuje podrobné informace o prostředcích služby Batch, jako jsou fondy, uzly a úlohy, a mnoha funkcích rozhraní API, které můžete použít při vytváření aplikace Batch.
* Další informace o dostupných [rozhraních API a nástrojích služby Batch](batch-apis-tools.md) pro sestavování řešení Batch.
* V kapitole [Začínáme s knihovnou Azure Batch pro .NET](batch-dotnet-get-started.md) zjistíte, jak použít C# a knihovnu Batch .NET ke spuštění jednoduché úlohy s použitím běžného pracovního postupu služby Batch. Při studiu používání služby Batch by měl být tento článek jednou z vašich prvních zastávek. Tento kurz je také k dispozici ve [verzi pro Python](batch-python-tutorial.md).
* Stáhněte si [ukázky kódu na GitHubu][github_samples] a podívejte se, jak můžou jazyk C# i Python komunikovat přes rozhraní se službou Batch při plánování a zpracování ukázkových úloh.
* Přečtěte si [Postup výuky pro Batch][learning_path], kde získáte představu o zdrojích, které jsou pro vás při studiu práce ve službě Batch dostupné.


[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png

