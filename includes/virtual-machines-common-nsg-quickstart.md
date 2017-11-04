Otevření portu nebo vytvořte koncového bodu, virtuální počítač (VM) v Azure vytvořením filtr sítě na podsíť nebo rozhraní sítě virtuálních počítačů. Tyto filtry, které řídí příchozí a odchozí přenosy, můžete umístit na skupinu zabezpečení sítě, který je připojen k prostředku, který přijímá provoz.

Použijeme Běžným příkladem webové přenosy na portu 80. Jakmile je virtuální počítač, který je nakonfigurován tak, aby webové požadavky na standardní TCP port 80 (nezapomeňte spustit příslušné služby a otevřete všechna pravidla brány firewall operačního systému na virtuálním počítači i), můžete:

1. Vytvořte skupinu zabezpečení sítě.
2. Vytvoření příchozího pravidla umožňuje provoz se:
   * Rozsah cílových portů "80"
   * Rozsah zdrojových portů z "*" (což jakéhokoli zdrojového portu)
   * hodnotou priority menší 65 500 (tak, aby se odepřel vyšší priorita než catch všechny výchozí pravidla pro příchozí)
3. Skupina zabezpečení sítě přidružte rozhraní sítě virtuálních počítačů nebo podsíť.

Můžete vytvořit konfiguraci komplexní sítě na zabezpečit vaše prostředí pomocí skupin zabezpečení sítě a pravidla. Naše Ukázka používá jenom jednu nebo dvě pravidla, která povolit provoz protokolu HTTP nebo pro vzdálenou správu. Další informace najdete v tématu následující ["Další informace"](#more-information-on-network-security-groups) části nebo [co je skupina zabezpečení sítě?](../articles/virtual-network/virtual-networks-nsg.md)

