## Co jsou témata a předplatné služby Service Bus?

Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*. Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení. K jednomu tématu můžete zaregistrovat několik předplatných. Při odeslání zprávy do tématu bude zpráva zpřístupněná všem předplatným, aby ji nezávisle zpracovala.

Předplatné tématu se podobá virtuální frontě, která obdrží kopii zpráv, které byly odeslány do tématu. Volitelně můžete zaregistrovat pravidla filtru pro téma na základě jednotlivých předplatných, což vám umožní filtrovat nebo omezit příjem zpráv pro téma podle předplatných tématu.

Témata a předplatné služby Service Bus vám umožní škálování a zpracování velkého počtu zpráv mnoha uživatelů a aplikací.

## Vytvoření oboru názvů

Pokud chcete začít používat témata a předplatné služby Service Bus v Azure, musíte nejdřív vytvořit *obor názvů služby*. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

Vytvoření oboru názvů:

1. Přihlaste se k webu [Azure Portal][].

2. V levém navigačním podokně portálu klikněte na **Nový**, pak klikněte na **Podniková integrace** a pak na **Service Bus**.

4. V dialogovém okně **Vytvořit obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.

5. Po kontrole, že je název oborů názvů k dispozici, zvolte cenovou úroveň (Basic, Standard nebo Premium).

7. V poli **Předplatné** zvolte předplatné Azure, ve které chcete vytvořit obor názvů.

9. V poli **Skupina prostředků** zvolte existující skupinu prostředků, ve které bude obor názvů fungovat, nebo vytvořte novou.      

8. V poli **Umístění**, vyberte zemi nebo oblast, ve které by měl být oboru názvů hostován.

    ![Vytvoření oboru názvů][create-namespace]

6. Klikněte na tlačítko **Vytvořit**. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.
 
### Získání přihlašovacích údajů

1. V seznamu oborů názvů klikněte na nově vytvořený obor názvů.
 
3. V okně **Obor názvů Service Bus** klikněte na **Zásady sdíleného přístupu**.

4. V okně **Zásady sdíleného přístupu** klikněte na **RootManageSharedAccessKey**.

    ![connection-info][connection-info]

5. V okně **Zásada: RootManageSharedAccessKey** klikněte na tlačítko kopírování vedle položky **Připojovací řetězec – primární klíč**, abyste zkopírovali připojovací řetězec do vaší schránky pro pozdější použití.

    ![connection-string][connection-string]

[Azure Portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Sep16_HO3-->


