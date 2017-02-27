

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Preview)

Až si [nainstalujete Azure CLI 2.0 (Preview)](https://docs.microsoft.com/cli/azure/install-az-cli2), zobrazte pomocí příkazu `az vm image list` seznam oblíbených imagí virtuálních počítačů v mezipaměti. Například následující použití příkazu `az vm image list -o table` zobrazí:

```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               14.04.4-LTS         Canonical:UbuntuServer:14.04.4-LTS:latest                       UbuntuLTS            latest
CentOS         OpenLogic               7.2                 OpenLogic:CentOS:7.2:latest                                     CentOS               latest
openSUSE       SUSE                    13.2                SUSE:openSUSE:13.2:latest                                       openSUSE             latest
RHEL           RedHat                  7.2                 RedHat:RHEL:7.2:latest                                          RHEL                 latest
SLES           SUSE                    12-SP1              SUSE:SLES:12-SP1:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

### <a name="finding-all-current-images"></a>Vyhledání všech aktuálních imagí

Pokud chcete získat aktuální seznam všech imagí, použijte příkaz `az vm image list` s možností `--all`. Na rozdíl od příkazů Azure CLI 1.0 vrací příkaz `az vm image list --all` ve výchozím nastavení všechny image v umístění **westus** (pokud nezadáte konkrétní argument `--location`), takže zpracování příkazu `--all` chvíli trvá. Pokud máte v úmyslu prozkoumávat interaktivně, použijte příkaz `az vm image list --all > allImages.json`, který vrátí seznam všech aktuálně dostupných imagí v Azure a uloží je v podobě souboru pro místní použití. 

Můžete zadat některou z mnoha možností pro omezení hledání na konkrétní umístění, nabídku, vydavatele nebo skladovou jednotku (SKU), pokud již máte nějakou image nebo více imagí vybraných. Pokud nezadáte umístění, vrátí se hodnoty pro umístění **westus**.

### <a name="find-specific-images"></a>Vyhledání konkrétních imagí

K nalezení konkrétních informací použijte příkaz `az vm image list` s [filtrem dotazu JMESPATH](https://docs.microsoft.com/cli/azure/query-az-cli2). Například následující příkaz zobrazí **skladové jednotky (SKU)**, které jsou k dispozici pro **Debian** (nezapomeňte, že bez přepínače `--all` se prohledá pouze místní mezipaměť obvyklých imagí):

```azurecli
az vm image list --query '[?contains(offer,`Debian`)]' -o table --all
```

Výstup bude podobný tomuto: 
```
You are viewing an offline list of images, use --all to retrieve an up-to-date list
  Sku  Publisher    Offer    Urn                       Version    UrnAlias
-----  -----------  -------  ------------------------  ---------  ----------
    8  credativ     Debian   credativ:Debian:8:latest  latest     Debian

<list shortened for the example>
```

Pokud víte, kam nasazujete, můžete využít obecné výsledky vyhledávání imagí společně s příkazy `az vm image list-skus`, `az vm image list-offers` a `az vm image list-publishers` k nalezení přesně toho, co hledáte a v umístění, kam můžete image nasadit. Pokud například z předchozího příkladu víte, že `credativ` má nabídku Debianu, můžete použít možnost `--location` a další k nalezení přesně toho, co chcete. Následující příklad hledá image Debianu 8 v umístění **westeurope**:

```azurecli 
az vm image show -l westeurope -f debian -p credativ --skus 8 --version 8.0.201701180
```

a výstupem bude:

```json
{
  "dataDiskImages": [],
  "id": "/Subscriptions/<guid>/Providers/Microsoft.Compute/Locations/westeurope/Publishers/credativ/ArtifactTypes/VMImage/Offers/debian/Skus/8/Versions/8.0.201701180",
  "location": "westeurope",
  "name": "8.0.201701180",
  "osDiskImage": {
    "operatingSystem": "Linux"
  },
  "plan": null,
  "tags": null
}
```

## <a name="azure-cli-10"></a>Azure CLI 1.0 

> [!NOTE]
> Tento článek popisuje, jak procházet a vybírat image virtuálních počítačů pomocí instalace Azure CLI 1.0 nebo Azure PowerShellu s podporou modelu nasazení Azure Resource Manager. Předpokladem je, že přepnete do režimu Resource Manager. V Azure CLI do tohoto režimu přejdete zadáním `azure config mode arm`. 
> 

Nejrychlejší způsob, jak najít image, je zavolat příkaz `azure vm image list` a předat mu umístění, název vydavatele (nerozlišují se velká a malá písmena!) a nabídku, pokud ji znáte. Například následující seznam je pouze krátkou ukázkou (mnoho seznamů je poměrně dlouhých), pokud víte, že „Canonical“ je vydavatelem nabídky „UbuntuServer“.

```azurecli
azure vm image list westus canonical ubuntuserver
info:    Executing command vm image list
warn:    The parameter --sku if specified will be ignored
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         Sku                OS     Version          Location  Urn
data:    ---------  ------------  -----------------  -----  ---------------  --------  --------------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS        Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607220  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607220
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607230  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607230
data:    canonical  ubuntuserver  16.10-DAILY        Linux  16.10.201607240  westus    canonical:ubuntuserver:16.10-DAILY:16.10.201607240
```

Sloupec **Urn** obsahuje formát, který předáte příkazu `azure vm quick-create`.

Často však nevíte, co je k dispozici. V takovém případě můžete image procházet pomocí příkazu `azure vm image list-publishers` a na vyzvání zadat umístění datového centra. Například následující příkaz zobrazí seznam všech vydavatelů imagí v umístění Západní USA (argument umístění předejte jako řetězec malých písmen bez mezer, který vytvoříte z názvu standardních umístění v angličtině, např. West US -> westus).

```azurecli
azure vm image list-publishers
info:    Executing command vm image list-publishers
Location: westus
+ Getting virtual machine and/or extension image publishers (Location: "westus")
data:    Publisher                                       Location
data:    ----------------------------------------------  --------
data:    a10networks                                     westus  
data:    aiscaler-cache-control-ddos-and-url-rewriting-  westus  
data:    alertlogic                                      westus  
data:    AlertLogic.Extension                            westus  
```

Tyto seznamy můžou být poměrně dlouhé, proto je předchozí příklad seznamu pouze fragmentem. Řekněme, že jste si všimli, že Canonical je opravdu vydavatelem imagí v umístění Západní USA. Nyní si můžete vyhledat jejich nabídky zavoláním příkazu `azure vm image list-offers` a na vyzvání předat umístění a vydavatele, jako v následujícím příkladu:

```azurecli
azure vm image list-offers
info:    Executing command vm image list-offers
Location: westus
Publisher: canonical
+ Getting virtual machine image offers (Publisher: "canonical" Location:"westus")
data:    Publisher  Offer                      Location
data:    ---------  -------------------------  --------
data:    canonical  Ubuntu15.04Snappy          westus
data:    canonical  Ubuntu15.04SnappyDocker    westus
data:    canonical  UbunturollingSnappy        westus
data:    canonical  UbuntuServer               westus
data:    canonical  Ubuntu_Snappy_Core         westus
data:    canonical  Ubuntu_Snappy_Core_Docker  westus
info:    vm image list-offers command OK
```

Teď víte, že v oblasti Západní USA vydavatel Canonical publikuje v Azure nabídku **UbuntuServer**. Ale co skladové jednotky (SKU)? Pro získání těchto hodnot zavoláte příkaz `azure vm image list-skus` a na výzvy odpovíte umístěním, vydavatelem a nabídkou, které jste zjistili.

```azurecli
azure vm image list-skus
info:    Executing command vm image list-skus
Location: westus
Publisher: canonical
Offer: ubuntuserver
+ Getting virtual machine image skus (Publisher:"canonical" Offer:"ubuntuserver" Location:"westus")
data:    Publisher  Offer         sku                Location
data:    ---------  ------------  -----------------  --------
data:    canonical  ubuntuserver  12.04.2-LTS        westus
data:    canonical  ubuntuserver  12.04.3-LTS        westus
data:    canonical  ubuntuserver  12.04.4-LTS        westus
data:    canonical  ubuntuserver  12.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  12.04.5-LTS        westus
data:    canonical  ubuntuserver  12.10              westus
data:    canonical  ubuntuserver  14.04-beta         westus
data:    canonical  ubuntuserver  14.04.0-LTS        westus
data:    canonical  ubuntuserver  14.04.1-LTS        westus
data:    canonical  ubuntuserver  14.04.2-LTS        westus
data:    canonical  ubuntuserver  14.04.3-LTS        westus
data:    canonical  ubuntuserver  14.04.4-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.4-LTS        westus
data:    canonical  ubuntuserver  14.04.5-DAILY-LTS  westus
data:    canonical  ubuntuserver  14.04.5-LTS        westus
data:    canonical  ubuntuserver  14.10              westus
data:    canonical  ubuntuserver  14.10-beta         westus
data:    canonical  ubuntuserver  14.10-DAILY        westus
data:    canonical  ubuntuserver  15.04              westus
data:    canonical  ubuntuserver  15.04-beta         westus
data:    canonical  ubuntuserver  15.04-DAILY        westus
data:    canonical  ubuntuserver  15.10              westus
data:    canonical  ubuntuserver  15.10-alpha        westus
data:    canonical  ubuntuserver  15.10-beta         westus
data:    canonical  ubuntuserver  15.10-DAILY        westus
data:    canonical  ubuntuserver  16.04-alpha        westus
data:    canonical  ubuntuserver  16.04-beta         westus
data:    canonical  ubuntuserver  16.04.0-DAILY-LTS  westus
data:    canonical  ubuntuserver  16.04.0-LTS        westus
data:    canonical  ubuntuserver  16.10-DAILY        westus
info:    vm image list-skus command OK
```

S těmito informacemi můžete najít přesně tu image, kterou hledáte, zavoláním původního příkazu v horní části.

```azurecli
azure vm image list westus canonical ubuntuserver 16.04.0-LTS
info:    Executing command vm image list
+ Getting virtual machine images (Publisher:"canonical" Offer:"ubuntuserver" Sku: "16.04.0-LTS" Location:"westus")
data:    Publisher  Offer         Sku          OS     Version          Location  Urn
data:    ---------  ------------  -----------  -----  ---------------  --------  --------------------------------------------------
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201604203  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201604203
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201605161  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201605161
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606100  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606100
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201606270  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201606270
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201607210  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201607210
data:    canonical  ubuntuserver  16.04.0-LTS  Linux  16.04.201608150  westus    canonical:ubuntuserver:16.04.0-LTS:16.04.201608150
info:    vm image list command OK
```

Nyní si můžete vybrat přesně tu image, kterou chcete použít. Pokud chcete rychle vytvořit virtuální počítač pomocí názvu URN, který jste právě zjistili, nebo pokud chcete použít šablonu s tímto názvem URN, přečtěte si téma věnované [použití Azure CLI pro Mac, Linux a Windows s Azure Resource Managerem](../articles/xplat-cli-azure-resource-manager.md).

## <a name="powershell"></a>PowerShell
> [!NOTE]
> Nainstalujte si a nakonfigurujte [nejnovější verzi Azure PowerShellu](/powershell/azureps-cmdlets-docs). Pokud používáte nižší verze modulů Azure PowerShellu než 1.0, pořád budete používat následující příkazy, ale nejprve je nutné provést příkaz `Switch-AzureMode AzureResourceManager`. 
> 
> 

Při vytváření nového virtuálního počítače pomocí Azure Resource Manageru je v některých případech nutné zadat image kombinací následujících vlastností image:

* Vydavatel
* Nabídka
* Skladová jednotka (SKU)

Tyto hodnoty jsou potřeba například pro rutinu PowerShellu `Set-AzureRMVMSourceImage` nebo v souboru šablony skupiny prostředků, kde je nutné zadat typ virtuálního počítače, který se má vytvořit.

Pokud potřebujete, můžete tyto hodnoty určit procházením imagí:

1. Vypsat vydavatele imagí.
2. Pro daného vydavatele vypsat jeho nabídky.
3. Pro danou nabídku vypsat její skladovou jednotku (SKU).

Nejprve vypište vydavatele pomocí následujících příkazů:

```powershell
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Vyplňte název zvoleného vydavatele a spusťte následující příkazy:

```powershell
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Vyplňte název zvolené nabídky a spusťte následující příkazy:

```powershell
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

V zobrazení příkazu `Get-AzureRMVMImageSku` najdete veškeré informace, které potřebujete k zadání image pro nový virtuální počítač.

Následuje úplný příklad:

```powershell
PS C:\> $locName="West US"
PS C:\> Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Pro vydavatele „MicrosoftWindowsServer“:

```powershell
PS C:\> $pubName="MicrosoftWindowsServer"
PS C:\> Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer

Offer
-----
WindowsServer
```

Pro nabídku „WindowsServer“:

```powershell
PS C:\> $offerName="WindowsServer"
PS C:\> Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus

Skus
----
2008-R2-SP1
2012-Datacenter
2012-R2-Datacenter
2016-Nano-Server-Technical-Previe
2016-Technical-Preview-with-Conta
Windows-Server-Technical-Preview
```

Z tohoto seznamu si zkopírujte název zvolené skladové jednotky (SKU) a máte veškeré informace pro rutinu PowerShellu `Set-AzureRMVMSourceImage` nebo pro šablonu skupiny prostředků.

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/

<!--HONumber=Feb17_HO3-->


