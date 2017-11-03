## <a name="microsoft-open-source-code-of-conduct"></a>Otevřete Microsoft zdrojový kód chování

Tento projekt přijal [pravidla chování pro Microsoft Open Source](https://opensource.microsoft.com/codeofconduct/).
Další informace najdete v článku [chování](https://opensource.microsoft.com/codeofconduct/faq/) nebo se obraťte na [ opencode@microsoft.com ](mailto:opencode@microsoft.com) s případnými dalšími dotazy nebo připomínkami.

## <a name="contribute-to-azure-technical-documentation"></a>Přispívání do technické dokumentace k Azure
Vítáme příspěvky naší komunity (uživatelé, zákazníky, partnery, zaměstnanci MSFT mimo core jednotky Azure produktu, atd.), a také zaměstnanci pracující v jednotkách Azure produktu jádra. Jak přispívat, závisí na kdo jste:

* **Komunita – dílčími aktualizacemi**: Pokud přispíváte dílčími aktualizacemi mimo přesnosti vaše srdce, můžete vyhledejte požadovaný článek v tomto úložišti, nebo článek navštivte na [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) a klikněte na tlačítko **Upravit** odkaz v článku na Githubu zdroj článku. Chcete-li vaše aktualizace použijte pak uživatelského rozhraní Githubu. Nebo, jsou Vítá vás rozvětvení úložiště a odeslat aktualizace z vaší větve.

* **Komunita – nové články**: Pokud jste součástí komunitou Azure a chcete vytvořit nový článek, potřebujete k práci s zaměstnanec tak aby tohoto nového obsahu v pomocí kombinace pracovní veřejné a privátní úložiště.

* **Zaměstnanci**: Pokud jste technické zapisovače, program Správce nebo vývojáře od týmu produktu pro služby Azure a je úlohu můžete přispět k nebo vytvářet technické články, měli byste použít privátní úložiště (https://github.com/ MicrosoftDocs/azure-docs-pr). Pokud provádíte podstatné změny existujícího článku, přidávání nebo měníte obrázky nebo přispíváte novým článkem, budete muset toto úložiště rozvětvit, nainstalovat Git Bash a markdown editor a další některé příkazy gitu. V tématu [interní Přispěvatel průvodce](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) Další informace.


## <a name="about-your-contributions-to-azure-content"></a>Vaše příspěvky k obsahu Azure
### <a name="minor-corrections"></a>Menší opravy
Menší opravy nebo objasnění, která odešlete k dokumentaci a ukázkám kódu v tomto úložišti, se vztahují [docs.microsoft.com podmínky použití](https://docs.microsoft.com/legal/termsofuse).

### <a name="larger-submissions"></a>Větší úpravy
Pokud odešlete žádost o přijetí změn s novými nebo důležitými změnami dokumentace a ukázky kódu, pošleme komentář v Githubu výzvou k odeslání online příspěvku licenční smlouvy CLA (), pokud není zaměstnanec společnosti Microsoft. Potřebujeme, abyste tento online formulář vyplnili. Teprve potom můžeme přijmout vaši žádost o přijetí změn.

## <a name="tools-and-setup"></a>Nástroje a nastavení
Přispěvatelé komunit můžete pomocí uživatelského rozhraní Githubu nebo rozvětvit úložišti přispívat. Zaměstnanci měli navštívit [interní Přispěvatel průvodce](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) Další informace o postupu při přispívání do technické dokumentace nastavit.

## <a name="repository-organization"></a>Uspořádání úložiště
Obsah v úložišti dokumentace azure odpovídá uspořádání dokumentace na https://docs.microsoft.com/azure. Toto úložiště obsahuje dvě kořenové složky:

### <a name="articles"></a>\articles
Složka *\articles* obsahuje články dokumentace naformátované jako soubory Markdown s příponou *.md*. Články jsou obvykle seskupené podle služby Azure.

*\Articles* složka obsahuje *\media* složku pro soubory článků kořenového adresáře média, která obsahuje podsložky pro obrázky jednotlivých článků.  Složky služeb obsahují samostatnou složku médií pro články, které jsou v nich obsažené. Složky pro obrázky článků mají stejný název jako příslušný článek, ale bez přípony *.md*.

### <a name="includes"></a>\includes
Je možné vytvořit opakovaně použitelný obsah, který bude zahrnutý v jednom nebo několika článcích. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Využití formátu Markdown k formátování témat
Všechny články v tomto úložišti využívají formát Markdown specifický pro GitHub.  Tady je seznam informačních materiálů.

* [Základy markdownu](https://help.github.com/articles/markdown-basics/)
* ["Tahák" k markdownu tisknutelná](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)


## <a name="labels"></a>Popisky
V úložišti veřejný azure dokumentace automatizované popisky přiřazené k žádosti o Pomozte nám spravovat pracovní postup žádosti o přijetí změn, abyste mohli umožňují vědět, co se děje s vaší žádosti o přijetí změn pro vyžádání obsahu:

* Související s příspěvkem licenční smlouvy
  * cla není nutné: změna je relativně malé a nevyžaduje přihlášení CLA.
  * požadované smlouvy cla: rozsahem prováděných změn je relativně velké a vyžaduje přihlášení CLA.
  * podepsané smlouvy cla: Přispěvatel podepsané smlouvy CLA, takže žádost o přijetí změn teď můžou přesouvat dál ke kontrole.
* Změnu odeslané vytvářet: Autor ohlášení žádosti čekající na vyžádání.
* připravené sloučení: připravené k revizi náš tým zkontrolujte žádost o přijetí změn.


