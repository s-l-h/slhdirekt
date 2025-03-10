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

> Benutzername: `user@domain.com`
> 
> Passwort: `geheim`



<details>
<summary>
<h3>Bash/cURL</h3>
</summary>

```sh
#!/usr/bin/env bash

# 1) Generate MD5 of the password
PASSWORD="geheim"
# If on Linux:
MD5_PASSWORD=$(echo -n "$PASSWORD" | md5sum | awk '{print $1}')
# If on macOS, you might do:
# MD5_PASSWORD=$(echo -n "$PASSWORD" | md5)

echo "MD5 of password: $MD5_PASSWORD"

# 2) Combine username + md5_password
USERNAME="user@domain.com"
COMBINED="${USERNAME}:${MD5_PASSWORD}"

# 3) Generate MD5 of the combined string
FINAL_CREDENTIAL=$(echo -n "$COMBINED" | md5sum | awk '{print $1}')
echo "Final credentials: $FINAL_CREDENTIAL"

# 4) Prepare JSON payload
JSON_PAYLOAD=$(cat <<EOF
{
  "data": {
    "credentials": "$FINAL_CREDENTIAL"
  }
}
EOF
)

# 5) Send the PUT request
RESPONSE=$(curl -s -w "\n%{http_code}" -X PUT \
  -H "Accept: application/json" \
  -H "Content-Type: application/json" \
  -d "$JSON_PAYLOAD" \
  "https://api.slhdirekt.de/auth")

# Separate the response body from the HTTP status code
BODY=$(echo "$RESPONSE" | sed '$d')
HTTP_CODE=$(echo "$RESPONSE" | tail -n1)

# 6) Parse the response
if [ "$HTTP_CODE" -eq 200 ]; then
  STATUS=$(echo "$BODY" | jq -r '.status')
  if [ "$STATUS" = "ok" ]; then
    AUTH_TOKEN=$(echo "$BODY" | jq -r '.auth_token')
    echo "Auth Token: $AUTH_TOKEN"
  else
    echo "Authentication failed: $BODY"
  fi
else
  echo "Error: $HTTP_CODE - $BODY"
fi

```

</details>



<details>
<summary><h3>Python</h3></summary>

```py
import hashlib
import requests
import json

# 1) Generate MD5 of the password
password = "geheim"
md5_password = hashlib.md5(password.encode()).hexdigest()
print("MD5 of password:", md5_password)

# 2) Combine username + md5_password
username = "user@domain.com"
combined = username + ":" + md5_password

# 3) Generate MD5 of the combined string
final_credential = hashlib.md5(combined.encode()).hexdigest()
print("Final credentials:", final_credential)

# 4) Send the request
url = "https://api.slhdirekt.de/auth"
headers = {
    "Accept": "application/json",
    "Content-Type": "application/json"
}
data = {
    "data": {
        "credentials": final_credential
    }
}

response = requests.put(url, headers=headers, data=json.dumps(data))

# 5) Parse the response
if response.status_code == 200:
    response_json = response.json()
    if response_json.get("status") == "ok":
        auth_token = response_json.get("auth_token")
        print("Auth Token:", auth_token)
    else:
        print("Authentication failed:", response_json)
else:
    print("Error:", response.status_code, response.text)

```
</details>



<details>
<summary><h3>Node.js</h3></summary>

```js
const crypto = require('crypto');
const fetch = require('node-fetch');

// 1) Generate MD5 of the password
const password = 'geheim';
const md5Password = crypto.createHash('md5').update(password).digest('hex');
console.log("MD5 of password:", md5Password);

// 2) Combine username + md5_password
const username = 'user@domain.com';
const combined = `${username}:${md5Password}`;

// 3) Generate MD5 of the combined string
const finalCredential = crypto.createHash('md5').update(combined).digest('hex');
console.log("Final credentials:", finalCredential);

// 4) Send the request
const url = 'https://api.slhdirekt.de/auth';
const headers = {
  'Accept': 'application/json',
  'Content-Type': 'application/json'
};

const bodyData = {
  data: {
    credentials: finalCredential
  }
};

fetch(url, {
  method: 'PUT',
  headers: headers,
  body: JSON.stringify(bodyData)
})
  .then(res => res.json())
  .then(json => {
    // 5) Parse the response
    if (json.status === 'ok') {
      console.log("Auth Token:", json.auth_token);
    } else {
      console.log("Authentication failed:", json);
    }
  })
  .catch(err => {
    console.error("Request error:", err);
  });

```
</details>



<details>
<summary><h3>Java</h3></summary>

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.security.MessageDigest;
import java.nio.charset.StandardCharsets;
import com.google.gson.Gson;  // For JSON serialization if you like
import java.util.HashMap;
import java.util.Map;

public class AuthExample {
    public static void main(String[] args) throws Exception {
        // 1) Generate MD5 of the password
        String password = "geheim";
        String md5Password = md5Hash(password);
        System.out.println("MD5 of password: " + md5Password);

        // 2) Combine username + md5_password
        String username = "user@domain.com";
        String combined = username + ":" + md5Password;

        // 3) Generate MD5 of the combined string
        String finalCredential = md5Hash(combined);
        System.out.println("Final credentials: " + finalCredential);

        // 4) Build request body JSON
        Map<String, Object> dataMap = new HashMap<>();
        Map<String, String> innerData = new HashMap<>();
        innerData.put("credentials", finalCredential);
        dataMap.put("data", innerData);

        Gson gson = new Gson();
        String requestBody = gson.toJson(dataMap);

        // 5) Send request
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://api.slhdirekt.de/auth"))
                .header("Accept", "application/json")
                .header("Content-Type", "application/json")
                .PUT(HttpRequest.BodyPublishers.ofString(requestBody))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        // 6) Parse response
        if (response.statusCode() == 200) {
            Map responseJson = gson.fromJson(response.body(), Map.class);
            if ("ok".equals(responseJson.get("status"))) {
                System.out.println("Auth Token: " + responseJson.get("auth_token"));
            } else {
                System.out.println("Authentication failed: " + response.body());
            }
        } else {
            System.out.println("Error: " + response.statusCode() + " - " + response.body());
        }
    }

    private static String md5Hash(String input) throws Exception {
        MessageDigest md = MessageDigest.getInstance("MD5");
        byte[] digest = md.digest(input.getBytes(StandardCharsets.UTF_8));
        StringBuilder sb = new StringBuilder();
        for (byte b : digest) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}
```
</details>


<details>
<summary><h3>C#</h3></summary>

```cs
using System;
using System.Net.Http;
using System.Text;
using System.Security.Cryptography;
using Newtonsoft.Json;
using System.Threading.Tasks;
using System.Collections.Generic;

class Program
{
    static async Task Main(string[] args)
    {
        // 1) Generate MD5 of the password
        string password = "geheim";
        string md5Password = Md5Hash(password);
        Console.WriteLine("MD5 of password: " + md5Password);

        // 2) Combine username + md5_password
        string username = "user@domain.com";
        string combined = username + ":" + md5Password;

        // 3) Generate MD5 of the combined string
        string finalCredential = Md5Hash(combined);
        Console.WriteLine("Final credentials: " + finalCredential);

        // 4) Prepare request
        var url = "https://api.slhdirekt.de/auth";
        var data = new
        {
            data = new {
                credentials = finalCredential
            }
        };
        string jsonData = JsonConvert.SerializeObject(data);

        var httpClient = new HttpClient();
        var requestContent = new StringContent(jsonData, Encoding.UTF8, "application/json");

        // 5) Send request
        var request = new HttpRequestMessage(HttpMethod.Put, url)
        {
            Content = requestContent
        };
        request.Headers.Add("Accept", "application/json");

        HttpResponseMessage response = await httpClient.SendAsync(request);

        // 6) Parse response
        string responseBody = await response.Content.ReadAsStringAsync();
        if (response.IsSuccessStatusCode)
        {
            var responseJson = JsonConvert.DeserializeObject<Dictionary<string, object>>(responseBody);
            if (responseJson != null && responseJson.ContainsKey("status") && (string)responseJson["status"] == "ok")
            {
                Console.WriteLine("Auth Token: " + responseJson["auth_token"]);
            }
            else
            {
                Console.WriteLine("Authentication failed: " + responseBody);
            }
        }
        else
        {
            Console.WriteLine($"Error: {(int)response.StatusCode} - {responseBody}");
        }
    }

    private static string Md5Hash(string input)
    {
        using (var md5 = MD5.Create())
        {
            byte[] hashBytes = md5.ComputeHash(Encoding.UTF8.GetBytes(input));
            StringBuilder sb = new StringBuilder();
            foreach (var b in hashBytes)
                sb.Append(b.ToString("x2"));
            return sb.ToString();
        }
    }
}

```
</details>



<details>
<summary><h3>Go</h3></summary>

```go
package main

import (
    "crypto/md5"
    "encoding/hex"
    "encoding/json"
    "fmt"
    "io/ioutil"
    "bytes"
    "net/http"
)

func main() {
    // 1) Generate MD5 of the password
    password := "geheim"
    md5Password := md5Hash(password)
    fmt.Println("MD5 of password:", md5Password)

    // 2) Combine username + md5_password
    username := "user@domain.com"
    combined := username + ":" + md5Password

    // 3) Generate MD5 of the combined string
    finalCredential := md5Hash(combined)
    fmt.Println("Final credentials:", finalCredential)

    // 4) Prepare request body
    requestBodyMap := map[string]interface{}{
        "data": map[string]string{
            "credentials": finalCredential,
        },
    }

    jsonData, err := json.Marshal(requestBodyMap)
    if err != nil {
        fmt.Println("Error marshaling JSON:", err)
        return
    }

    // 5) Send the request
    url := "https://api.slhdirekt.de/auth"
    req, err := http.NewRequest(http.MethodPut, url, bytes.NewBuffer(jsonData))
    if err != nil {
        fmt.Println("Error creating request:", err)
        return
    }
    req.Header.Set("Accept", "application/json")
    req.Header.Set("Content-Type", "application/json")

    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        fmt.Println("Error sending request:", err)
        return
    }
    defer resp.Body.Close()

    // 6) Parse the response
    body, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        fmt.Println("Error reading response:", err)
        return
    }

    if resp.StatusCode == http.StatusOK {
        var responseJson map[string]interface{}
        if err := json.Unmarshal(body, &responseJson); err != nil {
            fmt.Println("Error parsing JSON:", err)
            return
        }
        if responseJson["status"] == "ok" {
            fmt.Println("Auth Token:", responseJson["auth_token"])
        } else {
            fmt.Println("Authentication failed:", responseJson)
        }
    } else {
        fmt.Printf("Error: %d - %s\n", resp.StatusCode, string(body))
    }
}

func md5Hash(text string) string {
    hash := md5.Sum([]byte(text))
    return hex.EncodeToString(hash[:])
}
```
</details>




<details>
<summary><h3>PHP</h3></summary>

```php
<?php
// 1) Generate MD5 of the password
$password = "geheim";
$md5_password = md5($password);
echo "MD5 of password: $md5_password\n";

// 2) Combine username + md5_password
$username = "user@domain.com";
$combined = $username . ":" . $md5_password;

// 3) Generate MD5 of the combined string
$final_credential = md5($combined);
echo "Final credentials: $final_credential\n";

// 4) Prepare request
$url = "https://api.slhdirekt.de/auth";

$data = [
    "data" => [
        "credentials" => $final_credential
    ]
];

$payload = json_encode($data);

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
curl_setopt($ch, CURLOPT_POSTFIELDS, $payload);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    "Accept: application/json",
    "Content-Type: application/json",
    "Content-Length: " . strlen($payload)
]);

// 5) Execute request
$response = curl_exec($ch);
$http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

// 6) Parse response
if ($http_code == 200) {
    $response_data = json_decode($response, true);
    if (isset($response_data["status"]) && $response_data["status"] === "ok") {
        echo "Auth Token: " . $response_data["auth_token"] . "\n";
    } else {
        echo "Authentication failed: $response\n";
    }
} else {
    echo "Error: $http_code - $response\n";
}

```
</details>






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
