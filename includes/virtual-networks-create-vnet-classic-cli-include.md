## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Postup vytvoření klasické virtuální sítě pomocí rozhraní příkazového řádku Azure
Pomocí rozhraní příkazového řádku Azure můžete spravovat svoje prostředky Azure z příkazového řádku jakéhokoliv počítače se systémem Windows, Linux a OS X. Pokud chcete vytvořit síť VNet pomocí rozhraní příkazového řádku Azure, postupujte následujícím způsobem.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, přejděte na téma [Instalace a konfigurace rozhraní příkazového řádku Azure](../articles/cli-install-nodejs.md) a postupujte podle pokynů až do chvíle, kdy můžete vybrat svůj účet a předplatné Azure.
2. Spuštěním příkazu **azure network vnet create** vytvořte síť VNet a podsíť, jak vidíte níže. Seznam uvedený za výstupem vysvětluje použité parametry.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Očekávaný výstup:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Název sítě VNet, která se má vytvořit. V našem scénáři je to *TestVNet*.
   * **-e (nebo--adresní prostor)**. Adresní prostor sítě VNet. Pro náš scénář *192.168.0.0*
   * **-i (nebo - cidr)**. Maska sítě ve formátu CIDR. Pro náš scénář *16*.
   * **-n (nebo--název podsítě**). Název první podsíť. V našem scénáři je to *FrontEnd*.
   * **-p (nebo--IP adresu podsítě start)**. Počáteční IP adresa pro podsíť nebo adresního prostoru podsítě. Pro náš scénář *192.168.1.0*.
   * **-r (nebo--podsítě cidr)**. Maska sítě ve formátu CIDR podsítě. Pro náš scénář *24*.
   * **-l (nebo --location)**. Oblast Azure, ve které bude síť VNet vytvořena. Pro náš scénář *střed USA*.
3. Spuštěním příkazu **azure network vnet subnet create** vytvořte podsíť, jak vidíte níže. Seznam uvedený za výstupem vysvětluje použité parametry.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (nebo--vnet-name**. Název sítě VNet, ve které se vytvoří podsíť. V našem scénáři je to *TestVNet*.
   * **-n (nebo --name)**. Název nové podsítě. Pro náš scénář *back-end*.
   * **-a (nebo --address-prefixes)**. Blok CIDR podsítě. Čtyři našem scénáři *192.168.2.0/24*.
4. Spuštěním příkazu **azure network vnet show** zobrazíte vlastnosti nové sítě VNet, jak vidíte níže.
   
            azure network vnet show
   
    Toto je očekávaný výstup výše uvedeného příkazu:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

