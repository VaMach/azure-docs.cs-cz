## <a name="test-your-code"></a>Otestujte svůj kód

Chcete-li otestovat aplikaci v sadě Visual Studio, stiskněte **F5** ke spuštění projektu. Prohlížeči se otevře http://<span></span>localhost: {port} umístění a zobrazí **přihlásit pomocí Microsoft** tlačítko. Kliknutím na tlačítko Spustit proces přihlášení.

Pokud jste připravení spustit test, použít účet Microsoft Azure Active Directory (Azure AD) (pracovní nebo školní účet) nebo osobní účet Microsoft (<span>za provozu.</span> com nebo <span>aplikace outlook.</span> com) pro přihlášení.

![Přihlaste se pomocí Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Přihlaste se ke svému účtu Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Zobrazení výsledků aplikace
Po přihlášení se uživatel přesměruje na domovskou stránku vašeho webu. Domovská stránka je adresu URL HTTPS, který je uveden v informace o registraci aplikace v portálu pro registraci aplikace společnosti Microsoft. Domovská stránka obsahuje uvítací zprávu "Hello \<uživatele >," odkaz Odhlásit se a odkaz na zobrazení deklaracích identity uživatele. Přejde na odkaz pro deklarace identity uživatele k **Authorize** kontroleru, který jste vytvořili dříve.

### <a name="browse-to-see-the-users-claims"></a>Procházet zobrazíte deklaracích identity uživatele
Informace o deklaracích identity uživatele, vyberte odkaz a přejděte do zobrazení řadiče, které je k dispozici pouze pro ověřené uživatele.

#### <a name="view-the-claims-results"></a>Zobrazení výsledků deklarace identity
Po přejití řadiče zobrazení, měli byste vidět tabulku, která obsahuje základní vlastnosti pro uživatele:

|Vlastnost |Hodnota |Popis |
|---|---|---|
|**Název** |Jméno a příjmení uživatele | Jméno a příjmení uživatele.
|**Uživatelské jméno** |uživatel<span>@domain.com</span> | Uživatelské jméno, který se používá k identifikaci uživatele.
|**Předmět** |Předmět |Řetězec, který jednoznačně identifikuje uživatele na webu.|
|**ID klienta** |Guid | A **guid** který jedinečně reprezentuje uživatele organizaci Azure AD.|

Kromě toho měli byste vidět tabulku všechny deklarace identity, které jsou v žádosti o ověření. Další informace najdete v tématu [seznam deklarací identity, které jsou v tokenu ID služby Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Test přístupu k metodě, která má atribut autorizovat (volitelné)
K otestování přístup k **Autorizovat** řadiče pro deklarace identity uživatele jako anonymní uživatel, postupujte takto:
1. Vyberte odkaz Odhlásit uživatele a dokončete proces přihlášení.
2. V prohlížeči zadejte http://<span></span>localhost: {port} / ověřený přístup k řadiči, která je chráněná pomocí **Autorizovat** atribut.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Očekávané výsledky po přístup k chráněné řadiče
Se zobrazí výzva k ověření použít chráněné řadiče zobrazení.

## <a name="additional-information"></a>Další informace

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Chránit celého webu
K ochraně celého webu, v **Global.asax** soubor, přidejte **třídy AuthorizeAttribute** atribut **GlobalFilters** filtrovat v **aplikace S_pustit** metoda:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>Omezení přístupu k přihlášení do aplikace
Ve výchozím nastavení můžete osobní účty, jako je live.com, outlook.com a dalších přihlásit k aplikaci. Pracovní nebo školní účty v organizacích, které jsou integrované s Azure AD můžete také přihlásit ve výchozím nastavení.

Pokud chcete omezit přístup přihlášení uživatelů pro vaši aplikaci, jsou k dispozici několik možností.

#### <a name="restrict-access-to-a-single-organization"></a>Omezit přístup k jedné organizace
Můžete omezit přístup přihlášení pro vaši aplikaci pouze uživatelské účty, které se nacházejí v jedné organizaci Azure AD:
1. V **web.config** souboru, změňte hodnotu **klienta** parametr. Změňte hodnotu z **běžné** k názvu klienta organizace, jako například **contoso.onmicrosoft.com**.
2. Ve vaší **OWIN při spuštění** třídy, nastavte **atribut ValidateIssuer** argument **true**.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Omezit přístup k seznamu organizací
Můžete omezit přístup k přihlášení do pouze uživatelské účty, které jsou v organizaci Azure AD, která je v seznamu povolených organizací:
1. V **web.config** souborů ve vaší **OWIN při spuštění** třídy, nastavte **atribut ValidateIssuer** argument **true**.
2. Nastavte hodnotu **ValidIssuers** parametru do seznamu povolených organizací.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Použít vlastní metodu k ověření vystavitele
Můžete implementovat vlastní metodu pro ověření vystavitelů pomocí **IssuerValidator** parametr. Další informace o tom, jak tento parametr použijte, přečtěte si informace o [parametry tokenvalidationparameters třída](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) na webu MSDN.

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
