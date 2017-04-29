## <a name="scenario"></a>Scénář
V tomto dokumentu budeme používat následující scénář, abychom vám lépe předvedli, jak vytvořit síť VNet a podsítě.

![Scénář sítě VNet](./media/virtual-networks-create-vnet-scenario-include/vnet-scenario.png)

V tomto scénáři vytvoříte virtuální síť VNet s názvem **TestVNet** s vyhrazeným blokem CIDR **192.168.0.0./16**. Vaše síť VNet bude obsahovat tyto podsítě: 

* **FrontEnd** s blokem CIDR **192.168.1.0/24**
* **BackEnd** s blokem CIDR **192.168.2.0/24**

