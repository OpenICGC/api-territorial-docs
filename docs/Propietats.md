<p class="codepen" data-height="350" data-theme-id="light" data-slug-hash="WNmKZoe" data-editable="true" data-user="unitatgeostart" style="height: 350px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/unitatgeostart/pen/WNmKZoe">
  Exemple AddMarker</a> by unitatgeostart (<a href="https://codepen.io/unitatgeostart">@unitatgeostart</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

<a style="color: white" target="_blank" class=" button btn btn-primary" href="https://codepen.io/unitatgeostart/pen/WNmKZoe">CodePen</a>

```html
<html>
  <head>
    <meta charset="utf-8" />
    <title>API Territorial - Propietats</title>
    <meta
      name="viewport"
      content="initial-scale=1,maximum-scale=1,user-scalable=no"
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
    <pre id="info">Carregant dades...</pre>

    <script>
      let dades;
      let servei = "municipis"; //canvia'l pel que vulguis
      let lon = 2.01;
      let lat = 41.5;
      //Connexió amb l'API
      async function apiConnect() {
        const response = await fetch(
          `https://api.icgc.cat/territori/${servei}/geo/${lon}/${lat}`
        );
        dades = await response.json();
        if (dades) {
          document.getElementById("info").innerHTML = JSON.stringify(
            dades[0].features[0].properties,
            null,
            2
          );
        } else {
          document.getElementById("info").innerHTML =
            "La petició no ha obtingut resposta";
        }
      }
      apiConnect();
    </script>
  </body>
</html>
```
