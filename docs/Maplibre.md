<p class="codepen" data-height="500" data-theme-id="light" data-slug-hash="ZEPqNPb" data-editable="true" data-user="unitatgeostart" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/unitatgeostart/pen/ZEPqNPb">
  Exemple AddMarker</a> by unitatgeostart (<a href="https://codepen.io/unitatgeostart">@unitatgeostart</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<a style="color: white" target="_blank" class=" button btn btn-primary" href="https://codepen.io/unitatgeostart/pen/ZEPqNPb">CodePen</a>

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
    <link
      rel="stylesheet"
      href="https://unpkg.com/maplibre-gl@2.0.1/dist/maplibre-gl.css"
    />
    <script src="https://unpkg.com/maplibre-gl@2.0.1/dist/maplibre-gl.js"></script>

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
      var marker1;
      var map = new maplibregl.Map({
        container: "map",
        style:
          "https://geoserveis.icgc.cat/contextmaps/icgc_mapa_base_fosc.json", //(orto) "https://geoserveis.icgc.cat/contextmaps/icgc_orto_estandard.json",
        center: [2.0042, 41.4747],
        zoom: 8,
        attributionControl: false,
        hash: false,
      });
      map.addControl(new maplibregl.NavigationControl());

      map.on("click", function (e) {
        let lon = e.lngLat.lng;
        let lat = e.lngLat.lat;
        apiConnect(lat, lon, "municipis");
        if (!marker1) {
          marker1 = new maplibregl.Marker({ color: "#FF6E42" })
            .setLngLat([lon, lat])
            .addTo(map);
        } else {
          marker1.remove();
          marker1 = new maplibregl.Marker({ color: "#FF6E42" })
            .setLngLat([lon, lat])
            .addTo(map);
        }
      });

      async function apiConnect(lat, lon, service) {
        const response = await fetch(
          `https://api.icgc.cat/territori/${service}/geo/${lon}/${lat}`
        );
        const dades = await response.json();
        console.log("dades", dades);
        document.getElementById("info").innerHTML =
          "Municipi: " + dades[0].features[0].properties.NOMMUNI;

        if (!map.getLayer("punts")) {
          map.addSource("punts", {
            type: "geojson",
            data: {
              type: "FeatureCollection",
              features: dades[0].features,
            },
          });
          map.addLayer({
            id: "punts",
            type: "fill",
            source: "punts",
            paint: {
              "fill-color": "#f0f0f0",
              "fill-opacity": 0.6,
            },
          });
        } else {
          map.removeLayer("punts").removeSource("punts");
          map.addSource("punts", {
            type: "geojson",
            data: {
              type: "FeatureCollection",
              features: dades[0].features,
            },
          });
          map.addLayer({
            id: "punts",
            type: "fill",
            source: "punts",
            paint: {
              "fill-color": "#ffffff",
              "fill-opacity": 0.75,
            },
          });
        }
      }
    </script>
  </body>
</html>
```
