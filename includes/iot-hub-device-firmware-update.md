## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části:

* Vytvoříte konzolovou aplikaci Node.js, která bude reagovat na přímou metodu volanou cloudem.
* Aktivujete simulovanou aktualizaci firmwaru.
* Pomocí ohlášených vlastností umožníte dotazům na dvojčata zařízení identifikovat zařízení a čas jejich poslední dokončené aktualizace firmwaru.

1. Vytvořte prázdnou složku s názvem **manageddevice**.  Ve složce **manageddevice** vytvořte soubor package.json pomocí následujícího příkazu na příkazovém řádku. Přijměte všechny výchozí hodnoty:
   
    ```
    npm init
    ```
2. Na příkazovém řádku ve složce **manageddevice** spusťte následující příkaz k instalaci balíčků sady SDK pro zařízení **azure-iot-device** a **azure-iot-device-mqtt**:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Pomocí textového editoru vytvořte ve složce **manageddevice** soubor **dmpatterns_fwupdate_device.js**.

4. Přidejte následující příkazy „require“ na začátek souboru **dmpatterns_fwupdate_device.js**:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Přidejte proměnnou **connectionString** a použijte ji k vytvoření instance **klienta**. Nahraďte zástupný text `{yourdeviceconnectionstring}` připojovacím řetězcem, který jste si zaznamenali dříve v části Vytvoření identity zařízení:
   
    ```
    var connectionString = '{yourdeviceconnectionstring}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Přidejte následující funkci, která slouží k aktualizaci ohlášených vlastností:
   
    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };
   
      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported: ' + firmwareUpdateValue.status);
      });
    };
    ```
7. Přidejte následující funkce, které simulují stahování a použití image firmwaru:
   
    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
   
      console.log("Downloading image from " + imageUrl);
   
      callback(error, image);
    }
   
    var simulateApplyImage = function(imageData, callback) {
      var error = null;
   
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
   
      callback(error);
    }
    ```
8. Přidejte následující funkci, která prostřednictvím ohlášených vlastností aktualizuje stav aktualizace firmwaru na **waiting** (Čekání). Zařízení jsou obvykle informována o dostupné aktualizaci a správcem definovaná zásada způsobí, že začnou stahovat a aplikovat aktualizaci firmwaru. V této funkci by měla běžet logika, která povoluje tuto zásadu. Tato ukázka pro zjednodušení před stažením image firmwaru počká čtyři sekundy:
   
    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
   
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
9. Přidejte následující funkci, která prostřednictvím ohlášených vlastností aktualizuje stav aktualizace firmwaru na **downloading** (Stahování). Funkce pak simuluje stahování firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **downloadFailed** (Stahování selhalo), nebo **downloadComplete** (Stahování dokončeno):
   
    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
   
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
   
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
   
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
   
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
   
      }, 4000);
    }
    ```
10. Přidejte následující funkci, která prostřednictvím ohlášených vlastností aktualizuje stav aktualizace firmwaru na **applying** (Aplikování). Funkce pak simuluje aplikování image firmwaru a nakonec aktualizuje stav aktualizace firmwaru buď na **applyFailed** (Aplikování selhalo), nebo **applyComplete** (Aplikování dokončeno):
    
    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
    
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
    
      setTimeout(function() {
    
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
    
          }
        });
    
        setTimeout(callback, 4000);
    
      }, 4000);
    }
    ```
11. Přidejte následující funkci, která zpracuje přímou metodu **firmwareUpdate** a zahájí vícefázový proces aktualizace firmwaru:
    
    ```
    var onFirmwareUpdate = function(request, response) {
    
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });
    
      // Get the parameter from the body of the method request
      var fwPackageUri = request.payload.fwPackageUri;
    
      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
    
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });
    
        }
      });
    }
    ```
12. Nakonec přidejte následující kód, který se připojí k vaší službě IoT Hub:
    
    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
    
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate);
    });
    ```

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.
> 
> 

<!--HONumber=Feb17_HO1-->


