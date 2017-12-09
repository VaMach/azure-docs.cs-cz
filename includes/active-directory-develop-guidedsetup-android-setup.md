
## <a name="set-up-your-project"></a>Nastavení projektu

> Stáhněte si tento ukázkový projekt Android Studio místo dávají přednost? [Stažení projektu](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) a pokračujte [krok konfigurace](#create-an-application-express) před provedením konfigurace ukázka kódu.


### <a name="create-a-new-project"></a>Vytvoření nového projektu 
1.  Otevřete Android Studio a přejděte do:`File` > `New` > `New Project`
2.  Název aplikace a klikněte na tlačítko`Next`
3.  Je nutné vybrat *21 rozhraní API nebo novější (Android 5.0)* a klikněte na tlačítko`Next`
4.  Nechte `Empty Activity`, klikněte na tlačítko `Next`, pak`Finish`


### <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Do projektu přidejte knihovny ověřování společnosti Microsoft (MSAL)
1.  V nástroji Android Studio přejděte do:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Zkopírujte a vložte následující kód pod `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>O tomto balíčku

Výše uvedené balíček nainstaluje Microsoft ověřování knihovny (MSAL). MSAL zpracovává získávání, ukládání do mezipaměti a aktualizaci tokeny uživatel používá pro přístup k rozhraní API, které jsou chráněné službou Azure Active Directory v2 koncový bod.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Vytvoření uživatelského rozhraní aplikace

1.  Otevřete: `activity_main.xml` v části`res` > `layout`
2.  Změna rozložení aktivity z `android.support.constraint.ConstraintLayout` či jiné na`LinearLayout`
3.  Přidat `android:orientation="vertical"` vlastnost `LinearLayout` uzlu
4.  Zkopírujte a vložte následující kód do `LinearLayout` uzlu, nahraďte aktuální obsahu:

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```

