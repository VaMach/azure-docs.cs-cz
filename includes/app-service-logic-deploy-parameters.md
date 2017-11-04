S Azure Resource Manager, můžete definovat parametry pro hodnoty, které mají používat při nasazování šablony. Šablona obsahuje `parameters` oddíl, který obsahuje všechny hodnoty parametrů. Každá hodnota parametru se šablonou používá k definování prostředky, které chcete nasadit.

> [!NOTE]
> Nedefinujte parametry pro hodnoty, které jsou vždy stejné. Definujte parametry jenom pro hodnoty, které se liší podle prostředí, do kterého nasazujete nebo na základě projektu, který nasazujete.

Když definujete parametry:

* Chcete-li určit povolené hodnoty, které můžete během nasazení zadat uživatele, použijte **allowedValues** pole.

* Chcete-li přiřadit výchozí hodnoty na parametr, pokud jsou k dispozici žádné hodnoty během nasazení, použijte **defaultValue** pole. 
