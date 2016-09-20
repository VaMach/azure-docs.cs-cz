[Knihovna správce konfigurace Microsoft Azure pro .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) poskytuje třídu pro potřeby analýzy připojovacího řetězce z konfiguračního souboru. [Třída CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analyzuje nastavení konfigurace bez ohledu na to, jestli klientská aplikace běží na počítači, na mobilním zařízení, na virtuálním počítači Azure nebo v cloudové službě Azure.

Pokud chcete odkázat na balíček CloudConfigurationManager, přidejte do své třídy tento příkaz `using`:

    using Microsoft.Azure;  //Namespace for CloudConfigurationManager

Tady je příklad, který ukazuje, jak z konfiguračního souboru získat připojovací řetězec:

    // Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

Použití nástroje Azure Configuration Manager není povinné. Taky můžete použít API jako třeba [třídu ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) platformy .NET Framework.



<!--HONumber=sep16_HO2-->


