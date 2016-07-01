<properties
   pageTitle="Začínáme s Microsoft Power BI Embedded Preview"
   description="Power BI Embedded – přidejte do své aplikace business intelligence interaktivní sestavy Power BI."
   services="power-bi-embedded"
   documentationCenter=""
   authors="dvana"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="04/24/2016"
   ms.author="derrickv"/>

# Začínáme s Microsoft Power BI Embedded Preview

**Microsoft Power BI Embedded** je služba Azure, který vývojářům aplikací umožňuje přidávání interaktivních sestav Power BI do jejich vlastních aplikací. **Power BI Embedded** funguje s již existujícími aplikacemi, aniž by bylo nutné změnit jejich návrh nebo způsob, jakým se uživatelé přihlašují.

Další informace o této službě naleznete v tématu [Co je Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md).

Jak je popsáno v tématu [Co je Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md), prostředky pro **Microsoft Power BI Embedded** se zřizují prostřednictvím [rozhraní API Azure ARM](https://msdn.microsoft.com/library/mt712306.aspx). V tomto případě je prostředkem, který zřídíte, **kolekce Pracovních prostorů Power BI**. V další části se dozvíte, jak kolekci pracovních prostorů vytvořit.

![](media\power-bi-embedded-get-started\introduction.png)

## Vytvoření kolekce pracovních prostorů
**Kolekce pracovních prostorů** je prostředek Azure nejvyšší úrovně a kontejner pro obsah, který se vloží do vaší aplikace. **Kolekci pracovních prostorů** lze vytvořit dvěma způsoby:

   -    Ručně na Portálu Azure
   -    Programově pomocí rozhraní API Azure ARM (Azure Resource Manager)

Projděme si postup pro sestavení **kolekce pracovních prostorů** na Portálu Azure.

   1.   Otevřete a přihlaste se k **Portálu Azure**: [http://portal.azure.com](http://portal.azure.com).

   2.   Klikněte na **+ Nový** na horním panelu.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   V části **Data + analýzy** klikněte na **Power BI Embedded**.
   4.   V **okně pro vytvoření** zadejte požadované informace. Pokud jde o **cenu**, nahlédněte do [cen za Power BI Embedded](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klikněte na **Vytvořit**.

Zřizování **kolekce pracovních prostorů** bude chvíli trvat. Až se dokončí, otevře se **okno kolekce pracovních prostorů**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

Toto **okno pro vytvoření** obsahuje informace potřebné k volání rozhraní API, která slouží k vytváření pracovních prostorů a nasazování obsahu do nich.

Následující část popisuje, jak se na základě **přístupových klíčů** generují **tokeny aplikací**, které se používají k ověřování žádostí o rozhraní API.

<a name="view-access-keys"/>
## Zobrazení přístupových klíčů k rozhraním API v Power BI

Jednou z nejdůležitějších informací potřebných k volání rozhraní REST API pro Power BI jsou **přístupové klíče**. Na jejich základě se generují **tokeny aplikací** používané k ověřování vašich žádostí o rozhraní API. Pokud si chcete **přístupové klíče** zobrazit, klikněte na **Přístupové klíče** v **okně Nastavení**. Další informace o **tokenech aplikací** naleznete v článku o [jejich toku](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Jak si všimnete, máte dva klíče.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Zkopírujte tyto klíče a zabezpečeně je uložte do své aplikace. Je velmi důležité, abyste s těmito klíči zacházeli jako s heslem, protože poskytují přístup k veškerému obsahu ve vaší **kolekci pracovních prostorů**.

Ačkoli jsou uvedeny dva klíče, vždy je zapotřebí pouze jeden z nich. Druhý klíč je k dispozici proto, abyste mohli klíče pravidelně obnovovat, aniž by to ovlivnilo přístup ke službě.

Nyní když máte instanci Power BI pro vaši aplikaci a **přístupové klíče**, můžete do vlastní aplikace naimportovat sestavu. Než si řekneme, jak sestavu importovat, dozvíte se v další části, jak vytvářet datové sady a sestavy Power BI určené pro vložení do aplikace.

## Vytváření datových sad a sestav Power BI určených pro vložení do aplikace

Nyní když jste si pro svou aplikaci vytvořili instanci Power BI a máte **přístupové klíče**, musíte si vytvořit datové sady a sestavy Power BI, které do ní chcete vložit. Datové sady a sestavy lze vytvořit pomocí **Power BI Desktop**. [Power BI Desktop si můžete stáhnout zdarma](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/). Nebo pokud chcete rychle začít, můžete si stáhnout [ukázkový soubor PBIX prodejní analýzy] (http://go.microsoft.com/fwlink/?LinkID=780547). Další informace o použití **Power BI Desktop** najdete v části [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

Se službou **Power BI Desktop** se ke zdroji dat připojíte importem kopie dat do **Power BI Desktop** nebo přímým připojením zdroje dat pomocí **DirectQuery**.

Zde jsou rozdíly mezi **importem** a použitím **DirectQuery**.

|Import | DirectQuery
|---|---
|Tabulky, sloupce *a data* se naimportují, tedy zkopírují do **Power BI Desktop**. Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá kopii dat. Pokud se mají projevit změny, ke kterým došlo v základních datech, musíte provést aktualizaci, tzn. znovu naimportovat celou aktuální datovou sadu.|Do **Power BI Desktop** se naimportují, tedy zkopírují pouze *tabulky a sloupce*. Při práci s vizualizacemi si **Power BI Desktop** dotazem vyžádá základní zdroj dat, což znamená, že se vám vždy zobrazují aktuální data.

Další informace o připojení ke zdroji dat naleznete v tématu [Připojení ke zdroji dat](power-bi-embedded-connect-datasource.md).

Po uložení práce v **Power BI Desktop** se vytvoří soubor PBIX. Tento soubor obsahuje vaši sestavu. Kromě toho bude při importu dat soubor PBIX obsahovat úplnou datovou sadu. A pokud použijete **DirectQuery**, soubor PBIX bude obsahovat jen schéma datové sady. Soubor PBIX programově nasadíte do pracovního prostoru pomocí [rozhraní API pro import v Power BI](https://msdn.microsoft.com/library/mt711504.aspx).

> [AZURE.NOTE] **Power BI Embedded** má další rozhraní API pro změnu serveru a databáze, na které datová sada odkazuje, a nastavení přihlašovacích údajů pro účet služby, který datová sada bude používat pro připojení k databázi. Viz [Post SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) a [Patch Gateway Datasource](https://msdn.microsoft.com/library/mt711498.aspx).

## Další kroky
V předchozích krocích jste vytvořili kolekci pracovních prostorů a první sestavu a datovou sadu. Nyní je čas se dozvědět, jak napsat kód pro **Power BI Embedded**. Abychom vám pomohli začít, vytvořili jsme ukázkovou webovou aplikaci: [Začínáme s ukázkou](power-bi-embedded-get-started-sample.md). Na ukázce se dozvíte, jak:

  - Zřídit obsah
      - Vytvořit pracovní prostor
      - Naimportovat soubor PBIX
      - Aktualizovat připojovací řetězce a nastavit přihlašovací údaje pro vaše datové sady

  - Zabezpečené vložení sestavy

## Viz také
- [Začínáme s ukázkou](power-bi-embedded-get-started-sample.md)
- [Co je Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Začínáme s Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started)
- [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
- [Ceny za Power BI Embedded ](http://go.microsoft.com/fwlink/?LinkID=760527)



<!--HONumber=Jun16_HO2-->


