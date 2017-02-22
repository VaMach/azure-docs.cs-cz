> [!div class="op_single_selector"]
> * [Azure Portal](../articles/virtual-network/virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](../articles/virtual-network/virtual-network-multiple-ip-addresses-powershell.md)
> * [Rozhraní příkazového řádku](../articles/virtual-network/virtual-network-multiple-ip-addresses-cli.md)
> * [Šablona](../articles/virtual-network/virtual-network-multiple-ip-addresses-template.md)
>

Virtuální počítač (VM) Azure má připojené jedno nebo několik síťových rozhraní (NIC). Ke každému síťovému rozhraní může být přiřazená jedna nebo několik statických nebo dynamických veřejných a privátních IP adres. Možnost přiřadit virtuálnímu počítači několik IP adres umožňuje:

* Hostovat několik webů nebo služeb s různými IP adresami a certifikáty SSL na jednom serveru.
* Využívat počítač jako virtuální síťové zařízení, jako je třeba brána firewall nebo nástroj pro vyrovnávání zatížení.
* Přidat libovolnou IP adresu pro libovolné síťové rozhraní do back-endového fondu služby Azure Load Balancer. V minulosti bylo možné do back-endového fondu přidávat jenom primární IP adresy pro primární síťové rozhraní. Další informace o vyrovnávání zatížení u konfigurací s několika IP adresami najdete v článku věnovaném [vyrovnávání zatížení u konfigurací s několika IP adresami](../articles/load-balancer/load-balancer-multiple-ip.md).

Každé síťové rozhraní připojené k virtuálnímu počítači má přidruženu jednu nebo několik konfigurací IP. Každá konfigurace má přiřazenou jednu statickou nebo dynamickou privátní IP adresu. Každá konfigurace také může mít přiřazen jeden prostředek veřejné IP adresy. Prostředek veřejné IP adresy má přiřazenou buď dynamickou, nebo statickou veřejnou IP adresu. Další informace o IP adresách v Azure najdete v článku [IP adresy v Azure](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md). Každému síťovému rozhraní můžete přiřadit až 250 privátních IP adres. Přestože každému síťovému rozhraní můžete přiřadit víc veřejných IP adres, existují omezení pro počet veřejných IP adres, které jde použít v rámci předplatného Azure. Podrobnosti najdete v článku o [omezeních Azure](../articles/azure-subscription-service-limits.md#networking-limits).


<!--HONumber=Jan17_HO3-->


