Místně redundantní úložiště (LRS) replikuje data třikrát v rámci jednotky škálování úložiště, který je hostován v datacentru v oblasti, ve které jste vytvořili účet úložiště. Žádost o zápis vrátí úspěšně pouze po jeho byla zapsána na všechny tři repliky. Tyto tři repliky každý nacházet v domén samostatné selhání a upgradu domén v rámci jedné škálovací jednotky úložiště.

Jednotka škálování úložiště je kolekce stojany uzlů úložiště. (FD) domény selhání je skupina uzlů, které představují fyzická jednotka selhání a lze považovat za uzly, které patří do stejné fyzické racku. Upgradovací doméně (UD) je skupina uzlů, které jsou upgradovány společně během procesu upgradu služby (zavedení). Tři repliky jsou rozloženy UDs a FDs v rámci jedné jednotky škálování úložiště zajistit, že data je k dispozici i v případě selhání hardwaru má dopad na jednom racku, nebo když jsou uzly upgradován během zavedení.

LRS je nejnižší náklady na možnost a nabízí minimálně odolnost ve srovnání s dalšími možnostmi. V případě havárie úrovně datového centra (aktivuje, zahlcení atd.) může být všechny tři repliky ztracena nebo neopravitelné. Chcete-li toto riziko snížilo, geograficky redundantní úložiště (GRS) se doporučuje pro většinu aplikací.

Místně redundantní úložiště, stále může být žádoucí v některých scénářích:

* Poskytuje nejvyšší maximální šířka pásma možnosti replikace Azure Storage.
* Pokud aplikace ukládá data, která může být snadno znovu vytvořena, můžete zvolit LRS.
* Některé aplikace jsou omezeny na replikaci dat pouze v rámci země z důvodu požadavky zásad správného řízení data. Spárované oblast může být v jiné zemi. Další informace o páry oblast, najdete v části [oblastí Azure](https://azure.microsoft.com/regions/).