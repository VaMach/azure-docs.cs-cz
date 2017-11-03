

## <a name="multi-and-single-instance-vms"></a>Více a jedna Instance virtuálních počítačů
Mnoho zákazníků systémem Azure počet je důležité, aby můžete naplánovat při jejich virtuální počítače podstoupit plánované údržby kvůli výpadku – asi 15 minut – k tomu dojde během údržby. Aby se usnadnilo řízení, když virtuální počítače zřízené obdrží plánované údržbě, můžete použít skupiny dostupnosti.

Existují dvě možné konfigurace pro virtuální počítače spuštěné v Azure. Virtuální počítače jsou buď nakonfigurovány jako s více instancemi nebo s jednou instancí. Pokud jsou virtuální počítače v nastavení dostupnosti, pak jsou nakonfigurované jako s více instancemi. Všimněte si, i jeden virtuálních počítačů je možné nasadit v nastavení dostupnosti, takže jsou považovány za s více instancemi. Pokud virtuální počítače nejsou v nastavení dostupnosti, pak jsou nakonfigurované jako jedné instance.  Podrobnosti o dostupnosti sadách najdete v tématu [Správa dostupnosti virtuálních počítačů z Windows](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [Správa dostupnosti virtuálních počítačů systému Linux](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Plánovaná údržba aktualizace jedné instance a s více instancemi virtuálních počítačů dojít samostatně. Změnou virtuální počítače do jedné instance (pokud jsou s více instancemi) nebo na více instancemi (pokud jde o jednoduchou) můžete řídit, pokud jejich virtuální počítače přijímat plánované údržby. V tématu [plánované údržby pro virtuální počítače s Linuxem Azure](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) nebo [plánované údržby pro virtuální počítače s Azure Windows](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) podrobnosti o plánované údržby pro virtuální počítače Azure.

## <a name="for-multi-instance-configuration"></a>Pro konfiguraci s více instancemi
Můžete vybrat čas plánované údržby ovlivní vaše virtuální počítače, které jsou nasazeny v konfiguraci skupiny dostupnosti odebráním těchto virtuálních počítačů ze skupiny dostupnosti.

1. E-mail je odeslán vám sedm dní kalendáře před plánované údržby pro virtuální počítače v konfiguraci s více instancemi. ID předplatného a názvy ovlivněných virtuálních počítačů s více instancemi jsou zahrnuty v textu e-mailu.
2. Během těchto sedm dní které můžete vybrat dobu, kterou vaše instance jsou aktualizovány odebráním virtuální počítače s více instancemi v této oblasti z jejich sady dostupnosti. Tato změna konfigurace způsobí, že k restartování, jako virtuální počítač je přechod z jednoho fyzického hostitele, určený pro údržby jiného fyzického hostitele, který není cílová pro údržbu.
3. Virtuální počítač můžete odebrat z její dostupnosti na portálu Azure.

   1. Na portálu vyberte virtuální počítač odebrat ze skupiny dostupnosti.  

   2. V části **nastavení**, klikněte na tlačítko **sadu dostupnosti**.

      ![Výběr skupiny dostupnosti](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. V dostupnost nastavte rozevírací nabídce, vyberte "Není součástí skupiny dostupnosti."

      ![Odebrat ze sady](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. V horní části, klikněte na tlačítko **Uložit**. Klikněte na tlačítko **Ano** aby vzali na vědomí, že tato akce restartuje virtuální počítač.

   >[!TIP]
   >Je možné změnit konfiguraci virtuálního počítače s více instancemi později tak, že vyberete jednu z uvedených dostupnosti sad.

4. Virtuální počítače odebrat ze skupiny dostupnosti přesunou do jedné Instance hostitele a neaktualizují během plánované údržby dostupnosti nastavení konfigurace.
5. Po dokončení (podle plánu uvedených v původního e-mailu) aktualizaci, kterou chcete virtuální počítače nastavení dostupnosti, měli byste přidat virtuální počítače zpět do jejich skupiny dostupnosti. Stane součástí skupiny dostupnosti jako s více instancemi změní konfiguraci virtuálních počítačů a má za následek restart. Obvykle po dokončení všech aktualizací s více instancemi napříč celou prostředí Azure, odpovídá jedné instance údržby.

Odebrání virtuálního počítače ze skupiny dostupnosti můžete také dosáhnout pomocí Azure PowerShell:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Pro konfiguraci s jednou instancí
Můžete vybrat čas plánované údržby ovlivňuje můžete virtuální počítače v konfiguraci s jednou instancí přidáním těchto virtuálních počítačů do skupiny dostupnosti.

Podrobný postup

1. E-mail je odeslán vám sedm dní před plánované údržby pro virtuální počítače v konfiguraci s jednou instancí. ID předplatného a názvy virtuálních počítačů ovlivněných jedné Instance jsou zahrnuty v textu e-mailu.
2. Během těchto sedm dní které můžete vybrat dobu, kterou instanci restartuje přidáním jedné instance virtuálních počítačů v této oblasti stejnou sadu dostupnosti. Tato změna konfigurace způsobí, že k restartování, jako virtuální počítač je přechod z jednoho fyzického hostitele, určený pro údržby jiného fyzického hostitele, který není cílová pro údržbu.
3. Postupujte podle pokynů pro přidání existujících virtuálních počítačů do skupiny dostupnosti pomocí portálu Azure a prostředí Azure PowerShell. (Viz prostředí Azure PowerShell vzorku, který zahrnuje následující kroky.)
4. Jakmile tyto virtuální počítače jsou překonfigurovat jako s více instancemi, jsou vyloučeny ze plánované údržby k virtuálním počítačům jedné instance.
5. Po dokončení aktualizace jedné instance virtuálního počítače (podle plánu v původního e-mailu), může vrátit virtuální počítače do jedné instance odebráním virtuální počítače z jejich skupiny dostupnosti.

Přidání virtuálního počítače do také sadu dostupnosti můžete dosáhnout pomocí Azure PowerShell:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
