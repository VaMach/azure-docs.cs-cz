
Ke konfiguraci zařízení VPN budete potřebovat veřejnou IP adresu brány virtuálního zařízení, která umožňuje konfigurovat místní zařízení VPN. Pokud potřebujete znát konkrétní informace o konfiguraci a pomoct s konfigurací vašeho zařízení, obraťte se na výrobce zařízení. V článku [Zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) najdete další informace o zařízeních VPN, která dobře fungují s Azure.

Pokud chcete zjistit veřejnou IP adresu brány virtuální sítě pomocí prostředí PowerShell, použijte následující ukázku:

    Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG

Veřejnou IP adresu své brány virtuální sítě najdete také na portálu Azure. Přejděte na **Brány virtuální sítě** a potom klikněte na název své brány.



<!--HONumber=Nov16_HO2-->


