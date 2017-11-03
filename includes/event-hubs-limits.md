Následující tabulka uvádí kvóty a omezení specifické pro [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Informace o cenách služby Event Hubs naleznete v tématu [cenách služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Omezení | Rozsah | Typ | Chování při překročení | Hodnota |
| --- | --- | --- | --- | --- |
| Počet služby event hubs na obor názvů |obor názvů |Statická |Odeslání dalších žádostí o vytvoření nového centra událostí budou odmítnuty. |10 |
| Počet oddílů na Centrum událostí |Entita |Statická |- |32 |
| Počet skupin uživatelů na Centrum událostí |Entita |Statická |- |20 |
| Počet připojení protokolu AMQP na obor názvů |obor názvů |Statická |Odeslání dalších žádostí o další připojení se odmítne a volající kód přijme výjimku. |5,000 |
| Maximální velikost události Event Hubs|Systémové |Statická |- |256 kB |
| Maximální velikost název centra událostí |Entita |Statická |- |50 znaků |
| Počet příjemců bez epoch na skupiny uživatelů |Entita |Statická |- |5 |
| Maximální dobou uchování dat událostí |Entita |Statická |- |1-7 dní |
| Maximální počet jednotek propustnosti |obor názvů |Statická |Překročení omezení jednotek propustnosti způsobí, že data omezeny a generuje  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Můžete vyžádat větší počet jednotek propustnosti pro standardní vrstvy a podání [žádost o podporu](/azure/azure-supportability/how-to-create-azure-support-request). [Jednotky propustnosti Další](../articles/event-hubs/event-hubs-auto-inflate.md) jsou k dispozici v blocích po 20 na základě potvrdit nákupu. |20 |
| Počet pravidel autorizace na obor názvů |obor názvů|Statická |Odeslání dalších žádostí o vytvoření pravidla autorizace budou odmítnuty.|12 |
