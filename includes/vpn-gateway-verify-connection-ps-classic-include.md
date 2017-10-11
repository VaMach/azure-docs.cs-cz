Můžete ověřit, že bylo připojení úspěšně pomocí rutiny 'Get-AzureVNetConnection'.

1. Použijte následující příklad rutiny a nakonfigurujte hodnoty tak, aby odpovídaly vašemu prostředí. Název virtuální sítě musí být v uvozovkách, pokud obsahuje mezery.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Po dokončení zpracování rutiny si prohlédněte hodnoty. V následujícím příkladu zobrazuje stav připojení jako "Připojena" a vy vidíte příchozí a Odchozí bajty.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal