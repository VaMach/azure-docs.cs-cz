# Technická dokumentace k Azure – příručka přispěvatele
Našli jste úložiště GitHubu, ve kterém je uložený zdroj technické dokumentace publikované v Centru dokumentace Azure na adrese [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation).

Toto úložiště obsahuje také pokyny, které vám pomohou přispívat do naší technické dokumentace.  Seznam článků v příručce pro přispěvatele najdete v [rejstříku](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## Přispívání do dokumentace k Azure
Děkujeme vám za váš zájem o dokumentaci k Azure!

* [Způsoby přispívání](#ways-to-contribute)
* [Pravidla chování](#code-of-conduct)
* [Obsah příspěvků k obsahu Azure](#about-your-contributions-to-azure-content)
* [Uspořádání úložiště](#repository-organization)
* [Použití GitHubu, Gitu a tohoto úložiště](#use-github-git-and-this-repository)
* [Využití formátu Markdown k formátování témat](#how-to-use-markdown-to-format-your-topic)
* [Zpětná vazba, komentáře a podpora](./contributor-guide/feedback-and-comments.md)
* [Další zdroje informací](#more-resources)
* [Rejstřík všech článků příručky pro přispěvatele](./contributor-guide/contributor-guide-index.md) (otevře novou stránku)

## Způsoby přispívání
Do [dokumentace k Azure](http://azure.microsoft.com/documentation/) můžete přispívat několika různými způsoby:

* Můžete přispět k [diskuzi na fóru](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Můžete odeslat komentáře Disqus v dolní části článků.
* Pro zadání příspěvku k technickým článkům můžete snadno využít uživatelské rozhraní GitHubu. Vyhledejte požadovaný článek v tomto úložišti nebo tento článek navštivte na webu [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) a klikněte na odkaz, kterým přejdete ke zdroji tohoto článku na GitHubu.
* Pokud provádíte podstatné změny existujícího článku, přidáváte nebo měníte obrázky nebo přispíváte novým článkem, budete muset toto úložiště rozvětvit, nainstalovat Git Bash a Markdown Pad naučit se některé příkazy Gitu.

## Pravidla chování
Tento projekt přijal [pravidla chování pro Microsoft Open Source](https://opensource.microsoft.com/codeofconduct/). Další informace najdete v [nejčastějších dotazech k pravidlům chování](https://opensource.microsoft.com/codeofconduct/faq/). S případnými dalšími dotazy nebo připomínkami se obraťte na adresu [opencode@microsoft.com](mailto:opencode@microsoft.com).

## Vaše příspěvky k obsahu Azure
### Menší opravy
Na menší opravy nebo objasnění, která odešlete k dokumentaci a ukázkám kódu v tomto úložišti, se vztahují [Podmínky užívání webu Azure](http://azure.microsoft.com/support/legal/website-terms-of-use/).

### Větší úpravy
Pokud odešlete žádost o přijetí změn s novými nebo důležitými změnami dokumentace a ukázek kódu a patříte do jedné z následujících skupin, odešleme vám v GitHubu komentář s výzvou k odeslání online smlouvy CLA (Contribution License Agreement):

* Členové skupiny Microsoft Open Technologies.
* Přispěvatelé, kteří nepracují pro Microsoft.

Potřebujeme, abyste tento online formulář vyplnili. Teprve potom můžeme přijmout vaši žádost o přijetí změn.

Všechny podrobnosti jsou dostupné na adrese [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## Uspořádání úložiště
Uspořádání obsahu v úložišti obsahu Azure odpovídá uspořádání dokumentace na webu [Azure.Microsoft.com](http://azure.microsoft.com). Toto úložiště obsahuje dvě kořenové složky:

### \articles
Složka *\articles* obsahuje články dokumentace naformátované jako soubory Markdown s příponou *.md*.

Články v kořenovém adresáři jsou na webu Azure.Microsoft.com publikované v cestě *http://azure.microsoft.com/documentation/articles/{název-článku-bez-md}/*.

* **Názvy souborů článků:** Přečtěte si [naše pokyny pro pojmenování souborů](./contributor-guide/file-names-and-locations.md).

Články ve vlastní složce služby jsou na webu Azure.Microsoft.com publikované v cestě *http://azure.microsoft.com/documentation/articles/service-folder/{název-článku-bez-md}/*.

* **Podsložky médií:** Složka *\articles* obsahuje složku *\media* pro mediální soubory článků kořenového adresáře, která obsahuje podsložky pro obrázky jednotlivých článků.  Složky služeb obsahují samostatnou složku médií pro články, které jsou v nich obsažené. Složky pro obrázky článků mají stejný název jako příslušný článek, ale bez přípony *.md*.

### \includes
Je možné vytvořit opakovaně použitelný obsah, který bude zahrnutý v jednom nebo několika článcích. Další informace najdete v tématu věnovaném [vlastním rozšířením, která se používají v našem technickém obsahu](./contributor-guide/custom-markdown-extensions.md).

### \markdown templates
Tato složka obsahuje naši standardní šablonu Markdown se základními formáty, které jsou v článcích potřeba.

### \contributor-guide
Tato složka obsahuje články, které jsou součástí naší příručky pro přispěvatele.  

## Použití GitHubu, Gitu a tohoto úložiště
Informace o tom, jak přispívat, jak pomocí uživatelského rozhraní GitHubu navrhovat menší změny a jak rozvětvovat a klonovat úložiště v případě větších příspěvků, najdete v tématu [Instalace a nastavení nástrojů pro vytváření příspěvků v GitHubu](./contributor-guide/tools-and-setup.md).

Pokud jste si nainstalovali Git Bash a rozhodli se pracovat místně, kroky pro vytvoření nové místní pracovní větve, provedení změn a odeslání změn zpět do hlavní větve najdete v tématu [Příkazy Gitu pro vytvoření nového článku nebo aktualizaci existujícího článku](./contributor-guide/git-commands-for-master.md)

### Větve
Doporučujeme, abyste si vytvořili místní pracovní větve, které cílí na konkrétní rozsah změn. Každá větev by měla být omezená na jeden koncept/článek, a to jak kvůli zjednodušení pracovního postupu, tak kvůli omezení případných konfliktů při slučování.  Vytvoření nové větve je vhodné v následujících situacích:

* Nový článek (a přidružené obrázky)
* Pravopisné a gramatické úpravy článku
* Použití jedné formátovací změny napříč velkou sadou článků (například nové zápatí s copyrightem)

## Využití formátu Markdown k formátování témat
Všechny články v tomto úložišti využívají formát Markdown specifický pro GitHub.  Tady je seznam informačních materiálů.

* [Základy formátu Markdown](https://help.github.com/articles/markdown-basics/)
* [Stručný přehled formátu Markdown k vytištění](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)
* Seznam editorů formátu Markdown najdete v [tématu věnovaném nástrojům a nastavení](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## Metadata článků
Metadata článků umožňují využití určitých funkcí na webu azure.microsoft.com, jako je třeba uvedení autora a přispěvatele, popisy cest, popisy článků a optimalizace SEO a také vytváření sestav, které Microsoft využívá k vyhodnocení výkonu příslušného obsahu. Metadata jsou proto důležitá! [Tady najdete pokyny, abyste mohli zkontrolovat, že s metadaty pracujete správně](./contributor-guide/article-metadata.md).

## Další zdroje informací
Všechna témata s našimi pokyny najdete v [rejstříku příručky pro přispěvatele](./contributor-guide/contributor-guide-index.md).

<!--HONumber=Aug16_HO1-->


