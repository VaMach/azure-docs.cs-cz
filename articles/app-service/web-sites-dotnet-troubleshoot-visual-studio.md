---
title: "Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio"
description: "Zjistěte, jak řešení webové aplikace Azure pomocí vzdálené ladění, trasování a protokolování nástroje, které jsou součástí sady Visual Studio 2013."
services: app-service
documentationcenter: .net
author: tdykstra
manager: erikre
editor: 
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.author: rachelap
ms.openlocfilehash: e42ff64fdd2be87fc19be267d4e2a29e38f67ef5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-a-web-app-in-azure-app-service-using-visual-studio"></a>Řešení potíží s webovou aplikaci v Azure App Service pomocí sady Visual Studio
## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak používat nástroje Visual Studio, které pomáhají ladit webové aplikace ve [služby App Service](http://go.microsoft.com/fwlink/?LinkId=529714), protože běží na [režimu ladění](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) vzdáleně nebo zobrazením protokoly aplikací a protokoly webového serveru.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

Naučíte se:

* Funkcích správy, které Azure webové aplikace jsou k dispozici v sadě Visual Studio.
* Jak provádějte rychlé změny ve vzdálené webové aplikaci pomocí sady Visual Studio vzdálené zobrazení.
* Jak spustit režim ladění vzdáleně při projektu běží v Azure, pro webovou aplikaci a pro webovou úlohu.
* Postup vytvoření aplikace protokoly trasování a zobrazit při aplikace je jejich vytváření.
* Postup zobrazení protokolů webového serveru, včetně podrobné chybové zprávy a trasování neúspěšných žádostí.
* Postup odesílání diagnostických protokolů do služby Azure Storage účtu a zobrazit je.

Pokud máte Visual Studio Ultimate, můžete také použít [IntelliTrace](http://msdn.microsoft.com/library/vstudio/dd264915.aspx) pro ladění. V tomto kurzu nepopisuje IntelliTrace.

## <a name="prerequisites"></a>Požadavky
V tomto kurzu funguje s vývojového prostředí, webový projekt a Azure webové aplikace, které jste nastavili v [Začínáme s Azure a ASP.NET][GetStarted]. Pro webové části, musíte aplikaci, která vytvoříte v [Začínáme s Azure WebJobs SDK][GetStartedWJ].

Ukázky kódu, které jsou uvedené v tomto kurzu jsou pro MVC C# webovou aplikaci, ale řešení problémů s postupy jsou stejné pro aplikace Visual Basic a webových formulářů.

Kurz předpokládá, že používáte Visual Studio 2015 nebo 2013. Pokud používáte Visual Studio 2013, funkce webové úlohy vyžadují [aktualizace 4](http://go.microsoft.com/fwlink/?LinkID=510314) nebo novější.

Protokoly streamování funkce funguje pouze pro aplikace, které cílí na rozhraní .NET Framework 4 nebo novější.

## <a name="sitemanagement"></a>Správa a konfigurace webové aplikace
Poskytuje přístup k podmnožině funkce správy webové aplikace a nastavení konfigurace, které jsou k dispozici v sadě Visual Studio [portálu Azure](http://go.microsoft.com/fwlink/?LinkId=529715). V této části se zobrazí, co je dostupné pomocí **Průzkumníka serveru**. Vyzkoušet nejnovější funkce integrace se službou Azure najdete **Průzkumník cloudu** také. Můžete otevřít ze systému windows **zobrazení** nabídky.

1. Pokud už nejste přihlášení k Azure v sadě Visual Studio, klikněte **připojit k Azure** tlačítka na **Průzkumníka serveru**.

    Alternativou je nainstalovat certifikát správy, který umožňuje přístup k vašemu účtu. Pokud se rozhodnete nainstalovat certifikát, klikněte pravým tlačítkem **Azure** uzlu v **Průzkumníka serveru**a potom klikněte na **spravovat a odběry filtru** v místní nabídce. V **spravovat předplatná Azure** dialogové okno, klikněte **certifikáty** a pak klikněte **Import**. Postupujte podle pokynů ke stažení a následným importem souboru předplatné (také nazývané *.publishsettings* souboru) pro účet Azure.

   > [!NOTE]
   > Pokud chcete stáhnout soubor předplatné, uložit do složky mimo adresáře zdrojový kód (například ve složce soubory ke stažení) a pak odstraňte po dokončení importu. Uživatel se zlými úmysly, který získá přístup k souboru předplatného můžete upravit, vytvořit a odstranit služeb Azure.
   >
   >

    Další informace o připojení k prostředkům Azure ze sady Visual Studio najdete v tématu [Správa účtů, odběry a správu role](http://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert).
2. V **Průzkumníka serveru**, rozbalte položku **Azure** a rozbalte **služby App Service**.
3. Rozbalte skupinu prostředků, která zahrnuje webovou aplikaci, kterou jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted]a potom klikněte pravým tlačítkem na uzel webového aplikace a klikněte na tlačítko **nastavení zobrazení**.

    ![Nastavení zobrazení v Průzkumníku serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Webové aplikace Azure** se zobrazí karta a uvidíte existuje webové aplikace správu a konfiguraci úlohy, které jsou k dispozici v sadě Visual Studio.

    ![Azure okna webové aplikace](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    V tomto kurzu budete používat k protokolování a trasování rozevírací seznamy. Budete používat vzdálené ladění ale budete používat jinou metodu povolit.

    Informace o nastavení aplikace a připojovacích řetězců polí v tomto okně najdete v tématu [Azure Web Apps: fungování řetězců aplikace a připojovacích řetězců](http://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

    Pokud chcete provést úlohu správy webové aplikace, která není možné v tomto okně, klikněte na tlačítko **otevřít na portálu pro správu** otevřete okno prohlížeče na portál Azure.

## <a name="remoteview"></a>Přístup k webové aplikaci souborům v Průzkumníku serveru
Obvykle nasazení webového projektu s `customErrors` příznak v souboru Web.config nastaven `On` nebo `RemoteOnly`, tzn., neobdržíte užitečné chybová zpráva, pokud něco pokazí. Pro mnoho chyb všechny, které máte je jako jeden z následujících těm, které jsou na stránce.

**Chyba serveru v aplikaci '/':**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Došlo k chybě:**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Stránku nelze zobrazit stránku**

![Neužitečné chybová stránka](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Často je nejjednodušší způsob, jak najít příčinu chyby povolit podrobné chybové zprávy, které první předchozí snímky obrazovky vysvětluje, jak to provést. Který vyžaduje změnu v nasazeném souboru Web.config. Může upravit *Web.config* v projektu soubor a znovu nasaďte projekt nebo vytvořte [transformaci Web.config](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) a nasadit sestavení ladicí verze, ale existuje rychlejší způsob: v **Průzkumníku řešení**  přímo můžete zobrazit a upravit soubory ve vzdálené webové aplikaci pomocí *vzdálené zobrazení* funkce.

1. V **Průzkumníka serveru**, rozbalte položku **Azure**, rozbalte položku **služby App Service**, rozbalte skupinu prostředků, umístěné ve vaší webové aplikace a pak rozbalte uzel vaší webové aplikace.

    Zobrazí uzly, které umožňují přístup k obsahu souborů a souborů protokolů webové aplikace.
2. Rozbalte **soubory** uzel a klikněte dvakrát *Web.config* souboru.

    ![Otevřete soubor Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio otevře soubor Web.config ze vzdálené webové aplikace a ukazuje [vzdálené] vedle názvu souboru v záhlaví.
3. Přidejte následující řádek na `system.web` element:

    `<customErrors mode="Off"></customErrors>`

    ![Upravit soubor Web.config](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Aktualizujte stránku prohlížeče, který se zobrazuje neužitečné chybovou zprávu, a teď se podrobná chybová zpráva, jako je například v následujícím příkladu:

    ![Podrobná chybová zpráva](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Chyba zobrazí byl vytvořen tak, že přidáte řádku zobrazené červeně k *Views\Home\Index.cshtml*.)

Úprava souboru Web.config je pouze jeden příklad scénáře, ve kterých zkontrolujte možnost číst a upravovat soubory na vaše webové aplikace Azure při odstraňování problémů.

## <a name="remotedebug"></a>Vzdálené ladění webových aplikací
Pokud Podrobná chybová zpráva neposkytuje dostatek informací, a nelze znovu vytvořit chyba místně, jiný způsob řešení je spustit v režimu ladění vzdáleně. Můžete nastavit zarážky, zpracovávají paměti přímo, krok prostřednictvím kódu a i změňte cestu k kódu.

Vzdálené ladění nefunguje, pokud je v edice Express sady Visual Studio.

V této části ukazuje, jak ladit vzdáleně pomocí projektu vytvoříte v [Začínáme s Azure a ASP.NET][GetStarted].

1. Otevřete webový projekt, který jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted].
2. Otevřete *Controllers\HomeController.cs*.
3. Odstranit `About()` metoda a vložte následující kód na příslušné místo.

        public ActionResult About()
        {
            string currentTime = DateTime.Now.ToLongTimeString();
            ViewBag.Message = "The current time is " + currentTime;
            return View();
        }
4. [Nastavit zarážky](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na `ViewBag.Message` řádku.
5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **publikovat**.
6. V **profil** rozevíracího seznamu, vyberte stejný profil, že jste použili v [Začínáme s Azure a ASP.NET][GetStarted].
7. Klikněte na tlačítko **nastavení** kartě a změňte **konfigurace** k **ladění**a potom klikněte na **publikovat**.

    ![Publikování v režimu ladění](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)
8. Po dokončení nasazení a na adresu URL Azure vaší webové aplikace se zobrazí prohlížeč zavřete prohlížeč.
9. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace a pak klikněte na tlačítko **připojit ladicí program**.

    ![Připojit ladicí program](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    V prohlížeči se automaticky otevře na domovskou stránku běžící v Azure. Můžete chtít počkejte 20 sekund nebo tak Azure slouží k nastavení serveru pro ladění. Tato prodleva situace nastane pouze při prvním spuštění v režimu ladění na webovou aplikaci. Následné čas během následujících 48 hodin, při prvním spuštění ladění znovu existuje nebude zpoždění.

    **Poznámka:** Pokud máte jakékoli potíže při spuštění ladicího programu, zkuste to pomocí **Průzkumník cloudu** místo **Průzkumníka serveru**.
10. Klikněte na tlačítko **o** v nabídce.

     Visual Studio zastaví na zarážce a kód běží v Azure, není v místním počítači.
11. Najeďte myší `currentTime` proměnné a zobrazit tak hodnotu času.

     ![Zobrazení proměnné v režimu ladění běžící v Azure](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

     Zobrazí čas je čas Azure serveru, který může být v jiném časovém pásmu než místního počítače.
12. Zadejte novou hodnotu `currentTime` proměnné, jako je například "Teď spuštění v Azure".
13. Stisknutím klávesy F5 dál běžet.

     Na stránce o spuštění v Azure se zobrazí nová hodnota, kterou jste zadali do proměnné aktualnicas.

     ![O stránce s novou hodnotou](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remotedebugwj"></a>Vzdálené ladění webové úlohy
V této části ukazuje, jak ladit vzdáleně pomocí projektu a webové aplikace, které vytvoříte v [Začínáme s Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).

Funkce uvedené v této části jsou k dispozici pouze v sadě Visual Studio 2013 s aktualizací 4 nebo novější.

Vzdálené ladění pracuje pouze s nepřetržité webové úlohy. Naplánované i na vyžádání webové úlohy se nepodporuje ladění.

1. Otevřete webový projekt, který jste vytvořili v [Začínáme s Azure WebJobs SDK][GetStartedWJ].
2. Otevřete v projektu ContosoAdsWebJob *Functions.cs*.
3. [Nastavit zarážky](http://www.visualstudio.com/get-started/debug-your-app-vs.aspx) na prvním příkazem v `GnerateThumbnail` metoda.

    ![Sada zarážek](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt webové (nikoli projekt webové úlohy) a klikněte na tlačítko **publikovat**.
5. V **profil** rozevíracího seznamu, vyberte stejný profil, že jste použili v [Začínáme s Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki).
6. Klikněte na tlačítko **nastavení** kartě a změňte **konfigurace** k **ladění**a potom klikněte na **publikovat**.

    Visual Studio nasadí web a webové úlohy projekty a otevře prohlížeč na adresu URL Azure vaší webové aplikace.
7. V **Průzkumníka serveru** rozbalte **Azure > aplikační služby > vaší skupiny prostředků > vaší webové aplikace > webové úlohy > souvislý**a potom klikněte pravým tlačítkem na **ContosoAdsWebJob**.
8. Klikněte na tlačítko **připojit ladicí program**.

    ![Připojit ladicí program](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    V prohlížeči se automaticky otevře na domovskou stránku běžící v Azure. Můžete chtít počkejte 20 sekund nebo tak Azure slouží k nastavení serveru pro ladění. Tato prodleva situace nastane pouze při prvním spuštění v režimu ladění na webovou aplikaci. Při příštím připojit ladicí program existuje nebudou zpoždění, pokud je to provést v rámci 48 hodin.
9. Ve webovém prohlížeči, který se otevírá na domovskou stránku Contoso Ads vytvořte nové reklamy.

    Vytváření ad způsobí, že zprávu fronty bude vytvořen, která bude zachyceny pomocí vytvářené webové úlohy a zpracovat. Když WebJobs SDK volá funkci pro zpracování zprávy ve frontě, se setkají kód vaší zarážce.
10. Při ladicího programu dělí na vaší zarážce, můžete zkontrolovat a změnit hodnoty proměnné cloudu spuštěného programu. Na následujícím obrázku ladicí program zobrazí obsah blobInfo objektu, který byl předaný metodě GenerateThumbnail.

     ![objekt blobInfo v ladicím programu](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)
11. Stisknutím klávesy F5 dál běžet.

     Metoda GenerateThumbnail dokončí vytváření miniaturu.
12. V prohlížeči indexovou stránku aktualizujte a zobrazí miniaturu.
13. V sadě Visual Studio stiskněte klávesu SHIFT + F5 ukončete ladění.
14. V **Průzkumníka serveru**, klikněte pravým tlačítkem na uzel ContosoAdsWebJob a klikněte na **zobrazení řídicího panelu**.
15. Přihlaste se pomocí přihlašovacích údajů Azure a potom klikněte na název webové úlohy přejdete na stránku pro vaše webová úloha.

     ![Klikněte na tlačítko ContosoAdsWebJob](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Řídicího panelu ukazuje, že funkce GenerateThumbnail nedávno provést.

     (Při příštím kliknutí na tlačítko **zobrazení řídicího panelu**, nemusíte se přihlásit a prohlížeč přejde přímo na stránku pro vaše webová úloha.)
16. Klikněte na název funkce zobrazíte podrobnosti o spuštění funkce.

     ![Podrobností funkce](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Pokud funkce [napsali protokoly](https://github.com/Azure/azure-webjobs-sdk/wiki), uživatel může klepnout **ToggleOutput** k jejich zobrazení.

## <a name="notes-about-remote-debugging"></a>Poznámky o vzdálené ladění
* Spuštění v režimu ladění v produkčním prostředí se nedoporučuje. Pokud vaše produkční webová aplikace není škálovat na více instancí serveru, ladění nebude možné webového serveru neodpovídá na požadavky ostatních požadavků. Pokud máte několika instancemi webového serveru, pokud připojíte k ladicího programu získáte náhodných instance a mít žádný způsob, jak zajistit, že následné prohlížeče požadavky přejde do této instance. Navíc obvykle nenasazujete sestavení ladicí verze do produkčního prostředí a optimalizace kompilátoru pro verzi sestavení může znemožnit zobrazit, co se děje řádek po řádku ve zdrojovém kódu. Při řešení problémů produkční, je nejlepší prostředku aplikace protokoly trasování a webového serveru.
* Vyhněte se dlouho se zastavuje na zarážky při vzdáleném ladění. Azure zpracovává proces, který je zastavena po dobu delší než několik minut jako reagovat proces a ukončí se.
* Při ladění, server, který odesílá data na Visual Studio, které by mohly ovlivnit poplatky šířky pásma. Informace o sazbách šířky pásma najdete v tématu [Azure – ceny](https://azure.microsoft.com/pricing/calculator/).
* Ujistěte se, že `debug` atribut `compilation` element v *Web.config* soubor je nastaven na hodnotu true. Je nastaven na hodnotu true ve výchozím nastavení při publikování konfiguraci sestavení ladění.

        <system.web>
          <compilation debug="true" targetFramework="4.5" />
          <httpRuntime targetFramework="4.5" />
        </system.web>
* Pokud zjistíte, že nebude ladicího programu kroku do kódu, který chcete ladit, budete možná muset změnit nastavení pouze můj kód.  Další informace najdete v tématu [omezit zanoříte se pouze můj kód](http://msdn.microsoft.com/library/vstudio/y740d9d3.aspx#BKMK_Restrict_stepping_to_Just_My_Code).
* Časovač se spustí na serveru, když povolíte funkci vzdáleného ladění a po 48 hodinách je tato funkce automaticky vypnuta. Tento limit 48 hodin se provádí z důvodů zabezpečení a výkonu. Můžete snadno zapnout funkci zpět jako tolikrát, kolikrát chcete. Doporučujeme ponechat zakázané při nejsou aktivně ladění.
* Ladicí program můžete ručně připojit k libovolnému procesu, ne jenom webové aplikace proces (w3wp.exe). Další informace o tom, jak používat režim ladění v sadě Visual Studio najdete v tématu [ladění v sadě Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx).

## <a name="logsoverview"></a>Diagnostické protokoly – přehled
Aplikace ASP.NET, která běží ve webové aplikace Azure můžete vytvořit následující typy protokolů:

* **Protokoly trasování aplikací**<br/>
  Aplikace vytvoří tyto protokoly voláním metod [System.Diagnostics.Trace](http://msdn.microsoft.com/library/system.diagnostics.trace.aspx) třídy.
* **Protokoly webového serveru**<br/>
  Webový server vytvoří položku protokolu pro každý požadavek HTTP do webové aplikace.
* **Protokoly podrobné chybové zprávy**<br/>
  Webový server vytvoří stránku HTML se některé další informace o selhání požadavků HTTP (ty, které mít za následek stavový kód 400 nebo vyšší).
* **Protokoly trasování požadavku se nezdařilo**<br/>
  Webový server vytvoří soubor XML s informacemi o podrobného trasování pro chybné žádosti HTTP. Webový server poskytuje také soubor XSL do formátu XML v prohlížeči.

Protokolování ovlivňuje výkon webové aplikace, takže Azure vám dává možnost povolit nebo zakázat každého typu protokolu podle potřeby. Pro protokoly aplikací můžete zadat, že by měly být zapsány pouze protokoly do určité míry závažnosti. Při vytváření nové webové aplikace ve výchozím nastavení všechny protokolování je zakázána.

Protokoly se zapisují do souborů *LogFiles* složku v systému souborů webové aplikace a jsou přístupné přes FTP. Protokoly webového serveru a v protokolu aplikací můžete rovněž zapsány do účtu Azure Storage. Můžete zachovat větší objem protokolů v účtu úložiště, než je možné v systému souborů. Jste omezeny na maximálně 100 megabajtů protokolů při použití systému souborů. (Soubor systémové protokoly jsou pouze pro krátkodobé ukládání. Azure odstraní staré soubory protokolu, aby uvolnil prostor pro nové po dosažení limitu).  

## <a name="apptracelogs"></a>Vytvořit a zobrazit protokoly trasování aplikací
V této části, můžete to udělat následující úkoly:

* Přidání příkazů trasování do webového projektu, který jste vytvořili v [Začínáme s Azure a ASP.NET][GetStarted].
* Zobrazte protokoly při spusťte projekt lokálně.
* Zobrazte protokoly generovaná aplikace běžící v Azure.

Informace o tom, jak vytvořit aplikaci přihlásí webové úlohy, najdete v části [jak pracovat s Azure queue storage pomocí WebJobs SDK – jak napsat protokoly](https://github.com/Azure/azure-webjobs-sdk/wiki). Následující pokyny pro prohlížení protokolů a řídí, jak byly uloženy do Azure platí také pro protokoly aplikací, které jsou vytvořené webové úlohy.

### <a name="add-tracing-statements-to-the-application"></a>Přidání příkazů trasování do aplikace
1. Otevřete *Controllers\HomeController.cs*a nahraďte `Index`, `About`, a `Contact` metody následujícím kódem, aby bylo možné přidat `Trace` příkazy a `using` příkaz pro `System.Diagnostics`:

        public ActionResult Index()
        {
            Trace.WriteLine("Entering Index method");
            ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
            Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Index method");
            return View();
        }

        public ActionResult About()
        {
            Trace.WriteLine("Entering About method");
            ViewBag.Message = "Your app description page.";
            Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
            Trace.WriteLine("Leaving About method");
            return View();
        }

        public ActionResult Contact()
        {
            Trace.WriteLine("Entering Contact method");
            ViewBag.Message = "Your contact page.";
            Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
            Trace.WriteLine("Leaving Contact method");
            return View();
        }        
2. Přidat `using System.Diagnostics;` příkaz do horní části souboru.

### <a name="view-the-tracing-output-locally"></a>Zobrazit výstup trasování místně
1. Stisknutím klávesy F5 spusťte aplikaci v režimu ladění.

    Naslouchací proces výchozí trasování zapíše veškerý výstup trasování do **výstup** okno, společně s další výstup ladění. Následující obrázek znázorňuje výstupu trasovacích příkazů, které jste přidali do `Index` metoda.

    ![Trasování v okně ladění](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Následující kroky ukazují, jak zobrazit výstup trasování na webové stránce, bez kompilace v režimu ladění.
2. Otevřete soubor Web.config aplikace (jeden, umístěný ve složce projektu) a přidejte `<system.diagnostics>` prvek na konec souboru, těsně před uzavírací `</configuration>` element:

          <system.diagnostics>
            <trace>
              <listeners>
                <add name="WebPageTraceListener"
                    type="System.Web.WebPageTraceListener,
                    System.Web,
                    Version=4.0.0.0,
                    Culture=neutral,
                    PublicKeyToken=b03f5f7f11d50a3a" />
              </listeners>
            </trace>
          </system.diagnostics>

    `WebPageTraceListener` Umožňuje zobrazit výstup trasování procházením `/trace.axd`.
3. Přidat <a href="http://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trasování element</a> pod `<system.web>` v souboru Web.config, jako je například v následujícím příkladu:

        <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
4. Stiskněte klávesy CTRL+F5 a spusťte aplikaci.
5. Na panelu Adresa v okně prohlížeče přidat *trace.axd* na adresu URL a potom stiskněte klávesu Enter (adresa URL bude podobná http://localhost:53370/trace.axd).
6. Na **trasování aplikací** klikněte na tlačítko **zobrazit podrobnosti** na prvním řádku (ne řádek BrowserLink).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **Podrobností žádosti o** se zobrazí stránka a v **informace trasování** uvidíte výstup trasování příkazy, které jste přidali do části `Index` metoda.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Ve výchozím nastavení `trace.axd` je k dispozici pouze místně. Pokud jste chtěli zpřístupnit ze vzdálené webové aplikace, můžete přidat `localOnly="false"` k `trace` element v *Web.config* souboru, jak je znázorněno v následujícím příkladu:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Povolení však `trace.axd` v produkční webové aplikace se obecně nedoporučuje z bezpečnostních důvodů a v následujících částech se zobrazí snadný způsob, jak číst protokoly trasování v webové aplikace Azure.

### <a name="view-the-tracing-output-in-azure"></a>Zobrazit výstup trasování v Azure
1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na webový projekt a klikněte na **publikovat**.
2. V **Publikovat Web** dialogové okno, klikněte na tlačítko **publikovat**.

    Po Visual Studio publikuje aktualizace, otevře se okno prohlížeče na domovskou stránku (za předpokladu, že nebyla zrušíte **cílová adresa URL** na **připojení** karta).
3. V **Průzkumníka serveru**, klikněte pravým tlačítkem na vaší webové aplikace a vyberte **zobrazit protokoly streamování**.

    ![Protokoly streamování zobrazení v místní nabídce](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    **Výstup** okně se zobrazí připojeni ke službě protokolu streamování a přidá řádek oznámení každou minutu, které přejde bez protokolu pro zobrazení.

    ![Protokoly streamování zobrazení v místní nabídce](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. V okně prohlížeče, který ukazuje domovskou stránku aplikace, klikněte na tlačítko **kontaktujte**.

    Během pár sekund můžete přidat do trasování výstup z úrovně Chyba `Contact` metoda se zobrazí v **výstup** okno.

    ![Chyba trasování v okně výstupu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio je vzhledem k tomu, který je výchozí nastavení, když povolíte protokol sledování služby zobrazuje pouze chyby na úrovni trasování. Když vytvoříte novou Azure webovou aplikaci, všechny protokolování ve výchozím nastavení vypnutá, jako jste viděli při otevření stránky nastavení dříve:

    ![Aplikace odhlášení](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Ale když vyberete **zobrazit protokoly streamování**, Visual Studio automaticky změnit **aplikace Logging(File System)** k **chyba**, což znamená, že protokoly úroveň chyb získat ohlásil. Chcete-li zobrazit všechny protokoly trasování, můžete změnit toto nastavení **podrobné**. Když vyberete úroveň závažnosti, která je nižší než chyba, jsou rovněž uvedeny všechny protokoly pro vyšší úrovně závažnosti. Proto když vyberete podrobné, je také zobrazit informace, upozornění a protokoly chyb.  

1. V **Průzkumníka serveru**, klikněte pravým tlačítkem na webovou aplikaci a pak klikněte na tlačítko **nastavení zobrazení** stejně jako dříve.
2. Změna **protokolování aplikace (systém souborů)** k **podrobné**a potom klikněte na **Uložit**.

    ![Nastavení úrovně trasování na podrobné](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
3. V okně prohlížeče, který se teď zobrazuje vaše **obraťte se na** klikněte na tlačítko **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

    Během pár sekund **výstup** v okně se zobrazí všechny vaše výstup trasování.

    ![Výstup podrobného trasování](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    V této části povolit a zakázat protokolování pomocí nástroje nastavení Azure webové aplikace. Můžete také povolit nebo zakázat trasování – moduly naslouchání úpravou souboru Web.config. Ale upravovat soubor Web.config způsobí, že doména aplikace recyklovat, zatímco povolení protokolování prostřednictvím konfiguraci webové aplikace není to udělat. Pokud problém trvá dlouho reprodukovat, nebo je přerušované, recyklace doména aplikace může "Automatická oprava" a vynutit Počkejte, dokud se stane znovu. Povolení diagnostiky v Azure neprovádí toho, abyste mohli začít okamžitě zaznamenávání informací o chybách.

### <a name="output-window-features"></a>Funkce výstup – okno
**Protokolů Azure** kartě **výstup** okno obsahuje několik tlačítek a textové pole:

![Protokoly kartě tlačítka](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Tyto provádět následující funkce:

* Vymazat **výstup** okno.
* Povolit nebo zakázat zalamování řádků.
* Spuštění nebo zastavení monitorování protokolů.
* Zadejte, protokoly, které se mají monitorovat.
* Stažení protokolů.
* Filtrujte protokoly na základě hledaný řetězec nebo regulární výraz.
* Zavřít **výstup** okno.

Pokud zadáte hledaný řetězec nebo regulární výraz, Visual Studio filtruje informace o protokolování na straně klienta. To znamená, že můžete zadat kritéria po protokoly jsou zobrazeny v **výstup** okno nebo můžete změnit kritéria filtrování bez nutnosti znovu vygenerovat protokoly.

## <a name="webserverlogs"></a>Zobrazení protokolů webového serveru
Protokoly webového serveru zaznamenejte všechny aktivitu protokolu HTTP pro webovou aplikaci. Chcete-li je zobrazit **výstup** okno, musíte je povolit pro webovou aplikaci a říct sadě Visual Studio chcete monitorovat.

1. V **konfiguraci webové aplikace Azure** karty, které jste otevřeli z **Průzkumníka serveru**, změňte protokolování webového serveru na **na**a pak klikněte na tlačítko **Uložit**.

    ![Povolení protokolování webového serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. V **výstup** okně klikněte na tlačítko **zadejte, které protokoly Azure se mají monitorovat** tlačítko.

    ![Určete, které protokoly Azure se mají monitorovat](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. V **možnosti protokolování Azure** dialogové okno, vyberte **webové protokoly serveru**a potom klikněte na **OK**.

    ![Monitorování protokolů webového serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. V okně prohlížeče, který ukazuje webové aplikace, klikněte na tlačítko **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

    V protokolech aplikací obecně zobrazit jako první, za nímž následuje protokolů webového serveru. Možná budete muset chvíli počkejte protokolů se objeví.

    ![Protokoly webového serveru v okně výstupu](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Ve výchozím nastavení když poprvé povolíte protokolů webového serveru pomocí sady Visual Studio, Azure zapisuje protokoly do systému souborů. Jako alternativu můžete portál Azure k určení tento webový server, které protokoly, které mají být zapsána do kontejneru objektů blob v účtu úložiště.

Pokud používáte portál povolit webový server k účtu úložiště Azure a pak zakažte je protokolování v sadě Visual Studio, pokud znovu povolíte protokolování v sadě Visual Studio nastavení svého účtu úložiště se obnoví.

## <a name="detailederrorlogs"></a>Zobrazit podrobné chybové zprávy protokoly
Podrobné protokoly chyb zadat nějaké další informace o požadavcích HTTP, jejichž výsledkem odpovědi kódy chyb (400 nebo novější). Chcete-li je zobrazit **výstup** okno, musíte je povolit pro webovou aplikaci a říct sadě Visual Studio chcete monitorovat.

1. V **konfiguraci webové aplikace Azure** karty, které jste otevřeli z **Průzkumníka serveru**, změňte **podrobné chybové zprávy** k **na**a potom Klikněte na tlačítko **Uložit**.

    ![Povolit podrobné chybové zprávy](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)
2. V **výstup** okně klikněte na tlačítko **zadejte, které protokoly Azure se mají monitorovat** tlačítko.
3. V **možnosti protokolování Azure** dialogové okno, klikněte na tlačítko **všechny protokoly**a potom klikněte na **OK**.

    ![Monitorování všech protokolů](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)
4. Na panelu Adresa v okně prohlížeče přidat další znak na adresu URL způsobí chybu 404 (například `http://localhost:53370/Home/Contactx`), a stiskněte klávesu Enter.

    V sadě Visual Studio se zobrazí po několik sekund protokol podrobné informace o chybě **výstup** okno.

    ![Podrobné informace o chybě přihlášení výstup – okno](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Řízení a klikněte na odkaz zobrazíte výstup protokolu ve formátu v prohlížeči:

    ![Podrobné informace o chybě protokolů v okně prohlížeče](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="downloadlogs"></a>Stáhněte si soubor protokolu systému
Všechny protokoly, které můžete sledovat v **výstup** okno lze také stáhnout jako *.zip* souboru.

1. V **výstup** okně klikněte na tlačítko **stažení protokolů streamování**.

    ![Protokoly kartě tlačítka](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Otevře Průzkumníka souborů vaší *stáhne* složku s stažený soubor vybraný.

    ![Stažený soubor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. Extrahování *.zip* soubor a v tématu následující strukturu složek:

    ![Stažený soubor](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Protokoly trasování aplikace jsou ve *.txt* soubory *LogFiles\Application* složky.
   * Protokoly webového serveru jsou v *.log* soubory *LogFiles\http\RawLogs* složky. Nástroj můžete použít jako [analyzátoru protokolů](http://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) k zobrazení a zpracování těchto souborů.
   * Podrobné chybové zprávy protokoly jsou ve *.html* soubory *LogFiles\DetailedErrors* složky.

     ( *Nasazení* složka je pro soubory vytvořené serverem správy zdrojového kódu publikování; nemá nic souvisejících s publikováním sady Visual Studio. *Git* složka je pro trasování týkající se řízení zdroj publikování a protokol streamování služba souborů.)  

## <a name="storagelogs"></a>Zobrazit protokoly úložiště
Protokoly trasování aplikací lze také poslat účet úložiště Azure a lze je zobrazit v sadě Visual Studio. Provedete-li vytvoříte účet úložiště, povolit protokol úložiště na portálu classic a zobrazit v **protokoly** kartě **webové aplikace Azure** okno.

Protokoly můžete odeslat do některého nebo všech tří cíle:

* Systém souborů.
* Tabulky účet úložiště.
* Objekty BLOB účet úložiště.

Můžete zadat úroveň závažnosti různé pro jednotlivé cíle.

Tabulky usnadňují zobrazit podrobnosti o protokoly online, a podporují streamování; se můžete dotazovat protokoly v tabulkách a v tématu nové protokoly, jako během vytváření. Objekty BLOB můžete snadno ke stažení protokolů v souborech a analyzovat je pomocí HDInsight, protože HDInsight umí pro práci s úložišti objektů blob. Další informace najdete v tématu **Hadoop a MapReduce** v [možnosti ukládání dat (vytváření reálných cloudových aplikací s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

Aktuálně máte soubor protokolu systému nastavena na podrobné úrovni; Následující postup vás provede procesem nastavení úrovně protokoly informace přejít na tabulky účet úložiště. Informace o úrovni znamená všechny protokoly vytvořená voláním `Trace.TraceInformation`, `Trace.TraceWarning`, a `Trace.TraceError` se zobrazí, ale není protokoly vytvořená voláním `Trace.WriteLine`.

Účty úložiště nabízí další úložiště a jejich uchovávání dlouhodobou pro protokoly ve srovnání s systému souborů. Další výhodou odesílání protokolů trasování aplikace do úložiště je, že dostanete doplňující informace, přičemž každý protokol, který Nezískávat z protokolů systému souborů.

1. Klikněte pravým tlačítkem na **úložiště** pod Azure uzel a pak klikněte na tlačítko **vytvořit účet úložiště**.

![Vytvořit účet úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. V **vytvořit účet úložiště** dialogové okno, zadejte název pro účet úložiště.

    Název musí být musí být jedinečné (žádný jiný účet úložiště Azure můžete mít stejný název). Pokud název, který zadáte, je již používán získáte možnost ho změnit.

    Adresa URL pro přístup k účtu úložiště bude *{name}*. core.windows.net.
2. Nastavte **oblast nebo skupinu vztahů** rozevíracího seznamu pro danou oblast nejblíže k vám.

    Toto nastavení určuje, které datové centrum Azure bude hostitelem účtu úložiště. Pro účely tohoto kurzu nebude Zkontrolujte své volby, významné rozdíly, ale pro produkční webové aplikace, aby váš webový server a účet úložiště ve stejné oblasti k minimalizaci latence a data s nimi spojeným nákladům. Webové aplikace (aplikaci, kterou vytvoříte později) měly být spuštěny v oblasti jak nejblíže do prohlížečů, aby se minimalizoval latence přístupu k vaší webové aplikace.
3. V rozevíracím seznamu **Replikace** vyberte **Místně redundantní**.
   
    Když má účet úložiště povolenou geografickou replikaci, bude se uložený obsah replikovat do sekundárního datacentra, které zajistí převzetí služeb při selhání v případě významnější havárie v primárním umístění. Geografická replikace může způsobit dodatečné náklady. V případě testovacích a vývojových účtů je zbytečné za geografickou replikaci platit. Další informace naleznete v článku o [vytvoření, správě nebo odstranění účtu úložiště](../storage/common/storage-create-storage-account.md).
4. Klikněte na možnost **Vytvořit**.

    ![Nový účet úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. V sadě Visual Studio **webové aplikace Azure** okně klikněte **protokoly** a pak klikněte **konfigurace protokolování na portálu pro správu**.

    <!-- todo:screenshot of new portal if the VS page link goes to new portal -->
    ![Konfigurace protokolování](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    Tím se otevře **konfigurace** na portálu classic pro vaši webovou aplikaci.
6. Na portálu classic **konfigurace** , přejděte do části diagnostiky aplikace a pak změňte **protokolování aplikace (Table Storage)** k **na**.
7. Změna **úroveň protokolování** k **informace**.
8. Klikněte na tlačítko **spravovat úložiště Table**.

    ![Kliknutím na spravovat TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    V **spravovat úložiště table pro rozhraní application diagnostics** pole můžete vašeho účtu úložiště, pokud máte více než jeden. Můžete vytvořit novou tabulku nebo použijte existující.

    ![Správa úložiště table](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. V **spravovat úložiště table pro rozhraní application diagnostics** pole kliknutím na značku zaškrtnutí dialogové okno zavřete.
10. Na portálu classic **konfigurace** , klikněte na **Uložit**.
11. V okně prohlížeče, které zobrazuje aplikace webové aplikace, klikněte na **Domů**, pak klikněte na tlačítko **o**a potom klikněte na **kontaktujte**.

     K účtu úložiště se zapíšou informace o protokolování, které vytvořil procházením tyto webové stránky.
12. V **protokoly** kartě **webové aplikace Azure** oken v sadě Visual Studio, klikněte na tlačítko **aktualizovat** pod **diagnostiky Souhrn**.

     ![Klikněte na tlačítko Aktualizovat](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     **Diagnostiky Souhrn** části ve výchozím nastavení zobrazuje protokoly pro posledních 15 minut. Můžete změnit dobu zobrazíte další protokoly.

     (Pokud se zobrazí chyba "Tabulka nebyla nalezena", ověřte, můžete procházet na stránky, které provádějí trasování po jste povolili **protokolování aplikace (úložiště)** a po kliknutí na **Uložit**.)

     ![Protokoly úložiště](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Všimněte si, že v tomto zobrazení můžete najdete v části **ID procesu** a **ID vlákna** pro každý protokol, který není dostupná v protokolech systému souborů. Zobrazí se další pole přímo zobrazením tabulky úložiště Azure.
13. Klikněte na tlačítko **zobrazit všechny protokoly aplikací**.

     Tabulku protokolu trasování se zobrazí v prohlížeči tabulky úložiště Azure.

     (Pokud dojde k chybě "sekvence neobsahuje žádné elementy", otevřete **Průzkumníka serveru**, rozbalte uzel pro váš účet úložiště v rámci **Azure** uzel a potom klikněte pravým tlačítkem na **tabulky**a klikněte na tlačítko **aktualizovat**.)

     ![Protokoly úložiště v zobrazení tabulky](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     Toto zobrazení uvádí další pole, že se nezobrazí v ostatních zobrazeních. Toto zobrazení můžete také filtrovat protokoly pomocí speciální dotazu Tvůrce uživatelského rozhraní pro tvorbu dotazu. Další informace najdete v tématu práci s prostředky tabulky – filtrování entity v [procházení prostředků úložiště pomocí Průzkumníka serveru](http://msdn.microsoft.com/library/ff683677.aspx).
14. Chcete-li se podívat na podrobnosti pro jeden řádek, dvakrát klikněte na jednoho z řádků.

     ![Tabulka trasování v Průzkumníku serveru](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)

## <a name="failedrequestlogs"></a>Zobrazit protokoly trasování neúspěšných žádostí.
Protokoly trasování neúspěšných žádostí jsou užitečné, pokud budete potřebovat zjistit podrobnosti o tom, jak IIS zpracovává požadavek HTTP ve scénářích, jako je například adresa URL přepisování nebo ověřování problémy.

Webové aplikace Azure pomocí stejné funkce trasování chybných požadavků, které bylo k dispozici se službou IIS 7.0 nebo novější. Nemáte přístup k nastavení služby IIS, která konfigurace, které chyby se Zaprotokolují, ale. Pokud je povoleno trasování chybných požadavků, všechny chyby zaznamenání.

Trasování chybných požadavků můžete povolit pomocí sady Visual Studio, ale nelze je zobrazit v sadě Visual Studio. Tyto protokoly jsou soubory formátu XML. Streamování služby protokolování sleduje pouze soubory, které se považují za čitelný v režimu prostého textu: *.txt*, *.html*, a *.log* soubory.

Protokoly trasování chybných požadavků můžete zobrazit v prohlížeči přímo přes FTP nebo místně po pomocí nástroje FTP je stáhnout do místního počítače. V této části budete zobrazí je v prohlížeči přímo.

1. V **konfigurace** kartě **webové aplikace Azure** okno, které jste otevřeli z **Průzkumníka serveru**, změňte **trasování chybných požadavků** na **Na**a potom klikněte na **Uložit**.

    ![Povolit trasování neúspěšných žádostí.](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Na panelu Adresa v okně prohlížeče, který ukazuje webové aplikace přidat další znak na adresu URL a klikněte na Enter způsobí chybu 404.

    To způsobí, že protokolu pro trasování chybných požadavků, který se má vytvořit, a následující kroky ukazují, jak zobrazit nebo stáhnout v protokolu.
3. V sadě Visual Studio v **konfigurace** kartě **webové aplikace Azure** okně klikněte na tlačítko **otevřít na portálu pro správu**.
4. V [portálu Azure](https://portal.azure.com) **nastavení** pro webovou aplikaci, klikněte na **přihlašovací údaje nasazení**a poté zadejte nové uživatelské jméno a heslo.

    ![Nové FTP uživatelské jméno a heslo](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    ** Při přihlášení, budete muset použít úplné uživatelské jméno s předponu k němu název webové aplikace. Například pokud zadáte "myid" jako uživatelské jméno a lokalita je "myexample", můžete přihlásit jako "myexample\myid".
5. V nové okno prohlížeče, přejděte na adresu URL, které se zobrazí v části **název hostitele FTP** nebo **FTPS hostname** v **webové aplikace** okno pro vaši webovou aplikaci.
6. Přihlaste se pomocí přihlašovacích údajů FTP, které jste vytvořili dříve (včetně webových předpona názvu aplikace pro uživatelské jméno).

    Prohlížeč zobrazí kořenové složce webové aplikace.
7. Otevřete *LogFiles* složky.

    ![Otevřete složku LogFiles](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)
8. Otevřete složku s názvem W3SVC plus číselná hodnota.

    ![Otevřete složku W3SVC](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Složka obsahuje soubory XML pro všechny chyby, které byly zaprotokolovány po povolení trasování chybných požadavků a soubor XSL, můžete prohlížeč formátu XML.

    ![W3SVC složky](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)
9. Klikněte na soubor XML pro chybných požadavků, které chcete zobrazit informace o trasování pro.

    Následující obrázek znázorňuje součástí informací o trasování pro ukázkové chybu.

    ![Trasování neúspěšných žádostí v prohlížeči](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="nextsteps"></a>Další kroky
Už víte, jak Visual Studio lze snadno zobrazit protokoly vytvořené webové aplikace Azure. Následující části obsahují odkazy na další zdroje informací na související témata:

* Řešení potíží s Azure webové aplikace
* Ladění v sadě Visual Studio
* Vzdálené ladění v Azure
* Trasování v aplikacích ASP.NET
* Analýza protokolů webového serveru
* Analýza protokolů trasování neúspěšných žádostí.
* Ladění cloudové služby

### <a name="azure-web-app-troubleshooting"></a>Řešení potíží s Azure webové aplikace
Další informace o odstraňování potíží s webovými aplikacemi ve službě Azure App Service najdete v následujících zdrojích informací:

* [Postup monitorování webové aplikace](/manage/services/web-sites/how-to-monitor-websites/)
* [Nevracení paměti v Azure Web Apps s nástroji Visual Studio 2013 příčin](http://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Microsoft ALM příspěvku na blogu o funkcích nástroje Visual Studio pro analýzu spravovat problémy s pamětí.
* [Službě Azure web apps online nástroje byste měli vědět o](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/). Příspěvek blogu společností Apple Amitu.

Nápovědu k řešení problémů s konkrétní dotaz spusťte vlákno v jednom z následujících fóra:

* [Azure fórum na webu technologie ASP.NET](http://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET).
* [Azure fórum na webu MSDN](http://social.msdn.microsoft.com/Forums/windowsazure/).
* [StackOverflow.com](http://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Ladění v sadě Visual Studio
Další informace o tom, jak používat režim ladění v sadě Visual Studio najdete v tématu [ladění v sadě Visual Studio](http://msdn.microsoft.com/library/vstudio/sc65sadd.aspx) tématu MSDN a [ladění tipy sadou Visual Studio 2010](http://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx).

### <a name="remote-debugging-in-azure"></a>Vzdálené ladění v Azure
Další informace o vzdálené ladění pro webové aplikace Azure a webové úlohy najdete v následujících zdrojích informací:

* [Úvod do vzdálené ladění webových aplikací služby Azure App Service](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Úvod do vzdáleného ladění Azure App Service Web Apps část 2 - uvnitř vzdálené ladění](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Úvod do vzdáleného ladění na Azure App Service Web Apps část 3 - prostředí s více instancemi a GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [Webové úlohy ladění (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Pokud vaše webová aplikace používá back-end Azure webového rozhraní API nebo Mobile Services a je nutné ladit, najdete v části [ladění v rozhraní .NET back-end v sadě Visual Studio](http://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx).

### <a name="tracing-in-aspnet-applications"></a>Trasování v aplikacích ASP.NET
Na Internetu nejsou k dispozici žádné důkladné a aktuální přehled trasování rozhraní ASP.NET. Které může provádět nejvhodnější je začít pracovat s původní úvodní materiály, které jsou vytvořeny pro webové formuláře protože MVC nebyla ještě neexistuje a který doplnit s novější blog odešle které se zaměřují na konkrétní problémy. Některé místa vhodná zahájíte jsou v následujících zdrojích informací:

* [Monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  Elektronická kniha kapitoly s doporučeními pro trasování v cloudu Azure aplikace.
* [Trasování rozhraní ASP.NET](http://msdn.microsoft.com/library/ms972204.aspx)<br/>
  Starý, ale stále dobrý prostředku pro základní informace o subjektu.
* [Trasování – moduly naslouchání](http://msdn.microsoft.com/library/4y5y10s7.aspx)<br/>
  Informace o trasování – moduly naslouchání, ale není zmínili [WebPageTraceListener](http://msdn.microsoft.com/library/system.web.webpagetracelistener.aspx).
* [Návod: Integrace s trasováním System.Diagnostics trasování technologie ASP.NET](http://msdn.microsoft.com/library/b0ectfxd.aspx)<br/>
  To příliš je v minulosti, ale zahrnuje některé další informace, které nezahrnuje úvodní článek.
* [Trasování v zobrazení syntaxe Razor rozhraní ASP.NET MVC](http://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Kromě toho trasování v zobrazení syntaxe Razor v příspěvku taky vysvětluje, jak vytvořit filtr chyby, aby bylo protokolování všechny neošetřených výjimek v aplikaci MVC. Informace o tom, jak protokolování všechny neošetřených výjimek v aplikaci webových formulářů, podívejte se na příklad souboru Global.asax v [kompletní příklad pro obslužné rutiny chyba](http://msdn.microsoft.com/library/bb397417.aspx) na webu MSDN. V MVC nebo webového formuláře Pokud chcete protokolovat určité výjimky, ale nechat rozhraní výchozí zpracování vstoupí v platnost, můžete zachytit a opětovné jako v následujícím příkladu:

        try
        {
           // Your code that might cause an exception to be thrown.
        }
        catch (Exception ex)
        {
            Trace.TraceError("Exception: " + ex.ToString());
            throw;
        }
* [Streamování diagnostické trasování protokolování z příkazového řádku Azure (plus balíčku Glimpse!)](http://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Postup pomocí příkazového řádku provádět jaké tento kurz ukazuje, jak provést v sadě Visual Studio. [Balíčku glimpse](http://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) je nástroj pro ladění aplikací ASP.NET.
* [Webové aplikace, protokolování a diagnostiky - pomocí David Ebbo](/documentation/videos/azure-web-site-logging-and-diagnostics/) a [protokoly z webových aplikací – s David Ebbo streamování](/documentation/videos/log-streaming-with-azure-web-sites/)<br>
  Videa Scott Hanselman a David Ebbo.

Pro protokolování chyb, je použití rozhraní protokolování open source, jako alternativu k vytvoření vlastního kódu trasování [ELMAH](http://nuget.org/packages/elmah/). Další informace najdete v tématu [příspěvky Scott Hanselman o ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx).

Všimněte si také, že nemáte pomocí technologie ASP.NET nebo System.Diagnostics trasování, pokud chcete získat protokoly z Azure streamování. Webové aplikace Azure streamování služby protokolování bude stream žádné *.txt*, *.html*, nebo *.log* soubor, který najde v *LogFiles* složky. Proto můžete vytvořit vlastní protokolování systému, který zapíše do systému souborů webové aplikace a souboru budou automaticky streamování a stáhli. Stačí je zápisu aplikační kód, který vytvoří soubory v *d:\home\logfiles* složky.

### <a name="analyzing-web-server-logs"></a>Analýza protokolů webového serveru
Další informace o analýze protokolů webového serveru najdete v následujících zdrojích informací:

* [LogParser](http://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Nástroj pro zobrazení dat v protokolů webového serveru (*.log* soubory).
* [Řešení potíží s problémy s výkonem služby IIS nebo pomocí LogParser chyb aplikací](http://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Úvod do protokolu analyzátoru nástroj, který můžete použít k analýze protokolů webového serveru.
* [Příspěvky podle Roberta Mcmurrayho na pomocí LogParser](http://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [Stavový kód HTTP ve službě IIS 7.0, IIS 7.5 a IIS 8.0](http://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Analýza protokolů trasování neúspěšných žádostí.
Zahrnuje webu Microsoft TechNet [pomocí trasování chybných požadavků](http://www.iis.net/learn/troubleshoot/using-failed-request-tracing) oddíl, který může být užitečné pro pochopení jak používat tyto protokoly. Tato dokumentace je však zaměřen především na Konfigurace trasování neúspěšných žádostí ve službě IIS, což nelze provést ve službě Azure Web Apps.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki
