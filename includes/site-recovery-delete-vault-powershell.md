## <a name="delete-a-recovery-services-vault-powershell"></a>Odstranění trezoru Recovery Services (PowerShell)

1. Získání trezoru Recovery Services

        $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"

2. Odstranění trezoru

        Remove-AzureRmRecoveryServicesVault -Vault $vault

>[!WARNING]
>
> Výše uvedený příkaz používejte s nejvyšší obezřetností, protože když odstraníte trezor omylem, ztratíte veškerá data. Jedná se o trvalou akci a neexistuje způsob, jak ji vrátit.  




<!--HONumber=Feb17_HO3-->


