


## <a name="tagging-a-virtual-machine-through-templates"></a>Označování virtuálního počítače prostřednictvím šablony
První Podíváme se na označování prostřednictvím šablon. [Tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) umístí značky na následující prostředky: výpočetní (virtuálním počítači), úložiště (účet úložiště) a sítě (veřejnou IP adresu, virtuální sítě a síťové rozhraní). Tato šablona je pro virtuální počítač s Windows, ale můžete přizpůsobit pro virtuální počítače s Linuxem.

Klikněte **nasadit do Azure** tlačítko z [odkaz šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). To umožní přejít [portál Azure](https://portal.azure.com/) kde můžete nasadit tuto šablonu.

![Jednoduché nasazení pomocí značek](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Tato šablona obsahuje následující značky: *oddělení*, *aplikace*, a *vytvořil*. Je můžete přidat či upravit tyto značky přímo v šabloně, pokud chcete názvy různých značek.

![Azure značky v šabloně](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Jak vidíte, značky jsou definovány jako páry klíč/hodnota, oddělené dvojtečkou (:). Značky musí být definován v tomto formátu:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Uložte soubor šablony po dokončení úprav s tagy podle svého výběru.

Vedle **upravit parametry** části můžete vyplnit hodnoty pro značek.

![Úprava značek na portálu Azure](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klikněte na tlačítko **vytvořit** k nasazení této šablony se vaše hodnoty značky.

## <a name="tagging-through-the-portal"></a>Označování prostřednictvím portálu
Po vytvoření vaše prostředky pomocí značek, můžete zobrazit, přidat a odstranit značky na portálu.

Vyberte ikonu značky zobrazení značek:

![Ikona značky na portálu Azure](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Přidejte novou značku přes portál tak, že definujete vlastní dvojice klíč/hodnota a uložte ho.

![Přidat novou značku na portálu Azure](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Novou značku by se měla zobrazit v seznamu značky prostředku.

![Novou značku uložit na portálu Azure](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

