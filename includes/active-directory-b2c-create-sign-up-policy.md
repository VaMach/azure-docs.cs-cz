Pokud chcete ve své aplikaci povolit registraci, budete muset vytvořit zásadu registrace. Tato zásada popisuje prostředí, kterými uživatelů budou procházet při registraci, a obsah tokenů, které aplikace obdrží po úspěšné registraci.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Klikněte na **Zásady registrace**.

Klikněte na **Přidat** v horní části okna.

**Název** určuje název zásady registrace používaný vaší aplikací. Zadejte například **SiUp**.

Klikněte na **Zprostředkovatelé identity** a vyberte **Registrace pomocí e-mailu**. Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni. Klikněte na **OK**.

Klikněte na **Atributy registrace**. Tady zvolíte atributy, které od uživatele chcete během registrace shromažďovat. Vyberte například **Země/oblast**, **Zobrazované jméno** a **PSČ**. Klikněte na **OK**.

Klikněte na **Deklarace identit aplikace**. Tady zvolíte deklarace identit, které se mají vracet v tokenech odesílaných zpět do aplikace po úspěšné registraci. Vyberte například **Zobrazované jméno**, **Zprostředkovatel identity**, **PSČ**, **Uživatel je nový** a **ID objektu uživatele**.

Klikněte na možnost **Vytvořit**. Vytvořená zásada se zobrazí jako **B2C_1_SiUp** (fragment **B2C\_1\_** se přidá automaticky) v okně **Zásady registrace**.

Otevřete zásadu kliknutím na **B2C_1_SiUp**.

V rozevíracím seznamu **Aplikace** vyberte **Aplikace Contoso B2C** a v rozevíracím seznamu **Adresa URL odpovědi / Identifikátor URI přesměrování** vyberte `https://localhost:44321/`.

Klikněte na **Spustit**. Otevře se nová karta prohlížeče a můžete si projít uživatelské prostředí pro registraci ve vaší aplikaci.

> [!NOTE]
> Trvá až minutu, než se vytvoření zásady a aktualizace projeví.
>