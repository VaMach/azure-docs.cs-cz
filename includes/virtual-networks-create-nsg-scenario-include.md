## <a name="scenario"></a>Scénář
Chcete-li lépe předvedli, jak vytvořit skupiny Nsg, bude tento dokument používat následující scénář.

![Scénář sítě VNet](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

V tomto scénáři vytvoříte skupinu NSG pro každou podsíť v **TestVNet** virtuální sítě, jak je popsáno níže: 

* **Skupina NSG front-endu**. Skupina NSG se použijí pro front-endu *front-endu* podsítě a obsahují dvě pravidla:    
  * **pravidlo protokolu RDP**. Toto pravidlo povolí provoz protokolu RDP *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo povolí provoz protokolu HTTP *front-endu* podsítě.
* **Skupina NSG back-end**. Back-end NSG se použijí na *back-end* podsítě a obsahují dvě pravidla:    
  * **Pravidlo SQL**. Toto pravidlo umožňuje SQL přenos jenom z *front-endu* podsítě.
  * **pravidlo webové**. Toto pravidlo odmítne všechny internet vázaný provoz z *back-end* podsítě.

Kombinace tato pravidla vytvořit jako zóna DMZ scénář, kde podsíť back-end může přijímat pouze příchozí provoz SQL z podsítě front end a nemá přístup k Internetu, zatímco podsítě front end komunikují po Internetu a přijímat příchozí požadavky HTTP jenom.

