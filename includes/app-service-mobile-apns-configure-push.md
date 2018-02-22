

1. Na počítači Mac spusťte **přístup do řetězce klíčů**. V levém navigačním panelu v části **kategorie**, otevřete **Moje certifikáty**. Najděte certifikát SSL, který jste si stáhli v předchozí části a pak zveřejnit, její obsah. Vyberte jenom certifikát (nevybírejte privátní klíč). Potom [ho exportovat](https://support.apple.com/kb/PH20122?locale=en_US).
2. V [portál Azure](https://portal.azure.com/), vyberte **Procházet vše** > **App Services**. Potom vyberte Mobile Apps back-end. 
3. V části **nastavení**, vyberte **aplikace služby Push**. Pak vyberte vaším jménem centra oznámení. 
3. Přejděte na **služeb nabízených oznámení Apple** > **nahrát certifikát**. Nahrát soubor .p12, výběr správného **režimu** (v závislosti na tom, jestli vašeho klienta SSL certifikát z dříve je produkční nebo izolovaného prostoru). Uložte změny.

Služby je nyní nakonfigurováno pro práci s nabízenými oznámeními v systému iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
