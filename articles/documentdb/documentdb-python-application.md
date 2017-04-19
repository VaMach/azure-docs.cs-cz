---
title: "Kurz webové aplikace Python Flask pro Azure DocumentDB | Dokumentace Microsoftu"
description: "Projděte si databázový kurz na téma, jak ukládat a přistupovat k datům ve webové aplikaci Python Flask hostované v Azure pomocí DocumentDB. Naleznete zde řešení pro vývoj aplikací."
keywords: "Vývoj aplikací, python flask, webová aplikace python, vývoj pro web python"
services: documentdb
documentationcenter: python
author: syamkmsft
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: documentdb
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 11/16/2016
ms.author: syamk
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 72b2d9142479f9ba0380c5bd2dd82734e370dee7
ms.openlocfilehash: 4f05075efea0f0fd8ca4424f771d3991a65c6d67
ms.lasthandoff: 04/18/2017


---
# <a name="build-a-python-flask-web-application-using-documentdb"></a>Sestavení webové aplikace Python Flask pomocí DocumentDB
> [!div class="op_single_selector"]
> * [.NET](documentdb-dotnet-application.md)
> * [.NET pro MongoDB](documentdb-mongodb-application.md)
> * [Node.js](documentdb-nodejs-application.md)
> * [Java](documentdb-java-application.md)
> * [Python](documentdb-python-application.md)
> 
> 

V tomto kurzu se dozvíte, jak pomocí Azure DocumentDB zajistit ukládání a přístup k datům ve webové aplikace Python hostované v Azure. Předpokládá se, že máte zkušenosti s používáním Pythonu a Webů Azure.

Tento databázový kurz se zabývá:

1. Vytvořením a zřízením účtu DocumentDB
2. Vytvořením aplikace Python MVC
3. Připojením a používáním Azure DocumentDB z webové aplikace
4. Nasazením webové aplikace na Weby Azure

Podle postupu uvedeného v tomto kurzu sestavíte jednoduchou hlasovací aplikaci, která vám umožní hlasovat v anketě.

![Snímek obrazovky webové aplikace vytvořené v tomto databázovém kurzu](./media/documentdb-python-application/image1.png)

## <a name="database-tutorial-prerequisites"></a>Předpoklady pro databázový kurz
Než budete postupovat podle pokynů tohoto článku, měli byste se ujistit, že máte následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
 
    NEBO 

    Místní instalaci [emulátoru Azure DocumentDB](documentdb-nosql-local-emulator.md).
* [Visual Studio 2013](http://www.visualstudio.com/) nebo vyšší nebo bezplatnou verzi [Visual Studio Express](). Pokyny v tomto kurzu jsou psány konkrétně pro Visual Studio 2015. 
* Python Tools for Visual Studio z [GitHubu](http://microsoft.github.io/PTVS/). V tomto kurzu se používá Python Tools for VS 2015. 
* Azure Python SDK for Visual Studio, verze 2.4 nebo vyšší dostupná na [azure.com](https://azure.microsoft.com/downloads/). Použili jsme Microsoft Azure SDK for Python 2.7
* Python 2.7 z [python.org][2] Použili jsme Python 2.7.11 

> [!IMPORTANT]
> Pokud Python 2.7 instalujete poprvé, je zapotřebí na obrazovce Customize Python 2.7.11 (Přizpůsobit Python 2.7.11) vybrat **Add python.exe to Path** (Přidat python.exe do proměnné PATH).
> 
> ![Snímek obrazovky Customize Python 2.7.11 (Přizpůsobit Python 2.7.11), kde je nutné vybrat možnost Add python.exe to Path (Přidat python.exe do proměnné PATH)](./media/documentdb-python-application/image2.png)
> 
> 

* Microsoft Visual C++ Compiler for Python 2.7 z webu [Microsoft Download Center][3]

## <a name="step-1-create-a-documentdb-database-account"></a>Krok 1: Vytvoření databázového účtu DocumentDB
Začněme vytvořením účtu DocumentDB. Pokud již účet máte nebo pokud používáte pro účely tohoto kurzu emulátor DocumentDB, můžete přeskočit na [Krok 2: Vytvoření nové webové aplikace Python Flask](#step-2:-create-a-new-python-flask-web-application).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

<br/>
Nyní vám ukážeme, jak od základů vytvořit novou webovou aplikaci Python Flask.

## <a name="step-2-create-a-new-python-flask-web-application"></a>Krok 2: Vytvoření nové webové aplikace Python Flask
1. V nástroji Visual Studio najeďte myší v nabídce **Soubor** na **Nový** a klikněte na **Projekt**.
   
    Zobrazí se dialogové okno **Nový projekt**.
2. V levém podokně rozbalte **Šablony**, pak **Python** a klikněte na **Web**. 
3. Ve středním podokně vyberte **Webový projekt Flask**, do pole **Název** zadejte **tutorial** a klikněte na **OK**. Nezapomeňte, že názvy balíčků Python by měly být celé malými písmeny, jak je popsáno v [průvodci správným stylem pro kód Python](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).
   
    Je-li pro vás Python Flask nový, je to rozhraní pro vývoj webových aplikací, které urychluje tvorbu webových aplikací v Pythonu.
   
    ![Snímek obrazovky okna Nový projekt v nástroji Visual Studio se zvýrazněným Pythonem vlevo, vybraným webovým projektem Python Flask uprostřed a názvem tutorial v poli Název](./media/documentdb-python-application/image9.png)
4. V okně **Python Tools for Visual Studio** klikněte na **Nainstalovat do virtuálního prostředí**. 
   
    ![Snímek obrazovky databázového kurzu – okno Python Tools for Visual Studio](./media/documentdb-python-application/image10.png)
5. V okně **Přidání virtuálního prostředí** můžete přijmout výchozí hodnoty a použít Python 2.7 jako základní prostředí, protože PyDocumentDB v tuto chvíli nepodporuje Python 3.x. Pak klikněte na **Vytvořit**. Tím se pro projekt nastaví požadované virtuální prostředí Python.
   
    ![Snímek obrazovky databázového kurzu – okno Python Tools for Visual Studio](./media/documentdb-python-application/image10_A.png)
   
    Jakmile je prostředí úspěšně nainstalováno, ve výstupním okně se zobrazí `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`.

## <a name="step-3-modify-the-python-flask-web-application"></a>Krok 3: Úprava webové aplikace Python Flask
### <a name="add-the-python-flask-packages-to-your-project"></a>Přidání balíčků Python Flask do projektu
Po nastavení projektu bude nutné do projektu přidat požadované balíčky Flask, včetně pydocumentdb, tedy balíčku Python pro DocumentDB.

1. V Průzkumníkovi řešení otevřete soubor s názvem **requirements.txt** a nahraďte jeho obsah následujícím kódem:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Uložte soubor **requirements.txt**. 
3. V Průzkumníkovi řešení klikněte pravým tlačítkem na **env** a pak levým na **Nainstalovat z requirements.txt**.
   
    ![Snímek obrazovky, na kterém je zvoleno env (Python 2.7) a v seznamu je zvýrazněna možnost Instalovat z requirements.txt](./media/documentdb-python-application/image11.png)
   
    Po úspěšné instalaci se ve výstupním okně zobrazí následující:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > Ve výjimečných případech se ve výstupním okně může zobrazit informace o selhání. Pokud se to stane, podívejte se, jestli chyba nesouvisí s vyčištěním. Někdy se vyčištění nepovede, ale instalace je přesto úspěšná (posuňte se ve výstupním okně nahoru, kde to je možné ověřit). Instalaci můžete zkontrolovat [ověřením virtuálního prostředí](#verify-the-virtual-environment). Pokud se instalace nepovedla, ale ověření proběhlo úspěšně, můžete pokračovat.
   > 
   > 

### <a name="verify-the-virtual-environment"></a>Ověření virtuálního prostředí
Ujistěme se, že se vše správně nainstalovalo.

1. Sestavte řešení stisknutím **CTRL**+**SHIFT**+**B**.
2. Po úspěšném sestavení spusťte web klávesou **F5**. Tímto se spustí vývojový server Flask a webový prohlížeč. Měla by se zobrazit následující stránka:
   
    ![Prázdný webový vývojový projekt Python Flask zobrazený v prohlížeči](./media/documentdb-python-application/image12.png)
3. Klávesami **SHIFT**+**F5** v nástroji Visual Studio ukončete ladění webu.

### <a name="create-database-collection-and-document-definitions"></a>Vytvoření definic databáze, kolekcí a dokumentů
Nyní vytvořte hlasovací aplikaci tak, že přidáme nové soubory a jiné aktualizujeme.

1. V Průzkumníkovi řešení klikněte pravým tlačítkem na projekt **tutorial**, pak levým na **Přidat** a nakonec také levým na **Nová položka**. Vyberte **Prázdný soubor Python** a pojmenujte jej **forms.py**.  
2. Do souboru forms.py přidejte následující kód a soubor uložte.

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-viewspy"></a>Přidání požadovaných importů do views.py
1. V Průzkumníkovi řešení rozbalte složku **tutorial** a otevřete soubor **views.py**. 
2. Do horní části souboru **views.py** přidejte následující příkazy import a soubor uložte. Tyto příkazu importují balíčky PythonSDK pro DocumentDB a Flask.
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>Vytvoření databáze, kolekce a dokumentu
* Na konec téhož souboru **views.py** přidejte následující kód. Ten vytvoří databázi, která se použije ve formuláři. V souboru **views.py** neodstraňujte žádný kód, který tam již je. Pouze nový kód přidejte na konec.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```

> [!TIP]
> Metoda **CreateCollection** přijímá volitelný třetí parametr **RequestOptions**. Lze jej použít k určení typu nabídky kolekce. Pokud se nezadá žádná hodnota offerType, kolekce se vytvoří pomocí výchozího typu nabídky. Další informace o typech nabídek DocumentDB najdete v tématu [Úrovně výkonu v DocumentDB](documentdb-performance-levels.md).
> 
> 

### <a name="read-database-collection-document-and-submit-form"></a>Čtení databáze, kolekce a dokumentu a odeslání formuláře
* Na konec téhož souboru **views.py** přidejte následující kód. Tento kód má na starosti nastavení formuláře a čtení databáze, kolekce a dokumentu. V souboru **views.py** neodstraňujte žádný kód, který tam již je. Pouze nový kód přidejte na konec.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a>Vytvoření souborů HTML
1. V Průzkumníkovi řešení klikněte pravým tlačítkem ve složce **tutorial** na složku **šablony**, pak levým na **Přidat** a nakonec také levým na **Nová položka**. 
2. Vyberte **Stránka HTML** do pole název zadejte **create.html**. 
3. Opakováním kroků 1 a 2 vytvořte dva další soubory HTML: results.html a vote.html.
4. Do souboru **create.html** do elementu `<body>` přidejte následující kód. Tento kód zobrazí zprávu, že jsme vytvořili novou databázi, kolekci a dokument.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Do souboru **results.html** do elementu `<body` přidejte následující kód. Slouží k zobrazení výsledků ankety.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Do souboru **vote.html** do elementu `<body` přidejte následující kód. Zobrazuje anketu a přijímá hlasy. Při registraci hlasů se ovládací prvek předává do views.py, kde rozpoznáme udělené hlasy a patřičně připojíme dokument.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. Ve složce **šablony** nahraďte obsah souboru **index.html** následujícím obsahem. Tato stránka slouží jako cílová stránka aplikace.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + DocumentDB Voting Application.</h2>
    <h3>This is a sample DocumentDB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a>Přidejte konfigurační soubor a změňte \_\_init\_\_.py.
1. V Průzkumníkovi řešení klikněte pravým tlačítkem na projekt **tutorial**, pak levým na **Přidat**, dále na **Nová položka**, vyberte **Prázdný soubor Python** a pojmenujte jej **config.py**. Formuláře ve Flasku vyžadují tento konfigurační soubor. Můžete jej použít i k poskytnutí tajného klíče. Ten ale pro tento kurz není nezbytný.
2. Do souboru config.py přidejte následující kód. V dalším kroku bude nutné upravit hodnoty **DOCUMENTDB\_HOST** a **DOCUMENTDB\_KEY**.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. Na [Portálu Azure](https://portal.azure.com/) přejděte do okna **Klíče** tak, že kliknete na **Procházet**, **Účty DocumentDB**, dvakrát kliknete na název účtu, který chcete použít, a v oblasti **Základy** kliknete na tlačítko **Klíče**. V okně **Klíče** zkopírujte hodnotu **URI** a vložte ji do souboru **config.py** jako hodnotu vlastnosti **DOCUMENTDB\_HOST**. 
4. Zpět na Portálu Azure v okně **Klíče** zkopírujte hodnotu **Primární klíč** nebo **Sekundární klíč** a vložte ji do souboru **config.py** jako hodnotu vlastnosti **DOCUMENTDB\_KEY**.
5. Do souboru **\_\_init\_\_.py** přidejte následující řádek. 
   
        app.config.from_object('config')
   
    Obsah souboru tak bude následující:
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. Po přidání všech souborů by Průzkumník řešení měl vypadat takto:
   
    ![Snímek obrazovky okna Průzkumníka řešení v nástroji Visual Studio](./media/documentdb-python-application/image15.png)

## <a name="step-4-run-your-web-application-locally"></a>Krok 4: Místní spuštění webové aplikace
1. Sestavte řešení stisknutím **CTRL**+**SHIFT**+**B**.
2. Po úspěšném sestavení spusťte web klávesou **F5**. Na obrazovce byste měli vidět následující.
   
    ![Snímek obrazovky hlasovací aplikace vytvořené v Pythonu a DocumentDB zobrazené ve webovém prohlížeči](./media/documentdb-python-application/image16.png)
3. Klikněte na **Create/Clear the Voting Database** (Vytvořit nebo vyčistit hlasovací databázi), aby se vygenerovala databáze.
   
    ![Snímek obrazovky webové aplikace a její stránky Vytvořit – podrobnosti o vývoji](./media/documentdb-python-application/image17.png)
4. Pak vyberte svou možnost kliknutím na **Vote** (Hlasovat).
   
    ![Snímek obrazovky webové aplikace se zobrazenou otázkou k hlasování](./media/documentdb-python-application/image18.png)
5. Každý hlas, který udělíte, navýší příslušný čítač.
   
    ![Snímek obrazovky se zobrazenou stránkou Results of the vote (Výsledek hlasování)](./media/documentdb-python-application/image19.png)
6. Kombinací kláves SHIFT+F5 ukončete ladění projektu.

## <a name="step-5-deploy-the-web-application-to-azure-websites"></a>Krok 5: Nasazení webové aplikace na Weby Azure
Nyní, když je aplikace dokončena a správně funguje s DocumentDB, nasadíme ji na Weby Azure.

1. Klikněte pravým tlačítkem na projekt v Průzkumníkovi řešení (ujistěte se, že aplikace již není spuštěná místně) a vyberte **Publikovat**.  
   
     ![Snímek obrazovky projektu tutorial vybraného v Průzkumníkovi řešení se zvýrazněnou možností Publikovat](./media/documentdb-python-application/image20.png)
2. V okně **Publikování webu** vyberte **Microsoft Azure Web Apps** a klikněte na **Další**.
   
    ![Snímek obrazovky okna Publikování webu se zvýrazněnou možností Microsoft Azure Web Apps](./media/documentdb-python-application/image21.png)
3. V okně **Okno Microsoft Azure Web Apps** klikněte na **Nový**.
   
    ![Snímek obrazovky okna Okno Microsoft Azure Web Apps](./media/documentdb-python-application/select-existing-website.png)
4. V okně **Vytvoření webu na Microsoft Azure** zadejte **název webové aplikace**, **plán služby App Service**, **Skupinu prostředků** a **Oblast** a klikněte na **Vytvořit**.
   
    ![Snímek obrazovky okna při vytváření webu v Microsoft Azure](./media/documentdb-python-application/create-site-on-microsoft-azure.png)
5. V okně **Publikování webu** klikněte na **Publikovat**.
   
    ![Snímek obrazovky okna při vytváření webu v Microsoft Azure](./media/documentdb-python-application/publish-web.png)
6. Za několik sekund Visual Studio dokončí publikování webové aplikace a spustí prohlížeč, kde se můžete podívat, jak vaše práce běží v Azure!

## <a name="troubleshooting"></a>Řešení potíží
Pokud je toto první aplikace Python, kterou jste spustili na svém počítači, ujistěte se, že vaše proměnná PATH obsahuje následující složky (nebo ekvivalentní umístění instalací):

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Pokud jste projekt pojmenovali jinak než **tutorial** a na stránce hlasování se zobrazí chyba, ujistěte se, že soubor **\_\_init\_\_.py** odkazuje na následujícím řádku na správný projekt: `import tutorial.view`.

## <a name="next-steps"></a>Další kroky
Blahopřejeme! Právě jste dokončili svou první webovou aplikaci Python, která používá Azure DocumentDB, a publikovali jste ji na Weby Azure.

Toto téma často aktualizujeme a vylepšujeme podle vaší zpětné vazby.  Až kurz dokončíte, použijte prosím hlasovací tlačítka v horní a dolní části této stránky a sdělte nám svůj názor, jaká vylepšení byste si přáli vidět. Pokud chcete, abychom vás kontaktovali přímo, můžete nám nechat e-mailovou adresu v komentářích.

Pokud chcete do své webové aplikace přidat další funkce, podívejte se na rozhraní API dostupná v sadě [DocumentDB Python SDK](documentdb-sdk-python.md).

Další informace o Azure, nástroji Visual Studio a Pythonu najdete v [Centru pro vývojáře v Pythonu](https://azure.microsoft.com/develop/python/). 

Další kurzy Pythonu Flask najdete na stránce [Velký kurz na Flask, část I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com

