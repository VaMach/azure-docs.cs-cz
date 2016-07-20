## Co jsou témata a předplatné služby Service Bus?

Témata a předplatné služby Service Bus podporují komunikační model zasílání zpráv *publikování/přihlášení*. Součásti distribuované aplikace při používání témat a předplatných nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím tématu, které slouží jako zprostředkovatel.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

Na rozdíl od front služby Service Bus, ve kterých každou zprávu zpracuje jeden spotřebitel, témata a předplatné nabízejí komunikaci v podobě „1:N“ a používají vzor publikování/přihlášení. K jednomu tématu můžete zaregistrovat několik předplatných. Při odeslání zprávy do tématu bude zpráva zpřístupněná všem předplatným, aby ji nezávisle zpracovala.

Předplatné tématu se podobá virtuální frontě, která obdrží kopii zpráv, které byly odeslány do tématu. Volitelně můžete zaregistrovat pravidla filtru pro téma na základě jednotlivých předplatných, což vám umožní filtrovat nebo omezit příjem zpráv pro téma podle předplatných tématu.

Témata a předplatné služby Service Bus vám umožní škálování a zpracování velkého počtu zpráv mnoha uživatelů a aplikací.

## Vytvoření oboru názvů

Pokud chcete začít používat témata a předplatné služby Service Bus v Azure, musíte nejdřív vytvořit *obor názvů služby*. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

Vytvoření oboru názvů:

1.  Přihlaste se na [portál Azure Classic][].

2.  V levém navigačním podokně portálu klikněte na **Service Bus**.

3.  V dolním podokně portálu klikněte na **Vytvořit**.   
    ![][0]

4.  V dialogovém okně **Přidat nový obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.   
    ![][2]

5.  Po kontrole dostupnosti oboru názvů vyberte zemi nebo oblast, ve které chcete obor názvů hostovat (nezapomeňte použít stejnou zemi nebo oblast, ve které nasazujete svoje výpočetní prostředky).

    > [AZURE.IMPORTANT] Vyberte **stejnou oblast**, kterou chcete zvolit pro nasazení aplikace. Tím získáte nejlepší výkon.

6.  V ostatních polích dialogového okna nechte výchozí hodnoty (**Zasílání zpráv** a **Úroveň Standard**) a potom klikněte na značku zaškrtnutí OK. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.

    ![][6]

## Získání výchozích přihlašovacích údajů pro správu oboru názvů

Abyste mohli provádět operace správy, například vytváření tématu nebo předplatného v novém oboru názvů, potřebujete získat přihlašovací údaje pro správu oboru názvů. Tyto přihlašovací údaje můžete získat na portálu.

### Získání přihlašovacích údajů pro správu na portálu

1.  V levém navigačním podokně klikněte na uzel **Service Bus** a zobrazte seznam dostupných oborů názvů:   
    ![][0]

2.  V zobrazeném seznamu vyberte právě vytvořený obor názvů:   
    ![][3]

3.  Klikněte na **Informace o připojení**.   
    ![][4]

4.  V dialogovém okně **Informace o přístupovém připojení** najděte připojovací řetězec, který obsahuje klíč SAS a název klíče. Tyto hodnoty si poznamenejte, protože je budete později potřebovat k provádění operací s oborem názvů. 


  [portál Azure Classic]: http://manage.windowsazure.com
  [0]: ./media/howto-service-bus-topics/sb-queues-13.png
  [2]: ./media/howto-service-bus-topics/sb-queues-04.png
  [3]: ./media/howto-service-bus-topics/sb-queues-09.png
  [4]: ./media/howto-service-bus-topics/sb-queues-06.png
  
  [6]: ./media/howto-service-bus-topics/getting-started-multi-tier-27.png




<!--HONumber=Jun16_HO2-->


