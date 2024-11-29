[//]: # (https://stackoverflow.com/questions/11948245/markdown-to-create-pages-and-table-of-contents#)
# Inhaltsverzeichnis
1. [Connect Message](#Connect-Message)
2. [Topic Allgemeiner Aufbau](#Topic-Allgemeiner-Aufbau)
3. [Allgemein](#Allgemein)
4. [Relais](#Relais)
5. [Meteo](#Meteo)
6. [Jalousie / Rollläden](#Jalousie)
7. [Dimmer](#Dimmer)
8. [Director](#Director)
9. [Detector](#Detector)
10. [Button](#Button)
11. [Scene](#Scene)




# Connect Message

| Parameter     | Kommentar                        | Beispiel                |
|---------------|----------------------------------|-------------------------|
| ClientId      | muss einmalig sein               | IBC5418ABDBC            |
| Clean Session | false = dauerhafte Sitzung       | false                   |
| Username      | authentication and authorization | IBC5418ABDBC            |
| Password      | authentication and authorization | xxxxxxxxxxxxxxxxxxxxxxx |
| Will Message  | Wille und Testament              |                         |
| Keep Alive    | in s (ping/pong)                 | 60                      |


******
# Topic allgemeiner Aufbau

    {Service}/{ClientId}/{asset}/{channel}/{command}


******
# Allgemein

> ### Information Gerät
> #### Topic
>   `iBBfly/C8F09E1266B0/info`
>
>
> #### Payload
> `{"desc": "My Cello V2", "type": "CELLO", "subType": "1R1S1H"}`
>
> | Name    | Wert     | Beschreibung                              |
> |---------|----------|-------------------------------------------|
> | desc    | string   | Anzeige Name auf Gerät und Visualisierung |
> | type    | string   | Geräte Type                               |
> | subType | string   | Welche Hardware Funktion ist vorhanden    |
> | sw      | string   | Software Version                          |
>
>
> ### Heartbeat Gerät
> #### Topic
>   `iBBfly/C8F09E1266B0/heartbeat`
>
>
> #### Payload
> `{"timeStamp":"772010416","uptime":"83963020"}`
>
> | Name        | Wert   | Beschreibung                 |
> |-------------|--------|------------------------------|
> | timeStamp   | uint   | Ticks in sek seit 01.01.1970 |
> | uptime      | uint   | Ticks in sek seit start      |


> ### Cloud Infos
> #### Topic
>   `iBBfly/C8F09E1266B0/cloudinfo`
>
> Wird gesendet, wenn Topic info vom Gerät kommt
>
> #### Payload
> `{"hbGuid": "7bb1cfe6-b00e-4a1b-81b9-e760a561b301", "hbName": "Mein Zuhause", "mapName": "Koffer", "timeStamp":"772010416"}`
>
> | Name      | Wert   | Beschreibung                                    |
> |-----------|--------|-------------------------------------------------|
> | hbGuid    | string | HouseBase Guid (ist leer wenn nicht zugewiesen) |
> | hbName    | string | System Beschreibung (max 10 zeichen)            |
> | mapName   | string | Map Name (max 10 zeichen)                       |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970                    |


> ### Gerät Kommando
> #### Topic
>   `iBBfly/C8F09E1266B0/cloudcommand`
>
>
> #### Payload
> `{"command": "update","value": "1.25","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                          |
> |-----------|--------|---------------------------------------|
> | command   | enum   | Kommando siehe CommandsEnum           |
> | value     | string | Zusätzliche info, z.B. update version |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970          |
>
> #### CommandsEnum
>        Reboot,
>        Update,
>        Identify,
>        JumpOut,
>        MqttReconnect,
>        ChangeDescription,
>        GetAllStates,

> ### Information Hardware Funktionen
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/info`
>
>
> #### Payload
> `{"relC":"1","metC":"10","shtC":"1","dimC":"2","dirC":"1","binC":"0","butC":"4"}`
>
> | Name  | Wert | Beschreibung                |
> |-------|------|-----------------------------|
> | relC  | int  | Anzahl Relais               |
> | metC  | int  | Anzahl Messwerte            |
> | shtC  | int  | Anzahl Jalousie / Rollläden |
> | dimC  | int  | Anzahl Dimmer               |
> | dirC  | int  | Anzahl Regler               |
> | binC  | int  | Anzahl Binär Eingänge       |
> | butC  | int  | Anzahl Taster               |
>

>## Geräte Konfiguration
>
> - Konfigurationen werden erst angelegt (visu sichtbar) wenn einmalig vom Gerät geschickt
> - Pro Konfiguration Parameter gibt es eigene topics. state / command
> - Wenn im Payload "get" steht, sollte das Gerät im topic "state"
    > mit dem gleichen "ConfigSystemEnum" topic antworten.
    > Wenn die Konfiguration auf dem Gerät nicht vorhanden ist dann als "error" → "notFound"
> - Wenn die Konfiguration ausserhalb des Wertebereiches ist, dann den korrigierten wert im topic "state"
    > zurückschicken oder wenn nicht korrigiert werden kann dann als "error" → "wrongValue"
> - Konfigurationen, die nicht als Enum definiert sind, werden Server seitig verworfen
    > und als antwort kommt im topic "command" im payload "error" → "notFound"
>
>
>### vom Gerät
> #### Topic
>   `iBBfly/C8F09E1266B0/config/{ConfigSystemEnum}/state`
>
>### an das Gerät
> #### Topic
>   `iBBfly/C8F09E1266B0/config/{ConfigSystemEnum}/command`
>
>
> #### Payload
> ###### Wert "command"
> `{"get": "","timeStamp":"748695077"}` \
> `{"put": "Wert","timeStamp":"748695077"}`
>
> ###### Wert "state"
> `{"value": "","timeStamp":"748695077"}`
>
> ###### Fehler
> `{"error": "ConfigErrorEnum","timeStamp":"748695077"}`
>
>
>
>#### ConfigErrorEnum
> | Name       | Beschreibung                                                |
> |------------|-------------------------------------------------------------|
> | notFound   | Wenn Parameter nicht vorhanden                              |
> | wrongValue | Wen wert ausserhalb gültigen bereich oder wertetyp ungültig |

>
> #### ConfigSystemEnum
> | Name            | Kurz Name | Wert | Wert Bereich | Beschreibung                                            |
> |-----------------|-----------|------|--------------|---------------------------------------------------------|
> | centralFunction | CF        | bool | true / false | Sendet dieses Gerät Zentral Funktionen (z.B. Alles Aus) |
> |                 |           |      |              |                                                         |


******

# Relais

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/relais_0/info`
>
>
> #### Payload
> `{"desc":"Licht Links","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/relais_0/state`
>
>
> #### Payload
> `{"value":"off","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | value     | bool   | on / off                     |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/relais_0/command`
>
> #### Payload
> `{"value":"off","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | value     | bool   | on / off                     |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |
>
> Wird nach dem Ausführen vom Gerät wieder gelöscht

>
>### Relais Konfiguration
>
>
>### vom Relais
> #### Topic
>   `iBBfly/C8F09E1266B0/relais_0/config/{ConfigRelaisEnum}/state`
>
>### an das Relais
> #### Topic
>   `iBBfly/C8F09E1266B0/relais_0/config/{ConfigRelaisEnum}/command`
>
>
> #### Payload
> ###### Wert "command"
> `{"get": "","timeStamp":"748695077"}` \
> `{"put": "Wert","timeStamp":"748695077"}`
>
> ###### Wert "state"
> `{"value": "","timeStamp":"748695077"}`
>
> ###### Fehler
> `{"error": "ConfigErrorEnum","timeStamp":"748695077"}`
>
>
>
> #### ConfigRelaisEnum
> | Name            | Kurz Name | Wert | Wert Bereich   | Beschreibung                                                  |
> |-----------------|-----------|------|----------------|---------------------------------------------------------------|
> | centralFunction | CF        | bool | true / false   | Reagiert dieses Gerät auf Zentral Funktionen (z.B. Alles Aus) |
> | GreenSwitch     | GS        | uint | 0-86400 in Sek | Automatisch ausschalten nach eingestellte Zeit                |
>


******

# Meteo

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/meteo_0/info`
>
>
> #### Payload
> `{"desc":"","type":"temp","name":"temperature1","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | type   | enum      | Siehe MeteoType                                                    |
> | name   | string    | Technische Name                                                    |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |
>
>
> #### MeteoType
>        Temperature, // Temp
>        Humidity, // Hum
>        Luminosity, // Lum
>        Speed,
>        Wind,
>        Direction, // Dir
>        Pressure, // Bar
>        Uv,
>        Volt,
>        Setpoint,
>        Rain,
>        Power,
>        Voc,
>        Current,
>        Energy,
>        Volume,
>        Co2,

> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/meteo_0/state`
>
>
> #### Payload
> `{"value":"21.5","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | value     | float  |                              |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |


******
# Jalousie

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/shutter_0/info`
>
>
> #### Payload
> `{"desc":"","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/state`
>
>
> #### Payload
> `{"value":"stop","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                                           |
> |-----------|--------|--------------------------------------------------------|
> | value     | enum   | stop / up / down → Wenn nicht am Fahren dann ist Stopp |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970                           |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/command`
>
>
> #### Payload
> `{"position":"50","slat":"50","timeStamp":"748695077"}`
>
> | Name      | Wert         | Beschreibung                                                                  |
> |-----------|--------------|-------------------------------------------------------------------------------|
> | value     | enum / float | stop / up / down / open / close oder prozent 0.0-100.0 mit einer Komma Stelle |
> | slat      | enum / float | open / close oder prozent 0.0-100 mit einer Komma Stelle                      |
> | timeStamp | uint         | Ticks in sek seit 01.01.1970                                                  |


> ### Position
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/position`
>
>
> #### Payload
> `{"value":"50","timeStamp":"748695077"}`
>
> | Name      | Wert         | Beschreibung                                                                  |
> |-----------|--------------|-------------------------------------------------------------------------------|
> | value     | enum / float | stop / up / down / open / close oder prozent 0.0-100.0 mit einer Komma Stelle |
> | timeStamp | uint         | Ticks in sek seit 01.01.1970                                                  |


> ### Slat
>
> Winkel der Lamelle
>
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/slat`
>
>
> #### Payload
> `{"value":"50","timeStamp":"748695077"}`
>
> | Name      | Wert         | Beschreibung                                                 |
> |-----------|--------------|--------------------------------------------------------------|
> | value     | enum / float | open / close oder prozent 0.0-100.0 mit einer Komma Stelle   |
> | timeStamp | uint         | Ticks in sek seit 01.01.1970                                 |


******

# Dimmer

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/dimmer_0/info`
>
>
> #### Payload
> `{"desc":"Licht Links","type":"white","active":"true","visu":"1","mincoltemp":"2700","maxcoltemp":"6500"}`
>
> | Name       | Wert      | Beschreibung                                                       |
> |------------|-----------|--------------------------------------------------------------------|
> | desc       | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | type       | enum      | Siehe DimmerType                                                   |
> | active     | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu       | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |
> | mincoltemp | int       | Optional bei Farbtemperatur Tunable white                          |
> | maxcoltemp | int       | Optional bei Farbtemperatur Tunable white                          |
>
> #### DimmerType
>        white, // Standart Dimmer
>        c,     // Farbe h/s/v 
>        ct,    // FarbTemperatur Tunablewhite
>        cct,   // Farbe und FarbTemperatur

> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/dimmer_0/state`
>
>
>> #### Payload white
>> `{"value":"54.2","timeStamp":"748695077"}`
>>
>> | Name      | Wert   | Beschreibung                           |
>> |-----------|--------|----------------------------------------|
>> | value     | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp | uint   | Ticks in sek seit 01.01.1970           |
>
>> #### Payload color
>> `{"hue":"54.2","saturation":"54.2","value":"54.2","timeStamp":"748695077"}`
>>
>> | Name       | Wert   | Beschreibung                           |
>> |------------|--------|----------------------------------------|
>> | hue        | float  | 0.0-360 mit einer Komma Stelle         |
>> | saturation | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | value      | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp  | uint   | Ticks in sek seit 01.01.1970           |
>>
>
>> #### Payload color temperature
>> `{"value":"54.2","temperature":"54.2","timeStamp":"748695077"}`
>>
>> | Name        | Wert   | Beschreibung                           |
>> |-------------|--------|----------------------------------------|
>> | value       | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | temperature | int    | 2700 bis 6500 Kelvin                   |
>> | timeStamp   | uint   | Ticks in sek seit 01.01.1970           |
>>


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/dimmer_0/command`
>
>> #### Payload white
>> `{"value":"54.2","timeStamp":"748695077"}`
>>
>> | Name      | Wert   | Beschreibung                           |
>> |-----------|--------|----------------------------------------|
>> | value     | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp | uint   | Ticks in sek seit 01.01.1970           |
>
>> #### Payload color
>> `{"hue":"54.2","saturation":"54.2","value":"54.2","timeStamp":"748695077"}`
>>
>> | Name       | Wert   | Beschreibung                           |
>> |------------|--------|----------------------------------------|
>> | hue        | float  | 0.0-360 mit einer Komma Stelle         |
>> | saturation | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | value      | float  | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp  | uint   | Ticks in sek seit 01.01.1970           |
>>
>
>> #### Payload color temperature
>> `{"value":"54.2","temperature":"54.2","timeStamp":"748695077"}`
>>
>> | Name        | Wert   | Beschreibung                            |
>> |-------------|--------|-----------------------------------------|
>> | value       | float  | Prozent 0.0-100 mit einer Komma Stelle  |
>> | temperature | int    | 2700 bis 6500 Kelvin                    |
>> | timeStamp   | uint   | Ticks in sek seit 01.01.1970            |
>>
> Wird nach dem Ausführen vom Gerät wieder gelöscht

******

# Director

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/director_0/info`
>
>
> #### Payload
> `{"desc":"","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/director_0/state`
>
>
> #### Payload
> `{"value":"20.1","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                                   |
> |-----------|--------|------------------------------------------------|
> | value     | float  | °C Sollwert Temperatur mit einer Komma Stelle  |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970                   |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/director_0/command`
>
> #### Payload
> `{"value":"off","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                                    |
> |-----------|--------|-------------------------------------------------|
> | value     | float  | °C Sollwert Temperatur mit einer Komma Stelle   |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970                    |
>
> Wird nach dem Ausführen vom Gerät wieder gelöscht


******

# Detector

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/detector_0/info`
>
>
> #### Payload
> `{"desc":"","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/detector_0/state`
>
>
> #### Payload
> `{"value":"off","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | value     | bool   | on / off                     |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |


******

# Button

> ### Info
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/button_0/info`
>
>
> #### Payload
> `{"desc":"","active":"true","visu":"1"}`
>
> | Name   | Wert      | Beschreibung                                                       |
> |--------|-----------|--------------------------------------------------------------------|
> | desc   | string    | Anzeige Name auf Gerät und Visualisierung                          |
> | active | bool      | Ist lokale Funktion aktiv → true / false                           |
> | visu   | 0 / 1 / 2 | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


> ### Event
> #### Topic
> `iBBfly/C8F09E1266B0/asset/button_0/event`
>
>
> #### Payload
> `{"value":"click","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | value     | enum   | click / longClick            |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |



> ### Cello2
>
> #### Taster anordnung
>
> ` button_0 ¦ button_1 `\
> ` ---------¦--------- `\
> ` button_2 ¦ button_3 `
>
>


******

# Scene

> ### Available
> #### Topic
> `iBBfly/C8F09E1266B0/asset/scene/available`
>
> #### Payload
> `{"scenes":[{"id":"Aufraeumen","desc":"Zimmer an"},{"id":"Abend","desc":"Nacht"}]}`
>
> | Name   | Wert  | Beschreibung         |
> |--------|-------|----------------------|
> | scenes | array | {"id":"","desc":""}  |
>
> | Name | Wert   | Beschreibung  |
> |------|--------|---------------|
> | id   | string | Eindeutige ID |
> | desc | string | Anzeige Text  |
>

> ### Event
> #### Topic
> `iBBfly/C8F09E1266B0/asset/scene/event`
>
>
> #### Payload
> `{"id":"Aufraeumen","value":"on","timeStamp":"748695077"}`
>
> | Name      | Wert   | Beschreibung                 |
> |-----------|--------|------------------------------|
> | id        | string | Eindeutige ID                |
> | value     | bool   | on / off                     |
> | timeStamp | uint   | Ticks in sek seit 01.01.1970 |


******