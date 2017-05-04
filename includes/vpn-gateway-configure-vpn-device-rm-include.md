Připojení Site-to-Site k místní síti vyžadují zařízení VPN. Protože neposkytujeme kroky konfigurace pro všechna zařízení VPN, můžete najít užitečné informace v následujících odkazech:

- Další informace o kompatibilních zařízeních VPN najdete v tématu s popisem [zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md). 
- Odkazy na nastavení konfigurace zařízení najdete v popisu [ověřených zařízení VPN](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable). Při poskytování odkazů na konfigurace zařízení se snažíme maximálně vyhovět. Vždycky je nejlepší obrátit se na výrobce zařízení a vyžádat si nejnovější informace o konfiguraci.
- Informace o úpravách ukázek konfigurace zařízení najdete v tématu popisujícím [úpravy ukázek](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing).
- Parametry protokolu IPsec/IKE najdete v popisu [parametrů](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec).
- Před konfigurací zařízení VPN zkontrolujte [známé problémy s kompatibilitou zařízení](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) pro zařízení VPN, které chcete použít.

Při konfiguraci zařízení VPN potřebujete následující:

- Sdílený klíč. Jedná se o stejný sdílený klíč, který zadáváte při vytváření připojení VPN Site-to-Site. V našich ukázkách používáme základní sdílený klíč. Doporučujeme, abyste pro použití vygenerovali složitější klíč.
- Veřejnou IP adresu vaší brány virtuální sítě. Veřejnou IP adresu můžete zobrazit pomocí webu Azure Portal, PowerShellu nebo rozhraní příkazového řádku.