
## <a name="test-your-code"></a>Otestujte svůj kód

Chcete-li otestovat aplikaci v sadě Visual Studio, stiskněte **F5** ke spuštění projektu. **MainWindow** pro vaše aplikace se zobrazí:

![Testování vaší aplikace](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Pokud jste připravení spustit test, používáte pro přihlášení k účtu Microsoft Azure Active Directory (pracovní nebo školní účet) nebo účtu Microsoft (live.com, outlook.com). Při prvním spuštění aplikace, se zobrazí výzva k přihlášení:

![Přihlaste se k aplikaci](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Zadejte souhlasu pro přístup k aplikaci
Při prvním přihlášení do aplikace, se zobrazí výzva k souhlasíte s tím, aby aplikaci pro váš profil přístup a pro přihlášení: 

![Zadejte váš souhlas pro přístup k aplikaci](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Zobrazení výsledků aplikace
Po přihlášení, měli byste vidět informace o profilu uživatele vrácený volání rozhraní Microsoft Graph API. Výsledky se zobrazí v **výsledky volání rozhraní API** pole. Základní informace o token, který byl získán prostřednictvím volání **AcquireTokenAsync** nebo **AcquireTokenSilentAsync** má být zobrazen ve **informace o tokenu** pole. Výsledky obsahují následující vlastnosti:

|Vlastnost  |Formát  |Popis |
|---------|---------|---------|
|**Název** |Jméno a příjmení uživatele |Jméno a příjmení uživatele.|
|**Uživatelské jméno** |<span>user@domain.com</span> |Uživatelské jméno, který se používá k identifikaci uživatele.|
|**Platnost tokenu vyprší** |Data a času |Čas, kdy vyprší platnost tokenu. MSAL rozšiřuje datum vypršení platnosti obnovením token podle potřeby.|
|**Přístupový Token** |Řetězec |Řetězec tokenu, který je odeslán protokolu HTTP požadavků, které vyžadují **autorizace** záhlaví.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API **user.read** obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každou aplikaci, která je zaregistrovaná na portálu pro registraci. Ostatní rozhraní API pro Microsoft Graph, jakož i vlastní rozhraní API pro váš server back-end může vyžadovat další obory. Vyžaduje rozhraní Microsoft Graph API **Calendars.Read** oboru seznam kalendářích uživatele.

Chcete-li získat přístup k kalendářům uživatele v rámci aplikace, přidejte **Calendars.Read** delegovaná oprávnění k registraci informace o aplikaci. Poté, přidejte **Calendars.Read** obor na **acquireTokenSilent** volání. 

>[!NOTE]
>Uživatel může být vyzvání pro další souhlas všech uživatelů, jak zvýšit počet oborů.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
