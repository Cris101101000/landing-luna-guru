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

- El formulario de registro es el nativo de HubSpot de gurú (portal 251261). Se abre en la ventana de «Registrate».
- La página carga el Tag Manager de gurú (`GTM-KHV2Q6WD`).
- Cuando el formulario se envía bien, la página manda a Tag Manager el evento `lunaos_lead`, con `event_id` y `user_data` (email, phone, first_name, last_name). Lo recogen la etiqueta «LunaOS - GA4 lead» del contenedor web y «LunaOS - Meta CAPI - Lead» del server, que se lo envía a Meta como `Lead`.
