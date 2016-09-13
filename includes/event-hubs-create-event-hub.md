## Vytvoření centra událostí

1. Přihlaste se na web [Azure Portal][] a v levém horním rohu obrazovky klikněte na **Nový**.

2. Klikněte na **Data + Analýza** a poté klikněte na **Event Hubs**.

    ![][9]

3. V okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.

    ![][1]

4. Po kontrole, že je název oboru názvů k dispozici, zvolte cenovou úroveň (Basic nebo Standard). Zvolte také předplatné Azure, skupinu prostředků a umístění, ve kterém se má prostředek vytvořit. 

2. Kliknutím na **Vytvořit** vytvoříte obor názvů.

6. V seznamu oborů názvů Event Hubs klikněte na nově vytvořený obor názvů.

    ![][2]

7. V okně oboru názvů klikněte na **Event Hubs**.

    ![][3]

8. V horní části okna klikněte na **Přidat centrum událostí**.

    ![][4]

3. Zadejte název centra událostí a poté klikněte na **Vytvořit**.

    ![][5]

4. V seznamu Event Hubs klikněte na název nově vytvořeného centra událostí. 

    ![][6]

5. Po návratu do okna oboru názvů (ne v okně konkrétního centra událostí) klikněte na **Zásady sdíleného přístupu** a poté klikněte na **RootManageSharedAccessKey**.

    ![][7]

5. Kliknutím na tlačítko kopírovat zkopírujte připojovací řetězec **RootManageSharedAccessKey** do schránky. Uložte tento připojovací řetězec k pozdějšímu použití v tomto kurzu.

    ![][8]

Centrum událostí je teď vytvořené a vy máte připojovací řetězce, které potřebujete k odesílání a příjmu událostí.

[1]: ./media/event-hubs-create-event-hub/create-event-hub1.png
[2]: ./media/event-hubs-create-event-hub/create-event-hub2.png
[3]: ./media/event-hubs-create-event-hub/create-event-hub3.png
[4]: ./media/event-hubs-create-event-hub/create-event-hub4.png
[5]: ./media/event-hubs-create-event-hub/create-event-hub5.png
[6]: ./media/event-hubs-create-event-hub/create-event-hub6.png
[7]: ./media/event-hubs-create-event-hub/create-event-hub7.png
[8]: ./media/event-hubs-create-event-hub/create-event-hub8.png
[9]: ./media/event-hubs-create-event-hub/create-event-hub9.png

[Azure Portal]: https://portal.azure.com/


<!--HONumber=sep16_HO1-->


