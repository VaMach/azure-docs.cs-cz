## <a name="download-and-understand-the-arm-template"></a>Postup stažení a popis šablony ARM
Z webu github si můžete stáhnout existující šablonu ARM k vytvoření sítě VNet a dvou podsítí, libovolně ji upravit a opakovaně používat. Postupujte následujícím způsobem.

1. Přejděte na stránku [vzorové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klikněte na **azuredeploy.json** a potom klikněte na **RAW**.
3. Uložte soubor do místní složky v počítači.
4. Pokud již šablony ARM znáte, pokračujte krokem 7.
5. Otevřete soubor, který jste právě stáhli, a prohlédněte si jeho obsah v části **parameters** na řádku 5. Parametry šablony ARM představují zástupce hodnot, které můžete doplnit během nasazování.
   
   | Parametr | Popis |
   | --- | --- |
   | **location** |Oblast Azure, ve které bude síť VNet vytvořena |
   | **vnetName** |Název nové sítě VNet |
   | **addressPrefix** |Adresní prostor sítě VNet ve formátu CIDR |
   | **subnet1Name** |Název první sítě VNet |
   | **subnet1Prefix** |Blok CIDR pro první podsíť |
   | **subnet2Name** |Název druhé sítě VNet |
   | **subnet2Prefix** |Blok CIDR pro druhou podsíť |
   
   > [!IMPORTANT]
   > Šablony ARM udržované na webu github se můžou časem změnit. Před použitím proto šablonu vždy nejprve zkontrolujte.
   > 
   > 
6. Prohlédněte si obsah v části **resources** a všimněte si následujících parametrů:
   
   * **type**. Typ prostředku vytvořeného šablonou. V tomto případě se jedná o prostředek **Microsoft.Network/virtualNetworks**, který reprezentuje síť VNet.
   * **name**. Název prostředku. Všimněte si volání **[parameters('vnetName')]**, které znamená, že název bude dodán jako vstup uživatelem nebo ze souboru parametrů během nasazování.
   * **properties**. Seznam vlastností prostředku. Tato šablona používá během vytváření sítě VNet vlastnosti adresního prostoru a podsítě.
7. Přejděte zpět na stránku [vzorové šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klikněte na **azuredeploy-paremeters.json** a potom klikněte na **RAW**.
9. Uložte soubor do místní složky v počítači.
10. Otevřete soubor, který jste právě uložili, a upravte hodnoty parametrů. K nasazení sítě VNet popsané v tomto scénáři použijte níže uvedené hodnoty.
    
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
11. Uložte soubor.



<!--HONumber=Nov16_HO2-->


