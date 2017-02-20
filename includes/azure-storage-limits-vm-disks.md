Virtuální počítače Azure podporují připojení řady datových disků. Pro optimální výkon omezte počet vysoce využívaných disků připojených k virtuálnímu počítači, abyste zabránili možnému omezení. Pokud nejsou všechny disky vysoce využívané ve stejnou dobu, může účet úložiště podporovat větší počet disků.

* **Pro Azure Managed Disks:** Omezení počtu disků ve službě Managed Disks se vztahuje na oblast a předplatné. Výchozí doporučené omezení je 2 000. Pokud chcete toto omezení navýšit, obraťte se na podporu Azure.

    Do omezení služby Managed Disks se započítávají i spravované snímky a image.

* **Pro účty úložiště úrovně Standard:** Účet úložiště úrovně Standard má maximální celkovou frekvenci požadavků 20 000 IOPS. Celkový počet IOPS na všech discích virtuálních počítačů v účtu úložiště úrovně Standard by neměl překročit toto omezení.
  
    Na základě omezení frekvence požadavků si můžete přibližně spočítat, kolik vysoce využívaných disků podporuje jeden účet úložiště úrovně Standard. Například pro virtuální počítač úrovně Basic je počet vysoce využívaných disků asi 66 (20 000/300 IOPS na disk) a pro virtuální počítač úrovně Standard to je přibližně 40 (20 000/500 IOPS na disk), jak je znázorněno v tabulce níže. 
* **Pro účty úložiště úrovně Premium:** Účet úložiště úrovně Premium má maximální propustnost 50 Gb/s. Celková propustnost všech disků virtuálních počítačů by neměla překročit toto omezení.



<!--HONumber=Feb17_HO2-->


