
## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Použití knihovny ověřování společnosti Microsoft (MSAL) k získání tokenu pro rozhraní Microsoft Graph API

1.  Otevřete: `MainActivity` (v části `app`  >  `java`  >  `{domain}.{appname}`)
2.  Přidejte následující importy:

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Nahraďte `MainActivity` třídy s níže:
</li>
</ol>

```java
public class MainActivity extends AppCompatActivity {

    final static String CLIENT_ID = "[Enter the application Id here]";
    final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
    final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

    /* UI & Debugging Variables */
    private static final String TAG = MainActivity.class.getSimpleName();
    Button callGraphButton;
    Button signOutButton;

    /* Azure AD Variables */
    private PublicClientApplication sampleApp;
    private AuthenticationResult authResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        callGraphButton = (Button) findViewById(R.id.callGraph);
        signOutButton = (Button) findViewById(R.id.clearCache);

        callGraphButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onCallGraphClicked();
            }
        });

        signOutButton.setOnClickListener(new View.OnClickListener() {
            public void onClick(View v) {
                onSignOutClicked();
            }
        });

  /* Configure your sample app and save state for this activity */
        sampleApp = null;
        if (sampleApp == null) {
            sampleApp = new PublicClientApplication(
                    this.getApplicationContext(),
                    CLIENT_ID);
        }

  /* Attempt to get a user and acquireTokenSilent
   * If this fails we do an interactive request
   */
        List<User> users = null;

        try {
            users = sampleApp.getUsers();

            if (users != null && users.size() == 1) {
          /* We have 1 user */

                sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
            } else {
          /* We have no user */

          /* Let's do an interactive request */
                sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
            }
        } catch (MsalClientException e) {
            Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

        } catch (IndexOutOfBoundsException e) {
            Log.d(TAG, "User at this position does not exist: " + e.toString());
        }

    }

//
// App callbacks for MSAL
// ======================
// getActivity() - returns activity so we can acquireToken within a callback
// getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
// getAuthInteractiveCallback() - callback defined to handle acquireToken() case
//

    public Activity getActivity() {
        return this;
    }

    /* Callback method for acquireTokenSilent calls 
     * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
     * else errors that we need to do an interactive request.
     */
    private AuthenticationCallback getAuthSilentCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call Graph now */
                Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
                authResult = authenticationResult;

            /* call graph */
                callGraphAPI();

            /* update the UI to post call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed to acquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
                } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled the authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }


    /* Callback used for interactive request.  If succeeds we use the access
         * token to call the Microsoft Graph. Does not check cache
         */
    private AuthenticationCallback getAuthInteractiveCallback() {
        return new AuthenticationCallback() {
            @Override
            public void onSuccess(AuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
                Log.d(TAG, "Successfully authenticated");
                Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
                authResult = authenticationResult;

            /* call Graph */
                callGraphAPI();

            /* update the UI to post call Graph state */
                updateSuccessUI();
            }

            @Override
            public void onError(MsalException exception) {
            /* Failed to acquireToken */
                Log.d(TAG, "Authentication failed: " + exception.toString());

                if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside MsalError.java */
                } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
                }
            }

            @Override
            public void onCancel() {
            /* User canceled the authentication */
                Log.d(TAG, "User cancelled login.");
            }
        };
    }

    /* Set the UI for successful token acquisition data */
    private void updateSuccessUI() {
        callGraphButton.setVisibility(View.INVISIBLE);
        signOutButton.setVisibility(View.VISIBLE);
        findViewById(R.id.welcome).setVisibility(View.VISIBLE);
        ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                authResult.getUser().getName());
        findViewById(R.id.graphData).setVisibility(View.VISIBLE);
    }

    /* Use MSAL to acquireToken for the end-user
     * Callback will call Graph api w/ access token & update UI
     */
    private void onCallGraphClicked() {
        sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
    }

    /* Handles the redirect from the System Browser */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
    }

}
```
<!--start-collapse-->
### <a name="more-information"></a>Další informace
#### <a name="getting-a-user-token-interactive"></a>Získávání interaktivní token uživatele
Volání `AcquireTokenAsync` metoda výsledky v okně výzvy pro uživatele k přihlášení. Aplikace obvykle vyžadují uživatel interaktivní přihlášení při prvním potřebují přístup k chráněnému prostředku nebo když tichou operaci získat token selže (například heslo uživatele jeho platnost).

#### <a name="getting-a-user-token-silently"></a>Získání tokenu uživatele bez upozornění
`AcquireTokenSilentAsync`zpracovává tokenu pořízení a obnovení bez nutnosti zásahu uživatele. Po `AcquireTokenAsync` se spustí poprvé, `AcquireTokenSilentAsync` je metoda běžně používají k získání tokenů pro přístup k chráněným prostředkům pro následující volání - jako volání na vyžádání nebo obnovení tokeny jsou vytvářeny bezobslužně.
Nakonec `AcquireTokenSilentAsync` selže – např. uživatel má odhlášení, nebo došlo ke změně hesla na jiném zařízení. Když MSAL zjistí, že problém lze vyřešit tím, že vyžaduje interaktivní akce, aktivuje se ho `MsalUiRequiredException`. Aplikace může zpracovat výjimku dvěma způsoby:

1.  Volání proti `AcquireTokenAsync` okamžitě, výsledkem výzvy k přihlášení. Tento vzor se obvykle používá v online aplikace tam, kde není žádná offline v aplikaci k dispozici obsah pro uživatele. Ukázka generované touto s průvodcem instalací používá tento vzor: Zobrazí se v čase akce první spustit ukázku: vzhledem k tomu, že žádný uživatel nikdy použili aplikaci, `PublicClientApp.Users.FirstOrDefault` bude obsahovat hodnotu null a `MsalUiRequiredException` dojde k výjimce. Kód v ukázce pak ošetří výjimku voláním `AcquireTokenAsync` výsledkem výzvy k přihlášení.
2.  Aplikace můžete udělat taky vizuální označení uživateli, které interaktivní přihlášení je povinné, tak, aby si uživatel může vybrat správný čas pro přihlášení, nebo můžete zkusit aplikaci `AcquireTokenSilentAsync` později. To se často používá, když je uživatel moci funkce aplikace bez narušení – například je offline obsah k dispozici v aplikaci. V takovém případě se můžete rozhodnout uživatele, když chtějí přihlášení pro přístup k chráněnému prostředku nebo aktualizovat zastaralé informace, nebo aplikaci můžete rozhodnout opakujte `AcquireTokenSilentAsync` obnovení po síti poté, co byla dočasně nedostupná.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Volání rozhraní Graph API Microsoft pomocí tokenu, který jste obdrželi
1.  Přidejte následující metody do `MainActivity` třídy:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() throws AuthFailureError {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Další informace o tom, jak volání REST chráněné rozhraní API

V této ukázkové aplikaci `callGraphAPI` volání `getAccessToken` a potom provádí HTTP `GET` požadavek na prostředek, který vyžaduje token a vrátí obsah. Tato metoda přidá získal token v *HTTP autorizační hlavičky*. Tato ukázka je prostředek Microsoft Graph API *mi* koncového bodu – zobrazí informace o profilu uživatele.
<!--end-collapse-->

## <a name="setup-sign-out"></a>Instalační program odhlášení

1.  Přidejte následující metody do `MainActivity` třídy:

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information"></a>Další informace

`onSignOutClicked`výše odebere uživatele z mezipaměti uživatele MSAL – se efektivně tak dozví MSAL zapomenutí aktuálního uživatele, takže budoucí žádost o získání tokenu bude úspěšné pouze v případě, že je k interaktivní.
I když aplikace v této ukázce podporuje jenom jednoho konkrétního uživatele, MSAL podporuje scénáře, kde více účtů může být přihlášeného ve stejnou dobu – příklad je e-mailové aplikace, které má uživatel více účtů.
<!--end-collapse-->
