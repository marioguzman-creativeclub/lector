# LECTOR · prototipo 0.1

Leer para pensar. NamekyStudio.

## Abrir

Descomprimí el paquete conservando la estructura. La app debe servirse como sitio estático, no abrirse con doble clic como `file://`.

Para probar en una computadora con Python:

```sh
cd lector
python3 -m http.server 8765
```

Abrí `http://localhost:8765`. No hace falta compilar: `app.js` y todas las dependencias de ejecución ya están incluidas.

Para iPhone hace falta un sitio estático HTTPS de confianza. Este paquete está listo para GitHub Pages desde main/root. Después se puede abrir en Safari y usar Compartir > Añadir a pantalla de inicio. No requiere un servidor de procesamiento: el alojamiento sólo entrega archivos estáticos. La PWA exige HTTPS (localhost sirve para pruebas de escritorio).

## Recorrido

1. Pegá tu propia clave de Gemini en la pantalla de inicio de LECTOR, nunca en un chat ni en el código. También podés entrar sin clave.
2. Elegí PDF, EPUB, DOCX o TXT UTF-8. Revisá el texto extraído y las advertencias. El original mostrado es el texto extraído, no una réplica visual del archivo.
3. Elegí documento completo o un fragmento desde el alcance. Ajustá PROFUNDIDAD arrastrando la perilla o con las flechas del teclado. GUZMAN/NAMEKY es un selector de dos posiciones: arrastrá, tocá o usá flechas.
4. Tocá Leer. Cambiar las perillas no manda consultas. Con clave, cada lectura requiere confirmación del envío a Google y de las posibles condiciones de facturación.
5. Una cita literal verificada vuelve a su fragmento del original. Las citas que no se pueden comprobar se marcan y no son navegables. La comprobación ignora diferencias de espacios, pero no verifica que el argumento de la IA sea correcto.
6. Los botones cuadrados EXPORTAR activan/desactivan cada mirada para el .md. El botón de descarga incluye todas las versiones de las miradas activadas, con profundidad, alcance, citas y fecha.
7. LOCAL se enciende al iniciar una descarga. Confirmá el guardado en Archivos: una web no puede comprobar que lo terminaste. NUBE permanece apagado, sin integración con Drive.

## Qué funciona

- Cuatro extractores locales incluidos, sin CDN: PDF.js, Mammoth para DOCX, JSZip + análisis del manifiesto/spine para EPUB, TextDecoder para TXT.
- Texto editorial y consola inspirada en el mockup v2: perillas reales, valores legibles, arrastre y teclado, botones ON/OFF de exportación, consola plegable.
- Dos prompts independientes a partir de las guías de Mario Guzmán y Mae Nameky del 25/9/2026. La profundidad no cambia la mirada ni los hechos.
- Motor configurado para `gemini-2.5-flash`, llamadas directas a Google, JSON estructurado y errores sin sobrescribir el original. Cancelación y sin reintentos automáticos.
- Biblioteca IndexedDB de textos extraídos y lecturas; recuperación tras recargar, edición del título, búsqueda, índice, tamaño de letra y marca de posición.
- Clave únicamente en localStorage y botón para borrarla. La biblioteca se borra aparte. Los archivos exportados se borran desde Archivos.
- Manifest, iconos y service worker. La interfaz y extracción pueden funcionar offline después de la primera carga completa. Gemini siempre requiere internet.

## Qué no fue probado en vivo

No se usó una clave real y no hubo consultas reales a Gemini. El modo sin clave es una demostración claramente rotulada: muestra texto y referencias pero no pretende hacer una síntesis. Las pruebas automatizadas interceptan las llamadas API y devuelven un JSON simulado para verificar ambos prompts, consentimiento, citas, errores 429 y cancelación.

Se probaron TXT, DOCX, EPUB y PDF de dos páginas creados para la prueba; descarga .md, biblioteca tras recarga, claves guardadas y borradas, perillas, citas válidas y falsas, modo offline y pantallas de 320, 390 y 1280 px. No hubo errores JavaScript. Se inspeccionaron capturas de pantalla. Son pruebas de Chromium, no una certificación de Safari ni una prueba en un iPhone físico. Queda por probar calidad de las dos voces y compatibilidad real de la API con la clave del usuario.

## Límites y privacidad

- La extracción es local. El análisis con clave NO es local: el texto del alcance se envía a Google. No se sube el archivo binario.
- Google ofrece capa gratis con límites y, según su política, puede usar ese contenido para mejorar productos. Una clave de un proyecto con facturación puede generar cargos. La app no puede detectar tu plan ni estimar cargos: no garantiza costo cero.
- localStorage no es una caja fuerte. Quien controle este navegador/origen puede leer la clave. Usá sólo un alojamiento de confianza, no un equipo compartido. Nunca incorporés la clave al repo. Es un prototipo personal, no una arquitectura para compartir claves o usuarios.
- Máximo 25 MB por archivo, 600.000 caracteres y 700 páginas PDF. No se recorta a escondidas. DOCX/EPUB comprimidos tienen límite de expansión de 100 MB.
- Sin OCR, DRM, `.doc` antiguo, imágenes, tablas con fidelidad de diseño ni fórmulas garantizadas. PDF puede perder orden en columnas/tablas. Las páginas citadas son índices del PDF, no necesariamente números impresos. EPUB usa capítulos; DOCX/TXT, secciones y fragmentos.
- Los documentos quedan como texto extraído y referencias en IndexedDB. El archivo original no se conserva. El navegador puede borrar datos de almacenamiento; usá exportación para conservar lecturas.
- Borrar biblioteca es irreversible dentro de la app. No borra las descargas.
- Sin guardado Drive, nube, sincronización ni búsqueda externa.

## Desarrollo

```sh
npm ci
npm run build
npm test
```

Las pruebas requieren Chromium en `/usr/bin/google-chrome`, Python, y `pdf-lib`/`docx` en el entorno de prueba. Los fixtures y capturas están en la entrega de desarrollo, no en este paquete limpio de publicación. La app compilada no necesita Node ni Python del lado del teléfono.

## Fuentes del motor

Modelo verificado en https://ai.google.dev/gemini-api/docs/models/gemini-2.5-flash

Precios, capa gratis y tratamiento de datos: https://ai.google.dev/gemini-api/docs/pricing

Contrato generateContent: https://ai.google.dev/api/generate-content

Revisado el 30 de septiembre de 2026. No hay cuenta, clave ni facturación incluidos.
