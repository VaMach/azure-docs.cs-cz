Připojení Site-to-Site k místní síti vyžadují zařízení VPN. Protože neposkytujeme kroky konfigurace pro všechna zařízení VPN, můžete najít užitečné informace v následujících odkazech:

- Další informace o kompatibilních zařízeních VPN najdete v tématu s popisem [zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). 
- Odkazy na nastavení konfigurace zařízení najdete v popisu [ověřených zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable). Při poskytování odkazů na konfigurace zařízení se snažíme maximálně vyhovět. Vždycky je nejlepší obrátit se na výrobce zařízení a vyžádat si nejnovější informace o konfiguraci.
- Informace o úpravách ukázek konfigurace zařízení najdete v tématu popisujícím [úpravy ukázek](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing).
- Parametry protokolu IPsec/IKE najdete v popisu [parametrů](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec).
- Před konfigurací zařízení VPN zkontrolujte [známé problémy s kompatibilitou zařízení](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) pro zařízení VPN, které chcete použít.

Při konfiguraci zařízení VPN budete potřebovat následující položky:

- Veřejnou IP adresu vaší brány virtuální sítě.

    -  Chcete-li najít veřejnou IP adresu pomocí webu Azure Portal, přejděte na **Brány virtuální sítě** a klikněte na název brány. 
    - Pokud chcete zjistit veřejnou IP adresu brány virtuální sítě pomocí PowerShellu, použijte následující ukázku, ve které nahradíte hodnoty vlastními.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- Sdílený klíč. Jedná se o stejný sdílený klíč, který zadáváte při vytváření připojení VPN Site-to-Site. V našich ukázkách používáme velmi základní sdílený klíč. Pro použití byste měli generovat složitější klíč.




