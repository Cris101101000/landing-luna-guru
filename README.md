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

## Pendiente

El formulario envía cada registro al formulario de HubSpot de gurú «Piloto Luna – Registro landing (Gurú)» (portal 251261) por la API pública de envíos, con el rubro, el teléfono normalizado, las UTM y los campos de campaña acordados con gurú. La página también carga el código de seguimiento de HubSpot. Falta sumar odoo_producto_base cuando se confirme el valor.
