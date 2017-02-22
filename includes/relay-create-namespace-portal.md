1. Přihlaste se k webu [Azure Portal][Azure portal].
2. V levém navigačním podokně portálu klikněte na **Nový**, pak klikněte na **Podniková integrace** a pak na **Relay**.
3. V dialogovém okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.
4. V poli **Předplatné** zvolte předplatné Azure, ve které chcete vytvořit obor názvů.
5. V poli **[Skupina prostředků](../articles/azure-resource-manager/resource-group-portal.md)** zvolte existující skupinu prostředků, ve které bude obor názvů fungovat, nebo vytvořte novou.      
6. V poli **Umístění**, vyberte zemi nebo oblast, ve které by měl být oboru názvů hostován.
   
    ![Vytvoření oboru názvů][create-namespace]
7. Klikněte na možnost **Vytvořit**. Systém teď vytvoří obor názvů a povolí ho. Po několika minutách systém zřídí prostředky pro váš účet.

### <a name="obtain-the-management-credentials"></a>Získání přihlašovacích údajů pro správu
1. V seznamu oborů názvů klikněte na nově vytvořený obor názvů.
2. V okně oboru názvů klikněte na **Zásady sdíleného přístupu**.
3. V okně **Zásady sdíleného přístupu** klikněte na **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. V okně **Zásada: RootManageSharedAccessKey** klikněte na tlačítko kopírování vedle položky **Připojovací řetězec – primární klíč**, abyste zkopírovali připojovací řetězec do vaší schránky pro pozdější použití. Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.
   
    ![connection-string][connection-string]

5. Opakujte předchozí krok, zkopírujte si hodnotu pro **primární klíč** a vložte ji do dočasného umístění pro pozdější použití.  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com


<!--HONumber=Feb17_HO2-->


