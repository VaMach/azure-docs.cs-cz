Pomocí následujícího příkazu rozhraní příkazového řádku můžete ověřit, že vaše připojení bylo úspěšné. Nakonfigurujte hodnoty tak, aby odpovídaly vašemu prostředí. V příkladu odkazuje -n na název připojení, které jste vytvořili a chcete ho testovat.

```azurecli
az network vpn-connection show -n VNet1toSite2 -g TestRG1
```

Pokud navazování připojení stále probíhá, zobrazí se stav připojení Connecting (Připojování). Jakmile bude připojení navázáno, stav se změní na Connected (Připojeno) a vy vidíte příchozí a odchozí bajty.