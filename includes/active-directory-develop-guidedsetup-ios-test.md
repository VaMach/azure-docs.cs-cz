## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Test dotazování Microsoft Graph API z vaší aplikace iOS

Spustí kód v simulátoru, stiskněte klávesu **příkaz** + **R**.

![Testování vaší aplikace v simulátoru](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Až budete připraveni k testování, vyberte **volání Microsoft Graph API**. Po zobrazení výzvy zadejte své uživatelské jméno a heslo.

### <a name="provide-consent-for-application-access"></a>Zadejte souhlasu pro přístup k aplikaci
Při prvním přihlášení do aplikace, se zobrazí výzva k souhlasíte s tím, aby aplikaci pro váš profil přístup a pro přihlášení:

![Zadejte váš souhlas pro přístup k aplikaci](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Zobrazení výsledků aplikace
Po přihlášení, měli byste vidět uživatele informace z vašeho profilu Microsoft Graph API volání v vráceny **protokolování** části. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API **user.read** obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každou aplikaci, která je zaregistrovaná na portálu pro registraci. Ostatní rozhraní API pro Microsoft Graph, jakož i vlastní rozhraní API pro váš server back-end může vyžadovat další obory. Vyžaduje rozhraní Microsoft Graph API **Calendars.Read** oboru seznam kalendářích uživatele.

Chcete-li získat přístup k kalendářům uživatele v rámci aplikace, přidejte **Calendars.Read** delegovaná oprávnění k registraci informace o aplikaci. Poté, přidejte **Calendars.Read** obor na **acquireTokenSilent** volání. 

>[!NOTE]
>Uživatel může být vyzvání pro další souhlas všech uživatelů, jak zvýšit počet oborů.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
