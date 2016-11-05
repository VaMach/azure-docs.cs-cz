## Vytvoření centra událostí
1. Přihlaste se na web [Azure Portal][Azure Portal] a v levém horním rohu obrazovky klikněte na **Nový**.
2. Klikněte na **Data + Analýza** a poté klikněte na **Event Hubs**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)
3. V okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)
4. Po kontrole, že je název oboru názvů k dispozici, zvolte cenovou úroveň (Basic nebo Standard). Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. 
5. Kliknutím na **Vytvořit** vytvoříte obor názvů.
6. V seznamu oborů názvů Event Hubs klikněte na nově vytvořený obor názvů.      
   
    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)
7. V okně oboru názvů klikněte na **Event Hubs**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)
8. V horní části okna klikněte na **Přidat centrum událostí**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)
9. Zadejte název centra událostí a poté klikněte na **Vytvořit**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)
10. V seznamu Event Hubs klikněte na název nově vytvořeného centra událostí. 
    
     ![](./media/event-hubs-create-event-hub/create-event-hub6.png)
11. Po návratu do okna oboru názvů (ne v okně konkrétního centra událostí) klikněte na **Zásady sdíleného přístupu** a poté klikněte na **RootManageSharedAccessKey**.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub7.png)
12. Kliknutím na tlačítko kopírovat zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky. Uložte tento připojovací řetězec k pozdějšímu použití v tomto kurzu.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Centrum událostí je teď vytvořené a vy máte připojovací řetězce, které potřebujete k odesílání a příjmu událostí.

[Azure Portal]: https://portal.azure.com/

<!--HONumber=Sep16_HO3-->


