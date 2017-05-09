Pomocí příkazu [az network vpn-connection show](/cli/azure/network/vpn-connection#show) můžete ověřit, že vaše připojení bylo úspěšné. Nakonfigurujte hodnoty tak, aby odpovídaly vašemu prostředí. V příkladu odkazuje --name na název připojení, které jste vytvořili a chcete ho testovat.

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

Pokud navazování připojení stále probíhá, zobrazí se stav připojení Connecting (Připojování). Jakmile bude připojení navázáno, stav se změní na Connected (Připojeno) a vy vidíte příchozí a odchozí bajty.