# oriongames666.github.io

Sitio estático (GitHub Pages) de **OrionGames666**. Aloja la política de privacidad de cada app publicada en Google Play.

- Portada: https://oriongames666.github.io/
- Índice de políticas: https://oriongames666.github.io/privacy/
- Política de cada app: `https://oriongames666.github.io/privacy/<app>/`
  (p. ej. Dicee → https://oriongames666.github.io/privacy/dicee/)

## Estructura

```
oriongames666.github.io/
├── index.html                    ← portada del estudio
├── privacy/
│   ├── index.html                ← lista de apps con enlace a cada política
│   └── dicee/index.html          ← política de Dicee
├── _template/privacy-policy.html ← plantilla para nuevas apps (NO se publica)
├── _config.yml                   ← config de Jekyll (excluye README.md)
└── README.md
```

Todo es HTML estático, autocontenido (CSS en línea, sin dependencias externas) y bilingüe ES/EN.

**¿Por qué no se publica `_template`?** GitHub Pages usa Jekyll por defecto, y Jekyll ignora las carpetas que empiezan por `_`. Por eso **no hay `.nojekyll`**: si lo añades, la plantilla pasaría a ser pública en `/_template/privacy-policy.html`.

## Publicar el sitio (una sola vez)

La URL `https://oriongames666.github.io` solo funciona si el repo se llama exactamente `oriongames666.github.io` **y pertenece a la cuenta u organización `oriongames666`**. Un repo con ese nombre en otra cuenta (p. ej. tu cuenta personal) se publicaría en `https://<tu-usuario>.github.io/oriongames666.github.io/`, que no es la URL que quieres.

1. En GitHub, crea la organización `oriongames666` (si aún no existe): *Your organizations → New organization* (plan Free).
2. Dentro de la org, crea un repo **público** llamado `oriongames666.github.io`, vacío (sin README, sin .gitignore, sin licencia).
3. Apunta el remoto local a ese repo y sube `main`:
   ```bash
   git remote set-url origin git@github.com:oriongames666/oriongames666.github.io.git
   git add -A
   git commit -m "Sitio inicial con política de privacidad de Dicee"
   git push -u origin main
   ```
   (Si el remoto aún no existe: `git remote add origin ...` en lugar de `set-url`.)
4. En el repo: *Settings → Pages → Build and deployment*:
   - **Source:** *Deploy from a branch*
   - **Branch:** `main` / `/ (root)` → *Save*
5. Espera 1–2 minutos y abre https://oriongames666.github.io/privacy/dicee/. Comprueba que carga por HTTPS y que se ve en el móvil.
6. Pega esa URL en Play Console: *Política y programas → Contenido de la app → Política de privacidad*.

## Añadir una app nueva

Ejemplo para una app llamada «Mi App» con id `com.oriongames666.miapp`. Usa una carpeta corta, en minúsculas y sin espacios (será parte de la URL y **no debería cambiar nunca**, porque está registrada en Play Console).

1. **Crea la carpeta y copia la plantilla:**
   ```bash
   mkdir -p privacy/miapp
   cp _template/privacy-policy.html privacy/miapp/index.html
   ```
2. **Sustituye los marcadores** en `privacy/miapp/index.html` (a mano o con `sed`):
   | Marcador | Ejemplo |
   |---|---|
   | `{{APP_NAME}}` | `Mi App` |
   | `{{APPLICATION_ID}}` | `com.oriongames666.miapp` (el `applicationId` de `android/app/build.gradle`) |
   | `{{FECHA}}` | `25 de noviembre de 2026` |
   | `{{FECHA_EN}}` | `November 25, 2026` |
   | `{{DESCRIPCION_LOCAL}}` | Qué hace la app en el dispositivo, p. ej. «Las partidas se guardan solo en tu dispositivo.» |
   | `{{DESCRIPCION_LOCAL_EN}}` | Lo mismo en inglés |

   ```bash
   sed -i '' \
     -e 's/{{APP_NAME}}/Mi App/g' \
     -e 's/{{APPLICATION_ID}}/com.oriongames666.miapp/g' \
     -e 's/{{FECHA}}/25 de noviembre de 2026/g' \
     -e 's/{{FECHA_EN}}/November 25, 2026/g' \
     privacy/miapp/index.html
   ```
   (`DESCRIPCION_LOCAL` es una frase; suele ser más cómodo editarla a mano.)
3. **Ajusta las secciones según lo que use la app.** La plantilla, tal cual, dice que la app **no recoge datos**. Si usa alguno de estos, descomenta el bloque `[OPCIONAL: ...]` correspondiente **en español y en inglés**:
   - Anuncios (AdMob)
   - Analítica (Google Analytics para Firebase)
   - Informes de errores (Crashlytics)
   - Inicio de sesión / cuentas (incluye cómo pedir el borrado de la cuenta, obligatorio en Play)
   - Cámara
   - Ubicación

   Al activar cualquiera, borra los párrafos marcados `[SOLO SI NO HAY DATOS]` que ya no sean ciertos y cambia el texto de «Privacidad de los niños» como indica el comentario. Rellena los `[EXPLICAR ...]` de cámara/ubicación.

   Por último, **borra los bloques `[OPCIONAL: ...]` que no uses y el comentario de instrucciones del principio del archivo** (el HTML es público y cualquiera puede ver el código fuente).
4. **Comprueba que no quedan marcadores:**
   ```bash
   grep -nE '\{\{|\[EXPLICAR|\[EXPLAIN|\[AJUSTAR|\[ADJUST' privacy/miapp/index.html
   ```
   No debe imprimir nada.
5. **Añádela al índice** `privacy/index.html`, en la lista `<ul>` (orden alfabético):
   ```html
   <li><a href="/privacy/miapp/">Mi App</a> <span class="meta">(<code>com.oriongames666.miapp</code>)</span></li>
   ```
6. **Revisa en local** abriendo `privacy/miapp/index.html` en el navegador.
7. **Publica:**
   ```bash
   git add privacy/miapp privacy/index.html
   git commit -m "Política de privacidad de Mi App"
   git push
   ```
8. Usa `https://oriongames666.github.io/privacy/miapp/` en Play Console, y asegúrate de que lo que dice la política coincide con el formulario **Seguridad de los datos** de esa app.

## Actualizar una política existente

Edita `privacy/<app>/index.html`, cambia la fecha de «Última actualización» / «Last updated» en ambos idiomas, y haz commit + push. La URL no cambia, así que no hay que tocar Play Console (salvo el formulario de Seguridad de los datos si cambian los datos recogidos).

## Contacto

oriongames666@gmail.com
