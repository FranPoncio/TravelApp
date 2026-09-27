# Índice de documentación de TravelApp

## Proyecto

- `README.md`: funcionalidad, ruteo, GTFS, ejecución y estructura.
- `CLAUDE.md`: reglas existentes del proyecto.

## Código principal

- `index.html`: shell de la aplicación.
- `assets/js/app.js`: mapa, geolocalización, ruteo, filtros, GTFS y UI.
- `assets/js/data.js`: localidades, puntos, categorías y modos.
- `assets/css/style.css`: temas, layout y pines.
- `scripts/build-gtfs.mjs`: pipeline de feeds GTFS.
- `scripts/gtfs-sources.json`: fuentes configuradas.
- `assets/data/transporte-gtfs.json`: salida generada del pipeline.

## Qué consultar según la tarea

- Agregar un destino: `assets/js/data.js` y `README.md`.
- Cambiar ruteo: `assets/js/app.js`, especialmente Valhalla y fallback.
- Cambiar transporte público: `scripts/`, feeds GTFS y salida generada.
- Cambiar tema o pines: `assets/css/style.css` y vendor de Leaflet.
- Cambiar idioma: `assets/js/data.js` y `assets/js/app.js`.

## Riesgos importantes

- Precios y transporte son orientativos, no datos oficiales.
- Valhalla es un servicio público externo y puede fallar.
- La geolocalización requiere localhost o HTTPS.
- No editar manualmente el JSON generado si el cambio pertenece al pipeline GTFS.
