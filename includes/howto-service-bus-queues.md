## Co jsou fronty služby Service Bus?
Fronty služby Service Bus podporují komunikační model **zprostředkovaného zasílání zpráv**. Součásti distribuované aplikace při používání front nekomunikují navzájem přímo. Místo toho si zprávy vyměňují prostřednictvím fronty, která slouží jako zprostředkovatel. Autor zprávy (odesílatel) předá zprávu do fronty a potom pokračuje v jejím zpracování. Spotřebitel zprávy (příjemce) asynchronně přebírá zprávu z fronty a zpracovává ji. Autor nemusí čekat na odpověď od příjemce, aby mohl pokračovat se zpracováním a odesláním dalších zpráv. Fronty nabízejí doručování zpráv metodou **FIFO (First In First Out)** pro jednoho nebo několik konkurenčních spotřebitelů. To znamená, že příjemci zprávy obvykle přijímají a zpracovávají v pořadí, ve kterém byly přidány do fronty, a každou zprávu přijme a zpracuje jenom jeden příjemce zprávy.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Fronty služby Service Bus představují univerzální technologii, kterou můžete použít pro celou řadu scénářů:

* Komunikace mezi webovou rolí a rolí pracovního procesu ve vícevrstvé aplikaci Azure.
* Komunikace mezi místními aplikacemi a aplikacemi hostovanými v Azure v případě hybridního řešení.
* Komunikace mezi součástmi distribuované aplikace spuštěné místně v různých organizacích nebo odděleních organizace.

Použití fronty vám umožňuje snazší škálování aplikací a zvyšuje odolnost vaší architektury proti chybám.

## Vytvoření oboru názvů služby
Pokud chcete začít používat fronty služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů služby. Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.

Vytvoření oboru názvů:

1. Přihlaste se na [portál Azure Classic][portál Azure Classic].
2. V levém navigačním podokně portálu klikněte na **Service Bus**.
3. V dolním podokně portálu klikněte na **Vytvořit**.
   ![](./media/howto-service-bus-queues/sb-queues-03.png)
4. V dialogovém okně **Přidat nový obor názvů** zadejte název oboru názvů. Systém okamžitě kontroluje, jestli je název dostupný.   
   ![](./media/howto-service-bus-queues/sb-queues-04.png)
5. Po kontrole dostupnosti oboru názvů vyberte zemi nebo oblast, ve které chcete obor názvů hostovat (nezapomeňte použít stejnou zemi nebo oblast, ve které nasazujete svoje výpočetní prostředky).
   
   > [!IMPORTANT]
   > Vyberte **stejnou oblast**, kterou chcete zvolit pro nasazení aplikace. Tím získáte nejlepší výkon.
   > 
   > 
6. V ostatních polích dialogového okna nechte výchozí hodnoty (**Zasílání zpráv** a **Úroveň Standard**) a potom klikněte na značku zaškrtnutí OK. Systém teď vytvoří obor názvů a povolí ho. Pravděpodobně budete muset několik minut počkat, než systém zřídí prostředky pro váš účet.
   
   ![](./media/howto-service-bus-queues/getting-started-multi-tier-27.png)

Aktivace vytvořeného oboru názvů chvíli trvá, obor názvů se na portálu zobrazí hned potom. Před pokračováním počkejte, než se stav oboru názvů změní na **Aktivní**.

## Získání výchozích přihlašovacích údajů pro správu oboru názvů
Abyste mohli provádět operace správy, například vytváření fronty v novém oboru názvů, potřebujete získat přihlašovací údaje pro správu oboru názvů. Tyto přihlašovací údaje můžete získat na stránce[portál Azure Classic][portál Azure Classic].

### Získání přihlašovacích údajů pro správu na portálu
1. V levém navigačním podokně klikněte na uzel **Service Bus** a zobrazte seznam dostupných oborů názvů:   
   ![](./media/howto-service-bus-queues/sb-queues-13.png)
2. V zobrazeném seznamu vyberte právě vytvořený obor názvů:   
   ![](./media/howto-service-bus-queues/sb-queues-09.png)
3. Klikněte na **informace o připojení**.   
   ![](./media/howto-service-bus-queues/sb-queues-06.png)
4. V podokně **Informace o přístupovém připojení** najděte připojovací řetězec, který obsahuje klíč SAS a název klíče.   
   
   ![](./media/howto-service-bus-queues/multi-web-45.png)
5. Klíč si poznamenejte, nebo ho zkopírujte do schránky.

[portál Azure Classic]: http://manage.windowsazure.com




<!--HONumber=Jun16_HO2-->


