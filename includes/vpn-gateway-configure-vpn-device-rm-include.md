Připojení Site-to-Site k místní síti vyžadují zařízení VPN. Existuje mnoho různých zařízení VPN, která budou fungovat s Azure. Informace o zařízeních VPN a nastaveních konfigurace najdete v tématu [Zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). Před konfigurací zařízení VPN zkontrolujte [známé problémy s kompatibilitou zařízení](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) pro zařízení VPN, které chcete použít. Konkrétní informace o konfiguraci zařízení VPN získáte od výrobce zařízení.

Při konfiguraci zařízení VPN budete potřebovat následující položky:

- **Veřejnou IP adresu** vaší brány virtuální sítě.

    -  Chcete-li najít veřejnou IP adresu pomocí webu Azure Portal, přejděte na **Brány virtuální sítě** a klikněte na název brány. 

    - Pokud chcete zjistit veřejnou IP adresu brány virtuální sítě pomocí PowerShellu, použijte následující ukázku, ve které nahradíte hodnoty vlastními.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- **Sdílený klíč**. Jedná se o stejný sdílený klíč, který zadáte při vytváření připojení VPN Site-to-Site. V našich ukázkách používáme velmi základní sdílený klíč. Pro použití byste měli generovat složitější klíč.




