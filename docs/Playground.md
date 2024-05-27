<html>
  <head>
    <meta charset="utf-8" />
    <title>Visor API Territorial</title>
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
    />
    <!-- MapLibre -->
    <link rel="stylesheet" href="https://unpkg.com/maplibre-gl@2.0.1/dist/maplibre-gl.css" />
    <script src="https://unpkg.com/maplibre-gl@2.0.1/dist/maplibre-gl.js"></script>
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap"
    />
    <link
      rel="stylesheet"
      href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@24,400,0,0"
    />
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200" />    <style>
      #map {
        position: absolute;
        width: 45%;
        height: 380px;
        left: 30%;
        margin-left: 5px;
        top: 220px;
        margin-bottom: 80px;  
      }
      #resposta{
        position: absolute;
        background-color: #1F2020;
        top: 600px;
        margin-bottom:150px;
        max-width: 45%;
        color:#f0f0f0;
        padding: 10px;
        font-size: 0.8em;
      }
      .md-footer-meta {
        display: none !important;
      }
      @media(max-width: 1219px ){
        #map{
          width: 40%;
        }
      }
      #layers {
        position: absolute;
        background-color: white;
        z-index: 50;
        border-radius: 8px;
        bottom: 5%;
        margin-left: 70%;
      }
      #apidata {    
        position: absolute;
        text-align: center;
        background-color: #1F2020;
        color: #f0f0f0;
        width: 100%;
        height: auto;
        border-radius: 8px;
        padding: 10px;
        z-index: 50;
        font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
          Roboto, Oxygen, Ubuntu, Cantarell, "Open Sans", "Helvetica Neue",
          sans-serif;
        font-weight: 500;
        font-size: 1.2em;   
        margin-top: 0;
        margin-left: 0;
      }
      #serveiSelector {
        padding: 7px;
        font-size: 1em;   
        border: 1px solid #1F2020;
        border-radius: 5px;
        background-color: #1F2020;
        cursor: pointer;
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        color: #f0f0f0;
      }
      #serveiSelector:hover {
        border-color: #555;
      }
      #serveiSelector:focus {
        outline: none;
        border-color: #4d90fe;
        box-shadow: 0 0 5px rgba(77, 144, 254, 0.5);
      }
      ::-webkit-scrollbar {
        width: 7px;
      }
      ::-webkit-scrollbar-thumb {
        background-color: #333;
      }
      ::-webkit-scrollbar-track {
        background-color: #f3f3f3;
      }
      .servei{
        background:none;
        color:#f0f0f0;
        position: absolute;
        z-index: 50;
        border-radius: 8px;
        bottom: 15%;
        margin-left: 70%;
      }
      #formContainer {
        position: absolute;
        bottom: 18%;
        margin-left: 68%;
        color: #ffffff;
        z-index: 10000;
      }
      #textSelector {
        height: 35px;
        width: 200px;
        border-radius: 8px;
        background-color: #1F2020;
        color: white;
        padding: 5px;
      }
      #textForm button {
        height: 35px;
        border-radius: 8px;
        padding: 5px;
        color:#f0f0f0;
        background-color: #1F2020;
       }
        input::placeholder {
          color: white !important;
        }
        #boto{
          position: absolute;
          top: 65%;
          left: 41%;
          color: #FFFFFF;
          cursor: pointer;
        }
        #boto2{
          position: absolute;
          top: 65%;
          left: 44%;
          color: #FFFFFF;
          cursor: pointer;
        }
    </style>
    <script>
      var map;
      var marker1;
      var minZoom = 3;
      let service = "municipis";
      let resposta;
      let lan;
      let lon;
      function getBounds() {
        const bounds = map.getBounds();
        var bbox = {
          minX: bounds.getWest(),
          minY: bounds.getSouth(),
          maxX: bounds.getEast(),
          maxY: bounds.getNorth(),
        };
        return bbox;
      }
      async function apiConnect() {
        const response = await fetch(
          `https://api.icgc.cat/territori/${service}/geo/${lon}/${lat}`
        );
        const dades = await response.json();
        resposta = dades;
        document.getElementById("resposta").innerHTML = "Resposta GeoJSON: " + JSON.stringify(resposta, null, 2);
         if (resposta) {
        document.getElementById("boto").style.display = "inline-block";
        document.getElementById("boto2").style.display = "inline-block";
    }
      }
      function descarregar() {
        // Convertim la resposta a un string JSON
        const respostaString = JSON.stringify(resposta, null, 2);
        // Creem un element d'enllaç per descarregar
        const enllac = document.createElement('a');
        enllac.href = 'data:text/plain;charset=utf-8,' + encodeURIComponent(respostaString);
        enllac.download = 'resposta.geojson';
        enllac.click();
      }
      function copiar() {
      // Copiem el contingut de la resposta al porta-retalls
        const respostaString = JSON.stringify(resposta, null, 2);
        navigator.clipboard.writeText(respostaString)
        .then(() => {
            alert('El contingut ha estat copiat al porta-retalls!');
        })
        .catch(err => {
            console.error('Error al copiar:', err);
        });
}
      function onServeiChange() {
        // Obtenir el valor seleccionat
        const serveiSelector = document.getElementById("serveiSelector");
        service = serveiSelector.value;
      }
      function init() {
        map = new maplibregl.Map({
          container: "map",
          style:
            "https://geoserveis.icgc.cat/contextmaps/icgc_mapa_base_fosc.json", 
          center: [2.0042, 41.8747],
          zoom: 6,
          attributionControl: false,
          hash: false,
        });
        map.on("click", function (e) {
           lon = e.lngLat.lng;
           lat = e.lngLat.lat;
           document.getElementById("apidata").innerHTML =
              `Petició: <a style="color:white" href="https://api.icgc.cat/territori/${service}/geo/${lon}/${lat}" target="_blank"> https://api.icgc.cat/territori/${service}/geo/${lon.toFixed(3)}/${lat.toFixed(3)}</a>`   
          apiConnect()
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
      } 
    </script>

  </head>

  <body onload="init()">
    <div id="map">
    <div id="apidata">Clica sobre un punt del mapa</div>
    <div id="formContainer">
    </div>
    <div class='servei'>Servei seleccionat:</div>
    <!-- Afegit un selector per triar el servei -->
    <div id="layers">
      <select id="serveiSelector" onchange="onServeiChange()">
        <option selected value="municipis">Municipis</option>
        <option value="comarques">Comarques</option>
        <option value="provincies">Provincies</option>
        <option value="vegueries">Vegueries</option>
        <option value="seccionscensals">Seccions Censals</option>
        <option value="elevacions">Elevacions</option>
        <option value="agrupacionscensals">Agrupacions Censals</option>
        <option value="regionssanitaries">Regions Sanitàries</option>
        <option value="areesbasiquespolicials">Àrees Bàsiques Policials</option>
        <option value="areesbasiquesserveissocials">Àrees Bàsiques Serveis Socials</option>
        <option value="cobertessol">Cobertes del sòl</option>
        <option value="mucqualificacions">Mapa Urbà de Qualificacions</option>
        <option value="h3">H3 Grid System</option>
        <option value="recintessigpac">Recintes SIGPAC</option>
        <option value="geocoder">Geocodificador invers</option>
        <option value="conqueshidrologiques">Conques Hidrològiques</option>
        <option value="subconqueshidrologiques">
          Subconques Hidrològiques
        </option>
        <option value="sistemaorientaciocartografica">
          Sistema d'Orientació Cartogràfica
        </option>
        <option value="tall5me">Tall 5m E</option>
        <option value="aquifers">Aqüífers</option>
        <option value="unitatsgeologiques">Unitats geològiques</option>
        <option value="vegetacio">Vegetacio Hàbitats Catalunya</option>
        <option value="incendis">Incendis Forestals</option>
        <option value="cadastre">Cadastre</option>
      </select>
    </div>
    </div>
    <pre id="resposta">Resposta Geojson</pre>
    <button id="boto" style="display: none;" onclick="descarregar()" title="Descarregar geojson"><span class="material-symbols-outlined">
download
</span></button>
    <button id="boto2" style="display: none;" onclick="copiar()" title="Copiar"><span class="material-symbols-outlined">
content_copy
</span></button>
  </body>
</html>
