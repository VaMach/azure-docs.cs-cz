| Prostředek | Výchozí omezení | Maximální omezení |
| --- | --- | --- |
| Prostředky na [skupiny prostředků](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (podle typu prostředku) |800 |Se liší podle typu prostředku |
| Nasazení na skupiny prostředků |800 |800 |
| Prostředky na nasazení. |800 |800 |
| Zámky správy (podle jedinečný obor) |20 |20 |
| Počet značek (podle prostředku nebo skupinu prostředků) |15 |15 |
| Délka klíče značky |512 |512 |
| Délka hodnoty značky |256 |256 |


#### <a name="template-limits"></a>Omezení šablony

| Hodnota | Výchozí omezení | Maximální omezení |
| --- | --- | --- |
| Parametry |256 |256 |
| Proměnné |256 |256 |
| Prostředky (včetně počet kopií) |800 |800 |
| Výstupy |64 |64 |
| Výraz šablony |24,576 znaků |24,576 znaků |
| Prostředky v exportovanými šablonami |200 |200 | 
| Velikost šablony |1 MB |1 MB |
| Velikost souboru parametrů |64 kB |64 kB |

Některá omezení šablony můžete překročit pomocí vnořené šablony. Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Abyste snížili počet parametrů, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu. Další informace najdete v tématu [objektů jako parametry](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).