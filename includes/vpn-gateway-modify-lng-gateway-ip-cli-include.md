### <a name="to-modify-the-local-network-gateway-gatewayipaddress"></a>Úprava IP adresy brány místní sítě (gatewayIpAddress)

Pokud zařízení VPN, ke kterému se chcete připojit, změnilo svou veřejnou IP adresu, musíte upravit bránu místní sítě, aby odrážela tuto změnu. IP adresu brány je možné změnit bez odebrání existujícího připojení brány VPN (pokud nějaké máte). Pokud chcete upravit IP adresu brány, nahraďte hodnoty „Site2“ a „TestRG1“ vlastními hodnotami pomocí příkazu [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway#update).

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 --name Site2 --resource-group TestRG1
```

Na výstupu ověřte správnost IP adresy:

```
"gatewayIpAddress": "23.99.222.170",
```