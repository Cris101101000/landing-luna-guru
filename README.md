# Landing de Luna para gurú

Landing de la pauta de Meta de gurú (solo Argentina). Es una página estática: no se instala ni se compila nada.

- `index.html`: la página que se publica.
- `escenas/`: las seis animaciones de las habilidades de Luna.
- `posters/`: la imagen fija de cada animación, que se ve mientras carga o si el navegador no puede cargarla.
- `fotos/`: el carrusel de rubros.
- `vercel.json`: la página va sin indexar (`noindex`), como corresponde a una landing de pauta.

## Cambiar textos

Los textos viven en `fuente/`: `body.html` (página), `build.py` (habilidades, pasos y preguntas frecuentes) y `page.css` (estilos). Para regenerar la página:

```bash
cd fuente
python build.py
```

Eso genera `fuente/guru-landing-luna.html`. Después se copia a `index.html` en la raíz, se cambia la URL del logo por `logo-guru.png` y se agrega `<meta name="robots" content="noindex, nofollow">` debajo del viewport.

`escenas.py` y `voseo.py` copian las animaciones desde la web de Bewe y pasan sus textos a voseo. Solo hacen falta si cambian las animaciones.

## Formulario y medición

- El formulario de registro es propio de la landing y se abre en la ventana de «Registrate». Envía cada registro al formulario «Piloto Luna – Registro landing (Gurú)» de HubSpot (portal 251261) por la API pública de envíos, con rubro, teléfono +549, UTM guardadas 30 días (`luna_utm`) y los campos de campaña.
- El envío va con `pageUri` `https://www.gurusoluciones.com.ar/luna` y `pageName` «Luna – gurú» fijos: HubSpot descarta los envíos cuyo origen es lunaos.gurusoluciones.com.
- Solo si HubSpot responde 200 se muestra «¡Gracias!» y se avisa a Tag Manager; si no, la persona ve un error y puede reintentar.
- La página carga el Tag Manager de gurú (`GTM-KHV2Q6WD`).
- Cuando el envío sale bien, la página manda a Tag Manager el evento `lunaos_lead`, con `event_id` y `user_data` (email, phone, first_name, last_name). Lo recogen la etiqueta «LunaOS - GA4 lead» del contenedor web y «LunaOS - Meta CAPI - Lead» del server, que se lo envía a Meta como `Lead`.
