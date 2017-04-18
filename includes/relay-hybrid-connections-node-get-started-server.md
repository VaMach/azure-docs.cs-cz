### <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
* Vytvořte nový soubor JavaScript s názvem `listener.js`.

### <a name="add-the-relay-npm-package"></a>Přidání balíčku NPM služby Relay
* Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.

### <a name="write-some-code-to-receive-messages"></a>Napsání kódu pro přijímání zpráv
1. Na začátek souboru `listener.js` přidejte následující konstanty (`constant`):
   
    ```js
    const WebSocket = require('hyco-ws');
    ```
2. V podrobnostech o hybridním připojení přidejte do třídy `listener.js` následující konstanty (`constants`) služby Relay: Zástupné symboly v závorkách nahraďte odpovídajícími hodnotami, které jste získali při vytváření hybridního připojení.
   
   1. `const ns` – obor názvů služby Relay (použijte plně kvalifikovaný název domény, např. `{namespace}.servicebus.windows.net`)
   2. `const path` – název hybridního připojení
   3. `const keyrule` – název klíče SAS
   4. `const key` – hodnota klíče SAS
3. Do souboru `listener.js` přidejte následující kód:
   
    ```js
    var wss = WebSocket.createRelayedServer(
    {
        server : WebSocket.createRelayListenUri(ns, path),
        token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
    }, 
    function (ws) {
        console.log('connection accepted');
        ws.onmessage = function (event) {
            console.log(event.data);
        };
        ws.on('close', function () {
            console.log('connection closed');
        });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```
    Soubor listener.js by měl vypadat takto:
   
    ```js
    const WebSocket = require('hyco-ws');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var wss = WebSocket.createRelayedServer(
        {
            server : WebSocket.createRelayListenUri(ns, path),
            token: WebSocket.createRelayToken('http://' + ns, keyrule, key)
        }, 
        function (ws) {
            console.log('connection accepted');
            ws.onmessage = function (event) {
                console.log(event.data);
            };
            ws.on('close', function () {
                console.log('connection closed');
            });       
    });
   
    console.log('listening');
   
    wss.on('error', function(err) {
        console.log('error' + err);
    });
    ```

