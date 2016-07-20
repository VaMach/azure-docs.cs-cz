Azure určí, že vaše aplikace používá Python, **pokud jsou splněné obě tyto podmínky**:

- Soubor requirements.txt v kořenové složce
- Jakýkoli soubor .py v kořenové složce NEBO soubor runtime.txt, který určuje jazyk Python

V takovém případě se použije konkrétní skript nasazení jazyka Python, který provede standardní synchronizaci souborů a další operace jazyka Python, jako jsou tyto:

- Automatická správa virtuálního prostředí
- Instalace balíčků uvedených v souboru requirements.txt pomocí nástroje pip
- Vytvoření odpovídajícího souboru web.config na základě vybrané verze jazyka Python.
- Shromáždění statických souborů pro aplikace Django

Můžete ovládat některé aspekty výchozích kroků nasazení, aniž by bylo potřeba skript přizpůsobit.

Pokud chcete přeskočit všechny kroky nasazení specifické pro Python, můžete vytvořit tento prázdný soubor:

    \.skipPythonDeployment

Chcete-li u aplikace rozhraní Django přeskočit shromažďování statických souborů:

    \.skipDjango 

Pokud chcete mít větší kontrolu nad nasazením, můžete výchozí skript nasazení potlačit vytvořením následujících souborů:

    \.deployment
    \deploy.cmd

K vytvoření těchto souborů můžete použít [rozhraní příkazového řádku Azure][].  Použijte tento příkaz ze složky projektu:

    azure site deploymentscript --python

Pokud tyto soubory neexistují, Azure vytvoří dočasný skript nasazení a spustí ho.  Je stejný jako ten, který vytvoříte pomocí výše uvedeného příkazu.

[rozhraní příkazového řádku Azure]: http://azure.microsoft.com/downloads/



<!--HONumber=Jun16_HO2-->


