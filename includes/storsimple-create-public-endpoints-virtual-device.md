#### Vytvoření veřejných koncových bodů na virtuálním zařízení
1. Přihlaste se k portálu Azure Classic.

* Klikněte na **Virtuální počítače** a potom vyberte virtuální počítač, který používáte jako virtuální zařízení.
* Klikněte na **Koncové body**. Objeví se stránka **Koncové body**, která obsahuje všechny koncové body pro daný virtuální počítač.
* Klikněte na tlačítko **Add** (Přidat). Zobrazí se dialogové okno **Přidat koncový bod**. Pokračujte kliknutím na šipku.
* Do pole **Název** zadejte následující název koncového bodu: **WinRMHttps**.
* V poli **Protokol** uveďte **TCP**.
* Do pole **Veřejný port** zadejte čísla portů, která chcete pro připojení použít.
* Do pole **Privátní port** zadejte **5986**.
* Kliknutím na symbol zaškrtnutí vytvořte koncový bod.

Po vytvoření koncového bodu můžete zobrazit jeho podrobnosti, abyste zjistili virtuální IP adresu (VIP). Tuto adresu si poznamenejte.

<!--HONumber=Sep16_HO3-->


