<properties
   pageTitle="Začínáme s rozhraním příkazového řádku Azure Batch CLI | Microsoft Azure"
   description="Rychlý úvod k příkazům Batch v rozhraní příkazového řádku Azure CLI pro správu prostředků služby Azure Batch"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="09/06/2016"
   ms.author="marsma"/>

# Začínáme s rozhraním příkazového řádku Azure Batch CLI

Víceplatformové rozhraní příkazového řádku Azure (Azure CLI) umožňuje spravovat účty Batch a prostředky, jako jsou fondy, úlohy a úkoly v příkazových prostředích systémů Windows, Linux a Mac. Prostřednictvím rozhraní příkazového řádku Azure CLI můžete provádět a převádět na skripty řadu stejných úkolů, které se provádějí prostřednictvím rozhraní API služby Batch, webu Azure Portal a rutin prostředí PowerShell služby Batch.

Tento článek je založen na rozhraní příkazového řádku Azure CLI verze 0.10.3.

## Požadavky

* [Instalace rozhraní příkazového řádku Azure CLI](../xplat-cli-install.md)

* [Připojení rozhraní příkazového řádku Azure CLI k předplatnému Azure](../xplat-cli-connect.md)

* Přepnutí do **režimu Resource Manager**: `azure config mode arm`

>[AZURE.TIP] Instalaci rozhraní příkazového řádku Azure CLI doporučujeme často aktualizovat, abyste mohli využívat výhody, které vám přinášejí aktualizace a vylepšení služby.

## Nápověda k příkazům

Můžete zobrazit text nápovědy pro každý příkaz v rámci rozhraní příkazového řádku Azure CLI, pokud za názvem příkazu zadáte jediný parametr `-h`. Například:

* Chcete-li zobrazit nápovědu pro příkaz `azure`, zadejte: `azure -h`
* Chcete-li vypsat seznam všech příkazů Batch v rámci rozhraní příkazového řádku, zadejte: `azure batch -h`
* Chcete-li získat nápovědu k vytvoření účtu Batch, zadejte: `azure batch account create -h`

Pokud si nejste jisti, pomocí parametru příkazového řádku `-h` můžete zobrazit nápovědu pro kterýkoli příkaz rozhraní příkazového řádku Azure CLI.

## Vytvoření účtu Batch

Použití:

    azure batch account create [options] <name>

Příklad:

    azure batch account create --location "West US"  --resource-group "resgroup001" "batchaccount001"

Vytvoří nový účet Batch se zadanými parametry. Musíte zadat alespoň umístění, skupinu prostředků a název účtu. Pokud ještě nemáte skupinu prostředků, vytvořte ji spuštěním příkazu `azure group create` a určete jednu z oblastí Azure (například „Západní USA“) prostřednictvím parametru `--location`. Například:

    azure group create --name "resgroup001" --location "West US"

> [AZURE.NOTE] Název účtu Batch musí být v rámci oblasti Azure, pro kterou je účet vytvořen, jedinečný. Smí obsahovat jenom malá písmena a číslice a musí být dlouhý 3 až 24 znaků. V názvech účtů Batch nelze používat zvláštní znaky jako `-` nebo `_`.

### Propojený účet úložiště (automatické úložiště)

Při vytváření můžete s účtem Batch volitelně propojit účet Storage pro **obecné účely**. Funkce [balíčků aplikací](batch-application-packages.md) účtu Batch využívá úložiště Blob Storage v propojeném účtu úložiště pro obecné účely, stejně jako knihovna .NET [Batch File Conventions](batch-task-output.md). Tyto volitelné funkce vám pomohou při nasazení aplikací spouštěných vašimi úkoly Batch a při zachování dat, které vytvářejí.

Chcete-li propojit existující účet Azure Storage s novým účtem Batch při jeho vytváření, zadejte parametr `--autostorage-account-id`. Tato možnost vyžaduje plně kvalifikované ID prostředku účtu úložiště.

Nejprve zobrazte podrobnosti o účtu úložiště:

    azure storage account show --resource-group "resgroup001" "storageaccount001"

Potom použijte hodnotu **Url** hodnotu pro parametr `--autostorage-account-id`. Hodnota Url začíná řetězcem „/subscriptions/“ a obsahuje ID předplatného a cestu prostředku k účtu Storage:

    azure batch account create --location "West US"  --resource-group "resgroup001" --autostorage-account-id "/subscriptions/8ffffff8-4444-4444-bfbf-8ffffff84444/resourceGroups/resgroup001/providers/Microsoft.Storage/storageAccounts/storageaccount001" "batchaccount001"

## Odstranění účtu Batch

Použití:

    azure batch account delete [options] <name>

Příklad:

    azure batch account delete --resource-group "resgroup001" "batchaccount001"

Odstraní určený účet Batch. Po zobrazení výzvy potvrďte, že chcete účet odebrat (dokončení odebírání účtu může nějakou dobu trvat).

## Správa přístupových klíčů pro účet

Pro [vytváření a úpravu prostředků](#create-and-modify-batch-resources) v účtu Batch je potřebný přístupový klíč.

### Vypsání přístupových klíčů

Použití:

    azure batch account keys list [options] <name>

Příklad:

    azure batch account keys list --resource-group "resgroup001" "batchaccount001"

Vypíše přístupové klíče pro daný účet Batch.

### Vygenerování nového přístupového klíče

Použití:

    azure batch account keys renew [options] --<primary|secondary> <name>

Příklad:

    azure batch account keys renew --resource-group "resgroup001" --primary "batchaccount001"

Znovu vygeneruje určený klíč účtu pro daný účet Batch.

## Vytváření a úpravy prostředků služby Batch

Pro vytváření, čtení, aktualizaci či odstraňování (souhrnně provádění operací CRUD) prostředků Batch, jako jsou fondy, výpočetní uzly, úlohy a úkoly, můžete použít rozhraní příkazového řádku Azure CLI. Tyto operace CRUD vyžadují název účtu Batch, přístupový klíč a koncový bod. Tyto položky můžete určit pomocí parametrů `-a`, `-k`, a `-u`. Můžete také nastavit [proměnné prostředí](#credential-environment-variables), které jsou rozhraním příkazového řádku CLI využívány automaticky (pokud mají přiřazenou hodnotu).

### Proměnné prostředí pro pověření

Namísto zadávání parametrů `-a`, `-k`, a `-u` na příkazový řádek pro každý spouštěný příkaz můžete nastavit proměnné prostředí `AZURE_BATCH_ACCOUNT`, `AZURE_BATCH_ACCESS_KEY` a `AZURE_BATCH_ENDPOINT`. Rozhraní příkazového řádku Batch CLI tyto proměnné (pokud jsou nastavené) používá, takže lze parametry `-a`, `-k` a `-u`vynechat. Ve zbývající části tohoto článku se předpokládá použití uvedených proměnných prostředí.

>[AZURE.TIP] Pomocí příkazu `azure batch account keys list` můžete vypsat klíče a pomocí příkazu `azure batch account show` můžete zobrazit koncový bod účtu.

### Soubory JSON

Při vytváření prostředků Batch, jako jsou fondy a úlohy, můžete určit soubor JSON obsahující konfiguraci nového prostředku namísto předávání jejích parametrů v podobě parametrů příkazového řádku. Například:

`azure batch pool create my_batch_pool.json`

Ačkoli mnoho operací vytváření prostředků lze provádět pouze prostřednictvím parametrů příkazového řádku, některé funkce vyžadují soubor ve formátu JSON obsahující podrobnosti o prostředku. Soubor JSON je například třeba použít, pokud chcete určit soubory prostředků pro úkol při spuštění.

Chcete-li najít soubor JSON potřebný k vytvoření prostředku, vyhledejte potřebné informace v [referenční dokumentaci rozhraní Batch REST API][rest_api] na webu MSDN. Každé téma „Přidání prostředku *typ prostředku* “ obsahuje ukázkový soubor JSON pro vytvoření prostředku, který můžete použít jako šablony pro své soubory JSON. Soubor JSON pro vytvoření fondu například najdete v části [Přidání fondu k účtu][rest_add_pool].

>[AZURE.NOTE] Pokud při vytváření prostředku určíte soubor JSON, všechny ostatní parametry zadané na příkazovém řádku pro příslušný prostředek budou ignorovány.

## Vytvoření fondu

Použití:

    azure batch pool create [options] [json-file]

Příklad (konfigurace virtuálního počítače):

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "STANDARD_A1" --image-publisher "Canonical" --image-offer "UbuntuServer" --image-sku "14.04.2-LTS" --node-agent-id "batch.node.ubuntu 14.04"

Příklad (konfigurace služby Cloud Services):

    azure batch pool create --id "pool002" --target-dedicated 1 --vm-size "small" --os-family "4"

Vytvoří fond výpočetních uzlů v rámci služby Batch.

Jak je uvedeno v [přehledu funkcí Batch](batch-api-basics.md#pool), při výběru operačního systému pro uzly ve vašem fondu máte dvě možnosti: **konfigurace virtuálního počítače** a **konfigurace služby Cloud Services**. Pomocí parametrů `--image-*` můžete vytvářet fondy Možnosti pro vytváření fondů konfigurace virtuálního počítače a pomocí parametrů `--os-family` můžete vytvářet fondů konfigurace služby Cloud Services. Parametry `--os-family` a `--image-*` nelze zadat současně.

Můžete určit [balíčky aplikací](batch-application-packages.md) fondu a příkazový řádek pro [úkol při spuštění](batch-api-basics.md#start-task). Chcete-li však určit soubory prostředků pro úkol při spuštění, je třeba namísto uvedeného postupu použít [soubor JSON](#json-files).

Fond můžete odstranit příkazem:

    azure batch pool delete [pool-id]

>[AZURE.TIP] V [seznamu imagí virtuálních počítačů](batch-linux-nodes.md#list-of-virtual-machine-images) najdete hodnoty vhodné pro parametry `--image-*`.

## Vytvoření úlohy

Použití:

    azure batch job create [options] [json-file]

Příklad:

    azure batch job create --id "job001" --pool-id "pool001"

Přidá k účtu Batch úlohu a určí fond, ve kterém se budou příslušné úkoly provádět.

Úlohu můžete odstranit příkazem:

    azure batch job delete [job-id]

## Vypsání seznamu fondů, úloh, úkolů a dalších prostředků

Každý typ prostředku Batch podporuje příkaz `list`, který zadá dotaz na účet Batch a vypíše seznam prostředků příslušného typu. Můžete například vypsat seznam fondů v rámci vašeho účet a seznam úkolů v rámci úloh:

    azure batch pool list
    azure batch task list --job-id "job001"

### Efektivní výpisy seznamů prostředků

Dotazování lze urychlit, pokud pro operace `list` určíte parametry klauzule **select**, **filter** a **expand**. Pomocí těchto parametrů můžete omezit množství dat vrácené službou Batch. Jelikož veškeré filtrování probíhá na straně serveru, přenášejí se pouze data, která vás zajímají. Při provádění operací vypsání seznamu můžete pomocí těchto klauzulí snížit využití šířky pásma (a tedy čas).

Tento příkaz například vrátí pouze fondy, jejichž ID začíná řetězcem „renderTask“:

    azure batch task list --job-id "job001" --filter-clause "startswith(id, 'renderTask')"

Rozhraní příkazového řádku Batch CLI podporuje všechny tři klauzule podporované službou Batch:

* `--select-clause [select-clause]`  Vrácení podmnožiny vlastností pro každou entitu.
* `--filter-clause [filter-clause]`  Vrácení pouze těch entit, které odpovídají zadanému výrazu OData.
* `--expand-clause [expand-clause]`  Získání informací o entitách v rámci jediného základního volání REST. Klauzule expand v současné době podporuje pouze vlastnost `stats`.

Podrobnosti o uvedených třech klauzulích a jejich použití v dotazech pro vypsání seznamu najdete v článku [Efektivní dotazování na službu Azure Batch](batch-efficient-list-queries.md).

## Správa balíčků aplikací

Balíčky aplikací umožňují zjednodušené nasazování aplikací ve výpočetních uzlech ve vašich fondech. Prostřednictvím rozhraní příkazového řádku Azure CLI můžete odesílat balíčky aplikací, spravovat verze balíčků či balíčky odstraňovat.

Vytvoření nové aplikace a přidání verze balíčku:

**Vytvoření** aplikace:

    azure batch application create "resgroup001" "batchaccount001" "MyTaskApplication"

**Přidání** balíčku aplikace:

    azure batch application package create "resgroup001" "batchaccount001" "MyTaskApplication" "1.10-beta3" package001.zip

**Aktivace** balíčku:

    azure batch application package activate "resgroup002" "azbatch002" "MyTaskApplication" "1.10-beta3" zip

### Nasazení balíčku aplikace

Při vytváření nového fondu můžete určit jeden nebo více balíčků aplikací pro nasazení. Když určíte balíček při vytváření fondu, bude nasazen v každém uzlu při jeho přidání do fondu. Balíčky se také nasazují při restartování uzlu nebo jeho obnovení z image.

Tento příkaz určuje balíček při vytváření fondu a je nasazen při přidání jednotlivých uzlů do nového fondu:

    azure batch pool create --id "pool001" --target-dedicated 1 --vm-size "small" --os-family "4" --app-package-ref "MyTaskApplication"

V současné době nelze pomocí parametrů příkazového řádku určit verzi balíčku, která se má nasadit. Než budete moci přiřadit aplikaci do fondu, je třeba nastavit pro ni výchozí verzi prostřednictvím webu Azure Portal. Postup při nastavování výchozí verze najdete v článku [Nasazení aplikací pomocí balíčků aplikací v Azure Batch](batch-application-packages.md). Můžete však určit výchozí verze, pokud namísto parametrů příkazového řádku při vytváření fondu použijete [soubor JSON](#json-files).

>[AZURE.IMPORTANT] Chcete-li používat balíčky aplikací, je třeba [propojit účet Azure Storage](#linked-storage-account-autostorage) s vaším účtem Batch.

## Rady pro řešení potíží

Cílem této části je poskytnout vám prostředky, které můžete použít při řešení potíží s rozhraním příkazového řádku Azure CLI. Nemusí sice vyřešit všechny problémy, může vám ale pomoci zúžit rozsah možných příčin a odkázat vás na prostředky nápovědy.

* Použití parametru `-h` k získání **textu nápovědy** pro kterýkoli příkaz rozhraní příkazového řádku CLI

* Pomocí parametrů `-v` a `-vv` můžete zobrazit **podrobný** výstup příkazů; parametr `-vv` nastaví „obzvlášť“ podrobný výstup a zobrazí skutečné požadavky a odpovědi REST. Tyto přepínače jsou užitečné pro zobrazení úplného chybového výstupu.

* Pomocí parametru `--json` můžete zobrazit **výstup příkazu ve formátu JSON**. Příkaz `azure batch pool show "pool001" --json` například zobrazí vlastnosti fondu pool001 ve formátu JSON. Pak můžete tento výstup zkopírovat a upravit pro použití v příkazu `--json-file` (viz [soubory JSON](#json-files) dříve v tomto článku).

* [Fórum Batch na webu MSDN][batch_forum] je vynikajícím prostředkem nápovědy a je pečlivě sledováno členy týmu Batch. Pokud narazíte na problémy nebo hledáte pomoc s konkrétní operací, neváhejte uveřejnit zde své otázky.

* Rozhraní příkazového řádku Azure CLI v současné době nepodporuje všechny operace s prostředky Batch. Nyní například nelze pro fond určit *verzi* balíčku aplikace, nýbrž jen ID balíčku. V takových případech může být potřebné zadat pro příkaz parametr `--json-file` namísto použití parametrů příkazového řádku. Dbejte na to, abyste stále měli k dispozici nejnovější verzi rozhraní příkazového řádku CLI a mohli využívat budoucí vylepšení.

## Další kroky

*  V článku [Nasazení aplikace pomocí balíčků aplikací v Azure Batch](batch-application-packages.md) se dozvíte, jak lze tuto funkci použít ke správě a nasazení aplikací, které spouštíte ve výpočetních uzlech Batch.

* Další informace o snižování počtu položek a typů informací, které se vrací pro dotazy na službu Batch, najdete v článku [Efektivní dotazování na službu Batch](batch-efficient-list-queries.md).

[batch_forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_readme]: https://github.com/Azure/azure-xplat-cli/blob/dev/README.md
[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx


<!--HONumber=sep16_HO1-->



