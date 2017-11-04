## <a name="scenario"></a>Scénář
Chcete-li lépe předvedli, jak vytvořit udr, bude tento dokument používat následující scénář.

![POPISEK OBRÁZKU](./media/virtual-network-create-udr-scenario-include/figure1.png)

V tomto scénáři vytvoříte jeden UDR pro *podsítě Front end* a jiné UDR pro *podsítě Back end* , jak je popsáno níže: 

* **UDR front-endu**. Front-endu UDR použijí se *front-endu* podsítě a obsahovat jeden postup:    
  * **RouteToBackend**. Tato trasa bude odesílat veškerý přenos v back-end podsítě, která se **FW1** virtuálního počítače.
* **UDR back-end**. Back-end UDR použijí se *back-end* podsítě a obsahovat jeden postup:    
  * **RouteToFrontend**. Tato trasa bude odesílat veškerý přenos v podsítě front-endu, která se **FW1** virtuálního počítače.

Kombinace tyto trasy zajistí, že veškerý provoz, jehož z jedné podsítě do jiné, budou směrovány do **FW1** virtuální počítač, který je používán jako virtuální zařízení. Také budete muset zapnout předávání IP pro tento virtuální počítač, aby že mohl přijímat přenosy určené do ostatních virtuálních počítačů.

