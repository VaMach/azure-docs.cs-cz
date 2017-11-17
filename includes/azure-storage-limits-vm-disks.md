Virtuální počítače Azure podporují připojení řady datových disků. Tento článek popisuje, škálovatelnosti a cílech výkonnosti pro datové disky Virtuálního počítače. Použijte tyto cíle se můžete rozhodnout, počet a typ disku, který je potřeba splnění požadavků na výkon a kapacitu. 

> [!IMPORTANT]
> Pro optimální výkon omezte počet vysoce využívané disky připojené k virtuálnímu počítači, aby se zabránilo možné omezení. Pokud všechny připojené disky nejsou použity vysoce ve stejnou dobu, virtuální počítač může podporovat větší počet disků.

* **Pro spravované disky systému Azure:** je limit na disku pro spravované disky jednotlivých oblasti a typ disku. Maximální limit a také výchozí limit je 10 000 spravovaných disků na každou oblast a na typu disku pro předplatné. Například můžete vytvořit až 10 000 standardní spravované disky a také 10 000 premium spravované disky v oblasti, na jedno předplatné.

    Spravované snímky a bitové kopie započítává limit spravované disky.

* **Pro účty úložiště úrovně Standard:** Účet úložiště úrovně Standard má maximální celkovou frekvenci požadavků 20 000 IOPS. Celkový počet IOPS na všech discích virtuálních počítačů v účtu úložiště úrovně Standard by neměl překročit toto omezení.
  
    Na základě omezení frekvence požadavků si můžete přibližně spočítat, kolik vysoce využívaných disků podporuje jeden účet úložiště úrovně Standard. Například pro základní virtuální počítač vrstvy, maximální počet vysoce využívané disků je o 66 (20 000/300 IOPS na disk) a pro standardní vrstvy virtuálního počítače, je o 40 (20 000/500 IOPS na disk). 

* **Pro účty úložiště úrovně Premium:** Účet úložiště úrovně Premium má maximální propustnost 50 Gb/s. Celková propustnost všech disků virtuálních počítačů by neměla překročit toto omezení.

