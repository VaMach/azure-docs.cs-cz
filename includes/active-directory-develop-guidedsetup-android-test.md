## <a name="test-your-code"></a>Otestujte svůj kód

1. Nasazení kódu do vašeho zařízení nebo emulátor.

2. Až budete připraveni k testování aplikace, použijte účet s Azure Active Directory (pracovní nebo školní účet) nebo účtu Microsoft (live.com, outlook.com) pro přihlášení. 

    ![Testování vaší aplikace](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Zadejte uživatelské jméno a heslo](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Zadejte souhlasu pro přístup k aplikaci
Při prvním přihlášení do aplikace, se taky výzva k poskytování souhlas umožňuje aplikaci získat přístup na váš profil a přihlášení v, jak je vidět tady: 

![Zadejte váš souhlas pro přístup k aplikaci](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Zobrazení výsledků aplikace
Po přihlášení, měli byste vidět výsledky, které jsou při volání metody pro rozhraní Microsoft Graph API. Volání rozhraní Graph API Microsoft **mi** vrátí koncový bod [profil uživatele](https://graph.microsoft.com/v1.0/me). Seznam běžných Microsoft Graph koncových bodů najdete v tématu [dokumentaci pro vývojáře společnosti Microsoft Graph API](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Další informace o oborech a přidělená oprávnění

Vyžaduje rozhraní Microsoft Graph API *user.read* obory a čtení profilu uživatele. Tento obor je automaticky přidán ve výchozím nastavení každá aplikace, které je registrované v portálu pro registraci aplikace. Ostatní rozhraní API pro Microsoft Graph, jakož i vlastní rozhraní API pro váš server back-end může vyžadovat další obory. Vyžaduje rozhraní Microsoft Graph API *Calendars.Read* oboru seznam kalendářích uživatele. 

Chcete-li získat přístup k kalendářům uživatele v rámci aplikace, přidejte *Calendars.Read* delegovaná oprávnění k registraci informace o aplikaci. Poté, přidejte *Calendars.Read* obor na `acquireTokenSilent` volání. 

>[!NOTE]
>Uživatel může být vyzvání pro další souhlas všech uživatelů, jak zvýšit počet oborů.

<!--end-collapse-->

[!INCLUDE  [Help and support](active-directory-develop-help-support-include.md)]
