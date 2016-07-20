
## Spuštění relace prostředí PowerShell

Nejdřív potřebujete mít nainstalované a spuštěné prostředí [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) (1.0 nebo novější). Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](../articles/powershell-install-configure.md).


>[AZURE.NOTE] Podpora řady nových funkcí služby SQL Database je dostupná jenom v [modelu nasazení Azure Resource Manager](../articles/resource-group-overview.md), takže se v příkladech používají [rutiny prostředí PowerShell Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) pro Resource Manager. [Rutiny služby Azure SQL Database (klasické)](https://msdn.microsoft.com/library/azure/dn546723.aspx) ve stávajícím klasickém modelu nasazení nabízí podporu zpětné kompatibility, ale přesto vám doporučujeme používat rutiny Resource Manageru. 


Spusťte rutinu [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/mt619267.aspx). Zobrazí se přihlašovací obrazovka, na které můžete zadat své přihlašovací údaje. Použijte stejné přihlašovací údaje, pomocí kterých se přihlašujete na portál Azure.

    Add-AzureRmAccount

Pokud máte víc předplatných, pomocí rutiny [**Set-AzureRmContext**](https://msdn.microsoft.com/library/mt619263.aspx) vyberte, které předplatné má relace prostředí PowerShell použít. Pokud chcete zjistit, jaké předplatné používá aktuální relace prostředí PowerShell, spusťte rutinu [**Get-AzureRmContext**](https://msdn.microsoft.com/library/mt619265.aspx). Pokud chcete zobrazit všechna předplatná, spusťte rutinu [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/mt619284.aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'




<!--HONumber=Jun16_HO2-->


