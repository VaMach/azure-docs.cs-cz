<properties
    pageTitle="Začněte používat Azure Mobile Services pro aplikace HTML/JavaScript | Microsoft Azure"
    description="Využijte tento kurz a začněte využívat Azure Mobile Services pro vývoj pro HTML."
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="get-started-article" 
    ms.date="07/21/2016"
    ms.author="glenga"/>


# <a name="getting-started"> </a>Začínáme používat mobilní služby

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

##Přehled 

V tomto kurzu se dozvíte, jak přidat cloudovou back-end službu do mobilní aplikace HTML pomocí Azure Mobile Services. V tomto návodu vytvoříte novou mobilní službu a jednoduchou aplikaci *Seznam úkolů*, která ukládá data aplikací v rámci nové mobilní služby. Můžete zobrazit následující verzi videa tohoto kurzu. 

> [AZURE.VIDEO mobile-get-started-html]
 
Zde je snímek obrazovky dokončené aplikace:

![][0]

Ve všech dalších kurzech používání Mobile Services pro HTML aplikace se předpokládá dokončení tohoto kurzu. Pro aplikaci PhoneGap nebo Cordova si prostudujte téma [Verze PhoneGap/Cordova](mobile-services-javascript-backend-phonegap-get-started.md) tohoto kurzu.

##Požadavky

Pro dokončení tohoto kurzu potřebujete následující položky:

+ Musí mít v místním počítači spuštěný jeden z následujících webových serverů:

    +  **V systému Windows**: služby IIS Express. Služba IIS Express je nainstalována pomocí [instalačního programu webové platformy Microsoft].
    +  **V systému MacOS X**: Python, který by měl být již nainstalován.
    +  **V systému Linux**: Python. Je třeba nainstalovat [nejnovější verzi jazyka Python].

    K hostování aplikace můžete použít kterýkoli webový server, ale toto jsou webové servery, které jsou podporovány staženými skripty.  

+ Webový prohlížeč, který podporuje HTML5.
+ Účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v tématu [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-html%2F"%20target="_blank). 


## <a name="create-new-service"> </a>Vytvoření nové mobilní služby

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## Vytvoření nové aplikace HTML

Jakmile vytvoříte mobilní službu, můžete snadno využít postup rychlého spuštění portálu Azure Classic k vytvoření nové aplikace, nebo úpravě stávající aplikace pro připojení k mobilní službě.

V této části vytvoříte novou aplikaci pro HTML, která je připojena k mobilní službě.

1.  Na [portál Azure Classic] klikněte na tlačítko **Mobile Services** a pak klikněte na mobilní službu, kterou jste právě vytvořili.


2. Na kartě Rychlé spuštění klikněte na tlačítko **Windows** pod položkou **Vybrat platformu** a rozbalte možnost **Vytvořit novou aplikaci HTML**.

    ![][6]

    Zobrazí se tří jednoduché kroky k vytvoření a hostování aplikace HTML připojené k mobilní službě.

    ![][7]

3. Klikněte na tlačítko **Vytvořit tabulku TodoItem** a vytvořte tabulku pro ukládání dat aplikací.

4. V části **Stažení a spuštění aplikace** klikněte na tlačítko **Stáhnout**.

    Tato akce stáhne soubory webové stránky pro vzorovou aplikaci _Seznam úkolů_, která je připojena k mobilní službě. Uložte komprimovaný soubor do místního počítače a poznamenejte si, kam jste jej uložili.

5. Na kartě **Konfigurovat** ověřte, zda je `localhost` již uveden v seznamu **Povolit požadavky od názvů hostitelů** v rámci **Sdílení prostředků různých původů (CORS)**. Pokud není, zadejte `localhost` do pole **Název hostitele** a pak klikněte na tlačítko **Uložit**.

    ![][9]

    > [AZURE.IMPORTANT] Pokud nasadíte aplikaci quickstart na jiný webový server než localhost, musíte přidat název hostitele webového serveru do seznamu **Povolit požadavky od názvů hostitelů**. Další informace naleznete v tématu [Sdílení prostředků různého původu](http://msdn.microsoft.com/library/windowsazure/dn155871.aspx).

## Hostování a spuštění aplikace HTML

Závěrečnou fází tohoto kurzu je hostování a spuštění nové aplikace v místním počítači.

1. Přejděte do umístění, kam jste uložili komprimované soubory projektu, rozbalte soubory v počítači a spusťte jeden z následujících souborů příkazu z podsložky **server**.

    + **launch-windows** (počítače se systémem Windows)
    + **launch-mac.command** (počítače se systémem Mac OS X)
    + **launch-linux.sh** (počítače se systémem Linux)

    > [AZURE.NOTE] Na počítači se systémem Windows zadejte po zobrazení dotazu prostředí PowerShell pro potvrzení, zda chcete spustit skript, možnost `R`. Webový prohlížeč vás může varovat, abyste skript nespouštěli, protože byl stažen z internetu. V takovém případě musíte požádat, aby prohlížeč pokračovat v načítání skriptu.

    Pro hostování nové aplikace se otevře se webový server v místním počítači.

2. Otevřete adresu URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> ve webovém prohlížeči a spusťte aplikaci.

3. Do aplikace zadejte smysluplný text, například _Dokončit kurz_, v části **Zadat novou úlohu** a pak klikněte na tlačítko **Přidat**.

    ![][10]

    Tím se odešle požadavek POST do nové mobilní služby hostované v Azure. Data z požadavku se vloží do tabulky TodoItem. Položky uložené v tabulce se vrátí pomocí mobilní služby a v druhém sloupci aplikace se zobrazí data.

    > [AZURE.NOTE] Kód, který přistupuje k mobilní službě pro dotazování a vkládání dat a který se nachází v souboru page.js, můžete zkontrolovat.

4. Zpět v [portál Azure Classic] klikněte na kartu **Data** a pak klikněte na tabulku **TodoItems**.

    ![][11]

    Tato možnost vám umožňuje procházet data vložená aplikací do tabulky.

    ![][12]

## <a name="next-steps"> </a>Další kroky
Teď, když jste dokončili rychlé spuštění, se naučíte, jak provádět další důležité úkoly v Mobile Services:

* **[Přidání ověřování do aplikace]**  
  Zjistěte, jak ověřovat uživatele vaší aplikace pomocí zprostředkovatele identity.

* **[Reference konceptu postupu u Mobile Services HTML/JavaScript]**  
  Další informace o používání Mobile Services s HTML/JavaScript


[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Začínáme používat Mobile Services]:#getting-started
[Vytvoření nové mobilní služby]:#create-new-service
[Definování instance mobilních služeb]:#define-mobile-service-instance
[Další kroky]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-html-get-started/mobile-quickstart-completed-html.png

[6]: ./media/mobile-services-html-get-started/mobile-portal-quickstart-html.png
[7]: ./media/mobile-services-html-get-started/mobile-quickstart-steps-html.png

[9]: ./media/mobile-services-html-get-started/mobile-services-set-cors-localhost.png
[10]: ./media/mobile-services-html-get-started/mobile-quickstart-startup-html.png
[11]: ./media/mobile-services-html-get-started/mobile-data-tab.png
[12]: ./media/mobile-services-html-get-started/mobile-data-browse.png


<!-- URLs. -->
[Přidání ověřování do aplikace]: mobile-services-html-get-started-users.md

[portál Azure Classic]: https://manage.windowsazure.com/
[instalačního programu webové platformy Microsoft]:  http://go.microsoft.com/fwlink/p/?LinkId=286333
[nejnovější verzi jazyka Python]: http://go.microsoft.com/fwlink/p/?LinkId=286342
[Reference konceptu postupu u Mobile Services HTML/JavaScript]: mobile-services-html-how-to-use-client-library.md
[Sdílení prostředků různého původu]: http://msdn.microsoft.com/library/azure/dn155871.aspx
 



<!--HONumber=Aug16_HO4-->


