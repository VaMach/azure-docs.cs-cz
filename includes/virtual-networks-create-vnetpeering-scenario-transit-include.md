## Služba zřetězení – přenos prostřednictvím přehledu partnerských vztahů virtuálních sítí
Ačkoli použití systémových tras vašemu nasazení automaticky usnadňuje provoz, v některých případech je vhodné řídit směrování paketů prostřednictvím virtuálního zařízení.
V tomto scénáři existují dvě virtuální sítě v předplatném, HubVNet a VNet1, jako je popsáno v následujícím diagramu. Vy nasadíte virtuální zařízení sítě (NVA) ve virtuální síti HubVNet. Po přehledu partnerských vztahů virtuální sítě mezi HubVNet a VNet1 můžete nastavit trasy definované uživatelem a určit další směrování do NVA v HubVNet.

![Přenos NVA](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

> [!NOTE]
> Pro jednoduchost předpokládejte, že všechny zdejší virtuální sítě jsou v rámci stejného předplatného. Funguje to ovšem i pro scénáře různého předplatného.
> 
> 

Klíčovou vlastností pro povolení směrování přenosu je parametr „Povolit přesměrovaný provoz“. Ten umožňuje přijímání a odesílání přenosů z/do NVA v partnerských virtuálních sítích.  

<!--HONumber=Sep16_HO4-->


