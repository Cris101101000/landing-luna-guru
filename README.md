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

### Por qué el formulario es propio y no el embebido de HubSpot

HubSpot descarta todos los envíos cuya página de origen es lunaos.gurusoluciones.com (probado el 20-nov-2026: el mismo envío con origen `https://www.gurusoluciones.com.ar/luna`, o sin origen, sí se guarda). Con el formulario embebido (`hbspt.forms.create`) el origen es siempre la URL de la página, así que los registros se perdían.

Por eso el registro usa el formulario propio de la landing (ventana de «Registrate»), que envía por la API pública de formularios de HubSpot a «Piloto Luna – Registro landing (Gurú)» (portal 251261, formulario `cf62c5ba-7037-4dfe-a289-13f38ed98ca1`) con:

- `pageUri` `https://www.gurusoluciones.com.ar/luna` y `pageName` «Luna – gurú» **fijos**, más `hutk` si existe la cookie de seguimiento. No cambiar `pageUri` por la URL real hasta que gurú habilite el dominio en HubSpot.
- Nombre, teléfono normalizado a +549 (sin 0 ni 15), correo, negocio, rubro, consentimiento de WhatsApp, UTM y los campos de campaña.

### Si cambia el formulario de HubSpot, hay que cambiar la landing

La landing no lee el formulario de HubSpot: arma los campos en el código (`registroHubSpot` en `fuente/body.html` y en `index.html`). Cualquier campo que se agregue, cambie de nombre o de valores en el formulario de HubSpot hay que replicarlo en la landing. Por ejemplo, `odoo_producto_base` se suma a `CAMPANA` cuando gurú defina su valor; los valores de `pl_rubro` están en `RUBRO`.

### Medición

- La página carga el Tag Manager de gurú (`GTM-KHV2Q6WD`) y el código de seguimiento de HubSpot (portal 251261).
- Las UTM del anuncio se guardan 30 días en el navegador (`luna_utm`): si la persona vuelve sin UTM, el registro lleva las guardadas.
- Solo cuando HubSpot responde 200 (registro guardado) se muestra «¡Gracias! En unos minutos te escribimos por WhatsApp.» y se manda a Tag Manager el evento `lunaos_lead`, con `event_id` y `user_data` (email, phone, first_name, last_name). Lo recogen la etiqueta «LunaOS - GA4 lead» del contenedor web y «LunaOS - Meta CAPI - Lead» del server, que se lo envía a Meta como `Lead`. Si HubSpot no confirma, la persona ve un error y puede reintentar, y no se manda el evento.
