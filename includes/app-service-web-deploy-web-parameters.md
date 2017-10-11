Pomocí Azure Resource Manageru definujete parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddíl s názvem parametry obsahující všechny hodnoty parametru.
Měli byste parametr pro ty hodnoty, které se liší podle prostředí, ve kterém provádíte nasazení nebo na základě projektu, které nasazujete. Nedefinují parametry pro hodnoty, které zůstanou vždy stejná. Každá hodnota parametru se v šabloně použije k definování nasazovaných prostředků. 

Při definování parametrů, použijte **allowedValues** můžete během nasazení zadat pole, které chcete určit, které hodnoty uživatele. Použití **defaultValue** pole o přiřazení hodnoty na parametr, pokud je během nasazení zadána žádná hodnota.

Nemůžeme se popisují jednotlivé parametry v šabloně.

### <a name="sitename"></a>Název webu
Název webové aplikace, kterou chcete vytvořit.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Název plánu služby App Service pro hostování webové aplikace.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU
Cenovou úroveň pro hostování plánu.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Definuje hodnoty, které jsou povoleny pro tento parametr šablony a přiřadí výchozí hodnotu (S1), pokud není zadaná žádná hodnota.

### <a name="workersize"></a>workerSize
Velikost instance plánu hostingu (malé, střední a velké).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Šablona definuje hodnoty, které jsou povoleny pro tento parametr (0, 1 nebo 2) a přiřadí výchozí hodnota (0), pokud není zadaná žádná hodnota. Hodnoty odpovídají malé, střední a velké.

