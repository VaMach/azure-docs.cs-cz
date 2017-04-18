### <a name="create-a-nodejs-application"></a>Vytvoření aplikace Node.js
* Vytvořte nový soubor JavaScript s názvem `sender.js`.

### <a name="add-the-relay-npm-package"></a>Přidání balíčku NPM služby Relay
* Spusťte z příkazového řádku uzlu ve složce projektu příkaz `npm install hyco-ws`.

### <a name="write-some-code-to-send-messages"></a>Napsání kódu pro odesílání zpráv
1. Na začátek souboru `sender.js` přidejte následující konstanty (`constants`):
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. V podrobnostech o hybridním připojení přidejte do třídy `sender.js` následující konstanty (`constants`) služby Relay: Zástupné symboly v závorkách nahraďte odpovídajícími hodnotami, které jste získali při vytváření hybridního připojení.
   
   1. `const ns` – obor názvů služby Relay (použijte plně kvalifikovaný název domény, např. `{namespace}.servicebus.windows.net`)
   2. `const path` – název hybridního připojení
   3. `const keyrule` – název klíče SAS
   4. `const key` – hodnota klíče SAS
3. Do souboru `sender.js` přidejte následující kód:
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    Soubor listener.js by měl vypadat takto:
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```

