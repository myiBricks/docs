# Virtual Wire

## Generell

Bla Bla

## Protokollsequenz

| {      | Präambel  | Mac      | Device | Adresse  | CMD    | Index  | Par     | CRC16    |     } |
| :----- | --------- | -------- | ------ | -------- | ------ | ------ | ------- | -------- | ----: |
| {      | VW        | _mmmm_   | _dd_   | _xnnn_   | _cc_   | _i_    | _ppp_   | _cccc_   |     } |
| {      | VW        | 3F2C     | RE     | A001     | ON     | 0      | 000     | FFFF     |     } |



| Bereich   |Grösse|Syntax| Beschrieb | Beispiel |
| --------- | ---- | ---- | --------- | -------- |
| {         |1 Byte|| Start der Sequenz| '{' |
| Präambel  |2 Byte|| Ist Immer 'VW'  | 'VW' |
| Mac       |4 Byte|| Hinterste 4 Zeichen der MAC-Adresse des sendenden Device | '3F2C' |
| Device    |2 Byte|| Typ des anzusteuernden Devices | 'RE' = Relais|
| Adresse   |4 Byte|| Adresse des anzusprechenden Devices. Immer ein Buchstabe und 3 Zahlen | 'A001' |
| CMD       |2 Byte|| Befehl  | 'ON' = Einschelten|
| Index     |1 Byte|| Zahl oder Buchstabe zur Identifikation von Unterelementen (siehe Beschreibng Befehle) | '0' |
| Par       |3 Byte|| Wird zur übertragung von Werten oder Befehlsparameter verwendet   |'000'|
| CRC16     |4 Byte|| CRC16 des der Meldung von der Präambel bit zum Parameter nach Mod-Bus Standard |'FFFF'|
| }         |1 Byte|| Ende der Sequenz| '}'|

## Befehle

### Relais Einschalten
Schaltet das Relais mit einer bestimmten Adresse ein  

| Bereich   | Wert   | Beschreibung |
| --------- | -----  | -- |
| Device    | RE     | Device Relais |
| Adresse   | _xnnn_ | Adresse des Relais|
| CMD       | ON     | Befehl Ein |
| Index     | _i_    | (optional) Auslösung durch (Taste) |
| Par       | _000_  | (optional) Haltezeit der Taste 052 = 5.2 Sekunden |

Beispiel<br>
`{VW3FC5REA001ON0000FFFF}`

### Relais Ausschalten
Schaltet das Relais mit einer bestimmten Adresse aus  

| Bereich   | Wert   | Beschreibung |
| --------- | -----  | -- |
| Device    | RE     | Device Relais |
| Adresse   | _xnnn_ | Adresse des Relais|
| CMD       | OF     | Befehl Aus |
| Index     | _i_    | (optional) Auslösung durch (Taste) |
| Par       | _000_  | (optional) Haltezeit der Taste 052 = 5.2 Sekunden |

Beispiel<br>
`{VW3FC5REA001OF0000FFFF}`
