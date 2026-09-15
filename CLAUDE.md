# TRAVAPP / Rutas Argentinas — notas para Claude

Planificador de viajes a puntos turísticos de **Argentina y Nueva Zelanda**,
bilingüe (ES/EN), que calcula rutas desde donde estás según el medio de
transporte.

**Este archivo existe para no redescubrir el repo en cada sesión.** Si algo
acá quedó viejo, corregilo en el momento: cuesta menos que volver a explorar.

## Adónde va

La idea no es competir con Google Maps en ruteo: es **darle lo que Google
Maps no da**. Que al llegar a un lugar nuevo la app te diga qué hacer ahí, te
cuente algo de historia, te arme la ruta desde donde estás y te muestre cómo
llegar en transporte público real.

La vara para cualquier función nueva: **¿esto lo hace mejor que abrir Google
Maps?** Si la respuesta es "lo hace igual", no va. Lo que diferencia es la
ficha —foto, reseña histórica, precio, cómo llegar—, la ruta escénica que
pasa por otro punto de camino, y el filtro por tipo de actividad.

## Estado

En producción. **El catálogo creció mucho más que la documentación**: hoy son
**11 países, 65 localidades y más de 1.000 puntos**, no los "413 puntos en 14
localidades de Argentina y Nueva Zelanda" que se venían repitiendo. Los países
son Argentina, Nueva Zelanda, Japón, Tailandia, Australia, Vietnam, Camboya,
China, Laos, Singapur y Corea del Sur.

No hardcodees esos números: recalculalos cuando los necesites.

```bash
grep -c "puntos:" assets/js/data.js          # localidades
grep -cE "(?<![A-Za-z])PN?\(" assets/js/data.js   # puntos (P = sólo es, PN = bilingüe)
```

Lo que está flojo y conviene saber antes de planificar:

- **El GTFS cubre sólo dos regiones de Argentina** (subte de Buenos Aires e
  interurbano de Córdoba). Para los otros diez países y el resto del país, el
  transporte que se muestra es la nota curada, no datos reales. El discurso de
  "transporte público real" aplica a una porción chica del catálogo.
- **Los datos de Argentina son monolingües.** Usan el helper `P()`, sin par
  es/en; los otros diez países usan `PN()`, que sí es bilingüe. Si agregás
  puntos, fijate cuál corresponde.
- **Quedan cadenas sin pasar por i18n**: el `title` del botón de tema en
  `index.html`, el crédito del pie (Mapa © OpenStreetMap · CARTO | Ruteo ©
  Valhalla), y el `<title>` y `<meta description>` del documento.
- **No hay tests.** Con más de mil entradas cargadas a mano, un chequeo de
  esquema —ids únicos, lat/lng válidas, pares es/en completos donde
  corresponde— atajaría errores que hoy no avisan.

**Al terminar una sesión, actualizá estas líneas.**

## Dónde está cada cosa

```
index.html              la app entera: una sola página
assets/js/app.js        toda la lógica: mapa, ruteo, fichas, filtros, i18n
assets/js/data.js       los 413 puntos turísticos con sus fichas
assets/css/style.css    todos los estilos
assets/data/transporte-gtfs.json   paradas y líneas ya procesadas
assets/vendor/          Leaflet y las tipografías, servidas localmente
data/gtfs/              los feeds GTFS crudos
scripts/build-gtfs.mjs  el pipeline que regenera el JSON de transporte
scripts/gtfs-sources.json   de dónde salen los feeds
```

## Comandos

```bash
python3 -m http.server 8000   # y abrir http://localhost:8000
npm run gtfs                  # regenera assets/data/transporte-gtfs.json
npm run format                # prettier
```

**No hay build ni dependencias de producción.** Es JavaScript a mano servido
como archivos estáticos, y es deliberado.

## Lo que hay que saber antes de tocar

- **La geolocalización exige `localhost` o HTTPS.** Abrir `index.html` con
  doble clic no alcanza: `fetch` y la Geolocation API fallan desde `file://`.
  Por eso siempre se levanta un servidor.
- **El ruteo usa Valhalla público** (`valhalla1.openstreetmap.de`), sin API
  key. Es un servicio ajeno y gratuito: puede estar caído o lento. **La
  degradación elegante —línea recta con tiempo estimado— no es un adorno, es
  el camino que se usa cada vez que el ruteador no contesta.** No romperla.
- **Leaflet y las tipografías están vendorizadas** en `assets/vendor/` a
  propósito: la app tiene que andar sin depender de un CDN.
- **`assets/data/transporte-gtfs.json` es generado**, no se edita a mano. Se
  regenera con `npm run gtfs` desde `scripts/gtfs-sources.json`.
- Los precios de entrada y la info de transporte son **orientativos** y así
  están declarados en la interfaz. Con la inflación argentina, presentarlos
  como dato oficial sería mentir. Mantener el aviso.
- Hay un workflow que refresca los datos GTFS y commitea solo.

## Convenciones

- **Todo en castellano**: variables, comentarios, commits. Francisco escribe
  rioplatense; contestale igual.
- La interfaz es bilingüe ES/EN: **toda cadena visible va por el mecanismo de
  i18n**, nunca escrita directo en el HTML.
- Sin frameworks y sin build. Antes de sumar una dependencia, decilo y
  justificala: que no haya ninguna es una decisión del proyecto.
- Los comentarios explican POR QUÉ, no qué.

## Cómo trabajar acá sin quemar tokens

Francisco paga el consumo y las sesiones son largas.

- **`assets/js/app.js` y `data.js` son grandes: no los leas enteros.**
  `grep -n` acotado y `sed -n` del rango que vas a tocar.
- **Editá con reemplazo puntual**, no reescribiendo el archivo completo.
- Capturá pantalla sólo si cambiaste algo visual, y recortado.
- Agrupá comandos independientes en una sola llamada.
- Leé los archivos que necesites sin pedir permiso: cada ida y vuelta reenvía
  toda la conversación y sale más caro que abrir el archivo.

## Nombres

El repo se llama `TravelApp`, el paquete `rutas-argentinas` y la app se
presenta como **TRAVAPP**. Son tres nombres para lo mismo; conviene unificar,
pero preguntarle a Francisco antes de tocar nada publicado.
