

1. Na počítači Mac spusťte **přístup do řetězce klíčů**. V levém navigačním panelu v části **kategorie**, otevřete **Moje certifikáty**. Najděte certifikát SSL, které jste stáhli v předchozí části a zveřejnit, její obsah. Vyberte jenom certifikát (nevybírejte privátní klíč), a [ho exportovat](https://support.apple.com/kb/PH20122?locale=en_US).
2. V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **Procházet vše** > **App Services**a klikněte na vaše mobilní aplikace back-end. V části **nastavení**, klikněte na tlačítko **aplikace služby Push**a potom klikněte na název vašeho centra oznámení. Přejděte na **služeb nabízených oznámení Apple** > **nahrát certifikát**. Nahrát soubor .p12, výběr správného **režimu** (v závislosti na tom, jestli vašeho klienta SSL certifikát z dříve je produkční nebo izolovaného prostoru). Uložte změny.

Služby je nyní nakonfigurováno pro práci s nabízenými oznámeními v systému iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
