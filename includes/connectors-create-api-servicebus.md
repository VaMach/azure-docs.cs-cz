### <a name="prerequisites"></a>Požadavky
Musíte mít [Service Bus](https://azure.microsoft.com/services/service-bus/) účtu.  

Než v aplikaci logiky můžete použít váš účet Azure Service Bus, je nutné autorizovat aplikaci logiky pro připojení k účtu služby service bus. Naštěstí můžete k tomu snadno z v rámci aplikace logiky na portálu Azure.  

Tady jsou kroky k autorizaci aplikace logiky pro připojení k účtu služby Service Bus:  

1. Chcete-li vytvořit připojení k Service Bus v návrháři aplikace logiky, vyberte **zobrazit Microsoft spravované rozhraní API** v rozevíracím seznamu. Potom zadejte **služby sběrnice** do vyhledávacího pole. Vyberte aktivační události nebo akci, kterou chcete použít.  
    ![Obrázek připojení služby Service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Pokud jste nevytvořili žádné připojení k Service Bus před, budete vyzváni k zadání přihlašovacích údajů služby Service Bus. Tyto přihlašovací údaje se používají k autorizaci aplikace logiky připojit k a přístup k datům v účtu služby Service Bus. Konektor Service Bus je připojovací řetězec pro obor názvů sběrnice. Také budete potřebovat **spravovat** oprávnění. Dobrým způsobem, jak zjistit, pokud je připojovací řetězec pro obor názvů, nebo konkrétní entitu je, pokud obsahuje `EntityPath` parametr. Pokud ano, není pravé připojovací řetězec pro aplikace logiky.  
    ![Připojovací řetězec sběrnice služeb](./media/connectors-create-api-servicebus/connectionstring.png)
3. Po přijetí připojovací řetězec pro obor názvů, můžete pro připojení k rozhraní API v Logic Apps.  
    ![Obrázek připojení služby Service Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Všimněte si vytvořil připojení, a je nyní můžete pokračovat v dalších krocích v aplikaci logiky.  
    ![Obrázek připojení služby Service Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

