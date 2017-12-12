Pokud chcete začít používat entity zasílání zpráv služby Service Bus v Azure, musíte nejprve vytvořit obor názvů s jedinečným názvem v rámci Azure. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

Vytvoření oboru názvů:

1. Přihlaste se k webu [Azure Portal][Azure portal].
2. V levém navigačním podokně portálu klikněte na **+ vytvořit prostředek**, pak klikněte na tlačítko **Enterprise integrace**a potom klikněte na **Service Bus**.
3. V dialogovém okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.
4. Po kontrole, že je název oborů názvů k dispozici, zvolte cenovou úroveň (Basic, Standard nebo Premium).
5. V poli **Předplatné** zvolte předplatné Azure, ve které chcete vytvořit obor názvů.
6. V poli **Skupina prostředků** zvolte existující skupinu prostředků, ve které bude obor názvů fungovat, nebo vytvořte novou.      
7. V poli **Umístění**, vyberte zemi nebo oblast, ve které by měl být oboru názvů hostován.
   
    ![Vytvoření oboru názvů][create-namespace]
8. Klikněte na možnost **Vytvořit**. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.

### <a name="obtain-the-management-credentials"></a>Získání přihlašovacích údajů pro správu
Vytvoření nového oboru názvů automaticky generuje pravidlo počáteční sdíleného přístupového podpisu (SAS) k přidružené dvojice primární a sekundární klíče, aby každý udělte oprávnění úplného řízení nad všemi aspekty oboru názvů. V tématu [Service Bus ověřování a autorizace](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) informace o tom, jak vytvořit další pravidla pomocí více omezené oprávnění pro regulární odesílateli a příjemci. Pokud chcete zkopírovat počáteční pravidlo, postupujte následovně: 

1. V seznamu oborů názvů klikněte na nově vytvořený obor názvů.
2. V okně obor názvů, klikněte na **zásady sdíleného přístupu**.
3. V **zásady sdíleného přístupu** obrazovky, klikněte na tlačítko **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. V **zásady: RootManageSharedAccessKey** okna, klikněte na tlačítko Další kopie **připojovací řetězec – primární klíč**, zkopírujte připojovací řetězec do schránky pro pozdější použití. Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.
   
    ![connection-string][connection-string]

5. Opakujte předchozí krok, zkopírujte si hodnotu pro **primární klíč** a vložte ji do dočasného umístění pro pozdější použití.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
