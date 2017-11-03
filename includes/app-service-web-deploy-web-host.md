### <a name="app-service-plan"></a>Plán služby App Service
Vytvoří plánu služby pro hostování webové aplikace. Zadejte název plánu prostřednictvím **hostingPlanName** parametr. Umístění plánu je stejné umístění, které jsou používané pro skupinu prostředků. Cenová úroveň a pracovní velikosti jsou určené v **sku** a **workerSize** parametry

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

