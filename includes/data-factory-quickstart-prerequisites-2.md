### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-powershell"></a>Instalace PowerShellu
Pokud jej ve svém počítači nemáte, nainstalujte nejnovější PowerShell. 

1. Ve webovém prohlížeči přejděte na stránku [Položky ke stažení Azure](https://azure.microsoft.com/downloads/). 
2. V části **Nástroje příkazového řádku** -> **PowerShell** klikněte na **Instalace pro Windows**. 
3. Pokud chcete nainstalovat PowerShell, spusťte soubor **MSI**. 

Podrobné pokyny najdete v tématu [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/install-azurerm-ps). 

#### <a name="log-in-to-powershell"></a>Přihlášení do PowerShellu

1. Spusťte na svém počítači **PowerShell**. Nechte PowerShell otevřený až do konce tohoto rychlého startu. Pokud ho zavřete a znovu otevřete, tyto příkazy bude potřeba znovu spustit.

    ![Spuštění PowerShellu](media/data-factory-quickstart-prerequisites-2/search-powershell.png)
1. Spusťte následující příkaz a zadejte uživatelské jméno a heslo Azure, které používáte k přihlášení na Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Pokud se zobrazí několik předplatných přidružených vašem účtu, spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat. Místo **SubscriptionId** použijte ID vašeho předplatného Azure:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
