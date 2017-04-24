Kvůli připojení k instanci Azure Redis Cache potřebují klienti mezipaměti název hostitele, porty a klíče této mezipaměti. Někteří klienti můžou tyto položky označovat trochu odlišnými názvy. Tyto informace můžete načíst na webu Azure Portal nebo pomocí nástrojů příkazového řádku, jako je Azure CLI.

### <a name="retrieve-host-name-ports-and-access-keys-using-the-azure-portal"></a>Načtení názvu hostitele, portů a přístupových klíčů pomocí webu Azure Portal
Chcete-li načíst název hostitele, porty a přístupové klíče pomocí webu Azure Portal, [přejděte](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) k mezipaměti na webu [Azure Portal](https://portal.azure.com) a klikněte na **Přístupové klíče** a **Vlastnosti** v nabídce **Prostředek**. 

![Nastavení mezipaměti Redis](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Načtení názvu hostitele, portů a přístupových klíčů pomocí Azure CLI
Chcete-li načíst název hostitele a porty pomocí Azure CLI 2.0, můžete zavolat příkaz [az redis show](https://docs.microsoft.com/cli/azure/redis#show). K načtení klíčů můžete zavolat příkaz [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys). Následující skript volá tyto dva příkazy a vypíše název hostitele, porty a klíče na konzolu.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Další informace o tomto skriptu najdete v tématu o [získání názvu hostitele, portů a klíčů pro Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md). Další informace o Azure CLI 2.0 najdete v tématech [Instalace Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) a [Začínáme s Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
