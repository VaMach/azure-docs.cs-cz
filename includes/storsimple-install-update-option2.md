<!--author=SharS last changed: 03/17/2016-->

#### <a name="to-install-update-12-from-the-azure-classic-portal"></a>K instalaci aktualizace 1.2 z portálu Azure classic
1. V portálu Azure classic, přejděte do **zařízení** a vyberte zařízení.
2. Přejděte na **zařízení** > **konfigurace**.
3. V části **síťových rozhraní**, nejdřív ověřte, zda máte alespoň jeden síťové rozhraní, které je povolen iSCSI. Vyhledejte síťové rozhraní (jiné než DATA 0), který má bránu přiřazen.
4. Zakažte rozhraní sítě, který má přiřazenou brány a uložit změny konfigurace. Poznamenejte si nastavení síťového rozhraní jsou uchovány a proto pokud znovu povolíte toto síťové rozhraní později, portálu budou obnovit původní nastavení.
5. Teď můžete [použít portál Azure classic k instalaci aktualizace 1.2](#install-update-12-via-the-azure-classic-portal). Postupujte podle pokynů od kroku 3 tohoto postupu. Po nainstalování všech aktualizací, můžete znovu povolit síťového rozhraní, které jste zakázali.

