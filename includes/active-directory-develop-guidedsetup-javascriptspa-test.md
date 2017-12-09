
## <a name="test-your-code"></a>Otestujte svůj kód

### <a name="test-with-visual-studio"></a>Test pomocí sady Visual Studio
Pokud používáte Visual Studio, stiskněte klávesu **F5** ke spuštění projektu. Prohlížeči se otevře http://<span></span>localhost: {port} umístění a zobrazí **volání Microsoft Graph API** tlačítko.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Test s Pythonem nebo jiný webový server
Pokud nepoužíváte Visual Studio, ujistěte se, že váš webový server je spuštěna. Konfigurace serveru pro naslouchání na portu TCP, který je založen na umístění vaší **index.html** souboru. Pro jazyk Python spusťte pro naslouchání na portu spuštěním terminálu příkazového řádku ve složce aplikace:
 
```bash
python -m http.server 8080
```
Otevřete prohlížeč a zadejte http://<span></span>localhost: 8080 nebo http://<span></span>localhost: {port} kde **portu** je port, který váš webový server naslouchá. Měli byste vidět obsah souboru index.html a **volání Microsoft Graph API** tlačítko.

## <a name="test-your-application"></a>Testování vaší aplikace

Po načtení souboru index.html prohlížeče vyberte **volání Microsoft Graph API**. Při prvním spuštění aplikace, prohlížeč vás přesměruje na koncový bod v2.0 Microsoft Azure Active Directory (Azure AD) a se zobrazí výzva k přihlášení:
 
![Přihlaste se ke svému účtu JavaScript SPA](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Zadejte souhlasu pro přístup k aplikaci

Při prvním přihlášení do aplikace, se zobrazí výzva k souhlasíte s tím, aby aplikaci pro váš profil přístup a pro přihlášení:

![Zadejte váš souhlas pro přístup k aplikaci](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Zobrazení výsledků aplikace
Po přihlášení, měli byste vidět informace z vašeho profilu uživatele v **odpovědi volání rozhraní API grafu** pole.
 
![Očekávané výsledky z volání Microsoft Graph API](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

By se měla zobrazit základní informace o token, který byl získán v **tokenu přístupu** a **ID tokenu deklarací** polí.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API **user.read** obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každou aplikaci, která je zaregistrovaná na portálu pro registraci. Ostatní rozhraní API pro Microsoft Graph, jakož i vlastní rozhraní API pro váš server back-end může vyžadovat další obory. Vyžaduje rozhraní Microsoft Graph API **Calendars.Read** oboru seznam kalendářích uživatele.

Chcete-li získat přístup k kalendářům uživatele v rámci aplikace, přidejte **Calendars.Read** delegovaná oprávnění k registraci informace o aplikaci. Poté, přidejte **Calendars.Read** obor na **acquireTokenSilent** volání. 

>[!NOTE]
>Uživatel může být vyzvání pro další souhlas všech uživatelů, jak zvýšit počet oborů.

Pokud rozhraní API back-end nevyžaduje obor (nedoporučuje se), můžete použít **clientId** jako obor v **acquireTokenSilent** a **acquireTokenRedirect** volání.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
