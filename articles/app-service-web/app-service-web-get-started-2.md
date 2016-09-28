<properties 
    pageTitle="Přidání funkce do první webové aplikace" 
    description="Přidejte zajímavé funkce do své první webové aplikace během několika minut." 
    services="app-service\web"
    documentationCenter=""
    authors="cephalin" 
    manager="wpickett" 
    editor="" 
/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article"
    ms.date="05/12/2016" 
    ms.author="cephalin"
/>


# Přidání funkce do první webové aplikace

V části [Nasazení webové aplikace do Azure během 5 minut](app-service-web-get-started.md) jste nasadili ukázkovou webovou aplikaci do služby [Azure App Service](../app-service/app-service-value-prop-what-is.md). V tomto článku do nasazené webové aplikace rychle přidáte některé skvělé funkce. Během několik minut provedete tyto úkony:

- vynucení ověřování uživatelů
- automatické škálování aplikace
- doručování upozornění týkajících se výkonu aplikace

Tento kurz můžete absolvovat bez ohledu na to, kterou ukázkovou aplikaci jste v předchozím článku nasadili.

Tři aktivity obsažené v tomto kurzu jsou pouhou ukázkou celé řady užitečných funkcí, které získáte umístěním webové aplikace do služby App Service. Mnohé z těchto funkcí jsou k dispozici na úrovni **Free** (na této úrovni je spuštěna vaše první webová aplikace), přičemž k vyzkoušení funkcí vyžadujících vyšší cenové úrovně můžete použít zkušební kredity. Ujišťujeme vás, že webová aplikace zůstává na úrovni **Free**, dokud ji výslovně nezměníte na jinou cenovou úroveň.

>[AZURE.NOTE] Webová aplikace, již jste vytvořili pomocí rozhraní příkazového řádku Azure CLI, je spuštěna na úrovni **Free**, která umožňuje pouze jednu instanci sdíleného virtuálního počítače s kvótou prostředků. Další informace o možnostech poskytovaných na úrovni **Free** naleznete v tématu [Omezení služby App Service](../azure-subscription-service-limits.md#app-service-limits).

## Ověřování uživatelů

Nyní si ukážeme, jak snadné je přidat do aplikace ověřování (podrobnější informace naleznete v části [Ověřování/autorizace služby App Service](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)).

1. V okně portálu aplikace, které jste právě otevřeli, klikněte na možnost **Nastavení** > **Ověřování/autorizace**.  
    ![Ověřování – okno nastavení](./media/app-service-web-get-started/aad-login-settings.png)
    
2. Zapněte ověřování kliknutím na tlačítko **Zapnuto**.  
    
4. V části **Zprostředkovatelé ověřování** klikněte na možnost **Azure Active Directory**.  
    ![Ověřování – výběr možnosti Azure AD](./media/app-service-web-get-started/aad-login-config.png)

5. V okně **Nastavení služby Azure Active Directory** klikněte na možnost **Expresní** a poté klikněte na tlačítko **OK**. Výchozí nastavení vytvoří novou aplikaci Azure AD ve výchozím adresáři.  
 ![Ověřování – expresní konfigurace](./media/app-service-web-get-started/aad-login-express.png)

6. Klikněte na **Uložit**.  
    ![Ověřování – uložení konfigurace](./media/app-service-web-get-started/aad-login-save.png)

    Jakmile změna úspěšně proběhne, zobrazí se zelený zvonek indikující oznámení společně s přátelskou zprávou.

7. Zpět v okně portálu aplikace klikněte na odkaz **URL** (nebo na položku **Procházet** v řádku nabídek). Odkaz je adresa protokolu HTTP.  
    ![Ověřování – navigace na adresu URL](./media/app-service-web-get-started/aad-login-browse-click.png)  
    Po otevření aplikace na nové kartě však pole adresy URL provede opakované přesměrování a skončí u vaší aplikace s adresou protokolu HTTPS. Vidíte, že jste již přihlášeni k předplatnému Azure a jste automaticky ověřeni v aplikaci.  
    ![Ověřování – uživatel je přihlášen](./media/app-service-web-get-started/aad-login-browse-http-postclick.png)  
    Pokud tedy nyní spustíte neověřenou relaci v jiném prohlížeči a přejdete na tutéž adresu URL, otevře se obrazovka pro přihlášení.  
    <!-- ![Authenticate - login page](./media/app-service-web-get-started/aad-login-browse.png)  -->
    Pokud jste ještě nikdy nepracovali se službou Azure Active Directory, výchozí adresář pravděpodobně nebude obsahovat žádné uživatele Azure AD. V takovém případě zde nejspíše bude pouze jeden účet, a to účet Microsoft s předplatným Azure. Z tohoto důvodu jste předtím byli k aplikaci automaticky přihlášeni v tomtéž prohlížeči. Pomocí téhož účtu Microsoft se můžete přihlásit také na této přihlašovací stránce.

Blahopřejeme, nyní ověřujete veškerý provoz směřující do webové aplikace.

V okně **Ověřování/autorizace** jste si nejspíše všimli, že lze provádět mnoho dalších úkonů, například:

- Povolení přihlášení prostřednictvím sociální sítě
- Povolení více možností přihlášení
- Změna výchozího chování při první návštěvě uživatelů v aplikaci

Služba App Service poskytuje řešení na klíč pro některé běžné potřeby ověřování, a proto nemusíte sami poskytovat logiku ověřování. Další informace naleznete v tématu [Ověřování/autorizace služby App Service](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

## Automatické škálování aplikace na základě poptávky

Nyní budeme automaticky škálovat aplikaci tak, aby automaticky přizpůsobovala svou kapacitu podle poptávky uživatelů (podrobnější informace naleznete v částech [Vertikální navýšení kapacity aplikace v Azure](web-sites-scale.md) a [Ruční nebo automatické škálování počtu instancí](../azure-portal/insights-how-to-scale.md)). 

Stručně řečeno, webové aplikace lze škálovat dvěma způsoby:

- [Vertikální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Získání větší kapacity procesoru, větší paměti, většího místa na disku a speciálních funkcí, jako jsou vyhrazené virtuální počítače, vlastní domény a certifikáty, přípravné sloty, automatické škálování a další. Vertikální navýšení kapacity provádíte změnou cenové úrovně plánu služby App Service, do níž aplikace spadá.
- [Horizontální navýšení kapacity](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Zvýšení počtu instancí virtuálních počítačů, v nichž je aplikace spuštěna.
V závislosti na cenové úrovni můžete horizontálně navýšit kapacitu až na 50 instancí.

Nyní bez dalších okolků nastavíme automatické škálování.

1. Nejprve vertikálně navýšíme kapacitu tak, abychom povolili automatické škálování. V okně portálu aplikace klikněte na možnost **Nastavení** > **Vertikálně navýšit kapacitu (plán služby App Service)**.  
    ![Vertikální navýšení kapacity – okno nastavení](./media/app-service-web-get-started/scale-up-settings.png)

2. Posuňte zobrazení a vyberte úroveň **S1 Standard**, což je nejnižší úroveň podporující automatické škálování (na snímku obrazovky zakroužkováno), a klikněte na položku **Vybrat**.  
    ![Vertikální navýšení kapacity – volba úrovně](./media/app-service-web-get-started/scale-up-select.png)

    Dokončili jste vertikální navýšení kapacity.
    
    >[AZURE.IMPORTANT] Tato úroveň čerpá vaše bezplatné zkušební kredity. Pokud máte účet s platbou za použití, budou vám účtovány poplatky.
    
3. Nyní nakonfigurujeme automatické škálování. V okně portálu aplikace klikněte na možnost **Nastavení** > **Horizontální navýšení kapacity (plán služby App Service)**.  
    ![Horizontální navýšení kapacity – okno nastavení](./media/app-service-web-get-started/scale-out-settings.png)

4. Položku **Škálovat podle** změňte na možnost **Procento procesoru**. Posuvníky pod rozevíracím seznamem se aktualizují odpovídajícím způsobem. Poté definujte rozsah položky **Instance** od **1** do **2** a položku **Cílový rozsah** od **40** do **80**. To lze provést zadáním hodnoty do polí nebo přesunutím posuvníku.  
 ![Horizontální navýšení kapacity – konfigurace automatického škálování](./media/app-service-web-get-started/scale-out-configure.png)
    
    Na základě této konfigurace vaše aplikace automaticky horizontálně navýší kapacitu, překročí-li využití procesoru 80 %, a sníží kapacitu, poklesne-li využití procesoru pod 40 %. 
    
5. V řádku nabídek klikněte na položku **Uložit**.

Blahopřejeme, aplikace nyní provádí automatické škálování.

V okně **Nastavení škálování** jste si nejspíše všimli, že lze provádět mnoho dalších úkonů, například:

- Ruční škálování na konkrétní počet instancí
- Škálování podle jiných metrik výkonu, například podle procenta paměti či fronty disku
- Přizpůsobení chování škálování při aktivaci pravidla výkonu
- Automatické škálování podle plánu
- Nastavení chování automatického škálování pro budoucí událost

Další informace o vertikálním navýšení kapacity aplikace naleznete v tématu [Vertikální navýšení kapacity aplikace v Azure](../app-service-web/web-sites-scale.md). Další informace o horizontálním navýšení kapacity naleznete v tématu [Ruční nebo automatické škálování počtu instancí](../azure-portal/insights-how-to-scale.md).

## Doručování upozornění týkajících se aplikace

Aplikace nyní provádí automatické škálování. Co se stane, když aplikace dosáhne maximálního počtu instancí (2) a procesor překročí žádoucí míru využití (80 %)? Můžete například nastavit upozornění (podrobnější informace naleznete v části [Doručování oznámení o upozorněních](../azure-portal/insights-receive-alert-notifications.md)), které vás bude o tomto stavu informovat, abyste mohli dále vertikálně/horizontálně navýšit kapacitu aplikace. Nyní rychle nastavíme upozornění pro tento scénář.

1. V okně portálu aplikace klikněte na možnost **Nástroje** > **Upozornění**.  
    ![Upozornění – okno nastavení](./media/app-service-web-get-started/alert-settings.png)

2. Klikněte na možnost **Přidat upozornění**. V poli **Prostředky** vyberte prostředek, který končí řetězcem **(serverfarms)**. Toto je váš plán služby App Service.  
    ![Upozornění – přidání upozornění pro plán služby App Service](./media/app-service-web-get-started/alert-add.png)

3. V položce **Název** zadejte možnost `CPU Maxed`, v položce **Metrika** zadejte možnost **Procento procesoru** a v položce **Prahová hodnota** zadejte možnost `90`. Poté vyberte možnost **Vlastníci, přispěvatelé a čtenáři e-mailu** a klikněte na tlačítko **OK**.   
 ![Upozornění – konfigurace upozornění](./media/app-service-web-get-started/alert-configure.png)
    
    Jakmile Azure dokončí vytváření upozornění, toto upozornění se zobrazí v okně **Upozornění**.  
    ![Upozornění – zobrazení po dokončení](./media/app-service-web-get-started/alert-done.png)

Blahopřejeme, nyní vám jsou doručována upozornění. 

Toto nastavení upozornění kontroluje využití procesoru každých pět minut. Pokud toto číslo překročí 90 %, obdržíte společně se všemi oprávněnými uživateli e-mailové upozornění. Chcete-li zobrazit všechny uživatele, kteří jsou oprávněni přijímat upozornění, přejděte zpět do okna portálu aplikace a klikněte na tlačítko **Přístup**.  
![Zobrazení příjemců upozornění](./media/app-service-web-get-started/alert-rbac.png)

Měli byste vidět, že **Správci předplatného** jsou již **vlastníkem** aplikace. Tato skupina bude zahrnovat vás, pokud jste správci účtu předplatného Azure (například svého zkušebního předplatného). Další informace o řízení přístupu na základě role Azure naleznete v tématu [Řízení přístupu na základě role Azure](../active-directory/role-based-access-control-configure.md).

> [AZURE.NOTE] Pravidla upozornění jsou funkcí platformy Azure. Další informace naleznete v tématu [Doručování oznámení o upozorněních](../azure-portal/insights-receive-alert-notifications.md). 

## Další kroky

Během konfigurace upozornění jste si nejspíše všimli bohaté sady nástrojů v okně **Nástroje**. Zde můžete řešit problémy, sledovat výkon, testovat ohrožení zabezpečení, spravovat prostředky, pracovat s konzolou virtuálních počítačů a přidávat užitečná rozšíření. Doporučujeme kliknout na každý z nástrojů a seznámit se s jednoduchými, avšak výkonnými nástroji, které máte na dosah. 

Zjistěte, jak z nasazené aplikace vytěžit co nejvíce. Následující výčet není vyčerpávající:

- [Zakoupení a konfigurace vlastního názvu domény](custom-dns-web-site-buydomains-web-app.md) – Kupte pro svou webovou aplikaci atraktivní doménu namísto domény *.azurewebsites.net. Můžete také použít doménu, kterou již máte.
- [Nastavení přípravných prostředí](web-sites-staged-publishing.md) – Nasaďte aplikaci na přípravnou adresu URL předtím, než ji umístíte do produkce. S jistotou aktualizujte svou živou webovou aplikaci. Nastavte propracované řešení DevOps s více nasazovacími sloty. 
- [Nastavení průběžného nasazování](app-service-continuous-deployment.md) – Integrujte nasazení aplikace do systému správy zdrojů. Proveďte nasazení do Azure s každou potvrzenou změnou.
- [Přístup k místním prostředkům](web-sites-hybrid-connection-get-started.md) – Získejte přístup k stávající místní databázi nebo systému CRM.
- [Zálohování aplikace](web-sites-backup.md) – Nastavte zálohování a obnovení webové aplikace. Připravte se na neočekávaná selhání a zotavte se z nich.
- [Povolení diagnostických protokolů](web-sites-enable-diagnostic-log.md) – Přečtěte si protokoly služby IIS z Azure nebo trasování aplikací. Přečtěte si je pomocí streamu, stáhněte si je nebo je přizpůsobte pro službu [Application Insights](../application-insights/app-insights-overview.md) k analýze na klíč.
- [Kontrola chyb zabezpečení aplikace](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) -
Zkontrolujte, zda webová aplikace není zranitelná moderními hrozbami, a to pomocí služby poskytované společností [Tinfoil Security](https://www.tinfoilsecurity.com/).
- [Spouštění úloh na pozadí](../azure-functions/functions-overview.md) – Spouštějte úlohy zpracování dat, vytváření sestav atd.
- [Seznámení s fungováním služby App Service](../app-service/app-service-how-works-readme.md) 


<!--HONumber=Sep16_HO3-->


