Azure nabízí následující skladové jednotky (SKU) brány sítě VPN:

|**SKU**   | **Tunely<br> S2S/VNet-to-VNet** | **Připojení<br>P2S** | **Agregovaná<br>propustnost** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Max. 30                         | Max. 128               | 500 Mb/s                    |
|**VpnGw2**| Max. 30                         | Max. 128               | 1 Gb/s                      |
|**VpnGw3**| Max. 30                         | Max. 128               | 1,25 Gb/s                   |
|**Basic** | Max. 10                         | Max. 128               | 100 Mb/s                    | 
|          |                                 |                        |                             | 

- Propustnost je založená na měření více tunelů agregovaných prostřednictvím jedné brány. Nejedná se o zaručenou propustnost kvůli provozním podmínkám v internetu a chování aplikace.

- Informace o cenách najdete na stránce [Ceny](https://azure.microsoft.com/pricing/details/vpn-gateway).

- Informace o smlouvě SLA (smlouva o úrovni služeb) můžete najít na stránce [SLA](https://azure.microsoft.com/en-us/support/legal/sla/vpn-gateway/).