Je důležité si uvědomit, že existují dva způsoby, jak nakonfigurovat naslouchací proces skupiny dostupnosti v Azure. Způsobů, jak se liší v typu služby Vyrovnávání zatížení Azure, kterou použijete při vytvoření naslouchacího procesu. Následující tabulka popisuje rozdíly:

| Typ nástroje pro vyrovnávání zatížení | Implementace | Použijte, když: |
| --- | --- | --- |
| **Externí** |Používá *veřejná virtuální IP adresa* cloudové služby, který je hostitelem virtuálního počítače (VM). |Potřebujete přístup k naslouchání z mimo virtuální síť, včetně z Internetu. |
| **Interní** |Používá *nástroj pro vyrovnávání zatížení pro vnitřní* s privátní adresou pro naslouchací proces. |Můžete přistupovat naslouchací proces pouze z v rámci stejné virtuální síti. Tento přístup zahrnuje site-to-site VPN v hybridní scénáře. |

> [!IMPORTANT]
> Pro naslouchací proces, který používá cloudové službě veřejné VIP (externím vyrovnáváním zatížení), stejně dlouho jako klient naslouchací proces a databáze jsou ve stejné oblasti Azure, nebudou vám narůstat poplatky za odchozí data. Jinak hodnota žádná data vrácená prostřednictvím naslouchací proces považuje za odchozí a je účtovat normální přenosu dat tempem. 
> 
> 

ILB lze nastavit pouze na virtuální sítě s místní rozsah. Existující virtuální sítě, které byly nakonfigurovány pro skupiny vztahů nelze použít ILB. Další informace najdete v tématu [přehled nástroje pro vyrovnávání zatížení pro vnitřní](../articles/load-balancer/load-balancer-internal-overview.md).

