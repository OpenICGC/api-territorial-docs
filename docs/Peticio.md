### Peticions

!!! info "Exemple petició ID servei '''municipis''' amb lon/lat"

```
https://api.icgc.cat/territori/municipis/geo/2.746/41.914
```

!!! info "Exemple petició ID servei '''municipis''' amb UTM 31N X/Y"

```
https://api.icgc.cat/territori/municipis/utm/432648.873/4624697.432
```

!!! info "Exemple petició varis ID de serveis, concatenats amb <b>,<b> i lon/lat"

```
https://api.icgc.cat/territori/municipis,comarques,provincies,vegueries/geo/2.746/41.914
```

!!! info "Exemple petició tots els serveis amb lon/lat"

```
https://api.icgc.cat/territori/all/geo/2.746/41.914
```

### Resposta en format GeoJson

```json
[{"type":"FeatureCollection",
"features":[{"type":"Feature","geometry":{"type":"MultiPolygon",
"coordinates":[[[[2.74737,41.92599],..... ,[2.74737,41.92599]]]]},
"properties":{"CODIMUNI":"172333",....,"CAPPROV":"Girona"},
"id":"Municipis"}]},"NumResponses:1"]
```

[Veure visor playground](../Playground)

### Serveis disponibles

| Servei                                     | Organisme                                                 | **ID**                        |
| ------------------------------------------ | :-------------------------------------------------------- | ----------------------------- |
| Agrupacions censals                        | Institut d'Estadística de Catalunya                       | agrupacionscensals            |
| Àrees Bàsiques de Salut                    | Departament de Salut                                      | areesbasiquessalut            |
| Àrees Bàsiques de Serveis Socials          | Departament de Drets Socials                              | areasbasiquesservicisocials   |
| Àrees Bàsiques Policials                   | Departament Interior                                      | areasbasiquespolicials        |
| Àrees hidrogeològiques naturalesa aqüífers | Institut Cartogràfic i Geològic de Catalunya              | aquifers                      |
| Cobertes del sòl                           | Institut Cartogràfic i Geològic de Catalunya              | cobertessol                   |
| Comarques                                  | Institut Cartogràfic i Geològic de Catalunya              | comarques                     |
| Conques hidrològiques principals           | Agència Catalana de l'Aigua                               | conqueshidrologiques          |
| Fulls ETRS 89 1:5 000                      | Institut Cartogràfic i Geològic de Catalunya              | tall5me                       |
| Elevacions                                 | Institut Cartogràfic i Geològic de Catalunya              | elevacions                    |
| Geocodificador ICGC                        | Institut Cartogràfic i Geològic de Catalunya              | geocoder                      |
| Hex-based grid system                      | Uber                                                      | h3                            |
| Incendis forestals 1986-2022               | Departament d'Acció Climàtica, Alimentació i Agenda Rural | incendis                      |
| Mapa urbà Classificacions                  | Departament de Territori                                  | mucclassificacions            |
| Mapa urbà Qualificacions                   | Departament de Territori                                  | mucqualificacions             |
| Municipis                                  | Institut Cartogràfic i Geològic de Catalunya              | municipis                     |
| Parcel·les cadastrals                      | Dirección General del Catastro                            | cadastre                      |
| Provincies                                 | Institut Cartogràfic i Geològic de Catalunya              | provincies                    |
| Regions sanitàries                         | Departament de Salut                                      | regionssanitaries             |
| Seccions censals                           | Institut Cartogràfic i Geològic de Catalunya              | seccionscensals               |
| SIGPAC Recintes                            | Departament d'Acció Climàtica, Alimentació i Agenda Rural | recintessigpac                |
| Sistema d'orientacio cartogràfica          | Departament d'Interior                                    | sistemaorientaciocartografica |
| Subconques hidrològiques                   | Agència Catalana de l'Aigua                               | subconqueshidrologiques       |
| Unitats geològiques 50 m                   | Institut Cartogràfic i Geològic de Catalunya              | unitatsgeologiques            |
| Vegetació Hàbitats Catalunya               | Departament d'Acció Climàtica, Alimentació i Agenda Rural | vegetacio                     |
| Vegueries                                  | Institut Cartogràfic i Geològic de Catalunya              | vegueries                     |

<hr>
