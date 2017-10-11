Při přidávání datové disky na virtuální počítač s Linuxem, mohou nastat chyby, pokud disk neexistuje na logické jednotce 0. Pokud přidáváte ručně pomocí disku `azure vm disk attach-new` příkazu a zadáte logické jednotky (`--lun`) namísto povolení platformy Azure určit příslušnou logickou jednotku LUN, postará že disk již existuje / bude existovat na logické jednotce 0. 

Podívejte se na následující příklad zobrazuje fragment výstup `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

Existují dvě datové disky na LUN 0 a 1 logická jednotka (první sloupec v `lsscsi` výstup podrobnosti `[host:channel:target:lun]`). Oba disky by měl být accessbile z virtuálního počítače. Pokud byl ručně zadané první disk, který má být přidán na logickou jednotku LUN 1 a druhý disk na logické jednotce 2, nemusíte vidět disky správně ze v rámci virtuálního počítače.

> [!NOTE]
> Azure `host` hodnota je 5 v těchto příkladech, ale to se může lišit v závislosti na typu úložiště, které vyberete.
> 
> 

Toto chování disku není problém s Azure, ale způsob, ve kterém následuje jádra Linux specifikace SCSI. Sběrnice SCSI, hledá připojená zařízení prohledávání jádra Linux musí být zařízení nalezen na logickou jednotku 0 v pořadí pro systém pokračujte vyhledávání dalších zařízení. Takto:

* Prohlédněte si výstup `lsscsi` po přidání datový disk k ověření, že máte na disk na logické jednotce 0.
* Pokud disk nezobrazuje správně v rámci virtuálního počítače, ověřte, zda že disk na logické jednotce 0 existuje.

