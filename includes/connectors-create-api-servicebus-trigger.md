Tady je postup použití **Service Bus - při příjmu zprávy ve frontě** aktivační událost zahájíte pracovní postup aplikace logiky odeslání novou položku do fronty Service Bus.  

> [!NOTE]
> Zobrazí se výzva k Přihlaste se pomocí vaší připojovací řetězec sběrnice služeb, pokud jste ještě nevytvořili připojení k Service Bus.  
> 
> 

1. Do vyhledávacího pole v návrháři aplikace logiky, zadejte **služby sběrnice**. Vyberte **Service Bus - při příjmu zprávy ve frontě** aktivační události.  
   ![Service Bus aktivační událost obrázek 1](./media/connectors-create-api-servicebus/trigger-1.png)   
2. **Při příjmu zprávy ve frontě** se zobrazí dialogové okno.  
   ![Service Bus aktivační událost obrázek 2](./media/connectors-create-api-servicebus/trigger-2.png)   
3. Zadejte název fronty Service Bus chcete aktivační události k monitorování.   
   ![Obrázek aktivační události služby Service Bus 3](./media/connectors-create-api-servicebus/trigger-3.png)   

Aplikace logiky v tomto okamžiku je nakonfigurovaná s aktivační událost. Po přijetí nové položky ve frontě, které jste vybrali aktivační událost zahájíte spustit další aktivační události a akcí v pracovním postupu.    

