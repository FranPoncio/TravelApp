# Reglas de generación y pruebas

- Para regenerar transporte: `npm run gtfs`.
- Para servir localmente: `python3 -m http.server 8000` o `npm start`.
- Para formato: `npm run format`.
- No hacer requests al ruteador durante tests automatizados sin mock.
- Probar siempre fallback sin conexión al servicio Valhalla.
- Revisar que `transporte-gtfs.json` coincida con las fuentes configuradas.
