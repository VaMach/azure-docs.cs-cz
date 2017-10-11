## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Přidružit účet úložiště Azure do služby IoT Hub

Protože aplikaci simulovaného zařízení odešle soubor do objektu blob, musíte mít [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) účet přidružený ke službě IoT Hub. Když přidružíte účet úložiště Azure IoT hub, generuje službu IoT hub identifikátor URI SAS. Zařízení můžete použít tento identifikátor URI pro SAS se bezpečně nahrát soubor do kontejneru objektů blob. Služba IoT Hub a sady SDK zařízení koordinovat proces, který generuje identifikátor URI SAS a zpřístupní zařízení použít k nahrání souboru.

Postupujte podle pokynů v [nahrávání souborů konfigurovat pomocí portálu Azure](../articles/iot-hub/iot-hub-configure-file-upload.md) k přidružení účtu Azure Storage do služby IoT hub. Ujistěte se, že kontejner objektů blob je spojen s služby IoT hub a že jsou povolené soubor oznámení.

![Povolit oznámení soubor portálu](media/iot-hub-associate-storage/enable-file-notifications.png)