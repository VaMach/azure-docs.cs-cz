Po rozšíření mají záznamy pro název domény, byste měli moct ověřte, zda vlastní název domény může být použit pro přístup k vaší webové aplikace v Azure App Service pomocí prohlížeče.

> [!NOTE]
> Může trvat nějakou dobu, než vaše CNAME rozšíří v rámci systému DNS. Je třeba použít službu <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> k ověření, že CNAME je k dispozici.
> 
> 

Pokud jste ještě nepřidali vaší webové aplikace jako koncový bod Traffic Manager, musíte to provést, před překlad bude fungovat jako název trasy vlastní domény do Traffic Manageru. Traffic Manager se pak směruje na vaší webové aplikace. Pomocí informací v [přidat nebo odstranit koncové body](../articles/traffic-manager/traffic-manager-endpoints.md) přidání webové aplikace jako koncový bod ve vašem profilu Traffic Manageru.

> [!NOTE]
> Pokud vaše webová aplikace není uveden při přidávání koncový bod, ověřte, zda je nakonfigurován pro **standardní** režimu plán služby App Service. Je nutné použít **standardní** režimu pro webovou aplikaci, aby bylo možné pracovat s nástrojem Traffic Manager.
> 
> 

1. V prohlížeči otevřete [portálu Azure](https://portal.azure.com).
2. V **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte **nastavení**a potom vyberte **vlastní domény**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. V **vlastní domény** okně klikněte na tlačítko **přidat název hostitele**.
4. Použití **Hostname** textová pole zadejte název domény Traffic Manageru přidružení k této webové aplikace.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Klikněte na tlačítko **ověřením** pro uložení konfigurace názvu domény.
6. Po kliknutí na tlačítko **ověřením** Azure bude ji pracovní postup ověření domény. To bude kontrolovat vlastnictví domény a také název hostitele dostupnosti a sestava úspěch nebo podrobné chybové s doporučený guidence o tom, jak chybu opravit.    
7. Po úspěšném ověření **přidat název hostitele** tlačítko je aktivní a bude možné přiřadit název hostitele. Nyní přejděte do vlastního názvu domény v prohlížeči. Teď byste měli vidět spuštění vaší aplikace pomocí vlastního názvu domény. 
   
   Po dokončení konfigurace nebude uvedené vlastní název domény **názvy domén** část vaší webové aplikace.

V tomto okamžiku by měl být zadejte název pro název domény Traffic Manageru v prohlížeči a zobrazit tak, že je úspěšně přejdete do vaší webové aplikace.

