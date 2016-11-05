* Virtuální sítě se můžou nacházet ve stejné oblasti (umístění) Azure nebo v různých oblastech.
* Cloudová služba nebo koncový bod vyrovnávání zatížení NEMOHOU pracovat nad více virtuálními sítěmi ani v případě, že jsou propojeny.
* Propojení více virtuálních sítí Azure nevyžaduje žádné místní brány VPN, pokud není vyžadována možnost připojení mezi různými místy.
* Propojení VNet-to-VNet podporují propojování virtuálních sítí. Nepodporuje propojování virtuálních počítačů ani cloudových služeb MIMO virtuální síť.
* Propojení VNet-to-VNet vyžaduje brány VPN Azure s typy sítě VPN RouteBased (dříve nazývané dynamické směrování). 
* Možnost připojení k virtuální síti je možné využívat současně v případě sítí VPN s více servery s maximálně 10 (výchozí/standardní brány) nebo 30 (brány s vyšším výkonem) tunelovými propojeními VPN pro bránu VPN virtuální sítě, která slouží k připojení buď k jiným virtuálním sítím, nebo k místním serverům.
* Adresní prostory virtuálních sítí a místních serverů místních sítí se nesmějí překrývat. Překrývající se adresní prostory mají za následek selhání při vytváření propojení VNet-to-VNet.
* Redundantní tunelová propojení mezi dvěma virtuálními sítěmi nejsou podporována.
* Všechna tunelová propojení sítí VPN v rámci virtuální sítě sdílejí v bráně VPN Azure šířku pásma, která je k dispozici, a stejnou smlouvu SLA pro dostupnost brány VPN v Azure.
* Provoz VNet-to-VNet se přenáší prostřednictvím služby MSN, nikoli po internetu.
* Provoz VNet-to-VNet v rámci stejné oblasti je v obou směrech zdarma; v případě různých oblastí je výchozí přenos VNet-to-VNet zpoplatněn jako odchozí přenos dat mezi virtuálními sítěmi podle zdrojových oblastí. Podrobnosti najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/vpn-gateway/).

<!--HONumber=Sep16_HO3-->


