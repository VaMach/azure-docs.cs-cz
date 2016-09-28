Každý objekt blob v úložišti Azure se musí nacházet v kontejneru. Kontejner je součástí názvu objektu blob. Třeba `mycontainer` je název kontejneru v těchto identifikátorech URI ukázkových objektů blob:

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

Název kontejneru musí být platný název DNS, který odpovídá následujícím pravidlům pro pojmenování:

1. Názvy kontejnerů musí začínat písmenem nebo číslicí a smí obsahovat jenom písmena, číslice a pomlčky (-).
1. Bezprostředně před a za každým znakem pomlčky (-) se musí nacházet písmeno nebo číslice. Názvy kontejnerů nesmí obsahovat po sobě jdoucí pomlčky.
1. Název kontejneru musí být psaný malými písmeny.
1. Názvy kontejnerů musí mít délku 3 až 63 znaků.

> [AZURE.IMPORTANT] Upozorňujeme, že název kontejneru musí být psaný malými písmeny. Pokud v názvu kontejneru napíšete velké písmeno nebo jinak porušíte pravidla po pojmenování kontejnerů, může se zobrazit chyba 400 (Chybný požadavek). 

<!--HONumber=Sep16_HO3-->


