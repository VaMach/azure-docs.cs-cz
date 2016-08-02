<properties
  pageTitle="Nejčastější dotazy ke službě Azure IoT Suite | Microsoft Azure"
  description="Nejčastější dotazy k sadě IoT Suite"
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="get-started-article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="05/09/2016"
  ms.author="araguila"/>
   
# Nejčastější dotazy k sadě IoT Suite

### Jaký je rozdíl mezi odstranění skupiny prostředků na portálu Azure a kliknutím na Odstranit v předkonfigurovaném řešení na stránkách azureiotsuite.com?

- Pokud odstraníte předkonfigurované řešení na stránkách [azureiotsuite.com][lnk azureiotsuite], odstraníte všechny prostředky, které byly zřízeny při vytváření předkonfigurovaného řešení; a pokud jste do skupiny prostředků přidali další prostředky, odstraní se také. 

- Při odstranění skupinu prostředků na [portálu Azure][lnk-azure-portal] se odstraní pouze prostředky v této skupině prostředků; bude také nutné odstranit aplikaci Azure Active Directory přidruženou k předkonfigurovanému řešení na [portálu Azure Classic][lnk-classic-portal].

### Kolik instancí služby DocumentDB můžu zřídit v jednom předplatném?

Pět. Můžete vytvořit [lístek podpory Azure][link-azuresupportticket], kterým se tento limit zvýší, ve výchozím nastavení ale v jednom předplatném můžete zřídit pouze pět instancí služby DocumentDB. V důsledku toho můžete v daném předplatném zřídit nanejvýš pět předkonfigurovaných řešení vzdáleného monitorování.

### Kolik bezplatných rozhraní API Map Bing můžu zřídit v jednom předplatném?

Dvě. V jednom předplatném můžete vytvořit pouze dvě bezplatná rozhraní API Map Bing. U bezplatného rozhraní API Map Bing se ve výchozím nastavení zřídí řešení vzdáleného monitorování. V důsledku toho můžete v daném předplatném zřídit nanejvýš dvě předkonfigurovaná řešení vzdáleného monitorování bez možnosti úprav.

### Mám nasazené řešení vzdáleného monitorování se statickou mapou, jak přidám interaktivní mapu Bing? 
1. QueryKey pro rozhraní API Map Bing pro podniky získáte na [portálu Azure][lnk-azure-portal]: 
 1. Přejděte do skupiny prostředků, ve které se na [portálu Azure][lnk-azure-portal] nachází rozhraní API Map Bing pro podniky.
 2. Klikněte na tlačítko Všechna nastavení a potom na Správu klíčů. 
 3. Můžete si všimnout dvou klíčů: MasterKey a QueryKey. Zkopírujte hodnotu pro QueryKey.

     > [AZURE.NOTE] Nemáte účet rozhraní API Map Bing pro podniky? Vytvořte si ho na [portálu Azure][lnk-azure-portal] tím, že kliknete na + Nové, vyhledáte rozhraní API Map Bing pro podniky a budete postupovat podle pokynů pro vytvoření.

2. Nejnovější kód si stáhněte ze stránek [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].

3. Spusťte místní nebo cloudové nasazení podle pokynů k nasazení z příkazového řádku ve složce /docs/ v úložišti. 

4. Po spuštění místního nebo cloudového nasazení vyhledejte v kořenové složce soubor *. user.config vytvořený během nasazení. Otevřete tento soubor v textovém editoru. 

5. Změňte následující řádek tak, aby obsahoval hodnotu zkopírovanou z klíče QueryKey: 
   
  `<setting name="MapApiQueryKey" value="" />`

### Můžu vytvořit předkonfigurované řešení, když mám Microsoft Azure pro DreamSpark?
V současné době nelze pomocí účtu [Microsoft Azure pro DreamSpark][lnk-dreamspark] vytvářet předkonfigurovaná řešení. Během několika minut si ale můžete vytvořit [bezplatný zkušební účet Azure][lnk-30daytrial], který vám umožní vytvořit předkonfigurované řešení.

### Jak se odstraním klienta AAD?

Informace najdete v příspěvku blogu od Erica Golpeho [Walkthrough of Deleting an Azure AD Tenant (Průvodce odstraněním klienta Azure AD)][lnk-delete-aad-tennant].

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[lnk-azure-portal]: https://portal.azure.com
[lnk azureiotsuite]: https://www.azureiotsuite.com/
[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx



<!--HONumber=Jun16_HO2-->


