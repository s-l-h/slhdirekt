slhdirekt Schnittstellenbeschreibung & Beispiele
=============================


## REST API

#### Die aktuellste Version finden Sie hier:

https://slhdirekt.de/docs

( https://slhdirekt.de/assets/swagger.json) 


#### Wer oder was ist swagger?

https://www.ionos.de/digitalguide/websites/web-entwicklung/was-ist-swagger/

https://swagger.io/specification/v2/

---

### API Endpunkte

All URIs are relative to *https://api.slhdirekt.de*

Class | Method | HTTP request | Description
------------ | ------------- | ------------- | -------------
*AuthApi* | [**auth**](docs/AuthApi.md#auth) | **PUT** /auth | Authenticates the user.
*OrderApi* | [**callList**](docs/OrderApi.md#callList) | **GET** /orders/ | Retrieves a list of orders.
*OrderApi* | [**create**](docs/OrderApi.md#create) | **POST** /orders/create | Creates a new order.
*OrderApi* | [**delete**](docs/OrderApi.md#delete) | **DELETE** /orders/{uuid} | Deletes an unpublished order.
*OrderApi* | [**get**](docs/OrderApi.md#get) | **GET** /orders/{uuid} | Retrieves details of a specific order.
*OrderApi* | [**label**](docs/OrderApi.md#label) | **GET** /orders/{uuid}/labels/{nve} | Retrieves a label for a specific order and NVE number as PDF file.
*OrderApi* | [**labels**](docs/OrderApi.md#labels) | **GET** /orders/{uuid}/labels | Retrieves all labels for a specific order in a single PDF file.
*OrderApi* | [**publish**](docs/OrderApi.md#publish) | **POST** /orders/{uuid}/publish | Publishes a specific order.
*OrderApi* | [**publishOrders**](docs/OrderApi.md#publishOrders) | **POST** /orders/publish | Publishes multiple orders.
*OrderApi* | [**put**](docs/OrderApi.md#put) | **PUT** /orders/{uuid} | Updates an existing order.
*OrderApi* | [**status**](docs/OrderApi.md#status) | **GET** /orders/{uuid}/status | Retrieves the status of a specific order.
*OrderApi* | [**upload**](docs/OrderApi.md#upload) | **POST** /orders/upload | Uploads an order file in a custom import format.

---

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

---

## Datei Transfer

- halb-/vollautomatischer Import durch Anbindung von bestehenden (S)FTP(S) Strukturen, sowie Bereitstellung derjenigen bei Bedarf
- manueller Upload der Datei im eingestellten Format via Portal

## Formate

### JSON

- siehe https://slhdirekt.de/assets/swagger.json

- Alternative Schemata: auf Anfrage

### XML

- [SLH Schema](Transportauftrag_slhdirekt.xsd):
   - [Beispiel](example.xml)

- Alternative Schemata: auf Anfrage

### EDI

#### EDIFACT/IFTMIN

- siehe [GS1 Germany Anwendungsempfehlung zu EANCOM® 2002](https://www.publikationen.gs1-germany.de/Complete/eancom_v9.2/index.html)

#### Fortras 128
Beschreibung folgt
...

#### Fortras 512
Beschreibung folgt
...

### CSV
- auf Anfrage
