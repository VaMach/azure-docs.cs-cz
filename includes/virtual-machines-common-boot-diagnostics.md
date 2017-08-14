V Azure je teď dostupná podpora dvou funkcí ladění: Podpora výstupu konzoly a snímku obrazovky pro model nasazení virtuálních počítačů Azure Resource Manager. 

Při přenosu vlastní image do Azure nebo spouštění některé z imagí platformy může virtuální počítač přejít do nespustitelného stavu z mnoha důvodů. Tyto funkce umožňují snadnou diagnostiku a obnovení virtuálních počítačů v případě chyb při spuštění.

U virtuálních počítačů s Linuxem můžete snadno zobrazit výstup protokolu konzoly z portálu:

![portál Azure](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
U virtuálních počítačů s Windows i Linuxem však Azure umožňuje zobrazit také snímek obrazovky virtuálního počítače z hypervisoru:

![Chyba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Obě tyto funkce jsou podporované pro virtuální počítače Azure ve všech oblastech. Mějte na paměti, že může trvat až 10 minut, než se snímky obrazovky a výstup zobrazí v účtu úložiště.

## <a name="common-boot-errors"></a>Běžné chyby spuštění

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Operační systém nebyl nalezen](https://support.microsoft.com/help/4010142)
- [Chyba při spuštění nebo NEDOSTUPNÉ_SPOUŠTĚCÍ_ZAŘÍZENÍ](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Povolení diagnostiky na novém virtuálním počítači
1. Při vytváření nového virtuálního počítače na portálu Preview vyberte z rozevíracího seznamu modelu nasazení **Azure Resource Manager**:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. Nakonfigurujte možnost Monitorování pro výběr účtu úložiště, do kterého chcete tyto diagnostické soubory ukládat.
 
    ![Vytvoření virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Pokud provádíte nasazení z šablony Azure Resource Manageru, přejděte do prostředku vašeho virtuálního počítače a připojte část s diagnostickým profilem. Nezapomeňte použít hlavičku verze rozhraní API s hodnotou 2015-06-15.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. Diagnostický profil umožňuje vybrat účet úložiště pro ukládání protokolů.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

Pokud chcete nasadit ukázkový virtuální počítač s povolenou diagnostikou spuštění, podívejte se sem na naše úložiště.

## <a name="update-an-existing-virtual-machine"></a>Aktualizace stávajícího virtuálního počítače ##

Pokud chcete povolit diagnostiku přes portál, můžete přes portál také aktualizovat stávající virtuální počítač. Vyberte možnost Diagnostika spuštění a klikněte na Uložit. Restartujte virtuální počítač, aby se projevily změny.

![Aktualizace stávajícího virtuálního počítače](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

