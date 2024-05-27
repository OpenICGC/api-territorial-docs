<p class="codepen" data-height="500" data-theme-id="light" data-slug-hash="oNVyBbK" data-editable="true" data-user="unitatgeostart" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/unitatgeostart/pen/oNVyBbK">
  Exemple AddMarker</a> by unitatgeostart (<a href="https://codepen.io/unitatgeostart">@unitatgeostart</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<a style="color: white" target="_blank" class=" button btn btn-primary" href="https://codepen.io/unitatgeostart/pen/oNVyBbK">CodePen</a>

<style>
  .button{
    position: relative;
    top: 9px;
    z-index: 1;
    width: 80px;
    float: right;
    right: 119px;
    background-color: #0d58ff;
    border-radius: 10px;
    text-align: -webkit-center;
    font-size: smaller;
  }
    .button:hover{

    background-color: #032879;

  }
  </style>

```html hl_lines="33-35 45-50"
<html>
  <head>
    <meta charset="utf-8" />
    <title>Connexió a l'API</title>
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

    <style>
      #info {
        background-color: white;
        font-family: Arial;
        text-align: center;
        padding: 5px;
        font-weight: 500;
        font-size: 0.9em;
      }
    </style>
  </head>
  <body>
    <div id="info">Fes clic sobre el mapa.</div>
    <div id="map"></div>
    <script>
      let service = "municipis";
      let puntLayer;
      let dades;

      //Connexió amb l'API i afegir al mapa la resposta geojson
      async function apiConnect(lat, lon) {
        const response = await fetch(
          `https://api.icgc.cat/territori/${service}/geo/${lon}/${lat}`
        );
        dades = await response.json();

        //si volem veure la resposta completa a la consola:
        // console.log("dades", dades);

        if (puntLayer) {
          map.removeLayer(puntLayer);
        }

        puntLayer = L.geoJSON(dades[0].features, {
          style: {
            fillColor: "yellow",
            fillOpacity: 0.8,
          },
        }).addTo(map);

        if (dades[0].features) {
          document.getElementById("info").innerHTML =
            "Municipi: " + dades[0].features[0].properties.NOMMUNI;
        } else {
          document.getElementById("info").innerHTML =
            "No hi ha dades pel punt seleccionat";
        }
      }

      // Crear el mapa
      var map = L.map("map").setView([41.387, 2.169], 9);
      var marker1;
      L.tileLayer(
        "https://geoserveis.icgc.cat/servei/catalunya/contextmaps/wmts/contextmaps-mapa-estandard/MON3857NW/{z}/{x}/{y}.png"
      ).addTo(map);

      //Obtenim les coordenades i cridem la funció apiConnect()
      //també afegim un marcador al punt clicat
      map.on("click", function (e) {
        let lon = e.latlng.lng;
        let lat = e.latlng.lat;
        apiConnect(lat, lon);

        if (!marker1) {
          marker1 = L.marker([lat, lon]).addTo(map);
        } else {
          map.removeLayer(marker1);
          marker1 = L.marker([lat, lon]).addTo(map);
        }
      });
    </script>
  </body>
</html>
```
