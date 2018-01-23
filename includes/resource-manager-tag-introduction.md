Použijte značky na prostředky Azure pro jejich logické uspořádání podle kategorií. Každá značka se skládá z názvu a hodnoty. Můžete například použít název Prostředí a hodnotu Produkční na všechny prostředky v produkčním prostředí. Bez této značky může být obtížné určit, jestli je prostředek určený pro vývojové, testovací nebo produkční prostředí. „Prostředí“ a „Produkční“ jsou však jenom příklady. Definujte názvy a hodnoty, které jsou nejvhodnější pro uspořádání vašeho předplatného.

Po použití značek můžete načíst všechny prostředky v předplatném s daným názvem a hodnotou značky. Značky umožňují načíst související prostředky, které se nacházejí v různých skupinách prostředků. Tento přístup je užitečný v případě, že potřebujete uspořádat prostředky pro účely fakturace nebo správy.

Na značky se vztahují následující omezení:

* Každý prostředek nebo skupina prostředků může mít maximálně 15 párů název/hodnota značek. Toto omezení se vztahuje pouze na značky použité přímo na prostředek nebo skupinu prostředků. Skupina prostředků může obsahovat řadu prostředků, z nichž každý má 15 párů název/hodnota značek. Pokud k prostředku potřebujete přidružit více než 15 hodnot, použijte jako hodnotu značky řetězec JSON. Řetězec JSON může obsahovat mnoho hodnot, které se použijí pro jeden název značky. Tento článek ukazuje příklad přiřazení řetězce JSON ke značce.
* Název značky je omezen na 512 znaků a hodnota značky je omezena na 256 znaků. Pro účty úložiště je název značky omezen na 128 znaků a hodnota značky je omezena na 256 znaků.
* Prostředky ve skupině prostředků nedědí značky použité na danou skupinu prostředků.
* Tyto znaky se nepodporují:
  * `<`
  * `>`
  * `%`
  * `&`
  * `\\`
  * `?`
  * `/`
