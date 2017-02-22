
### <a name="start-your-powershell-session"></a>Spuštění relace prostředí PowerShell
Nejdříve byste měli nainstalovat a zprovoznit nejnovější [Azure PowerShell](https://msdn.microsoft.com/library/mt619274\(v=azure.300\).aspx). Podrobné informace najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Podpora řady nových funkcí služby SQL Database je dostupná jen v případě, že používáte [model nasazení Azure Resource Manageru](../articles/azure-resource-manager/resource-group-overview.md), takže se v příkladech používají [rutiny prostředí PowerShell Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) pro Resource Manager. [Rutiny služby Azure SQL Database](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) v modelu nasazení správy služby (klasické) podporují zpětnou kompatibilitu, ale přesto doporučujeme používat rutiny Resource Manageru.
> 
> 

Po spuštění rutiny [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) se zobrazí přihlašovací obrazovka, kam zadáte svoje přihlašovací údaje. Použijte stejné přihlašovací údaje, pomocí kterých se přihlašujete na portál Azure.

    Add-AzureRmAccount

Pokud máte více předplatných, pomocí rutiny [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) vyberte, které předplatné má relace prostředí PowerShell použít. Pokud chcete zjistit, jaké předplatné používá aktuální relace prostředí PowerShell, spusťte rutinu [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx). Pokud chcete zobrazit všechna předplatná, spusťte rutinu [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx).

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


<!--HONumber=Jan17_HO3-->


