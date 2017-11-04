
Předchozí příklad ukázal standardní přihlášení, která vyžaduje, aby klient kontaktovat poskytovatele identit a back-end služby Azure při každém spuštění aplikace. Tato metoda je neefektivní a může mít problémy související s využití, pokud mnoho zákazníků, pokuste se spustit aplikaci současně. Lepší přístup je ukládat do mezipaměti autorizační token vrácený služby Azure a pokuste se použít tento první před použitím u založenou na poskytovateli přihlášení.

> [!NOTE]
> Můžete mezipaměti token vystavený back-end služby Azure bez ohledu na to, jestli používáte ověřování klienta spravovat nebo spravované služby. Tento kurz používá službu spravovat ověřování.
>
>

1. Otevřete soubor ToDoActivity.java a přidejte následující příkazy pro import:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Přidejte následující členy `ToDoActivity` třídy.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. V souboru ToDoActivity.java, přidejte následující definice pro `cacheUserToken` metoda.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Tato metoda ukládá ID uživatele a token v souboru předvoleb, která je označena privátní. To by měla chránit přístup k mezipaměti, aby další aplikace v zařízení nebudete mít přístup k tokenu. Předvolba je v izolovaném prostoru pro aplikace. Ale pokud někdo získá přístup k zařízení, je možné, že získají přístup k tokenu mezipaměti jinými způsoby.

   > [!NOTE]
   > Token s šifrování, jde dál chránit, pokud token přístup k datům se považuje za vysoce citlivé a někdo může získat přístup k zařízení. Zcela bezpečné řešení je nad rámec tohoto kurzu, ale a závisí na požadavky na zabezpečení.
   >
   >
4. V souboru ToDoActivity.java, přidejte následující definice pro `loadUserTokenCache` metoda.

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. V *ToDoActivity.java* souboru, nahraďte `authenticate` metoda pomocí následující metody, která používá mezipamětí tokenů. Zprostředkovatel přihlášení změňte, pokud chcete použít účet, než Google.

        private void authenticate() {
            // We first try to load a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed to load a token cache, login and create a token cache
            else
            {
                // Login using the Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. Sestavení aplikace a testovací ověření pomocí platného účtu. Spusťte alespoň dvakrát. Při prvním spuštění mělo by se zobrazit výzva k přihlášení a vytvoření mezipamětí tokenů. Potom každé spuštění pokusí načíst mezipamětí tokenů pro ověřování. Nesmí se budete muset přihlásit.
