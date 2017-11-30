V následující tabulce jsou uvedeny vazby, které jsou podporovány v dvě hlavní verze modulu runtime Azure Functions.

| Typ | 1.x | 2.x | Trigger | Vstup | Výstup |  
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|  
| [Databáze Cosmos](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔<sup>1</sup>|✔|✔|✔|  
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|  
| [Externí soubor](../articles/azure-functions/functions-bindings-external-file.md)<sup>2</sup>    |✔|| |✔|✔|  
| [Externí tabulky](../articles/azure-functions/functions-bindings-external-table.md)<sup>2</sup>  |✔|| |✔|✔|  
| [HTTP](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [Microsoft Graph<br/>tabulky v aplikaci Excel](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔<sup>1</sup>| |✔|✔|
| [Microsoft Graph<br/>soubory na OneDrive](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔<sup>1</sup>| |✔|✔|
| [Microsoft Graph<br/>e-mailu aplikace Outlook](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔<sup>1</sup>| | |✔|
| [Microsoft Graph<br/>události](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔<sup>1</sup>|✔|✔|✔|
| [Microsoft Graph<br/>ověřování tokenů](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔<sup>1</sup>| |✔| |
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔|✔<sup>1</sup>| |✔|✔|  
| [Notification Hubs](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|  
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔<sup>1</sup>| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔<sup>1</sup>|✔| |✔|  
| [Table Storage](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|  
| [Časovač](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔<sup>1</sup>| | |✔|
| [Webhooky](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔||✔| |✔|
  
<sup>1</sup> musí být registrován jako rozšíření vazby v 2.x. V tématu [známé problémy v 2.x](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

<sup>2</sup> experimentální &mdash; není podporované a může být v budoucnu opuštění.

