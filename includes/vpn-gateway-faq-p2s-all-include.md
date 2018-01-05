### <a name="supportedclientos"></a>Které klientské operační systémy je možné používat s připojeními typu Point-to-Site?

Podporovány jsou následující operační systémy:

* Windows 7 (32bitové a 64bitové verze)
* Windows Server 2008 R2 (pouze 64bitové verze)
* Windows 8 (32bitové a 64bitové verze)
* Windows 8.1 (32bitové a 64bitové verze)
* Windows Server 2012 (pouze 64bitové verze)
* Windows Server 2012 R2 (pouze 64bitové verze)
* Windows Server 2016 (pouze 64bitové verze)
* Windows 10
* OSX verze 10.11 pro Mac (El Capitan)
* macOS verze 10.12 pro Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Kolik koncových bodů klienta VPN je možné mít v konfiguraci připojení Point-to-Site?

Podporujeme až 128 klientů VPN, kteří se mohou připojit k virtuální síti současně.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Je možné procházet proxy servery a brány firewall s využitím schopnosti Point-to-Site?

Azure podporuje dva typy možností Point-to-site VPN:

* SSTP (Secure Socket Tunneling Protocol). SSTP je proprietární řešení Microsoftu založené na SSL, které umožňuje pronikat branami firewall, protože většina bran firewall otevírá port TCP 443, který SSL používá.

* IKEv2 VPN. IKEv2 VPN řešení IPsec VPN založené na standardech, které využívá porty UDP 500 a 4500 a protokol IP č. 50. Brány firewall tyto porty neotvírají vždycky, takže je možné, že IKEv2 VPN nebude moct procházet servery proxy a branami firewall.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Pokud restartuji klientský počítač nakonfigurovaný pro připojení Point-to-Site, připojí se síť VPN automaticky znovu?

Ve výchozím nastavení nebude klientský počítač vytvářet připojení k síti VPN automaticky znovu.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Podporuje připojení Point-to-Site automatické připojení a DDNS u klientů sítě VPN?

Automatické opětné připojení a DDNS se u sítí VPN s připojením Point-to-Site aktuálně nepodporují.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Je možné současně používat konfigurace Site-to-Site a Point-to-Site pro stejnou virtuální síť?

Ano. Pro model nasazení Resource Manageru musíte mít bránu typu VPN RouteBased. Pro model nasazení Classic je potřebná dynamická brána. Připojení Point-to-Site se pro brány VPN se statickým směrováním ani pro brány VPN PolicyBased nepodporuje.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Je možné nakonfigurovat klienta Point-to-Site tak, aby se připojoval k několik virtuálním sítím současně?

Ne. Klient typu Point-to-Site se může připojit jenom k prostředkům ve virtuální síti, ve které je umístěná brána virtuální sítě.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Jakou propustnost je možné očekávat u připojení typu Site-to-Site nebo Point-to-Site?

Určit přesnou propustnost tunelových propojení sítí VPN je obtížné. IPsec a SSTP jsou kryptograficky náročné protokoly sítě VPN. Propustnost je také omezena latencí a šířkou pásma mezi vaší lokalitou a internetem. U služby VPN Gateway s připojeními Point-to-Site VPN jenom typu IKEv2 bude celková propustnost, kterou můžete očekávat, záviset na skladové jednotce Gateway. Další informace o propustnosti najdete v části [Skladové jednotky (SKU) brány](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Je možné pro připojení Point-to-Site použít libovolného softwarového klienta sítě VPN, pokud podporuje protokol SSTP a/nebo IKEv2?

Ne. Pro SSTP můžete použít jenom nativního klienta VPN v systému Windows a pro IKEv2 nativního klienta VPN v systému Mac. Podrobnosti najdete v seznamu podporovaných klientských operačních systémů.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Podporuje Azure IKEv2 VPN s Windows?

Uživatelé se mohou připojit k Azure pomocí integrovaného klienta Windows VPN, který podporuje IKEv2. Ale připojení IKEv2 ze zařízení s Windows nebudou fungovat v následujícím scénáři:

  Pokud zařízení uživatele obsahuje velký počet důvěryhodných kořenových certifikátů, velikost datové části zpráv během výměny IKE je velká a způsobuje fragmentaci na úrovni protokolu IP. Tyto fragmenty jsou na straně Azure odmítnuty, což způsobí selhání připojení. Přesný počet certifikátů, při kterém tento problém vzniká, je obtížné odhadnout. V důsledku toho není pro připojení IKEv2 ze zařízení s Windows zaručené fungování. Když konfigurujete SSTP i IKEv2 ve smíšeném prostředí (sestávajícím ze zařízení Windows i Mac), profil Windows VPN vždy nejdřív zkouší tunel IKEv2. Pokud kvůli popsanému problému selže, vrátí se zpět k SSTP.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Které další platformy (mimo Windows a Mac) Azure podporuje pro P2S VPN?

Azure pro P2S VPN podporuje pouze systém Windows a Mac.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>Už mám nasazenou Azure VPN Gateway. Můžete povolit RADIUS nebo IKEv2 VPN na něm?

Ano, můžete povolit tyto nové funkce na již nasazené Gateway pomocí prostředí Powershell nebo portálu Azure za předpokladu, že podporuje SKU brány, kterou používáte, RADIUS nebo IKEv2. Například základní SKU brány VPN nepodporuje, RADIUS nebo IKEv2.
