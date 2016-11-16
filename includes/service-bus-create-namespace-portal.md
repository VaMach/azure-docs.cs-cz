1. Přihlaste se na web [Azure Portal][Azure Portal].
2. V levém navigačním podokně portálu klikněte na **Nový**, pak klikněte na **Podniková integrace** a pak na **Service Bus**.
3. V dialogovém okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.
4. Po kontrole, že je název oborů názvů k dispozici, zvolte cenovou úroveň (Basic, Standard nebo Premium).
5. V poli **Předplatné** zvolte předplatné Azure, ve které chcete vytvořit obor názvů.
6. V poli **Skupina prostředků** zvolte existující skupinu prostředků, ve které bude obor názvů fungovat, nebo vytvořte novou.      
7. V poli **Umístění**, vyberte zemi nebo oblast, ve které by měl být oboru názvů hostován.
   
    ![Vytvoření oboru názvů][create-namespace]
8. Klikněte na možnost **Vytvořit**. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.

### <a name="obtain-the-management-credentials"></a>Získání přihlašovacích údajů pro správu
1. V seznamu oborů názvů klikněte na nově vytvořený obor názvů.
2. V okně oboru názvů klikněte na **Zásady sdíleného přístupu**.
3. V okně **Zásady sdíleného přístupu** klikněte na **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. V okně **Zásada: RootManageSharedAccessKey** klikněte na tlačítko kopírování vedle položky **Připojovací řetězec – primární klíč**, abyste zkopírovali připojovací řetězec do vaší schránky pro pozdější použití. Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.
   
    ![connection-string][connection-string]

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure Portal]: https://portal.azure.com

<!--HONumber=Nov16_HO2-->


