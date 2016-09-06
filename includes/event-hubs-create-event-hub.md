## Vytvoření centra událostí

1. Přihlaste se na [portál Azure Classic][] a v dolní části obrazovky klikněte na **NOVÝ**.

2. Klikněte na **App Services**, potom na **Service Bus**, **Centrum událostí**, a **Rychle vytvořit**.

    ![][1]

3. Zadejte název centra událostí, vyberte požadovanou oblast a potom klikněte na tlačítko **Create a new Event Hub** (Vytvořit nové centrum událostí).

    ![][2]

4. Pokud jste v dané oblasti explicitně nevybrali existující obor názvů, portál vám vytvoří nový (většinou ***název_centra_událostí*-ns**). Klikněte na tento obor názvů (v tomto příkladu **centrum_událostí-ns**).

    ![][3]

5. V dolní části stránky klikněte na **Informace o připojení**. Klikněte na tlačítko Kopírovat (zobrazené na následujícím obrázku) a zkopírujte do schránky řetězec připojení **RootManageSharedAccessKey**. Uložte tento připojovací řetězec k pozdějšímu použití v tomto kurzu.

    ![][4]

Centrum událostí je teď vytvořené a vy máte připojovací řetězce, které potřebujete k odesílání a příjmu událostí.

[1]: ./media/event-hubs-create-event-hub/create-event-hub1.png
[2]: ./media/event-hubs-create-event-hub/create-event-hub2.png
[3]: ./media/event-hubs-create-event-hub/create-event-hub3.png
[4]: ./media/event-hubs-create-event-hub/create-conn-str1.png

[portál Azure Classic]: https://manage.windowsazure.com/


<!--HONumber=ago16_HO5-->


