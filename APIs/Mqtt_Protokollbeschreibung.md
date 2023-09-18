
## Connect Message

| Parameter     | Kommentar                        | Beispiel                     |
|---------------|----------------------------------|------------------------------|
| ClientId      | muss einmalig sein               | IBC5418ABDBC                 |
| Clean Session | false = dauerhafte Sitzung       | false                        |
| Username      | authentication and authorization | ibricks_babylon_IBC5418ABDBC |
| Password      | authentication and authorization | MSIXQee5YBsAbzvwe4tfQrEr     |
| Will Message  | Wille und Testament              |                              |
| Keep Alive    | in s (ping/pong)                 | 60                           |


******
## Topic Allgemeiner Aufbau

    {Service}/{ClientId}/{asset}/{channel}/{command}


******
## Allgemein
| Topic                                        | Payload                                                                         | 
|----------------------------------------------|---------------------------------------------------------------------------------|
| iBBfly/C8F09E1266B0/info                     | {"desc": "My Cello V2", "type": "CELLO", "subType": "1R1S1H"}                   |
| iBBfly/C8F09E1266B0/asset/info               | {"relC":"1","metC":"10","shtC":"1","dimC":"2","dirC":"1","binC":"0","butC":"4"} |

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
> | Name   | Wert         | Beschreibung                                                       |
> |--------|--------------|--------------------------------------------------------------------|
> | desc   |              | Anzeige Name auf Gerät und Visualisierung                          |
> | active | true / false |                                                                    |
> | visu   | 0 / 1 / 2    | Automatisches Einfügen in Visu → 0 = Nie; 1 = Immer; 2 = Assistent |


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
> | value     | on / off     |                             |
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
> | value     | on / off     |                             |
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