
1. Otevřete projekt v Android Studio.

2. V **Project Exploreru** v nástroji Android Studio otevřete soubor ToDoActivity.java a přidejte následující příkazy pro import:

        import java.util.concurrent.ExecutionException;
        import java.util.concurrent.atomic.AtomicBoolean;

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;

        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
        import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;

3. Přidejte následující metodu do **ToDoActivity** třídy:

        // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
        public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

        private void authenticate() {
            // Login using the Google provider.
            mClient.login("Google", "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            // When request completes
            if (resultCode == RESULT_OK) {
                // Check the request code matches the one we send in the login request
                if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                    MobileServiceActivityResult result = mClient.onActivityResult(data);
                    if (result.isLoggedIn()) {
                        // login succeeded
                        createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                        createTable();
                    } else {
                        // login failed, check the error message
                        String errorMessage = result.getErrorMessage();
                        createAndShowDialog(errorMessage, "Error");
                    }
                }
            }
        }

    Tento kód vytvoří metodu pro zpracování procesu ověřování Google. Zobrazí se dialogové okno zobrazí ID ověřeného uživatele. Pouze můžete přejít na úspěšné ověření.

    > [!NOTE]
    > Pokud používáte zprostředkovatele identity než Google, změňte hodnotu předaný **přihlášení** metodu pro jednu z následujících hodnot: _MicrosoftAccount_, _Facebook_, _Twitter_, nebo _windowsazureactivedirectory_.

4. V **onCreate** metoda, přidejte následující řádek kódu po kód, který vytvoří instanci `MobileServiceClient` objektu.

        authenticate();

    Toto volání zahájí proces ověřování.

5. Přesunout kód zbývající po `authenticate();` v **onCreate** metoda na nový **createTable** metoda:

        private void createTable() {

            // Get the table instance to use.
            mToDoTable = mClient.getTable(ToDoItem.class);

            mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

            // Create an adapter to bind the items with the view.
            mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
            ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
            listViewToDo.setAdapter(mAdapter);

            // Load the items from Azure.
            refreshItemsFromTable();
        }

6. Aby přesměrování funguje podle očekávání, přidejte následující fragment _RedirectUrlActivity_ k _AndroidManifest.xml_:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="{url_scheme_of_your_app}"
                    android:host="easyauth.callback"/>
            </intent-filter>
        </activity>

7. Přidat redirectUriScheme k _build.gradle_ vaší aplikace Android.

        android {
            buildTypes {
                release {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
                debug {
                    // … …
                    manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
                }
            }
        }

8. Přidejte com.android.support:customtabs:23.0.1 závislosti ve vaší build.gradle:

      závislosti {/ / … zkompilovat 'com.android.support:customtabs:23.0.1'}

9. Z **spustit** nabídky, klikněte na tlačítko **spuštění aplikace** spusťte aplikaci a přihlaste se pomocí zprostředkovatele identity vybrané.

> [!WARNING]
> Schéma adresy URL uvedené rozlišuje velká a malá písmena.  Ujistěte se, že všechny výskyty `{url_scheme_of_you_app}` velká a malá písmena.

Pokud jste úspěšně přihlášeni, aplikace se budou spouštět bez chyby a nyní byste měli mít provést aktualizace dat a zadat dotaz na back-end službu.
