# Přehled
## [O virtuálních počítačích](../../virtual-machines-windows-about.md)
## [Disky a virtuální pevné disky (VHD)](../../../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
## [Virtuální sítě](../../../virtual-network/virtual-networks-overview.md)
## [Nejčastější dotazy](faq.md)
## [Porovnání virtuálních počítačů, webů a cloudových služeb](../../../app-service-web/choose-web-site-cloud-service-vm.md)
## [Kontejnery](../../virtual-machines-windows-containers.md)

# Začínáme
## [Vytvoření virtuálního počítače pomocí portálu](tutorial.md)
## [Přihlášení k virtuálnímu počítači](connect-logon.md)
## [Instalace Azure PowerShellu](/powershell/azureps-cmdlets-docs)
## [Instalace rozhraní příkazového řádku Azure](../../../cli-install-nodejs.md)

# Postup

## Použití služby Storage
### [Připojení datového disku](attach-disk.md)
### [Odpojení datového disku](detach-disk.md)
### [Použití disku D: jako datového disku](../../virtual-machines-windows-change-drive-letter.md)

## Síť
### [Jak nastavit koncové body](setup-endpoints.md)
### [Propojení virtuálních počítačů pomocí virtuální sítě nebo cloudové služby](connect-vms.md)
### [Připojení virtuálních sítí Classic k virtuálním sítím Resource Manageru](../../../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
### [Vytvoření nástroje pro vyrovnávání zatížení](../../../load-balancer/load-balancer-get-started-internet-classic-portal.md)
### [Správa skupin zabezpečení sítě přes Azure PowerShell](../../../virtual-network/virtual-networks-create-nsg-classic-ps.md)

## Nasazení
### [Vytvoření vlastního virtuálního počítače](createportal.md)
### [Vytvoření a konfigurace virtuálního počítače přes Azure PowerShell](create-powershell.md)
### [Zachycení virtuálního počítače s Windows](capture-image.md)
### [Vytvoření a nahrání virtuálního pevného disku přes PowerShell](createupload-vhd.md)
### [Automatizace nasazení virtuálních počítačů Azure pomocí Chefu](../../virtual-machines-windows-chef-automation.md)
### [Vytvoření a správa virtuálních počítačů v sadě Visual Studio](manage-visual-studio.md)
### [Vytvoření virtuálního počítače pro webovou aplikaci v sadě Visual Studio](web-app-visual-studio.md)
### [Spouštění úloh náročných na výpočetní výkon v Javě](java-run-compute-intensive-task.md)
### [Webová aplikace Hello World v Djangu](python-django-web-app.md)

## Konfigurace
### [Obnovení hesla služby Vzdálená plocha](../../virtual-machines-windows-reset-rdp.md)
### [Instalace a konfigurace nástroje Symantec Endpoint Protection](install-symantec.md)
### [Instalace a konfigurace nástroje Trend Micro Deep Security jako služby](install-trend.md)
### [Konfigurace skupiny dostupnosti](configure-availability.md)
### [Změna velikosti virtuálního počítače s Windows vytvořeného v klasickém modelu nasazení](resize-vm.md)

## Spravovat
### [Migrace z Classicu na Resource Manager](../../virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
### [Správa virtuálních počítačů přes Azure PowerShell](manage-psh.md)
### [O agentech a rozšířeních virtuálních počítačů](agents-and-extensions.md)
### [Správa rozšíření virtuálních počítačů](manage-extensions.md)
### [Rozšíření vlastních skriptů pro virtuální počítače](extensions-customscript.md)
### [Vkládání vlastních dat do virtuálního počítače Azure](inject-custom-data.md)

## Plánování
### [O imagích](about-images.md)
### [Velikosti pro virtuální počítače](../../virtual-machines-windows-sizes.md)
#### [O virtuálních počítačích řady A-series pro náročné výpočty a řady H-series](../../virtual-machines-windows-a8-a9-a10-a11-specs.md)
### [Plánovaná údržba pro virtuální počítače Azure](../../virtual-machines-windows-planned-maintenance.md)
### [Pokyny pro implementaci služeb infrastruktury Azure](../../virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md)

## Správa úloh
### [Vysokovýkonné výpočetní prostředí (HPC)](../../virtual-machines-windows-hpcpack-cluster-options.md)
#### [Automatické škálování prostředků](hpcpack-cluster-node-autogrowshrink.md)
#### [Správa výpočetních uzlů](hpcpack-cluster-node-manage.md)
#### [Vytvoření clusteru](hpcpack-cluster-powershell-script.md)
#### [Nastavení clusteru pro spouštění aplikací MPI](hpcpack-rdma-cluster.md)
#### [Spuštění úloh aplikace Excel a SOA](../../virtual-machines-windows-excel-cluster-hpcpack.md)
#### [Vytvoření hlavního uzlu s imagí Marketplace](../../virtual-machines-windows-hpcpack-cluster-headnode.md)
#### [Odeslání úloh z místního prostředí do Azure](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md)
### [MongoDB](install-mongodb.md)
### [MySQL](mysql-2008r2.md)
### [Oracle](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html#support)
####[Konfigurace Oracle Data Guard pro Azure](configure-oracle-data-guard.md)
####[Konfigurace Oracle GoldenGate pro Azure](configure-oracle-goldengate.md)
####[Různé důležité informace pro image](oracle-considerations.md)
####[Seznam imagí virtuálních počítačů Oracle](oracle-images.md)
### [SAP](sap-get-started.md)
### [SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
### [Tomcat](java-run-tomcat-app-server.md)

## Řešení potíží
### [Připojení Vzdálené plochy](../../virtual-machines-windows-troubleshoot-rdp-connection.md)
####[Podrobné kroky pro řešení problémů s připojením ke vzdálené ploše](../../virtual-machines-windows-detailed-troubleshoot-rdp.md)
### [Přístup k aplikaci](../../virtual-machines-windows-troubleshoot-app-connection.md)
### [Potíže s vytvářením nového virtuálního počítače v modelu nasazení Classic](troubleshoot-deployment-new-vm.md)
### [Potíže s restartováním nebo změnou velikosti stávajícího virtuálního počítače v modelu nasazení Classic](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
## [Resetování hesla protokolu RDP](reset-rdp.md)

# Referenční informace
## [PowerShell](/powershell/azureps-cmdlets-docs)
## [Azure CLI](/cli/azure/vm)
## [Java](/java/api)
## [.NET](/dotnet/api/microsoft.azure.management.compute)
## [Tvorba šablon Resource Manageru](../../../resource-group-authoring-templates.md)
## [Komunitní šablony](https://azure.microsoft.com/documentation/templates)
## [Rozhraní REST pro Compute](/rest/api/compute)
## [Rozhraní REST pro službu Network](/rest/api)
## [Rozhraní REST pro Storage](/rest/api/storageservices)

# Zdroje a prostředky
## [Ceny](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)
## [Regionální dostupnost](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-machine)
## [Videa](https://azure.microsoft.com/documentation/videos/index/?services=virtual-machines)
