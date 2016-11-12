# Přehled
## [Virtuální sítě](virtual-networks-overview.md)
## [Skupiny zabezpečení sítě](virtual-networks-nsg.md)
## [Uživatelem definované trasy a předávání IP](virtual-networks-udr-overview.md)
## Přidělování IP adres
### [Resource Manager](virtual-network-ip-addresses-overview-arm.md)
### [Classic](virtual-network-ip-addresses-overview-classic.md)
## [Partnerské vztahy virtuálních sítí](virtual-network-peering-overview.md)
## Virtuální počítače
### [Síťová rozhraní](virtual-network-network-interface-overview.md)
### [Překlad adres IP](virtual-networks-name-resolution-for-vms-and-role-instances.md)
## [Kontinuita podnikových procesů](virtual-network-disaster-recovery-guidance.md)
## [Ceny](https://azure.microsoft.com/pricing/details/virtual-network)

# Začínáme
## [Vytvoření virtuální sítě](virtual-networks-create-vnet-arm-pportal.md)
## [Nasazení virtuálního počítače do virtuální sítě](../virtual-machines/virtual-machines-windows-hero-tutorial.md)

# Postup
## Plánování a návrh
### [Virtuální sítě](virtual-network-vnet-plan-design-arm.md)
### [Skupiny zabezpečení sítě](virtual-networks-nsg.md)

## Nasazení
### Virtuální sítě
#### [Azure Portal](virtual-networks-create-vnet-arm-pportal.md)
#### [PowerShell](virtual-networks-create-vnet-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-networks-create-vnet-arm-cli.md)
#### [Šablona](virtual-networks-create-vnet-arm-template-click.md)
#### [Portál (Classic)](virtual-networks-create-vnet-classic-pportal.md)
#### [PowerShell (Classic)](virtual-networks-create-vnet-classic-netcfg-ps.md)
#### [Rozhraní příkazového řádku (Classic)](virtual-networks-create-vnet-classic-cli.md)

### Skupiny zabezpečení sítě
#### [Azure Portal](virtual-networks-create-nsg-arm-portal.md)
#### [PowerShell](virtual-networks-create-nsg-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-networks-create-nsg-arm-cli.md)
#### [Šablona](virtual-networks-create-nsg-arm-template.md)
#### [PowerShell (Classic)](virtual-networks-create-nsg-classic-ps.md)
#### [Rozhraní příkazového řádku (Classic)](virtual-networks-create-nsg-classic-cli.md)

### Trasy definované uživatelem
#### [PowerShell](virtual-network-create-udr-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-network-create-udr-arm-cli.md)
#### [Šablona](virtual-network-create-udr-arm-template.md)
#### [PowerShell (Classic)](virtual-network-create-udr-classic-ps.md)
#### [Rozhraní příkazového řádku (Classic)](virtual-network-create-udr-classic-cli.md)

### Partnerské vztahy virtuálních sítí
#### [Azure Portal](virtual-networks-create-vnetpeering-arm-portal.md)
#### [PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
#### [Šablona](virtual-networks-create-vnetpeering-arm-template-click.md)

### Virtuální počítače

#### Statické veřejné IP adresy
##### [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
##### [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
##### [Rozhraní příkazového řádku](virtual-network-deploy-static-pip-arm-cli.md)
##### [Šablona](virtual-network-deploy-static-pip-arm-template.md)
##### [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

#### Statické privátní IP adresy
##### [Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)
##### [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
##### [Rozhraní příkazového řádku](virtual-networks-static-private-ip-arm-cli.md)
##### [Portál (Classic)](virtual-networks-static-private-ip-classic-pportal.md)
##### [PowerShell (Classic)](virtual-networks-static-private-ip-classic-ps.md)
##### [Rozhraní příkazového řádku (Classic)](virtual-networks-static-private-ip-classic-cli.md)

#### Několik síťových rozhraní
##### [PowerShell](virtual-network-deploy-multinic-arm-ps.md)
##### [Rozhraní příkazového řádku](virtual-network-deploy-multinic-arm-cli.md)
##### [Šablona](virtual-network-deploy-multinic-arm-template.md)
##### [PowerShell (Classic)](virtual-network-deploy-multinic-classic-ps.md)
##### [Rozhraní příkazového řádku (Classic)](virtual-network-deploy-multinic-classic-cli.md)

#### [Několik IP adres](virtual-network-multiple-ip-addresses-powershell.md)

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

## Spravovat
### Skupiny zabezpečení sítě
#### [Azure Portal](virtual-network-manage-nsg-arm-portal.md)
#### [PowerShell](virtual-network-manage-nsg-arm-ps.md)
#### [Rozhraní příkazového řádku](virtual-network-manage-nsg-arm-cli.md)
#### [Protokoly](virtual-network-nsg-manage-log.md)
#### Řešení potíží
##### [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
##### [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
### Řešení potíží s trasami
#### [Azure Portal](virtual-network-routes-troubleshoot-portal.md)
#### [PowerShell](virtual-network-routes-troubleshoot-powershell.md)
### Virtuální počítače
#### [Zobrazení a úpravy názvů hostitelů](virtual-networks-viewing-and-modifying-hostnames.md)
#### [Přesun virtuálního počítače do jiné podsítě](virtual-networks-move-vm-role-to-subnet.md)

# Referenční informace
## [Rutiny PowerShellu (Resource Manager)](https://msdn.microsoft.com/library/mt163510(v=azure.300))
## [Rutiny PowerShellu (Classic)](https://msdn.microsoft.com/library/mt270335(v=azure.300))
## [Rozhraní API (Resource Manager)](https://msdn.microsoft.com/library/mt163658.aspx)
## [Rozhraní API (Classic)](https://msdn.microsoft.com/library/jj157182.aspx)
## [Blog o sítích](http://azure.microsoft.com/blog/topics/networking)
## [Nejčastější dotazy](virtual-networks-faq.md)

# Související
## [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines)
## [Application Gateway](https://azure.microsoft.com/documentation/services/application-gateway)
## [Azure DNS](https://azure.microsoft.com/documentation/services/dns)
## [Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager)
## [Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer)
## [VPN Gateway](https://azure.microsoft.com/documentation/services/vpn-gateway)
## [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute)



<!--HONumber=Nov16_HO2-->


