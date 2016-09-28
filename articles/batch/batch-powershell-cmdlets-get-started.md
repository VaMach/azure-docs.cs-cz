<properties
   pageTitle="Začínáme s prostředím Azure PowerShell ve službě Batch | Microsoft Azure"
   description="Rychlý úvod do rutin prostředí Azure PowerShell, jejichž pomocí lze spravovat službu Azure Batch"
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="07/28/2016"
   ms.author="danlep"/>


# Začínáme s prostředím PowerShell ve službě Azure Batch
Pomocí rutin prostředí PowerShell služby Batch můžete provádět a převádět na skripty řadu stejných úkolů, které se provádějí pomocí rozhraní API služby Batch, webu Azure Portal a rozhraní příkazového řádku (CLI) Azure. Tento článek obsahuje rychlý úvod do rutin, s jejichž pomocí můžete spravovat účty Batch a pracovat s prostředky služby Batch, jako jsou fondy, úlohy a úkoly. Tento článek popisuje rutiny v prostředí Azure PowerShell verze 1.6.0.

Úplný seznam rutin prostředí Batch a podrobný popis syntaxe rutin najdete v článku [Rutiny služby Azure Batch – reference](https://msdn.microsoft.com/library/azure/mt125957.aspx). 


## Požadavky

* **Azure PowerShell** – Pokyny ke stažení a instalaci prostředí Azure PowerShell najdete v článku [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md). 
   
    * Protože se rutiny služby Azure Batch dodávají v modulu Azure Resource Manager, bude nutné spuštěním rutiny **Login-AzureRmAccount** připojit vaše předplatné. 
    
    * Prostředí Azure PowerShell doporučujeme často aktualizovat, abyste mohli využívat výhody, které vám přinášejí aktualizace a vylepšení služby. 
    
* **Registrace v oboru názvů poskytovatele služby Batch (jednorázová operace)** – Před zahájením práce s účty Batch je nutné provést registraci v oboru názvů poskytovatele služby Batch. Tuto operaci je nutné provést jednou pro každé předplatné. Spusťte následující rutinu:

        Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch


## Správa účtů a klíčů služby Batch

### Vytvoření účtu Batch

**New-AzureRmBatchAccount** vytvoří nový účet Batch v určené skupině prostředků. Pokud skupinu prostředků nemáte, vytvořte ji spuštěním rutiny [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx) a zadáním jednoho z regionů Azure v parametru **Location** (Umístění), například Central US. Příklad:


    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"


Potom ve skupině prostředků vytvořte nový účet Batch a zadejte název účtu v parametru <*název_účtu*> a umístění a název skupiny prostředků. Vytváření účtu Batch může nějakou dobu trvat. Příklad:


    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup

> [AZURE.NOTE] Název účtu Batch musí být v dané oblasti Azure skupiny prostředků jedinečný, musí být tvořen 3 až 24 znaky a obsahovat pouze malá písmena a číslice.

### Získání přístupových klíčů k účtu
**Get-AzureRmBatchAccountKeys** zobrazí přístupové klíče asociované s účtem Azure Batch. Pokud například chcete získat primární a sekundární klíče vytvořeného účtu, spusťte následující rutinu:

    $Account = Get-AzureRmBatchAccountKeys –AccountName <accountname>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey


### Vygenerování nového přístupového klíče
**New-AzureRmBatchAccountKey** vygeneruje nový primární nebo sekundární přístupový klíč pro účet Azure Batch. Pokud například chcete vygenerovat nový primární klíč pro účet Batch, zadejte:


    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary


> [AZURE.NOTE] Pokud chcete vygenerovat nový sekundární klíč, zadejte jako parametr **KeyType** Secondary. Primární a sekundární klíče je nutné vygenerovat odděleně.

### Odstranění účtu Batch
**Remove-AzureRmBatchAccount** odstraní účet Batch. Příklad:


    Remove-AzureRmBatchAccount -AccountName <account_name>

Až k tomu budete vyzváni, potvrďte, že chcete odebrat účet. Odebrání účtu může trvat nějakou dobu.

## Vytvoření objektu BatchAccountContext

Pokud chcete ve službě Batch při vytváření a správě fondů, úloh, úkolů a dalších prostředků k ověřování používat rutiny prostředí PowerShell, je nutné nejprve vytvořit objekt BatchAccountContext, který bude uchovávat název vašeho účtu a klíče:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Objekt BatchAccountContext budete předávat rutinám, které pracují s parametrem **BatchContext**.

> [AZURE.NOTE] Ve výchozím nastavení se k ověřování používá primární klíč účtu, ale změnou hodnoty vlastnosti **KeyInUse** objektu BatchAccountContext lze klíč vybrat explicitně: `$context.KeyInUse = "Secondary"`.



## Vytváření a úpravy prostředků služby Batch
Pomocí rutin, jako třeba **New-AzureBatchPool**, **New-AzureBatchJob** a **New-AzureBatchTask**, můžete v účtu Batch vytvářet prostředky. Pomocí rutin **Get-** a **Set-** lze aktualizovat vlastnosti existujících prostředků a pomocí rutin **Remove-** lze prostředky v účtu Batch odebírat. 

Při použití řady těchto rutin musíte kromě předání objektu BatchContext navíc taky vytvořit nebo předat objekty, které obsahují podrobné nastavení prostředků, jak ukazuje následující příklad. Další příklady najdete v podrobné nápovědě k jednotlivým rutinám.

### Vytvoření fondu služby Batch

Při vytváření nebo aktualizaci fondu Batch vyberete konfiguraci cloudové služby nebo konfiguraci virtuálního počítače pro operační systém výpočetních uzlů (viz [Přehled funkcí služby Batch](batch-api-basics.md#pool)). Vaše volba určí, jestli vaše výpočetní uzly obdrží image některého z [vydání hostovaného operačního systému Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases) nebo některou z podporovaných imagí Linuxu nebo virtuálního počítače s Windows z Azure Marketplace. 

Když spouštíte rutinu **New-AzureBatchPool**, předejte nastavení operačního systému v objektu PSCloudServiceConfiguration nebo PSVirtualMachineConfiguration. Následující rutina třeba vytvoří nový fond Batch s výpočetními uzly malé velikosti v konfiguraci cloudové služby, které obdrží image nejnovější verze operačního systému z řady 3 (Windows Server 2012). Parametr **CloudServiceConfiguration** tady určuje proměnnou *$configuration* jako objekt PSCloudServiceConfiguration. Parametr **BatchContext** určuje jako objekt BatchAccountContext dříve definovanou proměnnou *$context*.


    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(3,"*")
    
    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Cílový počet výpočetních uzlů v novém fondu určuje vzorec automatického škálování. V tomto případě se používá jednoduchý vzorec **$TargetDedicated=4**, který značí, že maximální počet výpočetních uzlů ve fondu je 4. 

## Dotazy na fondy, úlohy, úkoly a další podrobnosti

Pomocí rutin, jako například **Get-AzureBatchPool**, **Get-AzureBatchJob** a **Get-AzureBatchTask**, lze zadávat dotazy na entity vytvořené v účtu Batch.


### Dotazy na data

Jako příklad použijte rutinu **Get-AzureBatchPools** k vyhledání fondů. Tato rutina se ve výchozím nastavení dotazuje na všechny fondy v účtu, za předpokladu, že jste už uložili objekt BatchAccountContext do hodnoty *$context*:


    Get-AzureBatchPool -BatchContext $context

### Použití filtru OData

Pomocí parametru **Filter** lze použít filtr OData a vyhledat pouze objekty, které vás zajímají. Například můžete vyhledat všechny fondy s ID začínajícími řetězcem myPool.


    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context


Tato metoda není tak účinná jako použití klauzule Where-Object v místním kanálu. Dotaz se však odešle do služby Batch přímo, takže se veškeré filtrování provede na straně serveru, což šetří šířku pásma připojení k internetu.

### Použití parametru Id

Alternativou k použití filtru OData je použití parametru **Id**. Postup zadání dotazu na konkrétní fond s parametrem Id myPool


    Get-AzureBatchPool -Id "myPool" -BatchContext $context


Parametr **Id** podporuje pouze vyhledávání úplných ID, nepodporuje zástupné znaky ani filtry stylu typu OData.



### Použití parametru MaxCount

Ve výchozím nastavení každá rutina vrací maximálně 1 000 objektů. Pokud tento limit překročíte, můžete buď upřesnit filtr, aby vracel méně objektů, nebo explicitně nastavit maximální hodnotu pomocí parametru **MaxCount**. Příklad:


    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Pokud chcete horní omezení počtu zcela odebrat, nastavte parametr **MaxCount** na hodnotu 0 nebo menší.

### Použití kanálu

Rutiny služby Batch mohou využívat kanál prostředí PowerShell k odesílání dat mezi rutinami. Tento postup odpovídá zadání parametru ale usnadňuje vypisování více entit. Následující kód například vyhledá všechny úkoly ve vašem účtu:


    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context


## Další kroky
* Podrobný popis syntaxe rutin najdete v článku [Rutiny služby Azure Batch – reference](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Další informace o snižování počtu položek a typů informací, které se vrací pro dotazy na službu Batch, najdete v článku [Efektivní dotazování na službu Batch](batch-efficient-list-queries.md). 



<!--HONumber=Sep16_HO3-->


