[//]: # (https://stackoverflow.com/questions/11948245/markdown-to-create-pages-and-table-of-contents#)
# Inhaltsverzeichnis
1. [Connect Message](#Connect-Message)
2. [Topic Allgemeiner Aufbau](#Topic-Allgemeiner-Aufbau)
3. [Allgemein](#Allgemein)
4. [Relais](#Relais)
5. [Meteo](#Meteo)
6. [Store / Rolläden](#Store)
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
# Topic Allgemeiner Aufbau

    {Service}/{ClientId}/{asset}/{channel}/{command}


******
# Allgemein

> ### Info Gerät
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


> ### Kommando an das Gerät
> #### Topic
>   `iBBfly/C8F09E1266B0/command`
>
>
> #### Payload
> `{"value": "reboot","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | enum         | Kommando siehe CommandsEnum |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |
>
> #### CommandsEnum
>        Reboot,
>        Update,
>        Identifier,
>        JumpOut,
>        MqttReconnect,

> ### Info Hardware Funktionen
> #### Topic
>   `iBBfly/C8F09E1266B0/asset/info`
>
>
> #### Payload
> `{"relC":"1","metC":"10","shtC":"1","dimC":"2","dirC":"1","binC":"0","butC":"4"}`
>
> | Name  | Wert | Beschreibung            |
> |-------|------|-------------------------|
> | relC  | int  | Anzahl Relais           |
> | metC  | int  | Anzahl Messwerte        |
> | shtC  | int  | Anzahl Store / Rolläden |
> | dimC  | int  | Anzahl Dimmer           |
> | dirC  | int  | Anzahl Regler           |
> | binC  | int  | Anzahl Binär Eingänge   |
> | butC  | int  | Anzahl Taster           |


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
> `{"value":"off","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | bool         | on / off                    |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/relais_0/command`
>
> #### Payload
> `{"value":"off","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | bool         | on / off                    |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |
>
> Wird nach dem Ausführen vom Gerät wieder gelöscht


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
> `{"value":"21.5","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | float        |                             |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |


******
# Store

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
> `{"value":"stop","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                                           |
> |-----------|--------------|--------------------------------------------------------|
> | value     | enum         | stop / up / down → Wenn nicht am Fahren dann ist Stopp |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss                            |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/command`
>
>
> #### Payload
> `{"position":"50","slat":"50","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                                                                  |
> |-----------|--------------|-------------------------------------------------------------------------------|
> | value     | enum / float | stop / up / down / open / close oder prozent 0.0-100.0 mit einer Komma Stelle |
> | slat      | enum / float | open / close oder prozent 0.0-100 mit einer Komma Stelle                      |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss                                                   |


> ### Position
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/position`
>
>
> #### Payload
> `{"value":"50"}`
>
> | Name    | Wert  | Beschreibung                           |
> |---------|-------|----------------------------------------|
> | value   | float | Prozent 0.0-100 mit einer Komma Stelle |


> ### Slat
> 
> Winkel der Lamelle
> 
> #### Topic
> `iBBfly/C8F09E1266B0/asset/shutter_0/slat`
>
>
> #### Payload
> `{"value":"50"}`
>
> | Name  | Wert  | Beschreibung                           |
> |-------|-------|----------------------------------------|
> | value | float | Prozent 0.0-100 mit einer Komma Stelle |


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
> | mincoltemp | int       | Optional bei FarbTemperatur Tunablewhite                           |
> | maxcoltemp | int       | Optional bei FarbTemperatur Tunablewhite                           |
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
> #### Payload
> `{"value":"22.3","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                           |
> |-----------|--------------|----------------------------------------|
> | value     | float        | Prozent 0.0-100 mit einer Komma Stelle |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss            |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/dimmer_0/command`
>
>> #### Payload white
>> `{"value":"54.2","timeStamp":"01.01.1970 22:24:21"}`
>>
>> | Name      | Wert         | Beschreibung                           |
>> |-----------|--------------|----------------------------------------|
>> | value     | float        | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss            |
>
>> #### Payload color
>> `{"hue":"54.2","saturation":"54.2","value":"54.2","timeStamp":"01.01.1970 22:24:21"}`
>>
>> | Name       | Wert         | Beschreibung                           |
>> |------------|--------------|----------------------------------------|
>> | hue        | float        | 0.0-360 mit einer Komma Stelle         |
>> | saturation | float        | Prozent 0.0-100 mit einer Komma Stelle |
>> | value      | float        | Prozent 0.0-100 mit einer Komma Stelle |
>> | timeStamp  | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss            |
>>
>
>> #### Payload color temperature
>> `{"value":"54.2","temperature":"54.2","timeStamp":"01.01.1970 22:24:21"}`
>>
>> | Name        | Wert         | Beschreibung                           |
>> |-------------|--------------|----------------------------------------|
>> | value       | float        | Prozent 0.0-100 mit einer Komma Stelle |
>> | temperature | int          | 2700 bis 6500 Kelvin                   |
>> | timeStamp   | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss            |
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
> `{"value":"20.1","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                                  |
> |-----------|--------------|-----------------------------------------------|
> | value     | float        | °C Sollwert Temperatur mit einer Komma Stelle |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss                   |


> ### Command
> #### Topic
> `iBBfly/C8F09E1266B0/asset/director_0/command`
>
> #### Payload
> `{"value":"off","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                                  |
> |-----------|--------------|-----------------------------------------------|
> | value     | float        | °C Sollwert Temperatur mit einer Komma Stelle |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss                   |
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
> `{"value":"off","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | bool         | on / off                    |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |


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


> ### State
> #### Topic
> `iBBfly/C8F09E1266B0/asset/button_0/state`
>
>
> #### Payload
> `{"value":"click","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | enum         | press / release             |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |

> ### Event
> #### Topic
> `iBBfly/C8F09E1266B0/asset/button_0/event`
>
>
> #### Payload
> `{"value":"click","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | value     | enum         | click / longClick           |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |


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
>>
>> | Name | Wert   | Beschreibung  |
>> |------|--------|---------------|
>> | id   | string | Eindeutige ID |
>> | desc | string | Anzeige Text  |
>>

> ### Event
> #### Topic
> `iBBfly/C8F09E1266B0/asset/scene/event`
>
>
> #### Payload
> `{"value":"click","timeStamp":"01.01.1970 22:24:21"}`
>
> | Name      | Wert         | Beschreibung                |
> |-----------|--------------|-----------------------------|
> | id        | string       | Eindeutige ID               |
> | value     | bool         | on / off                    |
> | timeStamp | Datum / Zeit | Format: dd.MM.yyyy HH:mm:ss |


******