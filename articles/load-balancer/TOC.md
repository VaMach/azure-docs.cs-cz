# [Dokumentace k nástroji pro vyrovnávání zatížení](index.md)

# Přehled
## [Co je nástroj pro vyrovnávání zatížení?](load-balancer-overview.md)
## [Co je Load Balancer Standard?](load-balancer-standard-overview.md)
## [Veřejný nástroj pro vyrovnávání zatížení](load-balancer-internet-overview.md)
## [Interní nástroj pro vyrovnávání zatížení.](load-balancer-internal-overview.md)
## [Porozumění testům nástroje pro vyrovnávání zatížení](load-balancer-custom-probe-overview.md)
## [Principy portů s vysokou dostupnost](load-balancer-ha-ports-overview.md)
## [Podpora Azure Resource Manageru](load-balancer-arm.md)
## [Podpora protokolu IPv6](load-balancer-ipv6-overview.md)
## [Více virtuálních IP adres](load-balancer-multivip-overview.md)
## [Principy odchozích připojení](load-balancer-outbound-connections.md)

# Začínáme

## [Konfigurace interního nástroje pro vyrovnávání zatížení](load-balancer-get-started-ilb-arm-portal.md)
### [Konfigurace interního nástroje pro vyrovnávání zatížení (PowerShell)](load-balancer-get-started-ilb-arm-ps.md)
### [Konfigurace interního nástroje pro vyrovnávání zatížení (rozhraní příkazového řádku)](load-balancer-get-started-ilb-arm-cli.md)
### [Konfigurace interního nástroje pro vyrovnávání zatížení (šablona)](load-balancer-get-started-ilb-arm-template.md)

## [Konfigurace veřejného nástroje pro vyrovnávání zatížení](load-balancer-get-started-internet-portal.md)
### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (PowerShell)](load-balancer-get-started-internet-arm-ps.md)
### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (rozhraní příkazového řádku)](load-balancer-get-started-internet-arm-cli.md)
### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (šablona)](load-balancer-get-started-internet-arm-template.md)

## [Vytvoření veřejného nástroje pro vyrovnávání zatížení s využitím IPv6](load-balancer-ipv6-internet-ps.md)
### [Vytvoření veřejného nástroje pro vyrovnávání zatížení s využitím IPv6 (rozhraní příkazového řádku)](load-balancer-ipv6-internet-cli.md)
### [Vytvoření veřejného nástroje pro vyrovnávání zatížení s využitím IPv6 (šablona)](load-balancer-ipv6-internet-template.md)

## [Vytvoření zónově redundantní veřejné služby Load Balancer Standard](load-balancer-get-started-internet-az-portal.md)
### [Vytvoření zónově redundantní veřejné služby Load Balancer Standard (PowerShell)](load-balancer-get-started-internet-az-powershell.md)
### [Vytvoření zónově redundantní veřejné služby Load Balancer Standard (rozhraní příkazového řádku)](load-balancer-get-started-internet-az-cli.md)

# Postup
## [Konfigurace časového limitu nečinnosti protokolu TCP nástroje Load Balancer](load-balancer-tcp-idle-timeout.md)
## [Konfigurace distribučního režimu nástroje Load Balancer](load-balancer-distribution-mode.md)
## [Konfigurace interního nástroje pro SQL AlwaysOn](load-balancer-configure-sqlao.md)
## [Konfigurace více virtuálních IP adres cloudové služby](load-balancer-multivip.md)
## [Kombinace služeb pro vyrovnávání zatížení](../traffic-manager/traffic-manager-load-balancing-azure.md?toc=%2fazure%2fload-balancer%2ftoc.json)
## [Použití více konfigurací IP](load-balancer-multiple-ip.md)
### [Použití několika konfigurací IP (CLI)](load-balancer-multiple-ip-cli.md)
### [Použití několika konfigurací IP (PowerShell)](load-balancer-multiple-ip-powershell.md)
## [Log Analytics pro Azure Load Balancer](load-balancer-monitor-log.md)
## [Konfigurace protokolu DHCPv6 pro virtuální počítače s Linuxem](load-balancer-ipv6-for-linux.md)
## [Konfigurace portů s vysokou dostupností pro interní nástroj pro vyrovnávání zatížení](load-balancer-configure-ha-ports.md)

## Řešení potíží
### [Řešení potíží s nástrojem pro vyrovnávání zatížení Azure](load-balancer-troubleshoot.md)

## Články věnované modelu nasazení Classic
### [Konfigurace interního nástroje pro vyrovnávání zatížení pro Cloud Services](load-balancer-get-started-ilb-classic-cloud.md)
#### [Konfigurace interního nástroje pro vyrovnávání zatížení pro Cloud Services (PowerShell)](load-balancer-get-started-ilb-classic-ps.md)
#### [Konfigurace interního nástroje pro vyrovnávání zatížení pro Cloud Services (rozhraní příkazového řádku)](load-balancer-get-started-ilb-classic-cli.md)
### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (PowerShell Classic)](load-balancer-get-started-internet-classic-ps.md)
#### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (cloud Classic)](load-balancer-get-started-internet-classic-cloud.md)
#### [Konfigurace veřejného nástroje pro vyrovnávání zatížení (rozhraní příkazového řádku Classic)](load-balancer-get-started-internet-classic-cli.md)

# Referenční informace
## [Ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=load-balancer)
## [Azure PowerShell](/powershell/module/azurerm.network)
## [Azure CLI](/cli/azure/network/lb)
## [.NET](/dotnet/api/microsoft.azure.management.network.models)
## [Java](/java/api/com.microsoft.azure.management.network)
## [Node.js](http://azure.github.io/azure-sdk-for-node/azure-arm-network/latest/LoadBalancers.html)
## [Ruby](http://www.rubydoc.info/gems/azure_mgmt_network/Azure/ARM/Network/LoadBalancers)
## [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.mgmt.network.operations.html#azure.mgmt.network.operations.LoadBalancersOperations)
## [REST](https://msdn.microsoft.com/library/azure/mt163651.aspx)

# Související
## [Application Gateway](/azure/application-gateway/)
## [Express Route](/azure/expressroute/)
## [Virtual Network](/azure/virtual-network/)
## [VPN Gateway](/azure/vpn-gateway/)
## [Virtuální počítač](/azure/virtual-machines/)
## [Traffic Manager](/azure/traffic-manager/)
## [DNS](/azure/dns/)

# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/?category=networking)
## [Ceny](https://azure.microsoft.com/pricing/details/load-balancer/)
## [ Cenová kalkulačka](https://azure.microsoft.com/pricing/calculator/)
## [Aktualizace služeb](https://azure.microsoft.com/updates/?product=load-balancer)
