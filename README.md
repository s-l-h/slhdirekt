slhdirekt Schnittstellenbeschreibung & Beispiele
=============================


## REST API

#### Die aktuellste Version finden sie hier:

https://slhdirekt.de/assets/swagger.json


#### Wer oder was ist swagger?

https://www.ionos.de/digitalguide/websites/web-entwicklung/was-ist-swagger/

https://swagger.io/specification/v2/




### Authentifizierung

Initiale Erzeugung des Zugriffstokens:

Benutzername: user@domain.com

Passwort: geheim

![grafik](https://user-images.githubusercontent.com/64684760/138544882-d7ac31f2-9308-41de-8cb1-60eb4ac07ae8.png)

Auth Request:
```bash 
curl -X 'PUT' \
  'https://api.slhdirekt.de/auth' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "data": {
    "credentials": "6c1b3584d6........"
  }
}'
```
Auth Response:
``` 
{
 "status": "ok",
 "timestamp": "1982-11-19T22:56:56.456Z",
 "auth_token": "b6a91ab579a944853917d4fca63bc8de" # <--- Access Token
}
```


## Datei Transfer

- halb-/vollautomatischer Import durch Anbindung von bestehenden (S)FTP(S) Strukturen, sowie Bereitstellung derjenigen bei Bedarf
- manueller Upload der Datei im eingestellten Format via Portal

### XML Format

- [SLH Schema](Transportauftrag_slhdirekt.xsd):
   - [Beispiel](example.xml)


- Alternative Schemata:
  - auf Anfrage

...

### EDI

#### EDIFACT/IFTMIN
 - siehe [GS1 Germany Anwendungsempfehlung zu EANCOM® 2002](https://www.publikationen.gs1-germany.de/Complete/eancom_v9.2/index.html)
...

#### Fortras 128

...

#### Fortras 512
...

### CSV
- auf Anfrage
