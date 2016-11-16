Pokud jste tak ještě neučinili, můžete získat [bezplatnou zkušební verzi předplatného Azure](https://azure.microsoft.com/pricing/free-trial/) a [rozhraní příkazového řádku Azure](../articles/xplat-cli-install.md) [připojené k vašemu účtu Azure](../articles/xplat-cli-connect.md). Jakmile to uděláte, můžete spuštěním následujících příkazů rychle vytvořit sadu škálování:

```bash
# make sure we are in Resource Manager mode (https://azure.microsoft.com/en-us/documentation/articles/resource-manager-deployment-model/)
azure config mode arm

# quick-create a scale set
#
# generic syntax:
# azure vmss quick-create -n SCALE-SET-NAME -g RESOURCE-GROUP-NAME -l LOCATION -u USERNAME -p PASSWORD -C INSTANCE-COUNT -Q IMAGE-URN
#
# example:
azure vmss quick-create -n negatvmss -g negatvmssrg -l westus -u negat -p P4ssw0rd -C 5 -Q Canonical:UbuntuServer:14.04.4-LTS:latest
```

Chcete-li přizpůsobit umístění nebo IMAGE-URN, podívejte se na příkazy `azure location list` a `azure vm image {list-publishers|list-offers|list-skus|list|show}`.

Po vrácení tohoto příkazu již bude vytvořena sada škálování. Tato sada škálování bude obsahovat nástroj pro vyrovnávání zatížení s pravidly překladu adres (NAT) mapujícími port 50 000+i v tomto nástroji pro vyrovnávání zatížení na port 22 ve virtuálním počítači s číslem i. Díky tomu se po zjištění plně kvalifikovaného názvu domény nástroje pro vyrovnávání zatížení budeme moci připojit k virtuálním počítačům přes SSH:

```bash
# (if you decide to run this as a script, please invoke using bash)

# list load balancers in the resource group we created
#
# generic syntax:
# azure network lb list -g RESOURCE-GROUP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb list -g negatvmssrg | grep negatvmssrg)
split_line=( $line )
lb_name=${split_line[1]}

# now that we have the name of the load balancer, we can show the details to find which Public IP (PIP) is associated to it
#
# generic syntax:
# azure network lb show -g RESOURCE-GROUP-NAME -n LOAD-BALANCER-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network lb show -g negatvmssrg -n $lb_name | grep loadBalancerFrontEnd)
split_line=( $line )
pip_name=${split_line[4]}

# now that we have the name of the public IP address, we can show the details to find the FQDN
#
# generic syntax:
# azure network public-ip show -g RESOURCE-GROUP-NAME -n PIP-NAME
#
# example with some quick-and-dirty grep-fu to store the result in a variable:
line=$(azure network public-ip show -g negatvmssrg -n $pip_name | grep FQDN)
split_line=( $line )
FQDN=${split_line[3]}

# now that we have the FQDN, we can use ssh on port 50,000+i to connect to VM i (where i is 0-indexed)
#
# example to connct via ssh into VM "0":
ssh -p 50000 negat@$FQDN
```

<!--HONumber=Nov16_HO2-->


