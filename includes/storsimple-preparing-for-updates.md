<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Příprava pro aktualizace
Budete muset před vyhledání a použít aktualizaci provést následující kroky:

1. Pořízení snímku cloudu dat zařízení.
2. Zajistěte, aby vaše pevné IP adresy řadiče jsou směrovatelné a může připojit k Internetu. Tyto pevné IP adresy se použije k servis aktualizací na vaše zařízení. Toto můžete otestovat spuštěním následující rutiny v každém řadiči z rozhraní Windows PowerShell zařízení:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Ukázkový výstup pro Test-Connection, když pevné IP adresy můžete připojit k Internetu**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Jakmile úspěšně dokončíte tyto ruční předběžné kontroly, můžete pokračovat kontrolovat a instalovat aktualizace.

