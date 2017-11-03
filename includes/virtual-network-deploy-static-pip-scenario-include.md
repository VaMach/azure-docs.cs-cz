## <a name="scenario"></a>Scénář
Tento dokument provede nasazení, které používá statickou veřejnou IP adresu přidělené k virtuálnímu počítači (VM). V tomto scénáři máte jeden virtuální počítač s vlastním statickou veřejnou IP adresu. Virtuální počítač je součástí podsíť s názvem **front-endu** a také se statickou privátní IP adresou (**192.168.1.101**) v této podsíti.

Může být nutné statickou IP adresu pro webové servery, které vyžadují připojení SSL, ve kterých je propojený certifikát protokolu SSL na IP adresu. 

![POPISEK OBRÁZKU](./media/virtual-network-deploy-static-pip-scenario-include/figure1.png)

Provedením následujících nasazení prostředí vidět na obrázku výše uvedených kroků.

