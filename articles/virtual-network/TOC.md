# Přehled
## [Virtuální sítě](virtual-networks-overview.md)
## [Uživatelem definované trasy a předávání IP](virtual-networks-udr-overview.md)
## [Partnerské vztahy virtuálních sítí](virtual-network-peering-overview.md)
## [Kontinuita podnikových procesů](virtual-network-disaster-recovery-guidance.md)
## [Nejčastější dotazy](virtual-networks-faq.md)
## [Přidělování IP adres](virtual-network-ip-addresses-overview-arm.md)
## Classic
### [Přidělování IP adres](virtual-network-ip-addresses-overview-classic.md)
### [Seznamy řízení přístupu](virtual-networks-acl.md)

# Začínáme
## [Vytvoření první virtuální sítě](virtual-network-get-started-vnet-subnet.md)

# Postup
## Plánování a návrh
### [Virtuální sítě](virtual-network-vnet-plan-design-arm.md)
### [Skupiny zabezpečení sítě](virtual-networks-nsg.md)

## Nasazení
### [Virtuální sítě](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
#### [Šablona](virtual-networks-create-vnet-arm-template-click.md)

### Skupiny zabezpečení sítě
#### [Azure Portal](virtual-networks-create-nsg-arm-pportal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-networks-create-nsg-arm-cli.md)
#### [Šablona](virtual-networks-create-nsg-arm-template.md)
#### Classic
##### [PowerShell](virtual-networks-create-nsg-classic-ps.md)
##### [Rozhraní příkazového řádku](virtual-networks-create-nsg-classic-cli.md)

### Trasy definované uživatelem
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-network-create-udr-arm-cli.md)
#### [Šablona](virtual-network-create-udr-arm-template.md)
#### Classic
##### [PowerShell](virtual-network-create-udr-classic-ps.md)
##### [Rozhraní příkazového řádku](virtual-network-create-udr-classic-cli.md)

### Partnerské vztahy virtuálních sítí
#### [Stejný model nasazení, stejné předplatné](virtual-network-create-peering.md)
#### [Stejný model nasazení, různá předplatná](create-peering-different-subscriptions.md)
#### [Různé modely nasazení, stejné předplatné](create-peering-different-deployment-models.md)
#### [Různé modely nasazení, různá předplatná](create-peering-different-deployment-models-subscriptions.md)

### Virtuální počítače
#### Vytvoření virtuálního počítače se statickou veřejnou IP adresou
##### [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Rozhraní příkazového řádku](virtual-network-deploy-static-pip-arm-cli.md)
##### [Šablona](virtual-network-deploy-static-pip-arm-template.md)
##### Classic
###### [PowerShell](virtual-networks-reserved-public-ip.md)

#### Vytvoření virtuálního počítače se statickou privátní IP adresou
##### [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Rozhraní příkazového řádku](virtual-networks-static-private-ip-arm-cli.md)
##### Classic
###### [Azure Portal](virtual-networks-static-private-ip-classic-pportal.md)
###### [PowerShell](virtual-networks-static-private-ip-classic-ps.md)
###### [Rozhraní příkazového řádku](virtual-networks-static-private-ip-classic-cli.md)

#### Vytvoření virtuálního počítače s několika síťovými rozhraními
##### [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Rozhraní příkazového řádku](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### Classic
###### [PowerShell](virtual-network-deploy-multinic-classic-ps.md)
###### [Rozhraní příkazového řádku](virtual-network-deploy-multinic-classic-cli.md)

#### Vytvoření virtuálního počítače s několika IP adresami
##### [Azure Portal](virtual-network-multiple-ip-addresses-portal.md)
##### [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
##### [Rozhraní příkazového řádku](virtual-network-multiple-ip-addresses-cli.md)
##### [Šablona](virtual-network-multiple-ip-addresses-template.md)

#### [Vytvoření virtuálního počítače s urychlením sítě](virtual-network-create-vm-accelerated-networking.md)

### Scénáře připojení
#### [Virtuální síť k virtuální síti](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Virtuální síť (Resource Manager) k virtuální síti (Classic)](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Virtuální síť k místní síti (VPN)](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Virtuální síť k místní síti (ExpressRoute)](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Vysoce dostupná hybridní síťová architektura](../guidance/guidance-hybrid-network-expressroute-vpn-failover.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

### Scénáře zabezpečení
#### [Zabezpečení sítí pomocí virtuálních zařízení](virtual-network-scenario-udr-gw-nva.md)
#### [DMZ mezi Azure a internetem](../guidance/guidance-iaas-ra-secure-vnet-dmz.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
#### [Zabezpečení cloudové služby a sítě](../best-practices-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [Jednoduché DMZ pomocí skupin zabezpečení sítě](virtual-networks-dmz-nsg-asm.md)
##### [DMZ pomocí brány firewall a skupin zabezpečení sítě](virtual-networks-dmz-nsg-fw-asm.md)
##### [DMZ pomocí brány firewall, směrování definovaného uživatelem a skupin zabezpečení sítě](virtual-networks-dmz-nsg-fw-udr-asm.md)
##### [Ukázková aplikace](virtual-networks-sample-app.md)

### Classic
#### [Virtuální síť](create-virtual-network-classic.md)
##### [Azure Portal](virtual-networks-create-vnet-classic-pportal.md)
##### [PowerShell](virtual-networks-create-vnet-classic-netcfg-ps.md)
##### [Rozhraní příkazového řádku](virtual-networks-create-vnet-classic-cli.md)

## Konfigurace
### Virtuální počítače
#### [Přidání nebo odebrání síťových rozhraní](virtual-network-network-interface-vm.md)
#### [Překlad názvů pro virtuální počítače a cloudové služby](virtual-networks-name-resolution-for-vms-and-role-instances.md)
#### [Optimalizace propustnosti sítě](virtual-network-optimize-network-bandwidth.md)
#### [Zobrazení a úpravy názvů hostitelů](virtual-networks-viewing-and-modifying-hostnames.md)
### Classic
#### Seznamy řízení přístupu
##### [Azure Portal](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
##### [PowerShell](virtual-networks-acl-powershell.md)

## Spravovat
### [Virtuální sítě](virtual-network-manage-network.md)
#### [Podsítě](virtual-network-manage-subnet.md)
#### [Partnerské vztahy](virtual-network-manage-peering.md)
#### Classic
##### [Konfigurační soubory sítě](virtual-networks-using-network-configuration-file.md)
##### [Migrace ze skupiny vztahů do oblasti](virtual-networks-migrate-to-regional-vnet.md)
### Skupiny zabezpečení sítě
#### [Azure Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-network-manage-nsg-arm-cli.md)
#### [Protokoly](virtual-network-nsg-manage-log.md)
### Síťová rozhraní (NIC)
#### [Vytvoření, změna nebo odstranění síťových rozhraní](virtual-network-network-interface.md)
#### [Přidání, změna nebo odstranění IP adresy](virtual-network-network-interface-addresses.md)
### Virtuální počítače
#### [Přesun virtuálního počítače do jiné podsítě](virtual-networks-move-vm-role-to-subnet.md)
### [Veřejné IP adresy](virtual-network-public-ip-address.md)

## Řešení potíží
### Skupiny zabezpečení sítě
#### [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
#### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Trasy
#### [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### [Testování propustnosti](virtual-network-bandwidth-testing.md)
### [Nelze odstranit virtuální sítě](virtual-network-troubleshoot-cannot-delete-vnet.md)
### [Problémy propojení virtuálních počítačů](virtual-network-troubleshoot-connectivity-problem-between-vms.md)

# Referenční informace
## [Ukázky kódu](https://azure.microsoft.com/en-us/resources/samples/?service=virtual-network)
## [PowerShell (Resource Manager)](/powershell/module/azurerm.network)
## [PowerShell (Classic)](/powershell/module/azure/)
## [Azure CLI](/cli/azure/network)
## [Java](/java/api/)
## [REST (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [REST (Classic)](https://msdn.microsoft.com/library/jj157182.aspx)


# Související
## [Virtual Machines](/azure/virtual-machines/)
## [Application Gateway](/azure/application-gateway/)
## [Azure DNS](/azure/dns/)
## [Traffic Manager](/azure/traffic-manager/)
## [Load Balancer](/azure/load-balancer/)
## [VPN Gateway](/azure/vpn-gateway/)
## [ExpressRoute](/azure/expressroute/)

# Zdroje a prostředky
## [Plány Azure do budoucna](https://azure.microsoft.com/roadmap/?category=networking)
## [Blog o sítích](http://azure.microsoft.com/blog/topics/networking)
## [Fórum o sítích](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesVirtualNetwork)
## [Ceny](https://azure.microsoft.com/pricing/details/virtual-network)
## [ Cenová kalkulačka](https://azure.microsoft.com/pricing/calculator/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-network)
