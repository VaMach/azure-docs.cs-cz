## <a name="scenario"></a>Scénář
Tento dokument provede nasazení, které používá několik síťových adaptérů ve virtuálních počítačích v určité scénáře. V tomto scénáři máte Dvojúrovňová IaaS zatížení hostované v Azure. Každý vrstva je nasazená ve vlastní podsíti ve virtuální síti (VNet). Front-endu vrstvy se skládá z několika webových serverů, seskupeny ve službě Vyrovnávání zatížení nastavena pro zajištění vysoké dostupnosti. Úroveň back-end se skládá z několika databázových serverů. Nasadí se tyto databázové servery se dvěma síťovými adaptéry, každý, jeden pro přístup k databázi, druhý pro správu. Tento scénář zahrnuje taky skupin zabezpečení sítě (Nsg) pro přenosy, které mohou každé podsítě a síťový adaptér v nasazení. Následující obrázek ukazuje základní architekturu tohoto scénáře.  

![Scénář MultiNIC](./media/virtual-network-deploy-multinic-scenario-include/Figure1.png)

