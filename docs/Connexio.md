### JavaScript

#### Via Fetch

```javascript hl_lines="4-6"
let servei = "vegueries"; //canvia'l pel que vulguis

//Connexió amb l'API
async function apiConnect() {
  const response = await fetch(
    `https://api.icgc.cat/territori/${servei}/geo/${lat}/${lon}`
  );
  dades = await response.json();
}
```

#### Via Axios

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

```javascript hl_lines="5-7"
let serveis = "municipis, provincies, comarques"; //afegeix els que vulguis

//Connexió amb l'API
async function apiConnect() {
  const response = await axios.get(
    `https://api.icgc.cat/territori/${serveis}/utm/${x}/${y}`
  );
  const dades = response.data;
}
```

### Node

!!!warning "Cal tenir [Node.js](https://nodejs.org/en) instal·lat"
Instal·lar axios

```bash
npm install axios
```

Petició:

```javascript hl_lines="1 7-9"
const axios = require("axios");
let servei = 'all'; //tots els serveis

//Connexió amb l'API
async function apiConnect() {
  try {
    const response = await axios.get(
      `https://api.icgc.cat/territori/${servei}/geo/${lat}/${lon}`
    );
    const dades = response.data;
  }
}

```

### Python

```python
import requests

servei = "vegueries"  # canvia'l pel que vulguis
lat = 41.38879  # substitueix amb la latitud desitjada
lon = 2.15899  # substitueix amb la longitud desitjada

# Connexió amb l'API
def api_connect():
    url = f"https://api.icgc.cat/territori/{servei}/geo/{lat}/{lon}"
    response = requests.get(url)
    dades = response.json()
    return dades

dades = api_connect()
print(dades)  # mostra les dades obtingudes de l'API

```

### JAVA

```java
import java.io.IOException;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        String servei = "vegueries"; // canvia'l pel que vulguis
        double lat = 41.38879; // substitueix amb la latitud desitjada
        double lon = 2.15899; // substitueix amb la longitud desitjada

        try {
            String urlStr = "https://api.icgc.cat/territori/" + servei + "/geo/" + lat + "/" + lon;
            URL url = new URL(urlStr);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");
            conn.connect();

            // Llegir la resposta de l'API
            Scanner scanner = new Scanner(url.openStream());
            StringBuilder response = new StringBuilder();
            while (scanner.hasNext()) {
                response.append(scanner.nextLine());
            }
            scanner.close();

            System.out.println(response.toString()); // mostra les dades obtingudes de l'API

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

### PHP

```php
<?php
$servei = "vegueries"; // canvia'l pel que vulguis
$lat = 41.38879; // substitueix amb la latitud desitjada
$lon = 2.15899; // substitueix amb la longitud desitjada

$url = "https://api.icgc.cat/territori/$servei/geo/$lat/$lon";

// Realitza la petició a l'API
$response = file_get_contents($url);

// Decodifica la resposta JSON
$dades = json_decode($response);

// Mostra les dades obtingudes de l'API
print_r($dades);
?>

```

### R

```r
library(httr)

servei <- "vegueries"  # canvia per al servei que desitgis
lat <- 41.38879  # substitueix per la latitud desitjada
lon <- 2.15899  # substitueix per la longitud desitjada

url <- sprintf("https://api.icgc.cat/territori/%s/geo/%f/%f", servei, lat, lon)

# Realitza la petició GET a l'API
resposta <- GET(url)

# Extreu les dades de la resposta
dades <- content(resposta, "parsed")

# Mostra les dades obtingudes de l'API
print(dades)

```
