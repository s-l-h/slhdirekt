# slhdirekt

Schnittstellenbeschreibung & Beispiele


### Wer oder was ist swagger?

https://www.ionos.de/digitalguide/websites/web-entwicklung/was-ist-swagger/

https://swagger.io/specification/v2/


### Aktuelle Version
Die aktuellste Version finden sie immer unter
https://slhdirekt.de/assets/swagger.json


### Rest API Auth

Initiale Erzeugung des Zugriffstokens:

Benutzername: user@domain.com

Passwort: geheim

![grafik](https://user-images.githubusercontent.com/64684760/138544882-d7ac31f2-9308-41de-8cb1-60eb4ac07ae8.png)

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

```
{
 "status": "ok",
 "timestamp": "1982-11-19T22:56:56.456Z",
 "auth_token": "b6a91ab579a944853917d4fca63bc8de" # <--- Access Token
}
```
