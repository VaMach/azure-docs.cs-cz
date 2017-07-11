---
title: "Přehled služby Azure Batch pro vývojáře | Dokumentace Microsoftu"
description: "Informace o funkcích služby Batch a jejích rozhraní API z hlediska vývoje."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 416b95f8-2d7b-4111-8012-679b0f60d204
ms.service: batch
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 06/28/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 0120a63ed398cf3e0e91cd4329c4024ba2fbfdca
ms.contentlocale: cs-cz
ms.lasthandoff: 06/30/2017


---
<a id="develop-large-scale-parallel-compute-solutions-with-batch" class="xliff"></a>

# Vývoj rozsáhlých paralelních výpočetních řešení pomocí služby Batch

V tomto přehledu základních součástí služby Azure Batch probereme primární funkce a prostředky služby, které mohou vývojáři služby Batch použít k tvorbě rozsáhlých paralelních výpočetních řešení.

Ať vyvíjíte distribuovanou výpočetní aplikaci nebo službu, která vystavuje přímá volání rozhraní [REST API][batch_rest_api], nebo pracujete s některou ze [sad SDK služby Batch](batch-apis-tools.md#azure-accounts-for-batch-development), používáte mnoho prostředků a funkcí popsaných v tomto článku.

> [!TIP]
> Obecný úvod do služby Batch najdete v části [Základy služby Azure Batch](batch-technical-overview.md).
>
>

<a id="batch-service-workflow" class="xliff"></a>

## Pracovní postup služby Batch
Následující obecný pracovní postup je typický pro téměř všechny aplikace a služby, které používají službu Batch pro zpracování paralelních úloh:

1. Nahrajte **datové soubory**, které chcete zpracovat, do účtu [Azure Storage][azure_storage]. Služba Batch obsahuje integrovanou podporu pro přístup ke službě Azure Blob Storage a vaše úkoly si při spuštění mohou stáhnout tyto soubory do [výpočetních uzlů](#compute-node).
2. Nahrajte **soubory aplikace**, které plánujete svými úkoly spouštět. Tyto soubory mohou být binární soubory nebo skripty a jejich závislosti a jsou spouštěny úkoly v úlohách. Vaše úkoly si mohou stahovat tyto soubory z vašeho účtu úložiště, nebo můžete použít funkci [balíčků aplikací](#application-packages) služby Batch pro správu aplikací a nasazení.
3. Vytvořte [fond](#pool) výpočetních uzlů. Při vytváření fondu zadejte počet výpočetní uzlů pro fond, jejich velikost a operační systém. Při spuštění každého úkolu v úloze je tento úkol přiřazen ke spouštění na jednom z uzlů ve vašem fondu.
4. Vytvořte [úlohu](#job). Úloha spravuje kolekci úkolů. Každou úlohu přidružíte ke konkrétnímu fondu, kde budou spouštěny úkoly této úlohy.
5. Přidejte do úlohy [úkoly](#task). Každý úkol spustí aplikaci nebo skript, které jste nahráli, pro zpracování datových souborů, které si úkol stahuje z vašeho účtu úložiště. Při dokončení může úkol nahrát svůj výstup do Azure Storage.
6. Monitorujte průběh úlohy a načtěte výstup úkolu z Azure Storage.

V následujících částech se dozvíte o těchto a o dalších prostředcích služby Batch, které umožňují váš distribuovaný výpočetní scénář.

> [!NOTE]
> K používání služby Batch potřebujete [účet Batch](#account). Kromě toho téměř všechna řešení používají pro ukládání a načítání souborů účet [Azure Storage][azure_storage]. Služba Batch aktuálně podporuje pouze účty úložiště s typem **Obecné účely**, jak je popsáno v kroku č. 5 článku [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v dokumentu [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md).
>
>

<a id="batch-service-resources" class="xliff"></a>

## Prostředky služby Batch
Některé z následujících prostředků – účty, výpočetní uzly, fondy, úlohy a úkoly – jsou vyžadovány všemi řešeními, která používají službu Batch. Další, jako například plány úloh nebo balíčky aplikací, jsou užitečné, ale volitelné funkce.

* [Účet](#account)
* [Výpočetní uzel](#compute-node)
* [Fond](#pool)
* [Úloha](#job)

  * [Plány úlohy](#scheduled-jobs)
* [Úkol](#task)

  * [Spouštěcí úkol](#start-task)
  * [Úkol správce úloh](#job-manager-task)
  * [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
  * [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
  * [Závislosti úkolů](#task-dependencies)
* [Balíčky aplikací](#application-packages)

<a id="account" class="xliff"></a>

## Účet
Účet Batch je jednoznačně identifikovaná entita v rámci služby Batch. Veškeré zpracování je přidruženo k účtu Batch.

Účet Batch můžete vytvořit prostřednictvím webu [Azure Portal](batch-account-create-portal.md) nebo prostřednictvím programu, například s použitím [knihovny Batch Management .NET](batch-management-dotnet.md). Při vytváření účtu můžete přidružit účet úložiště Azure.

Služba Batch podporuje dvě konfigurace účtu. Při vytváření účtu Batch musíte vybrat odpovídající konfiguraci. Rozdíl mezi těmito dvěma konfiguracemi účtu spočívá v tom, jak se pro jednotlivé účty přidělují [fondy](#pool) služby Batch. Můžete buď přidělit fond výpočetních uzlů v předplatném spravovaném službou Azure Batch, nebo je můžete přidělit ve svém vlastním předplatném. Vlastnost *režimu přidělení fondu* pro účet určuje, která konfigurace se používá. 

Při rozhodování o tom, kterou konfiguraci účtu použít, zvažte, který z nich nejlépe odpovídá vaší situaci:

* **Služba Batch:** Služba Batch je výchozí konfigurace účtu. U účtu vytvořeného s touto konfigurací se fondy Batch přidělují na pozadí v předplatných, které spravuje Azure. U konfigurace účtu Služba Batch je nutné pamatovat si tyto klíčové body:

    - Konfigurace účtu Služba Batch podporuje fondy virtuálních počítačů i cloudových služeb.
    - Konfigurace účtu Služba Batch podporuje přístup k rozhraním API Batch buď s použitím ověřování pomocí sdíleného klíče, nebo s použitím [ověřování Azure Active Directory](batch-aad-auth.md). 
    - V konfiguraci účtu Služba Batch můžete ve fondech použít buď vyhrazené výpočetní uzly, nebo výpočetní uzly s nízkou prioritou.
    - Konfiguraci účtu Služba Batch nepoužívejte, pokud máte v plánu vytvořit fondy virtuálních počítačů Azure z vlastních imagí virtuálních počítačů nebo pokud plánujete využití virtuální sítě. Místo toho vytvořte účet s konfigurací Předplatné uživatele.
    - Fondy virtuálních počítačů zřízené v rámci účtu s konfigurací účtu předplatného Služba Batch musí být vytvořené z imagí [Azure Virtual Machines Marketplace][vm_marketplace].

* **Předplatné uživatele:** U konfigurace účtu Předplatné uživatele se fondy Batch přidělují v předplatném Azure, ve kterém je tento účet vytvořený. U konfigurace účtu Předplatné uživatele je nutné pamatovat si tyto klíčové body:
     
    - Konfigurace účtu Předplatné uživatele podporuje jenom fondy virtuálních počítačů. Nepodporuje fondy cloudových služeb.
    - Pokud chcete vytvořit fondy virtuálních počítačů z vlastních imagí virtuálních počítačů nebo pro fondy virtuálních počítačů použít virtuální síť, musíte použít konfiguraci účtu Předplatné.  
    - K ověření požadavků na službu Batch musíte použít [ověřování Azure Active Directory](batch-aad-auth.md). 
    - Konfigurace účtu Předplatné uživatele vyžaduje, abyste pro účet Batch nastavili Azure Key Vault. 
    - V účtu vytvořeném s konfigurací Předplatné uživatele smíte ve fondech použít jenom vyhrazené výpočetní uzly. Uzly s nízkou prioritou se nepodporují.
    - Fondy virtuálních počítačů zřízené v rámci účtu s konfigurací účtu předplatného Předplatné uživatele musí být vytvořené z imagí [Azure Virtual Machines Marketplace][vm_marketplace] nebo z vlastních imagí, které poskytnete.

> [!IMPORTANT]
> Služba Batch aktuálně podporuje jenom účty úložiště pro obecné účely, jak popisuje krok 5 části [Vytvoření účtu úložiště](../storage/storage-create-storage-account.md#create-a-storage-account) v tématu [Informace o účtech Azure Storage](../storage/storage-create-storage-account.md). Úkoly služby Batch (včetně standardních úkolů, spouštěcích úkolů, úkolů přípravy úloh a úkolů uvolnění úloh) musí určovat soubory prostředků, které jsou umístěné jenom v účtech úložiště pro obecné účely.
>
>


<a id="compute-node" class="xliff"></a>

## Výpočetní uzel
Výpočetní uzel je virtuální počítač Azure nebo virtuální počítač cloudové služby, který je vyhrazený pro zpracování části úloh vaší aplikace. Velikost uzlu určuje počet jader procesoru, kapacita paměti a velikost místního systému souborů, který je přidělen k uzlu. Fondy uzlů Windows nebo Linux můžete vytvořit pomocí imagí Azure Cloud Services nebo Virtual Machines Marketplace. Další informace o těchto možnostech najdete v následující části [Fond](#pool).

Uzly mohou spustit libovolný spustitelný soubor nebo skript, který je podporován prostředím operačního systému uzlu. To zahrnuje soubory s příponami \*.exe, \*cmd, \*.bat a skripty PowerShellu pro Windows a binární soubory, shell a Python skripty pro Linux.

Součástí všech výpočetních uzlů ve službě Batch také jsou:

* Standardní [struktura složek](#files-and-directories) a přidružené [proměnné prostředí](#environment-settings-for-tasks), které jsou úkolu k dispozici.
* Nastavení **brány firewall**, která jsou nakonfigurována pro řízení přístupu.
* [Vzdálený přístup](#connecting-to-compute-nodes) k uzlům Windows (Remote Desktop Protocol (RDP)) i Linux (Secure Shell (SSH)).

<a id="pool" class="xliff"></a>

## Fond
Fond je kolekce uzlů, na kterých běží příslušná aplikace. Fond lze vytvořit ručně nebo automaticky pomocí služby Batch při zadání práce, kterou je zapotřebí vykonat. Můžete vytvořit a spravovat fond, který splňuje požadavky na prostředky příslušné aplikace. Fond může být používán pouze účtem Batch, ve kterém byl vytvořen. Účet Batch může mít více než jeden fond.

Fondy Azure Batch jsou postavené na základní platformě Azure Compute. Zajišťují ve velkém měřítku přidělování, instalaci aplikací, distribuci dat, monitorování stavu a flexibilní úpravy počtu výpočetních uzlů v rámci fondu ([škálování](#scaling-compute-resources)).

Každému uzlu, který je přidán do fondu, je přiřazen jedinečný název a IP adresa. Pokud je uzel odebrán z fondu, veškeré změny provedené v operačním systému nebo souborech jsou ztraceny a jeho název a IP adresa jsou uvolněny pro pozdější použití. Jestliže uzel opustí fond, je jeho životnost ukončena.

Při vytváření fondu můžete zadat následující atributy. Některá nastavení se liší v závislosti na režimu přidělení fondu pro [účet](#account) Batch:

- Operační systém a verze výpočetního uzlu
- Typ výpočetního uzlu a cílový počet uzlů
- Velikost výpočetních uzlů
- Zásady škálování
- Zásady plánování úkolů
- Stav komunikace pro výpočetní uzly
- Spouštěcí úkoly pro výpočetní uzly
- Balíčky aplikací
- Konfigurace sítě

Každé z těchto nastavení je podrobněji popsané v následujících částech.

> [!IMPORTANT]
> Účty služby Batch vytvořené s konfigurací Služba Batch mají výchozí kvótu, která omezuje počet jader účtu Batch. Počet jader odpovídá počtu výpočetních uzlů. Výchozí kvóty a pokyny pro [navýšení kvóty](batch-quota-limit.md#increase-a-quota) najdete v článku [Kvóty a omezení služby Azure Batch](batch-quota-limit.md). Pokud váš fond nedosahuje cílového počtu uzlů, může být důvodem právě kvóta jader.
>
>Účty Batch vytvořené s konfigurací Předplatné uživatele nedodržují kvóty služby Batch. Místo toho sdílejí kvótu jader pro zadané předplatné. Další informace najdete v tématu [Omezení virtuálních počítačů](../azure-subscription-service-limits.md#virtual-machines-limits) v tématu [Limity, kvóty a omezení předplatného a služeb Azure](../azure-subscription-service-limits.md).

<a id="compute-node-operating-system-and-version" class="xliff"></a>

### Operační systém a verze výpočetního uzlu

Když vytváříte fond Batch, můžete zadat konfiguraci virtuálního počítače Azure a typ operačního systému, který chcete spustit na každém výpočetním uzlu tohoto fondu. Ve službě Batch jsou dostupné tyto dva typy konfigurací:

- **Konfigurace virtuálního počítače**, která určuje, že se fond skládá z virtuálních počítačů Azure. Tyto virtuální počítače mohou být vytvořené na základě image Linuxu nebo Windows. 

    Při vytváření fondu založeného na konfiguraci virtuálního počítače je nutné zadat nejen velikost uzlů a zdroj imagí použitých pro jejich vytvoření, ale také **referenční image virtuálního počítače** a **SKU agenta uzlu** služby Batch, které se budou na uzly instalovat. Další informace o zadávání těchto vlastností fondů najdete v článku [Zřízení linuxových výpočetních uzlů ve fondech Azure Batch](batch-linux-nodes.md) 

- **Konfigurace Cloud Services**, která určuje, že se fond skládá z uzlů Azure Cloud Services. Služba Cloud Services poskytuje *jenom* výpočetní uzly Windows.

    Operační systémy, které jsou k dispozici pro fondy konfigurace služby Cloud Services, jsou uvedeny v [matici kompatibility verzí hostovaného operačního systému Azure a sad SDK](../cloud-services/cloud-services-guestos-update-matrix.md). Při vytváření fondu, který obsahuje uzly Cloud Services, je nutné zadat velikost uzlu a příslušnou *řadu OS*. Cloud Services se nasazuje do Azure rychleji než virtuální počítače s Windows. Pokud chcete fondy výpočetních uzlů Windows, můžete zjistit, že služba Cloud Services představuje výhodu z hlediska času nasazení.

    * *Řada operačního systému* také určuje, jaké verze rozhraní .NET jsou s operačním systémem nainstalovány.
    * Podobně jako u rolí pracovního procesu v rámci služby Cloud Services lze zadat *verzi operačního systému* (další informace o rolích pracovního procesu najdete v části [Více informací o cloudových službách](../cloud-services/cloud-services-choose-me.md#tell-me-about-cloud-services) v článku [Přehled služby Cloud Services](../cloud-services/cloud-services-choose-me.md)).
    * Stejně jako u rolí pracovního procesu se doporučuje jako *Verze operačního systému* zadat `*`, aby se uzly automaticky upgradovaly a nebyla potřeba žádná práce k ošetření nově vydaných verzí. Hlavním případem použití s výběrem konkrétní verze operačního systému scénář zajištění kompatibility aplikací, který umožní testovat zpětnou kompatibilitu, než se povolí aktualizace verze. Po ověření funkčnosti je možné aktualizovat *verzi operačního systému* pro fond a nainstalovat image nového operačního systému – jakékoli spuštěné úkoly se přeruší a zařadí do fronty.

Informace o nastavení režimu přidělení fondu při vytváření účtu Batch najdete v části [Účet](#account).

<a id="custom-images-for-virtual-machine-pools" class="xliff"></a>

#### Vlastní image pro fondy virtuálních počítačů

Pokud chcete pro fondy virtuálních počítačů použít vlastní image, vytvořte účet Batch s konfigurací Předplatné uživatele. S touto konfigurací se fondy Batch přidělují do předplatného, ve které je tento účet umístěný. Informace o nastavení režimu přidělení fondu při vytváření účtu Batch najdete v části [Účet](#account).

Pokud chcete vytvořit fond virtuálních počítačů s využitím vlastní image, budete potřebovat jeden nebo několik standardních účtů Azure Storage pro uložení vlastních imagí virtuálního pevného disku. Vlastní image se ukládají jako objekty blob. Pokud chcete při vytváření fondu zadat odkazy na vlastní image, zadejte pro vlastnost [osDisk](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_osdisk) vlastnosti [virtualMachineConfiguration](https://docs.microsoft.com/rest/api/batchservice/add-a-pool-to-an-account#bk_vmconf) URI objektů blob VHD vlastní image.

Ověřte, že vaše účty úložiště splňují následující kritéria:   

- Účty úložiště, které obsahují objekty blob VHD vlastní image musí být ve stejném předplatném jako účet Batch (předplatné uživatele).
- Zadané úložiště účty musí být ve stejné oblasti jako účet Batch.
- V současnosti jsou podporované jenom standardní účty úložiště. Azure Storage úrovně Premium se bude podporovat v budoucnu.
- Můžete zadat jeden účet úložiště s několika vlastními objekty blob VHD, nebo několik účtů úložiště, z nichž každý má jeden objekt blob. K zajištění lepšího výkonu doporučujeme použít několik účtů úložiště.
- Jeden jedinečný objekt blob VHD vlastní image může podporovat až 40 instancí virtuálního počítače s Linuxem nebo 20 instancí virtuálního počítače s Windows. Pokud chcete vytvořit fondy s větším počtem virtuálních počítačů, budete muset vytvořit kopie objektu blob VHD. Například fond s 200 virtuálními počítači s Windows potřebuje 10 jedinečných objektů blob VHD zadaných pro vlastnost **osDisk**.

Když vytvoříte fond, je nutné vybrat odpovídající **nodeAgentSkuId**, v závislosti na operačním systému základní image vašeho disku VHD. Voláním operace [Seznam podporovaných SKU agenta uzlu](https://docs.microsoft.com/rest/api/batchservice/list-supported-node-agent-skus) můžete získat mapování dostupných ID SKU agenta uzlu na odpovídající reference image OS.

Pokud chcete vytvořit fond z vlastní image pomocí webu Azure Portal:

1. Na webu Azure Portal přejděte ke svému účtu Batch.
2. V okně **Nastavení** vyberte položku nabídky **Fondy**.
3. V okně **Fondy** vyberte příkaz **Přidat**. Zobrazí se okno **Přidat fond**.
4. V rozevíracím seznamu **Typ image** vyberte **Vlastní image (Linux/Windows)**. Na portálu se zobrazí výběr **Vlastní image**. Vyberte jeden nebo několik disků VHD ze stejného kontejneru a klikněte na tlačítko **Vybrat**. 
    V budoucnu se přidá podpora pro více virtuálních pevných disků z různých účtů úložiště a různých kontejnerů.
5. Vyberte správné nastavení **Vydavatel/nabídka/SKU** pro vaše vlastní disky VHD, vyberte požadovaný režim **Mezipaměť** a potom pro příslušný fond vyplňte všechny ostatní parametry.
6. Pokud chcete zkontrolovat, jestli je fond založený na vlastní imagi, podívejte se na vlastnost **Operační systém** v souhrnu prostředků okna **Fond**. Hodnota této vlastnosti by měla být **Vlastní image virtuálního počítače**.
7. Všechny vlastní disky VHD přidružené k fondu se zobrazí v okně **Vlastnosti** fondu.

<a id="compute-node-type-and-target-number-of-nodes" class="xliff"></a>

### Typ výpočetního uzlu a cílový počet uzlů

Při vytváření fondu můžete určit, které typy výpočetních uzlů chcete, a zadat jejich cílový počet. Existují dva typy výpočetních uzlů:

- **Vyhrazené výpočetní uzly.** Vyhrazené výpočetní uzly jsou rezervované pro vaše úlohy. Jsou dražší než uzly s nízkou prioritou, ale nabízejí záruku toho, že nikdy nedojde k jejich zrušení.

- **Výpočetní uzly s nízkou prioritou.** Uzly s nízkou prioritou mají tu výhodu, že ke spouštění úloh služby Batch využívají nadbytečnou kapacitu v Azure. Uzly s nízkou prioritou mají nižší hodinové náklady než vyhrazené uzly a umožňují používat úlohy náročné na výpočetní výkon. Další informace najdete v tématu [Použití virtuálních počítačů s nízkou prioritou ve službě Batch](batch-low-pri-vms.md).

    Pokud má Azure nedostatek nadbytečné kapacity, může dojít ke zrušení výpočetních uzlů s nízkou prioritou. Pokud dojde ke zrušení uzlu během spouštění úloh, tyto úlohy se znovu zařadí do fronty, a jakmile bude uzel zase dostupný, znovu se spustí. Uzly s nízkou prioritou jsou dobrou volbou pro úlohy s flexibilním časem dokončení a pro úkony distribuované mezi velký počet uzlů. Než se rozhodnete použít pro svůj scénář uzly s nízkou prioritou, ujistěte se, že ztráta práce z důvodu přerušení bude minimální a bude ji snadné vytvořit znovu.

    Výpočetní uzly s nízkou prioritou jsou dostupné jenom u účtů služby Batch vytvořených s režimem přiřazování fondů nastaveným na hodnotu **Služba Batch**.

Ve stejném fondu můžete mít výpočetní uzly s nízkou prioritou i vyhrazené uzly. Každý typ uzlu (uzly s nízkou prioritu a vyhrazené uzly) má vlastní cílové nastavení, ve kterém můžete určit požadovaný počet uzlů. 
    
Počet výpočetních uzlů se označuje jako *cílový* z toho důvodu, že v některých situacích nemusí váš fond dosáhnout požadovaného počtu uzlů. Fond třeba nemusí dosáhnout cílového nastavení v případě, že dřív dosáhne [základní kvóty](batch-quota-limit.md) daného účtu služby Batch. Fond taky nemusí dosáhnout cílového nastavení tehdy, když ve fondu použijete vzorec pro automatické škálování, který omezí maximální počet uzlů.

Informace o cenách výpočetních uzlů s nízkou prioritou a vyhrazených uzlů najdete v tématu [Ceny služby Batch](https://azure.microsoft.com/pricing/details/batch/).

<a id="size-of-the-compute-nodes" class="xliff"></a>

### Velikost výpočetních uzlů

Velikosti výpočetních uzlů z **konfigurace služby Cloud Services** jsou uvedeny v seznamu [Velikosti pro Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Služba Batch podporuje všechny velikosti pro Cloud Services kromě `ExtraSmall`, `STANDARD_A1_V2` a `STANDARD_A2_V2`.

Velikosti výpočetních uzlů z **konfigurace virtuálního počítače** jsou uvedeny v seznamech [Velikosti pro virtuální počítače v Azure](../virtual-machines/linux/sizes.md) (Linux) a [Velikosti pro virtuální počítače v Azure](../virtual-machines/windows/sizes.md) (Windows). Služba Batch podporuje všechny velikosti VM Azure kromě `STANDARD_A0` a těch, které mají úložiště Premium (série `STANDARD_GS`, `STANDARD_DS` a `STANDARD_DSV2`).

Při výběru velikosti výpočetního uzlu zvažte charakteristiku a požadavky aplikací, které budete v uzlech spouštět. To, jestli je aplikace vícevláknová, kolik paměti používá, a další aspekty vám mohou pomoci při určování nejvhodnější a cenově nejefektivnější velikosti uzlu. Velikost uzlu se obvykle volí s předpokladem, že na uzlu poběží současně pouze jeden úkol. Ve výpočetních uzlech je však možné mít během zpracování úkolu [paralelně spuštěno](batch-parallel-node-tasks.md) několik úkolů (a tudíž i několik instancí aplikace). V takovém případě je běžné zvolit větší velikost uzlu, aby dostačovala i v případě zvýšené poptávky na paralelní zpracování úkolů. Další informace najdete v části [Zásady plánování úkolů](#task-scheduling-policy).

Všechny uzly ve fondu mají stejnou velikost. Pokud máte v úmyslu spouštět aplikace s různými požadavky na systém nebo úrovně zatížení, doporučujeme vám používat oddělené fondy.

<a id="scaling-policy" class="xliff"></a>

### Zásady škálování

Pro dynamické úlohy můžete pro fond napsat a použít [vzorec automatického škálování](#scaling-compute-resources). Služba Batch pak vzorec pravidelně vyhodnocuje a upravuje počet uzlů v rámci fondu na základě různých vámi zadaných parametrů fondu, úloh a úkolů.

<a id="task-scheduling-policy" class="xliff"></a>

### Zásady plánování úkolů

Možnost konfigurace [maximálního počtu úkolů na uzel](batch-parallel-node-tasks.md) určuje maximální počet úkolů, které lze spustit souběžně na jednotlivých výpočetních uzlech v rámci fondu.

Výchozí konfigurace určuje, že na uzlu běží současně jen jeden úkol, ale existují scénáře, kdy je výhodné mít na uzlu spuštěno ve stejnou dobu dva a více úkolů. Jaké výhody vám může přinést více úkolů na jednom uzlu je popsáno v části [příklad scénáře](batch-parallel-node-tasks.md#example-scenario) v článku [souběžné úkoly na uzlu](batch-parallel-node-tasks.md).

Dále je možné zadat *typ vyplnění*, který určuje, jestli služba Batch rozdělí úkoly rovnoměrně mezi všechny uzly ve fondu, nebo zaneprázdní každý uzel maximálním počtem úkolů před přiřazením úkolů jinému uzlu.

<a id="communication-status-for-compute-nodes" class="xliff"></a>

### Stav komunikace pro výpočetní uzly

Ve většině scénářů pracují úkoly nezávisle a nepotřebují mezi sebou komunikovat. Existují však aplikace, ve kterých úkoly musí komunikovat, například [scénáře MPI](batch-mpi.md).

Fond můžete nakonfigurovat tak, aby umožňoval **komunikaci mezi uzly**, takže uzly v rámci fondu spolu budou moci za běhu komunikovat. Pokud je komunikace mezi uzly povolena, uzly ve fondech z konfigurace služby Cloud Services mohou vzájemně komunikovat na portech vyšších než 1100. Fondy z konfigurace virtuálního počítače neomezují provoz na žádném portu.

Poznámka: povolení komunikace mezi uzly má vliv i na umístění uzlů v clusterech a z důvodu omezení platných pro nasazení může být omezen maximální počet uzlů ve fondu. Pokud aplikace nevyžaduje komunikaci mezi uzly, služba Batch může fondu přidělit potenciálně velký počet uzlů z mnoha různých clusterů a datových center, aby umožnila zvýšený výkon paralelního zpracování.

<a id="start-tasks-for-compute-nodes" class="xliff"></a>

### Spouštěcí úkoly pro výpočetní uzly

Volitelný *spouštěcí úkol* se spustí na každém uzlu, když se takový uzel připojí k fondu, a také pokaždé, když se uzel restartuje nebo obnoví z image. Spouštěcí úkol je obzvlášť užitečný pro přípravu výpočetních uzlů k provádění úkolů, jako je instalace aplikací, které budou vaše úkoly na výpočetních uzlech spouštět.

<a id="application-packages" class="xliff"></a>

### Balíčky aplikací

Můžete určit [balíčky aplikací](#application-packages) pro nasazení do výpočetních uzlů ve fondu. Balíčky aplikací poskytují možnost zjednodušeného nasazení a správy verzí aplikace, která provádí vaše úlohy. Balíčky aplikací, které pro fond určíte, se nainstalují na každý uzel, který se do daného fondu připojí, a také pokaždé, když se uzel restartuje nebo obnoví z image. Balíčky aplikací nejsou v současné době na linuxových výpočetních uzlech podporované.

<a id="network-configuration" class="xliff"></a>

### Konfigurace sítě

Můžete zadat podsíť [virtuální sítě (VNet)](../virtual-network/virtual-networks-overview.md) Azure, ve které se mají výpočetní uzly fondu vytvářet. Další informace najdete v části [Konfigurace sítě fondu](#pool-network-configuration).


<a id="job" class="xliff"></a>

## Úloha
Úloha je kolekce úkolů. Řídí, jak se provádí výpočet pomocí jejích úkolů na výpočetních uzlech ve fondu.

* Úloha určuje **fond**, ve kterém běží práce. Můžete vytvořit nový fond pro každou úlohu nebo použít jeden fond pro mnoho úloh. Můžete vytvořit fond pro každou úlohu, která je přidružená k plánu úlohy, nebo pro všechny úlohy, které jsou spojeny s plánem úlohy.
* Volitelně můžete zadat **prioritu úlohy**. Když je úloha odeslána s vyšší prioritou než aktuálně probíhající úlohy, jsou úkoly úlohy s vyšší prioritou vloženy do fronty před úkoly úloh s nižší prioritou. Úkoly v rámci úloh s nižší prioritou, které jsou již spuštěny, se neruší.
* Můžete použít **omezení** úlohy k zadání určitých omezení pro úlohy.

    Můžete nastavit **maximální uplynulý čas**, takže pokud některá úloha běží po dobu delší než určený maximální uplynulý čas, úloha a všechny její úkoly budou ukončeny.

    Služba Batch umí zjistit a poté opakovat neúspěšné úkoly. Jako omezení lze zadat **maximální počet opakování úkolů**, včetně toho, jestli je úkol opakován *vždy* nebo *nikdy*. Opakování úkolu znamená, že je úkol znovu zařazen do fronty k opětovnému spuštění.
* Klientská aplikace si může přidat úkoly do úlohy, nebo můžete zadat [úkol správce úloh](#job-manager-task). Úkol správce úloh obsahuje informace potřebné k vytvoření požadovaných úkolů pro úlohu, přičemž úkol správce úloh běží na jednom výpočetním uzlu v rámci fondu. Úkol správce úloh je službou Batch spravován specificky – je zařazen do fronty okamžitě po vytvoření úlohy a restartován, pokud selže. Úkol správce úloh je *vyžadován* pro úlohy vytvořené [plánem úloh](#scheduled-jobs), protože to je jediný způsob, jak definovat úkoly před vytvořením instance úlohy.
* Ve výchozím nastavení zůstanou úlohy v aktivním stavu po dokončení všech úkolů v rámci úlohy. Toto chování můžete změnit tak, aby se úlohy automaticky ukončily po dokončení všech úkolů v úloze. Pokud chcete automaticky ukončovat úlohy poté, co všechny jejich úkoly budou v dokončeném stavu, nastavte vlastnost úlohy **onAllTasksComplete** ([OnAllTasksComplete][net_onalltaskscomplete] v Batch .NET) na *terminatejob*.

    Všimněte si, že služba Batch považuje úlohu *bez úloh* také za úlohu, jejíž úkoly jsou dokončené. Tato možnost se proto nejčastěji používá pro [úkoly správce úloh](#job-manager-task). Pokud chcete použít automatické ukončování úloh bez správce úloh, měli byste na začátku nastavit pro novou úlohu vlastnost **onAllTasksComplete** na *noaction* a na hodnotu *terminatejob* ji nastavit až poté, až dokončíte přidávání úkolů do úlohy.

<a id="job-priority" class="xliff"></a>

### Priorita úloh
Úlohám, které vytvoříte ve službě Batch, lze přiřadit prioritu. Hodnotu priority úlohy používá služba Batch k určení pořadí úlohy v rámci účtu (nezaměňovat s [naplánovanou úlohou](#scheduled-jobs)). Hodnoty priority se pohybují v rozsahu -1000 až 1000, kdy -1000 znamená nejnižší prioritu a 1000 nejvyšší. Prioritu úlohy je možné aktualizovat zavoláním operace [Aktualizovat vlastnosti úlohy][rest_update_job] (Batch REST) nebo úpravou vlastnosti [CloudJob.Priority][net_cloudjob_priority] (Batch .NET).

V rámci stejného účtu mají úlohy s vyšší prioritou přednost při plánování před úlohami s nižší prioritou. Úloha s vyšší hodnotou priority v jednom účtu nemá přednost při plánování před jinou úlohou s nižší hodnotou priority v jiném účtu.

Plánování úloh mezi fondy je nezávislé. Mezi různými fondy není zaručeno, že úloha s vyšší prioritou je naplánována jako první, pokud její přidružený fond nemá dostatek nečinných uzlů. Ve stejném fondu mají úlohy se stejnou úrovní priority stejnou šanci být naplánované.

<a id="scheduled-jobs" class="xliff"></a>

### Naplánované úlohy
[Plány úloh][rest_job_schedules] umožňují vytvořit opakované úlohy v rámci služby Batch. Plán úloh určuje, kdy spustit úlohy a obsahuje specifikace pro úlohy, které mají být spuštěny. Můžete určit dobu trvání plánu – na jak dlouho a kdy bude plán platný – a jak často se během tohoto naplánovaného období mají vytvářet úlohy.

<a id="task" class="xliff"></a>

## Úkol
Úkol je jednotka výpočtu, která je přidružena k úloze. Běží na uzlu. Úkoly jsou přiřazeny k uzlu pro provádění nebo jsou zařazeny do fronty, dokud se uzel neuvolní. Jednoduše řečeno, úkol spustí na výpočetním uzlu jeden nebo více programů nebo skriptů k provedení potřebné práce.

Při vytvoření úkolu můžete zadat:

* **Příkazový řádek** pro úkol. Toto je příkazový řádek, kterým se na výpočetním uzlu spouští vaše aplikace nebo skript.

    Je důležité si uvědomit, že příkazový řádek ve skutečnosti není spuštěn v shellu. Proto nemůže nativně využívat funkce shellu, jako například rozšíření [proměnné prostředí](#environment-settings-for-tasks) (sem patří i `PATH`). Pokud chcete takové funkce využívat, musíte vyvolat shell v příkazovém řádku, například spuštěním `cmd.exe` na uzlech Windows nebo `/bin/sh` na Linuxu:

    `cmd /c MyTaskApplication.exe %MY_ENV_VAR%`

    `/bin/sh -c MyTaskApplication $MY_ENV_VAR`

    Pokud vaše úkoly potřebují spustit aplikaci nebo skript, které nejsou v `PATH` uzlu nebo v referenčních proměnných prostředí, vyvolejte shell explicitně v příkazovém řádku úkolu.
* **Soubory prostředků** obsahující data, která mají být zpracována. Tyto soubory se před provedením příkazového řádku úkolu automaticky zkopírují do uzlu ze služby Blob Storage v účtu Azure Storage pro obecné účely. Další informace najdete v částech [Spouštěcí úkol](#start-task) a [Soubory a adresáře](#files-and-directories).
* **Proměnné prostředí**, které jsou požadovány příslušnou aplikací. Další informace najdete v části [Nastavení prostředí pro úkoly](#environment-settings-for-tasks).
* **Omezení**, za kterých by měl být proveden úkol. Mezi omezení patří například: maximální doba, po kterou smí úkol běžet, maximální počet pokusů o opakování neúspěšného úkolu a maximální doba, po kterou jsou zachovány soubory v pracovním adresáři úkolu.
* **Balíčky aplikací** pro nasazení do výpočetního uzlu, na kterém je naplánováno spuštění úkolu. [Balíčky aplikací](#application-packages) poskytují možnost zjednodušeného nasazení a správy verzí aplikací, které provádějí vaše úkoly. Balíčky aplikací na úrovni úkolů jsou zvláště užitečné v prostředích sdíleného fondu, kde se různé úlohy spouštějí v jednom fondu a kde se fond po dokončení úlohy neodstraňuje. Pokud má vaše úloha méně úkolů, než je uzlů ve fondu, balíčky aplikací úkolů můžou omezit přenosy dat, protože se aplikace může nasadit jen na uzly, které úkoly budou skutečně provádět.

Vedle úkolů, které definujete pro provádění výpočtů na uzlu, poskytuje služba Batch také následující zvláštní úkoly:

* [Spouštěcí úkol](#start-task)
* [Úkol správce úloh](#job-manager-task)
* [Úkoly přípravy a uvolnění úloh](#job-preparation-and-release-tasks)
* [Úkoly s více instancemi (MPI)](#multi-instance-tasks)
* [Závislosti úkolů](#task-dependencies)

<a id="start-task" class="xliff"></a>

### Spouštěcí úkol
Přidružením **spouštěcího úkolu** k fondu můžete připravit provozní prostředí jeho uzlů. Můžete například provádět akce, jako je instalace aplikací, které vaše úkoly spouštějí, nebo spouštění procesů na pozadí. Spouštěcí úkol se spustí při každém spuštění uzlu, a běží tak dlouho, dokud zůstává ve fondu, včetně okamžiků prvního přidání uzlu do fondu a jeho restartování či obnovení z image.

Hlavní výhodou spouštěcího úkolu je, že obsahuje všechny informace potřebné ke konfiguraci výpočetního uzlu a instalaci aplikací nezbytných k provádění úkolu. Proto je navýšení počtu uzlů ve fondu pouze otázkou zadání nového cílového počtu uzlů. Spouštěcí úkol poskytuje službě Batch informace potřebné ke konfiguraci nových uzlů a k jejich přípravě pro příjem úkolů.

Stejně jako u každého úkolu služby Azure Batch můžete také zadat seznam **souborů prostředků** ve službě [Azure Storage][azure_storage] (vedle **příkazového řádku**, který se má provést). Služba Batch nejprve zkopíruje soubory prostředků ze služby Azure Storage do uzlu a pak spustí příkazový řádek. U spouštěcího úkolu fondu obsahuje seznam souborů obvykle aplikaci úkolu a její závislosti.

Spouštěcí úkol ale může obsahovat také referenční data, která budou k dispozici k použití ve všech úkolech spouštěných na výpočetním uzlu. Příkazový řádek spouštěcího úkolu může například provést operaci `robocopy` kopírování souborů aplikace (které byly zadány jako soubory prostředků a staženy do uzlu) z [pracovního adresáře](#files-and-directories) spouštěcího úkolu do [sdílené složky](#files-and-directories) a potom spustit soubor MSI nebo `setup.exe`.

Obvykle je žádoucí, aby služba Batch čekala na dokončení spouštěcího úkolu předtím, než bude uzel považovat za připravený k přiřazení úkolů, ale tuto možnost lze nakonfigurovat.

Pokud na výpočetním uzlu selže spouštěcí úkol, je stav tohoto uzlu aktualizován, aby odrážel selhání, a uzlu se nepřiřazují žádné úlohy. Spouštěcí úkol může selhat, jestliže se vyskytl problém s kopírováním jeho souborů prostředků z úložiště nebo pokud proces spuštěný pomocí jeho příkazového řádku vrátí nenulový ukončovací kód.

Pokud přidáváte nebo aktualizujete spouštěcí úkol pro už existující fond, musíte restartovat jeho výpočetní uzly, aby se na nich spouštěcí úkol provedl.

>[!NOTE]
> Celková velikost spouštěcího úkolu nesmí přesahovat 32768 znaků, včetně souborů prostředků a proměnných prostředí. K zajištění, že spouštěcí úkol splňuje tento požadavek, můžete použít jednu ze dvou následujících metod:
>
> 1. K distribuci aplikací nebo dat napříč všemi uzly ve fondu Batch můžete použít balíčky aplikací. Další informace o balíčcích aplikací najdete v tématu [Nasazení aplikace pomocí balíčků aplikací Azure Batch](batch-application-packages.md).
> 2. Můžete ručně vytvořit komprimovaný archiv obsahující vaše soubory aplikací. Tento komprimovaný archiv potom uložte do služby Azure Storage jako objekt blob. Zadejte tento komprimovaný archiv jako soubor prostředků pro spouštěcí úkol. Před spuštěním příkazového řádku pro spouštěcí úkol rozbalte archiv z příkazového řádku. 
>
>    K rozbalení archivu můžete použít archivační nástroj podle vašeho výběru. Nástroj, který použijete k rozbalení archivu, budete muset zahrnout jako soubor prostředků pro tento spouštěcí úkol.
>
>

<a id="job-manager-task" class="xliff"></a>

### Úkol správce úloh
**Úkol správce úloh** se obvykle používá k řízení nebo monitorování provádění úlohy – například k vytvoření a odeslání úkolů pro úlohu, určení dalších úkolů ke spuštění a zjištění, jestli je práce dokončená. Úkol správce úloh však není omezen na tyto aktivity. Jedná se o plnohodnotný úkol, který může provádět všechny akce potřebné pro úlohu. Úkol správce úloh může například stáhnout soubor zadaný jako parametr, analyzovat obsah tohoto souboru a odeslat další úkoly na základě těchto obsahů.

Úkol správce úloh je spuštěn před všemi ostatními úkoly. Má následující funkce:

* Je službou Batch automaticky odeslán jako úkol při vytvoření úlohy.
* Je naplánován ke spuštění před dalšími úkoly v rámci úlohy.
* Jeho přidružený uzel je poslední, který se odebere z fondu, když je fond zmenšován.
* Jeho ukončení může být vázáno na ukončení všech úkolů v úloze.
* Úkolu správce úloh je přiřazena nejvyšší priorita, když je nutné jej restartovat. Pokud není k dispozici nečinný uzel, může služba Batch ukončit jeden z ostatních spuštěných úkolů ve fondu, aby uvolnila prostor pro spuštění úkolu správce úloh.
* Úkol správce úloh v jedné úloze nemá přednost před úkoly jiných úloh. Mezi úlohami jsou dodržovány pouze priority s úrovní úlohy.

<a id="job-preparation-and-release-tasks" class="xliff"></a>

### Úkoly přípravy a uvolnění úloh
Služba Batch zajišťuje úkoly přípravy úloh pro nastavení před provedením úlohy. Úkoly uvolnění úloh jsou určeny pro údržbu nebo čištění po provedení úlohy.

* **Úkol přípravy úlohy**: Úkol přípravy úlohy je spuštěn před spuštěním jiných úkolů úlohy na všech výpočetních uzlech, na kterých je naplánované spuštění úkolů. Úkol přípravy úlohy lze například použít ke zkopírování dat, která jsou sdílena všemi úkoly, ale jsou jedinečná pro úlohu.
* **Úkol uvolnění úlohy**: Po dokončení úlohy se na každém uzlu ve fondu, na kterém se spustil alespoň jeden úkol, spustí úkol uvolnění úlohy. Pomocí úkolu uvolnění úlohy můžete například odstranit data zkopírovaná pomocí úkolu přípravy úlohy nebo zkomprimovat a nahrát data diagnostického protokolu.

Úkoly přípravy i uvolnění úloh umožňují zadat příkazový řádek, který se provede, když je úkol vyvolán. Nabízejí funkce, jako například stahování souborů, provádění se zvýšenými oprávněními, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru.

Další informace ohledně úkolů přípravy a uvolnění úloh najdete v části [Spouštění úkolů přípravy a dokončení úlohy na výpočetních uzlech Azure Batch](batch-job-prep-release.md).

<a id="multi-instance-task" class="xliff"></a>

### Úkoly s více instancemi
[Úkol s více instancemi](batch-mpi.md) je úkol, který je nakonfigurován, aby byl současně spuštěn na více než jednom výpočetním uzlu. Pomocí úkolů s více instancemi můžete povolit výpočetní scénáře s vysokým výkonem, které vyžadují skupinu výpočetních uzlů přidělených společně na zpracování jediné úlohy (například rozhraní MPI (Message Passing Interface)).

Podrobné informace o spouštění úloh MPI ve službě Batch pomocí knihovny Batch .NET najdete v článku [Použití úkolů s více instancemi ke spouštění aplikací rozhraní MPI (Message Passing Interface) v Azure Batch](batch-mpi.md).

<a id="task-dependencies" class="xliff"></a>

### Závislosti úkolů
[Závislosti úkolů](batch-task-dependencies.md), jak již název napovídá, umožňují určit, jestli úkol závisí před svým spuštěním na dokončení jiných úkolů. Tato funkce poskytuje podporu pro situace, ve kterých „podřízený“ úkol spotřebovává výstup „nadřazeného“ úkolu, nebo kde nadřazený úkol provádí inicializaci, která je požadovaná podřízeným úkolem. Pokud chcete tuto funkci používat, je nutné nejprve v úloze služby Batch povolit závislosti úkolů. Pak pro každý úkol, který závisí na jiném (nebo mnoha jiných), zadáte úkoly, na kterých tento úkol závisí.

Pomocí závislosti úkolů lze nakonfigurovat například následující scénáře:

* *Úkol B* závisí na *úkolu A* (*úkol B* nezahájí provádění, dokud se nedokončí *úkol A*).
* *Úkol C* závisí na *úkolu A* i *úkolu B*.
* *Úkol D* závisí na celé řadě úkolů, například na úkolu *1* až *10*, než se provede

Podívejte se na článek [Závislosti úkolů v Azure Batch](batch-task-dependencies.md) a ukázku kódu [TaskDependencies][github_sample_taskdeps] v úložišti GitHubu [azure-batch-samples][github_samples], kde najdete podrobnější informace o této funkci.

<a id="environment-settings-for-tasks" class="xliff"></a>

## Nastavení prostředí pro úlohy
Každý úkol prováděný službou Batch má přístup k proměnným prostředí, které nastaví na výpočetních uzlech. To zahrnuje proměnné prostředí, které jsou definovány službou Batch ([definované službou][msdn_env_vars]) a vlastní proměnné prostředí, které můžete definovat pro své úkoly. Aplikace a skripty zpracovávané vašimi úkoly mají během zpracování přístup k těmto proměnným prostředí.

Vlastní proměnné prostředí můžete nastavit na úrovni úkolů či úloh tím, že vyplníte vlastnost *nastavení prostředí* pro tyto entity. Viz například operace [Přidání úkolu do úlohy][rest_add_task] (Batch REST API) nebo vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] a [CloudJob.CommonEnvironmentSettings][net_job_env] v Batch .NET.

Vaše klientská aplikace nebo služba může získat službou i uživatelem definované proměnné prostředí úkolu pomocí operace [Získat informace o úkolu][rest_get_task_info] (Batch REST) nebo přístupem k vlastnosti [CloudTask.EnvironmentSettings][net_cloudtask_env] (Batch .NET). Procesy prováděné na výpočetním uzlu mohou také přistupovat k těmto i ostatním proměnným prostředí na uzlu například pomocí známé syntaxe `%VARIABLE_NAME%` (Windows) nebo `$VARIABLE_NAME` (Linux).

Úplný seznam všech proměnných prostředí definovaných službou najdete v článku [Proměnné prostředí výpočetního uzlu][msdn_env_vars].

<a id="files-and-directories" class="xliff"></a>

## Soubory a adresáře
Každý úkol má *pracovní adresář*, pod kterým vytváří žádný nebo více souborů a adresářů. Tento pracovní adresář lze použít pro ukládání programu, který je spuštěn úkolem, dat, která zpracovává, a výstupu zpracování, které provádí. Všechny soubory a adresáře úkolu jsou vlastněné uživatelem úkolu.

Služba Batch zveřejňuje část systému souborů na uzlu jako *kořenový adresář*. Úkoly mohou získat přístup do kořenového adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_ROOT_DIR`. Další informace o používání proměnných prostředí najdete v tématu [Nastavení prostředí pro úkoly](#environment-settings-for-tasks).

Kořenový adresář obsahuje následující adresářovou strukturu:

![Adresářová struktura výpočetního uzlu][1]

* **shared**: Tento adresář poskytuje přístup pro čtení a zápis pro *všechny* úkoly, které jsou spouštěny na uzlu. Každý úkol spuštěný na uzlu může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_SHARED_DIR`.
* **startup**: Tento adresář je využíván spouštěcím úkolem jako jeho pracovní adresář. Jsou sem uloženy všechny soubory, které byly staženy do uzlu spouštěcím úkolem. Spouštěcí úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Úkoly mohou získat přístup do tohoto adresáře odkazem na proměnnou prostředí `AZ_BATCH_NODE_STARTUP_DIR`.
* **Úkoly**: Pro každý úkol, který běží na uzlu, je vytvořen adresář. Přistupuje se k němu odkazem na proměnnou prostředí `AZ_BATCH_TASK_DIR`.

    V rámci každého adresáře úkolu vytvoří služba Batch pracovní adresář (`wd`), jehož jedinečná cesta je určena proměnnou prostředí `AZ_BATCH_TASK_WORKING_DIR`. Tento adresář poskytuje přístup pro čtení a zápis pro úkol. Úkol může vytvořit, číst, aktualizovat a odstranit soubory v tomto adresáři. Tento adresář je zachován podle pravidel omezení *RetentionTime*, které je zadáno pro úkol.

    `stdout.txt` a `stderr.txt`: Tyto soubory jsou zapsány do složky úkolu během provádění úkolu.

> [!IMPORTANT]
> Pokud je uzel odebrán z fondu, jsou odebrány *všechny* soubory, které jsou uloženy na uzlu.
>
>

<a id="application-packages" class="xliff"></a>

## Balíčky aplikací
Funkce [balíčků aplikací](batch-application-packages.md) poskytuje snadnou správu a nasazení aplikací na výpočetní uzly ve fondech. Můžete nahrávat a spravovat více verzí aplikací spouštěných prostřednictvím úkolů, včetně jejich binárních a podpůrných souborů. Poté můžete automaticky nasadit jednu nebo více těchto aplikací na výpočetní uzly ve fondu.

Balíčky aplikací můžete specifikovat na úrovni fondu nebo úkolu. Když určíte balíčky aplikací na úrovni fondu, aplikace se nasadí na každý uzel ve fondu. Když určíte balíčky aplikací na úrovni úkolu, aplikace se nasadí jen na ty uzly, na kterých je naplánované spuštění alespoň jednoho úkolu úlohy, a to těsně před spuštěním úkolu na příkazovém řádku.

Služba Batch zpracovává podrobnosti o práci s úložištěm Azure Storage a uchovává a nasazuje balíčky aplikací na výpočetní uzly, čímž lze zjednodušit režijní náklady na kód a správu.

Pokud chcete získat další informace o funkci balíčku aplikace, projděte si dokument [Nasazení aplikací pomocí balíčků aplikací Azure Batch](batch-application-packages.md).

> [!NOTE]
> Pokud přidáváte balíčky aplikací fondu pro *existující* fond, je nutné restartovat jeho výpočetní uzly, aby se na nich balíčky aplikací mohly nasadit.
>
>

<a id="pool-and-compute-node-lifetime" class="xliff"></a>

## Životnost fondu a výpočetního uzlu
Při navrhování řešení Azure Batch musíte přijmout rozhodnutí o návrhu ohledně toho, kdy a jak jsou fondy vytvářeny a jak dlouho jsou výpočetní uzly v rámci těchto fondů udržovány dostupné.

Na jednom konci spektra můžete vytvořit fond pro každou úlohu, kterou odešlete, a odstranit jej, jakmile se dokončí provádění příslušných úkolů. Tím se maximalizuje využití, neboť uzly se přidělují pouze tehdy, když je to nutné, a vypínají se, jakmile jsou nečinné. To znamená, že úloha musí čekat, až budou uzly přiděleny. Je důležité si uvědomit, že zpracování úkolů se naplánuje, jakmile jsou uzly jednotlivě k dispozici, jsou přiděleny a spouštěcí úkol je dokončen. Služba Batch před přiřazením úkolů k uzlům *nečeká*, až budou všechny uzly v rámci fondu dostupné. Tím je zajištěno maximální využití všech dostupných uzlů.

Na druhém konci spektra, pokud je nejvyšší prioritou okamžité spuštění úloh, můžete fond vytvořit s předstihem, aby jeho uzly byly k dispozici před odesláním úloh. V tomto scénáři se mohou úkoly spustit okamžitě, ale uzly mohou „nečinně sedět“ a čekat na jejich přiřazení.

V případě zpracovávání proměnlivého, ale stálého zatížení se obvykle používá kombinovaný přístup. Můžete mít fond, do nějž je odesíláno více úloh a který může podle zatížení úlohami měnit počet uzlů nahoru nebo dolů (viz [Škálování výpočetních prostředků](#scaling-compute-resources) v následující části). Toto přizpůsobování kapacity můžete provádět reaktivně, na základě aktuálního zatížení, nebo proaktivně, pokud lze zatížení předpovídat.

<a id="pool-network-configuration" class="xliff"></a>

## Konfigurace sítě fondu

Při vytváření fondu výpočetních uzlů v Azure Batch můžete zadat ID podsítě [virtuální sítě](../virtual-network/virtual-networks-overview.md) Azure, ve které se mají výpočetní uzly fondu vytvářet.

* Virtuální síť musí být:

   * Ve stejné **oblasti** Azure jako účet Azure Batch.
   * Ve stejném **předplatném** jako účet Azure Batch.

* Typ podporované virtuální sítě závisí na přidělování fondů pro účet Batch:
    - Pokud byl účet Batch vytvořený s vlastností **poolAllocationMode** nastavenou na hodnotu BatchService, zadanou virtuální sítí musí být klasická virtuální síť.
    - Pokud byl účet Batch vytvořený s vlastností **poolAllocationMode** nastavenou na hodnotu UserSubscription, zadanou virtuální sítí může být klasická virtuální síť nebo virtuální síť Azure Resource Manageru. Aby bylo možné používat virtuální síť, musí být fondy vytvořené s konfigurací virtuálního počítače. Fondy vytvořené s konfigurací cloudové služby se nepodporují.

* Pokud byl účet Batch vytvořený s vlastností **poolAllocationMode** nastavenou na hodnotu BatchService, je pro přístup k virtuální síti nutné zadat oprávnění pro instanční objekt Batch. Instanční objekt Batch, který má název MicrosoftAzureBatch nebo MicrosoftAzureBatch, musí mít pro zadanou virtuální síť roli [Řízení přístupu na základě role Přispěvatel klasických virtuálních počítačů](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/#classic-virtual-machine-contributor). Pokud příslušná role RBAC není dostupná, služba Batch vrátí 400 (Chybný požadavek).

* Zadaná podsíť musí mít dostatek volných **IP adres** k obsluze celkového počtu cílových uzlů, to znamená součtu hodnot vlastností fondu `targetDedicatedNodes` a `targetLowPriorityNodes`. Pokud podsíť nemá dostatek volných IP adres, služba Batch částečně přidělí výpočetní uzly ve fondu a vrátí chybu změny velikosti.

* Určená podsíť musí umožňovat komunikaci ze služby Batch, aby mohla plánovat úlohy ve výpočetních uzlech. Pokud je komunikace s výpočetními uzly zakázána **skupinou zabezpečení sítě (NSG)** přidruženou k virtuální síti, nastaví služba Batch stav výpočetních uzlů na **nepoužitelné**.

* Pokud zadaná virtuální síť nemá žádné přidružené skupiny zabezpečení sítě (NSG), musí být pro několik vyhrazených systémových portů povolena příchozí komunikace. Pro fondy vytvořené s konfigurací virtuálního počítače povolte porty 29876 a 29877 také port 22 pro Linux a port 3389 pro Windows. Pro fondy vytvořené s konfigurací cloudové služby povolte porty 10100, 20100 a 30100. Dál povolte příchozí připojení služby Azure Storage na portu 443.

    Následující tabulka popisuje příchozí porty, které je nutné povolit pro fondy vytvořené s konfigurací virtuálního počítače:

    |    Cílové porty    |    Zdrojová IP adresa      |    Přidala služba Batch skupiny NSG?    |    Vyžaduje se pro využitelnost virtuálních počítačů?    |    Akce ze strany uživatele   |
    |---------------------------|---------------------------|----------------------------|-------------------------------------|-----------------------|
    |    <ul><li>Pro fondy vytvořené s konfigurací virtuálního počítače: 29876, 29877</li><li>Pro fondy vytvořené s konfigurací cloudové služby: 10100, 20100, 30100</li></ul>         |    Jenom IP adresy role služby Batch |    Ano. Batch přidá skupiny NSG na úrovni síťových rozhraní (NIC) připojených k virtuálním počítačům. Tyto skupiny zabezpečení sítě povolují přenos jenom z IP adres role služby Batch. I když tyto porty otevřete pro celý web, přenos se bude na síťové kartě blokovat. |    Ano  |  Není nutné zadávat NSG, protože Batch povoluje jenom IP adresy Batch. <br /><br /> Pokud ale NSG zadáte, nezapomeňte prosím zajistit, aby tyto porty byly otevřené pro příchozí přenos. <br /><br /> Pokud jako zdrojovou IP adresu pro NSG zadáte *, Batch přesto přidá skupiny NSG na úrovni síťové karty připojené k virtuálním počítačům. |
    |    3389, 22               |    Počítače uživatelů používané pro účely ladění, aby byl možný vzdálený přístup k virtuálnímu počítači.    |    Ne                                    |    Ne                     |    Pokud chcete povolit vzdálený přístup (RDP/SSH) k virtuálnímu počítači, přidejte NSG.   |                 

    Následující tabulka popisuje odchozí port, který je nutné povolit tak, aby byl možný přístup ke službě Azure Storage:

    |    Odchozí porty    |    Cíl    |    Přidala služba Batch skupiny NSG?    |    Vyžaduje se pro využitelnost virtuálních počítačů?    |    Akce ze strany uživatele    |
    |------------------------|-------------------|----------------------------|-------------------------------------|------------------------|
    |    443    |    Azure Storage    |    Ne    |    Ano    |    Pokud jste přidali nějaké NSG, ujistěte se, že tento port je otevřený pro odchozí provoz.    |


Další nastavení pro virtuální síť se liší v závislosti na režimu přidělování fondů pro účet Batch.

<a id="vnets-for-pools-provisioned-in-the-batch-service" class="xliff"></a>

### Virtuální sítě pro fondy zřízené v rámci služby Batch

V režim přidělování služby Batch je možné přiřadit virtuální síť pouze k fondům **konfigurace služby Cloud Services**. Kromě toho musí být určená virtuální síť **klasickou** virtuální sítí. Virtuální sítě vytvořené pomocí modelu nasazení Azure Resource Manager se nepodporují.



* Instanční objekt *MicrosoftAzureBatch* musí mít pro zadanou virtuální síť roli řízení přístupu na základě rolí [Přispěvatel virtuálních počítačů modelu Classic](../active-directory/role-based-access-built-in-roles.md#classic-virtual-machine-contributor). Na webu Azure Portal:

  * Vyberte **Virtuální síť** a pak **Řízení přístupu (IAM)** > **Role** > **Přispěvatel virtuálních počítačů modelu Classic** > **Přidat**.
  * Do pole **Vyhledávání** zadejte MicrosoftAzureBatch.
  * Zaškrtněte políčko **MicrosoftAzureBatch**.
  * Klikněte na tlačítko **Vybrat**.



<a id="vnets-for-pools-provisioned-in-a-user-subscription" class="xliff"></a>

### Virtuální sítě pro fondy zřízené v rámci předplatného uživatele

V režimu přidělování podle předplatného uživatele jsou podporovány pouze fondy **konfigurace virtuálního počítače** a virtuální síť lze přiřadit pouze k nim. Kromě toho musí být určená virtuální síť založena na **Resource Manageru**. Virtuální sítě vytvořené pomocí klasického modelu nasazení se nepodporují.



<a id="scaling-compute-resources" class="xliff"></a>

## Škálování výpočetních prostředků
Služba Batch může díky [automatickému škálování](batch-automatic-scaling.md) dynamicky upravit počet výpočetních uzlů ve fondu podle aktuálního zatížení a využití prostředků výpočetního scénáře. To umožňuje snížit celkové náklady na běh aplikace, protože se využívají pouze prostředky, které jsou nutné, a aktuálně nepotřebné se uvolňují.

Automatické škálování můžete zapnout napsáním [vzorce automatického škálování](batch-automatic-scaling.md#automatic-scaling-formulas) a jeho přidružením k fondu. Služba Batch používá tento vzorec k určování cílového počtu uzlů ve fondu pro další interval škálování (ten můžete nakonfigurovat). Nastavení automatického škálování můžete pro fond zadat při jeho vytvoření, nebo můžete škálování povolit ve fondu později. Také můžete aktualizovat nastavení škálování ve fondu, v němž je škálování povoleno.

Jako příklad si vezměme úlohu, která může vyžadovat odeslání velkého počtu úkolů ke spuštění. Fondu můžete přiřadit vzorec škálování na základě aktuálního počtu úkolů čekajících ve frontě a rychlosti dokončování těchto úkolů v úloze, který přizpůsobí počet uzlů ve fondu. Služba Batch vzorec pravidelně vyhodnocuje a mění velikost fondu na základě vytížení a dalších nastavení vzorce. Služba podle potřeby přidává uzly, když ve frontě čeká velký počet úkolů, a naopak uzly odebírá, když ve frontě nejsou žádné úkoly nebo žádné úkoly právě neběží.

Vzorec škálování může být založen na následujících metrikách:

* **Časové metriky** jsou založeny na statistikách shromažďovaných každých pět minut po zadaný počet hodin.
* **Metriky prostředků** jsou založeny na využití procesoru, šířky pásma a paměti a na počtu uzlů.
* **Metriky úkolů** jsou založeny na stavu úkolů, jako je například *Aktivní* (zařazený do fronty), *Spuštěný* nebo *Dokončený*.

Když automatické škálování snižuje počet výpočetních uzlů ve fondu, je nutné zvážit, jak naložit s úkoly, které v okamžiku snižování již běží. K tomuto účelu poskytuje služba Batch *možnost zrušení přidělení uzlu*, kterou můžete ve vzorcích používat. Můžete například zadat, že spuštěné úkoly se mají okamžitě zastavit, okamžitě zastavit a pak znovu zařadit do fronty pro provedení na jiném uzlu, nebo nechat dokončit před odebráním uzlu z fondu.

Další informace o automatickém škálování aplikace najdete v tématu [Automatické škálování výpočetních uzlů ve fondu Azure Batch](batch-automatic-scaling.md).

> [!TIP]
> Pokud chcete maximalizovat využití výpočetních prostředků, nastavte cílový počet uzlů na konci úlohy na hodnotu nula, ale povolte dokončení spouštěných úkolů.
>
>

<a id="security-with-certificates" class="xliff"></a>

## Zabezpečení pomocí certifikátů
Při šifrování nebo dešifrování citlivých informací pro úkoly, jako je klíč pro [účet Azure Storage][azure_storage], je obvykle třeba použít certifikáty. Z toho důvodu můžete na uzly nainstalovat certifikáty. Šifrované tajné klíče jsou předány na úkoly prostřednictvím parametrů příkazového řádku nebo vložené v jednom prostředků úkolu a nainstalované certifikáty lze použít pro jejich dešifrování.

K přidání certifikátu do účtu služby Batch můžete použít operaci [Přidat certifikát][rest_add_cert] (Batch REST) nebo metodu [CertificateOperations.CreateCertificate][net_create_cert] (Batch .NET). Poté můžete certifikát přidružit k novému nebo existujícímu fondu. Pokud je certifikát přidružený k fondu, služba Batch nainstaluje certifikát na každý uzel ve fondu. Služba Batch nainstaluje příslušné certifikáty při spuštění uzlu, ještě před spuštěním úkolů (včetně spouštěcího úkolu a úkolu správce úloh).

Pokud přidáváte certifikáty pro *existující* fond, je nutné restartovat jeho výpočetní uzly, aby se na ně mohly certifikáty použít.

<a id="error-handling" class="xliff"></a>

## Zpracování chyb
V rámci vašeho řešení Batch může být nutné ošetřit selhání úkolů a aplikací.

<a id="task-failure-handling" class="xliff"></a>

### Zpracování selhání úkolů
Selhání úkolů spadá do následujících kategorií:

* **Selhání předběžného zpracování**

    Pokud úkol se nepodaří spustit, nastaví se pro něj chyba předběžného zpracování.  

    Chyby předběžného zpracování mohou nastat, pokud byly soubory prostředků úkolu přesunuty, pokud účet Storage už není k dispozici nebo pokud byl došlo k jinému problému, který zabránil úspěšnému zkopírování souborů z uzlu.

* **Selhání nahrání souborů**

    Pokud z nějakého důvodu selže odeslání souborů zadaných pro úkol, nastaví se pro tento úkol chyba odeslání souborů.

    K chybám nahrávání souborů může dojít, pokud podpis SAS zadaný pro přístup k Azure Storage je neplatný nebo neposkytuje oprávnění k zápisu, pokud účet úložiště už není dostupný nebo pokud byl došlo k jinému problému, který zabránil úspěšnému zkopírování souborů z uzlu.    

* **Selhání aplikace**

    Selhat může i proces zadaný pomocí příkazového řádku úkolu. Proces se považuje za neúspěšný, pokud proces provedený úkolem vrátí ukončovací kód s nenulovou hodnotou (viz *Ukončovací kódy úkolů* v následující části).

    Pro případy selhání aplikace je možné nakonfigurovat službu Batch tak, aby automaticky opakovala úkol až do zadaného počtu opakování.

* **Selhání omezení**

    Pomocí proměnné *maxWallClockTime* můžete nastavit omezení, které určuje maximální dobu trvání provádění úlohy nebo úkolu. To může být užitečné pro ukončení neodpovídajících úloh.

    Při překročení maximální doby je úkol označen jako *dokončený*, ale ukončovací kód je nastaven na hodnotu `0xC000013A` a pole *schedulingError* bude označeno jako `{ category:"ServerError", code="TaskEnded"}`.

<a id="debugging-application-failures" class="xliff"></a>

### Ladění chyb aplikace
* `stderr` a `stdout`

    Během provádění může aplikace generovat diagnostický výstup, který lze použít k řešení potíží. Jak je uvedeno v části [Soubory a adresáře](#files-and-directories) výše, zapíše služba Batch standardní výstup a standardní chybový výstup do souborů `stdout.txt` a `stderr.txt` v adresáři úkolů na výpočetním uzlu. Tyto soubory můžete stáhnout pomocí webu Azure Portal nebo jedné ze sad SDK služby Batch. Tyto a další soubory můžete pro účely odstraňování potíží načíst například v knihovně Batch .NET pomocí metod [ComputeNode.GetNodeFile][net_getfile_node] a [CloudTask.GetNodeFile][net_getfile_task].

* **Ukončovací kódy úkolů**

    Jak bylo zmíněno výše, je úkol službou Batch označen jako neúspěšný, pokud proces, který je úkolem spuštěn, vrátí nenulový ukončovací kód. Pokud úkol spustí proces, služba Batch vyplní vlastnost ukončovacího kódu úkolu *návratovým kódem procesu*. Je důležité si uvědomit, že ukončovací kód úkolu **neurčuje** služba Batch. Ukončovací kód úkolu se určuje podle samotného procesu nebo operačního systému, na kterém byl proces spuštěn.

<a id="accounting-for-task-failures-or-interruptions" class="xliff"></a>

### Monitorování účtů pro selhání úkolů nebo přerušení
Úkoly mohou občas selhat nebo být přerušeny. Selhat může samotná aplikace úkolu, může dojít k restartování uzlu, na kterém je úkol spuštěn, nebo může být uzel odebrán z fondu během operace změny velikosti fondu, pokud jsou zásady deaktivace přidělení fondu nastaveny na odebrání uzlů okamžitě bez čekání na dokončení úkolů. Ve všech případech může být úkol službou Batch automaticky znovu zařazen do fronty, aby byl spuštěn na jiném uzlu.

Je také možné, že občasný problém způsobí, že se úkol zablokuje nebo že jeho spuštění trvá příliš dlouho. Pro úkol můžete nastavit maximální interval provádění. Pokud dojde k překročení maximálního intervalu provádění, služba Batch přeruší aplikaci úkolu.

<a id="connecting-to-compute-nodes" class="xliff"></a>

### Připojení k výpočetním uzlům
Další ladění a řešení potíží můžete provádět při vzdáleném přihlášení k výpočetnímu uzlu. Pro uzly Windows si můžete na portálu Azure stáhnout soubor protokolu RDP (Remote Desktop) a pro uzly Linux získat informace o připojení Secure Shell (SSH). Můžete to také provést pomocí rozhraní API služby Batch – například s [Batch .NET][net_rdpfile] nebo [Batch Python](batch-linux-nodes.md#connect-to-linux-nodes-using-ssh).

> [!IMPORTANT]
> Pokud se chcete připojit k uzlu prostřednictvím protokolu RDP nebo SSH, musíte na uzlu nejprve vytvořit uživatele. Můžete to provést takto: na webu Azure Portal [přidáte uživatelský účet do uzlu][rest_create_user] pomocí rozhraní Batch REST API a zavoláte metodu [ComputeNode.CreateComputeNodeUser][net_create_user] v Batch .NET nebo zavoláte metodu [add_user][py_add_user] v modulu Batch Python.
>
>

<a id="troubleshooting-problematic-compute-nodes" class="xliff"></a>

### Řešení potíží s problematickými výpočetními uzly
V situacích, kdy některé úkoly selhávají, může klientská aplikace nebo služba Batch prozkoumat metadata neúspěšných úkolů, aby identifikovala uzel, který se chová nesprávně. Každý uzel ve fondu má přiřazeno jedinečné číslo ID a uzel, na kterém je spuštěn úkol, je zahrnut v metadatech úkolu. Po identifikaci problémového uzlu s ním můžete provést několik akcí:

* **Restartování uzlu** ([REST][rest_reboot] | [.NET][net_reboot])

    Restartování uzlu může někdy odstranit latentní problémy, jako jsou zablokované nebo zhroucené procesy. Vezměte na vědomí, že spouštěcí úkol nebo úkol přípravy úlohy je po restartování uzlu proveden, pokud jej váš fond používá.
* **Obnovení uzlu z image** ([REST][rest_reimage] | [.NET][net_reimage])

    Tato možnost přeinstaluje operační systém na uzlu. Stejně jako u restartování uzlu jsou spouštěcí úkoly a úkoly přípravy úlohy znovu spuštěny poté, co uzel byl obnoven z image.
* **Odebrání uzlu z fondu** ([REST][rest_remove] | [.NET][net_remove])

    Někdy je nezbytné úplně odebrat uzel z fondu.
* **Zakázání plánování úkolů na uzlu** ([REST][rest_offline] | [.NET][net_offline])

    To efektivně převede uzel do režimu offline, aby se mu nepřiřazovaly žádné další úkoly, ale umožňuje, aby zůstal spuštěný a ve fondu. Díky tomu můžete provést další šetření příčin selhání bez ztráty dat neúspěšného úkolu, aniž by uzel způsobil selhání dalších úkolů. Můžete například zakázat plánování úloh na uzlu a pak se [přihlásit vzdáleně](#connecting-to-compute-nodes) a prohlédnout si protokoly událostí uzlu nebo provádět jiné řešení potíží. Jakmile dokončíte šetření, můžete převést uzel zpět do režimu online povolením plánování úkolů ([REST][rest_online] | [.NET][net_online]) nebo provést některou z dalších akcí popsaných výše.

> [!IMPORTANT]
> U každé akce uvedené v této části – restartování, obnovení z image, odebrání, zakázání plánování úkolů – můžete určit, jak se při provedení akce naloží s úkoly, které na uzlu aktuálně běží. Pokud například zakážete plánování úkolů na uzlu pomocí klientské knihovny Batch .NET, můžete zadat hodnotu výčtu [DisableComputeNodeSchedulingOption][net_offline_option] a určit, jestli spuštěné úkoly ukončit (**Terminate**), znovu zařadit do fronty (**Requeue**) pro naplánování na jiných uzlech, nebo umožnit dokončení běžících úkolů před provedením akce (**TaskCompletion**).
>
>

<a id="next-steps" class="xliff"></a>

## Další kroky
* Další informace o dostupných [rozhraních API a nástrojích služby Batch](batch-apis-tools.md) pro sestavování řešení Batch.
* Projděte si podrobně ukázkovou aplikaci služby Batch v tématu [Začínáme s knihovnou služby Azure Batch pro .NET](batch-dotnet-get-started.md). K dispozici je také [verze pro Python](batch-python-tutorial.md) tohoto kurzu, která spouští úlohy na výpočetních uzlech systému Linux.
* Stáhněte si a sestavte ukázkový projekt [Batch Explorer][github_batchexplorer] pro použití při vývoji řešení Batch. Pomocí projektu Batch Explorer můžete provést následující a další akce:

  * Monitorování a manipulace s fondy, úlohami a úkoly v rámci vašeho účtu Batch
  * Stažení `stdout.txt`, `stderr.txt` a dalších souborů z uzlů
  * Vytvořit uživatele na uzlech a stáhnout soubory protokolu RDP pro vzdálené přihlášení
* Zjistěte, jak [vytvořit fondy výpočetních uzlů Linux](batch-linux-nodes.md).
* Navštivte [fórum Azure Batch][batch_forum] na webu MSDN. Fórum je vhodné místo pro kladení dotazů, ať se teprve učíte, nebo jste odborníky na používání služby Batch.

[1]: ./media/batch-api-basics/node-folder-structure.png

[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurebatch
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[msdn_env_vars]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_job_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.commonenvironmentsettings.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_onalltaskscomplete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.onalltaskscomplete.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx
[net_rdpfile]: https://msdn.microsoft.com/library/azure/Mt272127.aspx
[vnet]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_netconf

[py_add_user]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.ComputeNodeOperations.add_user

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_job_schedules]: https://msdn.microsoft.com/library/azure/mt282179.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

