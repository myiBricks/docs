
### zu beachten
- Im topic wird zwischen Gross und Kleinschreibung unterschieden (Bla/Test  !=  bla/test)
- Die $SYS ist für die Broker-Nutzung/Statistik gedacht. https://github.com/mqtt/mqtt.org/wiki/SYS-Topics
- Version Header 3 = 3.1 // 4 = 3.1.1  // 5 = 5
- Filter -> topic / Inhalt / Typ
- Sender / Empfänger sind völlig entkoppelt
- MQTT ist asynchron
- Der Broker kann Nachrichten speichern wenn:
    - Der Client hatte sich mit einer dauerhaften Sitzung verbunden (CleanSession = false)
    - QOS > 0
- Es ist keine Nachrichtenwarteschlange
- wildcard ->
    - Single Level: +
    - Multi Level: #
- Das MQTT-Topic sind UTF-8-codierte Zeichenfolgen und sollten nicht mehr als 65535 Bytes umfassen. Die Verwendung kürzerer MQTT-Topic Namen führt zu einem geringeren Ressourcenverbrauch.


### Das sollte man nicht machen

- Keinen ersten vorangestellten Schrägstrich (z. B. /mytopics ) verwenden.
- Nicht verwenden $SYS als Startthema, dies ist dem Broker vorbehalten
- Keine Leerzeichen im topic verwenden
- Topic ist zwar UTF8 aber man sollte nur ASCI verwenden.
- ein MQTT-Topic für Nachrichten mit unterschiedlicher Bedeutung verwenden
    - verschiedene Topics für sie erstellen
- Abonnieren Sie nicht alle Nachrichten auf einem Broker, indem Sie einen MQTT-Client verwenden und einen mehrstufigen Platzhalter abonnieren. (#)



### Vorschläge

- Client-ID vorne im topic einbetten
- spezifische topic für die individuelle Adressierung (z.B. von Sensoren) erstellen,
  anstatt alle Werte über ein topic zu senden - dadurch wird vermieden,
  dass Nachrichten an Empfänger gesendet werden, die sie nicht benötigen
- Versuchen die topic namen kurz zu halten (da sie bei jeder Veröffentlichung und Nachricht gesendet werden)
- überlegen, welche Daten wie oft gesendet werden müssen
  o z.B. erwägen, Metadaten in ein eigenes Thema auszugliedern und mit einem "Retain"-Flag
  zu veröffentlichen
- Befehls-/Cmd- und Status für ein Gerät trennen - auf diese Weise wird eine bidirektionale Kommunikation möglich, und das Gerät muss seine eigenen Nachrichten nicht filtern.
- eine testamentarische Nachricht verwenden, um auf eine unerwartete Trennung des gerätes hinzuweisen

