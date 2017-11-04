## <a name="scenario"></a>Scénář
Virtuální počítač s jednu síťovou kartu se vytvoří a připojené k virtuální síti. Virtuální počítač vyžaduje tři různé *privátní* IP adresy a dvě *veřejné* IP adresy. IP adresy přiřazené k následující konfigurace protokolu IP:

* **IPConfig-1:** přiřadí *statické* privátní IP adresu a *statické* veřejnou IP adresu.
* **IPConfig-2:** přiřadí *statické* privátní IP adresu a *statické* veřejnou IP adresu.
* **IPConfig – 3:** přiřadí *statické* privátní IP adresu a žádné veřejnou IP adresu.
  
    ![Několik IP adres](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

Konfigurace protokolu IP jsou přidruženy ke síťový adaptér, je-li síťový adaptér na síťový adaptér je připojen k virtuálnímu počítači při vytvoření virtuálního počítače. Typy adres IP použitých pro tento scénář jsou pro obrázek. Můžete přiřadit libovolnou IP adresy a přiřazení typy budete potřebovat.

> [!NOTE]
> I když kroky v tomto článku přiřadí všechny konfigurace protokolu IP pro jednu síťovou kartu, můžete také přiřadit víc konfigurací IP adres na všechny síťové adaptéry ve virtuálním počítači více síťovými Kartami. Naučte se vytvořit virtuální počítač s více síťovými kartami, přečtěte si téma [vytvoření virtuálního počítače s více síťovými kartami](../articles/virtual-machines/windows/multiple-nics.md) článku.