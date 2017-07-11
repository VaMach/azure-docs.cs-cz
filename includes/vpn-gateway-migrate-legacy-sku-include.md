> [!NOTE]
> Při migraci ze staré SKU na novou SKU se změní veřejná IP adresa VPN Gateway.
> 

Při přechodu ze starých řad SKU na nové není možné přímo měnit velikost bran VPN Azure. Pokud máte brány VPN v modelu nasazení Resource Manageru, které používají starší verze SKU, můžete migrovat na nové SKU. Pokud chcete provést migraci, odstraňte stávající bránu VPN pro vaši virtuální síť a potom vytvořte novou.

Pracovní postup migrace:

1. Odeberte všechna připojení k bráně virtuální sítě.
2. Odstraňte starou bránu VPN.
3. Vytvořte novou bránu VPN.
4. Aktualizujte místní zařízení VPN novou IP adresou brány VPN (pro připojení typu Site-to-Site).
5. Aktualizujte hodnotu IP adresy brány pro všechny místní síťové brány typu VNet-to-VNet, které se připojují k této bráně.
6. Stáhněte nové klientské balíčky konfigurace klienta VPN pro klienty P2S, kteří se připojují k virtuální síti přes tuto bránu VPN.
7. Znovu vytvořte všechna připojení k bráně virtuální sítě.