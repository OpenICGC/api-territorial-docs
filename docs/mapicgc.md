<p class="codepen" data-height="500" data-theme-id="light" data-slug-hash="wvOLWPJ" data-editable="true" data-user="unitatgeostart" style="height: 500px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/unitatgeostart/pen/wvOLWPJ">
  Exemple AddMarker</a> by unitatgeostart (<a href="https://codepen.io/unitatgeostart">@unitatgeostart</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<a style="color: white" target="_blank" class=" button btn btn-primary" href="https://codepen.io/unitatgeostart/pen/wvOLWPJ">CodePen</a>

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
    <script src="https://tilemaps.icgc.cat/cdn/mapicgc-gl-js/mapicgc-gl.js"></script>
    <link
      href="https://tilemaps.icgc.cat/cdn/mapicgc-gl-js/mapicgc-gl.css"
      rel="stylesheet"
    />

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
    <div id="map" style="height:375px;"></div>
    <div id="info">Fes clic sobre el mapa.</div>
    <script>
      var marker1;
      let geojsonLayer;
      const map = new mapicgcgl.Map({
        container: "map",
        style: mapicgcgl.Styles.TOPO,
        center: [2.1464, 41.306],
        zoom: 7.4,
        maxZoom: 14,
        hash: true,
        pitch: 0,
      });

      map.on("click", function (e) {
        let lon = e.lngLat.lng;
        let lat = e.lngLat.lat;
        apiConnect(lat, lon, "municipis");
        if (!marker1) {
          marker1 = map.addMarker({ color: "#FF6E42", coord: [lon, lat] });
        } else {
          marker1.remove();
          marker1 = map.addMarker({ color: "#FF6E42", coord: [lon, lat] });
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

        geojsonLayer = {
          id: "punts",
          data: {
            type: "Feature",
            geometry: {
              type: "MultiPolygon",
              coordinates: dades[0].features[0].geometry.coordinates,
            },
          },
          layerType: "fill",
          layout: {},
          paint: {
            "fill-color": "red",
            "fill-opacity": 0.8,
          },
        };
        if (!map.getSource("punts-sourceIcgcMap")) {
          map.addLayerGeoJSON(geojsonLayer);
        } else {
          map.removeLayer("punts-layerIcgcMap");
          map.removeSource("punts-sourceIcgcMap");
          map.addLayerGeoJSON(geojsonLayer);
        }
      }
    </script>
  </body>
</html>
```
