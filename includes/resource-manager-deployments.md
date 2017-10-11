## <a name="incremental-and-complete-deployments"></a>Přírůstkové a úplné nasazení
Při nasazení vašich prostředků, zadejte, že nasazení je k přírůstkové aktualizaci nebo kompletní aktualizace. Hlavní rozdíl mezi tyto dva režimy je, jak Resource Manager zpracovává existující prostředky ve skupině prostředků, které nejsou v šabloně:

* V dokončení režimu Resource Manager **odstraní** prostředky, které existují ve skupině prostředků, ale nejsou zadané v šabloně. 
* V přírůstkové režimu Resource Manager **zůstane beze změny** prostředky, které existují ve skupině prostředků, ale nejsou zadané v šabloně.

Pro oba režimy Resource Manager pokusí zřídit všechny prostředky zadané v šabloně. Pokud prostředek již existuje ve skupině prostředků a jsou stejné jako jeho nastavení, výsledkem operace žádná změna. Pokud změníte nastavení pro prostředek, prostředek je opatřen tyto nové nastavení. Pokud budete chtít aktualizovat umístění nebo typ existující prostředek, nasazení se nezdaří s chybou. Místo toho nasaďte nový prostředek s umístění nebo typu, je nutné.

Ve výchozím nastavení používá přírůstkové režimu Resource Manager.

Pro ilustraci rozdíl mezi režimy přírůstkové a úplné, zvažte následující scénář.

**Existující skupinu prostředků** obsahuje:

* Prostředek A
* Prostředek B
* Prostředek C

**Šablona** definuje:

* Prostředek A
* Prostředek B
* Prostředek D

Při nasazení v **přírůstkové** režimu, skupina prostředků obsahuje:

* Prostředek A
* Prostředek B
* Prostředek C
* Prostředek D

Při nasazení v **dokončení** režimu C prostředků se odstraní. Skupina prostředků obsahuje:

* Prostředek A
* Prostředek B
* Prostředek D
