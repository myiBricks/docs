Virtual Wire

### Generell

Bla Bla

### Protokollsequenz

| {   | Präambel | Mac | Device | Adresse | CMD | Index | Par | CRC16 | }   |
| --- | -------- | --- | ------ | ------- | --- | ----- | --- | ----- | --- |
|     |          |     |        |         |     |       |     |       |     |

| {   | P   | P | M | M | M | M | D | D | A | A | A | A | C | C | I | P | P | P | C | C | C | C | } |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| {   | V | W | *m* | *m* | *m* | *m* |  |  | x | *0* | *0* | *0* |  |  | *i* | *9* | *9* | *9* | *c* | *c* | *c* | *c* | } | Protokollrahmen |
|     |  |  |  |  |  |  | **R** | **E** | x | *0* | *0* | *0* | **O** | **N** | *i* | *9* | *9* | 9 |  |  |  |  |  | Relais Einschalten *x000* = Adresse des anzusprechenden Device <br>*i* = Auslösung durch (Tasten Nr) (optional) *999* = Haltezeit der Taste 052 = 5.2 Sekunde (optional) |
|  |  |  |  |  |  |  | **R** | **E** | x | 0 | 0 | 0 | **O** | **F** | *i* | 9 | 9 | 9 |  |  |  |  |  | Relais Ausschalten *x000* = Adresse des anzusprechenden Device *i* = Auslösung durch (Tasten Nr) (optional) *999* = Haltezeit der Taste 052 = 5.2 Sekunden (optional) |