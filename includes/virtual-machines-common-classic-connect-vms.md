

![Virtuální počítače v samostatné cloudové služby](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Pokud vaše virtuální počítače ve virtuální síti, můžete se rozhodnout, kolik cloudové služby, kterou chcete použít pro načtení sady vyrovnávání a dostupnost. Kromě toho můžete uspořádat virtuální počítače v podsítích stejným způsobem jako místní sítě a připojení virtuální sítě k síti na pracovišti. Tady je příklad:

![Virtuální počítače ve virtuální síti](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuální sítě jsou doporučené způsob, jak připojit virtuální počítače v Azure. Osvědčeným postupem je ke konfiguraci jednotlivých úrovní vaší aplikace v samostatných cloudové službě. Potřebujete však kombinovat některé virtuální počítače z vrstvy jinou aplikaci do stejné cloudové služby za účelem zůstat v rámci maximálně 200 cloudové služby na jedno předplatné. Tato a další omezení najdete v tématu [předplatné Azure a omezení služby, kvóty a omezení](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Připojit virtuální počítače ve virtuální síti
Pro připojení virtuálních počítačů ve virtuální síti:

1. Vytvoření virtuální sítě v [portál Azure](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) a zadejte 'nasazení classic'.
2. Vytvořte sadu cloudových služeb pro vaše nasazení, aby odrážel návrhu pro skupiny dostupnosti a vyrovnávání zatížení. Na portálu Azure klikněte na tlačítko **vytvořit prostředek > výpočetní > Cloudová služba** pro jednotlivých cloudových služeb.

  Při vyplňování podrobnosti cloudové služby, vyberte stejnou _skupiny prostředků_ používat s virtuální sítě.

3. Chcete-li vytvořit každý nový virtuální počítač, klikněte na tlačítko **vytvořit prostředek > výpočetní**, pak vyberte bitovou kopii odpovídající virtuální počítač z **vybrané aplikace**.

  Ve virtuálním počítači **Základy** okně vyberte stejnou _skupiny prostředků_ používat s virtuální sítě.

  ![Okno základy virtuálních počítačů při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Při vyplňování virtuální počítač **nastavení**, vyberte správné _Cloudová služba_ nebo _virtuální sítě_ pro virtuální počítač.

  Azure vybere jiné položky na základě vašeho výběru.

  ![Okno nastavení virtuálního počítače při použití virtuální sítě](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Připojit virtuální počítače v samostatné cloudové služby
Pro připojení virtuálních počítačů v samostatné cloudové služby:

1. Vytvoření cloudové služby v rámci [portál Azure](http://portal.azure.com). Klikněte na tlačítko **nový > výpočetní > Cloudová služba**. Nebo můžete vytvořit cloudovou službu pro nasazení, když vytvoříte první virtuální počítač.
2. Když vytvoříte virtuální počítače, zvolte stejnou skupinu prostředků použít s cloudovou službou.

  ![Přidat virtuální počítač do existující cloudové služby](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3.  Při vyplňování podrobnosti virtuálního počítače, vyberte název cloudové služby vytvořili v prvním kroku.

  ![Výběr cloudové služby pro virtuální počítač](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
