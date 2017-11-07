## <a name="prepare-your-intel-nuc"></a>Příprava vašeho Intel NUC

Chcete-li dokončit nastavení hardwaru, je potřeba:

- Připojte vaše Intel NUC k napájení součástí sady.
- Vaše Intel NUC připojte k síti pomocí kabelu Ethernet.

Teď jste dokončili nastavení hardwaru vaše zařízení brány Intel NUC.

### <a name="sign-in-and-access-the-terminal"></a>Přihlaste se a přístup k terminálu

Máte dvě možnosti pro přístup k Terminálové prostředí na vaše Intel NUC:

- Pokud máte klávesnici a monitorování, které jsou připojené k vaší NUC Intel, můžete přímo přistupovat prostředí. Výchozí přihlašovací údaje jsou uživatelské jméno **kořenové** a heslo **kořenové**.

- Přístup k prostředí na vaše Intel NUC pomocí protokolu SSH ze stolního počítače.

#### <a name="sign-in-with-ssh"></a>Přihlaste se pomocí protokolu SSH

Přihlásit se přes SSH, musíte na IP adresu vašeho NUC Intel. Pokud máte klávesnici a monitorování, které jsou připojené k vaší NUC Intel, použijte `ifconfig` příkazu najděte IP adresu. Můžete taky připojte ke směrovači seznam adres zařízení ve vaší síti.

Přihlaste se pomocí uživatelského jména **kořenové** a heslo **kořenové**.

#### <a name="optional-share-a-folder-on-your-intel-nuc"></a>Volitelné: Sdílené složky na vaše NUC Intel

Volitelně můžete sdílet složky na vaše Intel NUC s prostředí plochy. Sdílení složky vám umožní použít upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) Chcete-li upravit soubory na vaše Intel NUC místo použití `nano` nebo `vi`.

Sdílení složky s Windows, konfigurace serveru Samba na Intel NUC. Můžete taky použijte SFTP server v NUC Intel s klientem SFTP na stolního počítače.
