# TextPad WordPress

Un bloc de notas en texto plano que se ejecuta directamente en el navegador, con funciones de guardado local, descarga y publicación en WordPress.
Esta app nada tiene que ver con el editor de Windows.

## Versión

v1.0
Este software se basa en este repositorio: https://github.com/syndicatefx/textpad/
Se ha respetado el código original de Paulo Nunes todo lo posible y se ha añadido la función de envío del texto escrito a WordPress, como borrador (draft).

## Características

- Edición de texto plano simple.
- Auto-guardado en **LocalStorage** del navegador.
- Descarga de notas como archivo `.txt`.
- **Publicación directa** a WordPress vía AJAX.

## Integración con WordPress

La aplicación permite enviar el contenido de la nota al servidor de WordPress para crear un nuevo post en estado **borrador**.

**Requisitos**:

1. Instalar WordPress y configurar la ruta a `wp-load.php` en `index.php`.
2. El usuario debe **estar logueado** en WordPress (se usa `auth_redirect()`).
3. Uso de **nonce** de WordPress para seguridad (`wp_create_nonce('textpad_create_post')`).
4. Acceso al endpoint AJAX (`admin-ajax.php`) o manejo de la petición POST en `index.php`.

**Flujo de publicación**:

1. El botón **Publish** envía una petición AJAX con los campos:
   - `action=textpad_create_post`
   - `nonce`
   - `post_title` (título)
   - `post_content` (contenido)
   - `category` (slug de categoría)
   - `tags` (etiquetas separadas por comas)
2. El servidor valida el nonce y comprueba `is_user_logged_in()`.
3. Se crea el post con `wp_insert_post()` en estado `draft`, asignando categoría y etiquetas.
4. La respuesta AJAX devuelve el ID del nuevo post o un error.

## Instalación

1. Copia `index.php`, `app.css` y `app.js` en una carpeta de tu servidor.
2. Ajusta la ruta a `wp-load.php` en la parte superior de `index.php`.
3. Asegura que la aplicación solo sea accesible por usuarios autenticados (con `auth_redirect()`).
4. Verifica el registro del callback AJAX con `add_action('wp_ajax_textpad_create_post', ...)` o el manejo de POST en `index.php`.
5. Incluye tu sprite SVG inline en `index.php` (símbolos para iconos).

## Uso

- Abre la página de la app en tu navegador.
- Escribe tu nota.
- Opcionalmente define **File Name**, **Category Slug** y **Tags**.
- Haz clic en **Publish** para crear un post en WordPress.
- Haz clic en **Save** para descargar un `.txt`.
- Haz clic en **Clear** para borrar la nota actual.

## Pruebas recomendadas

> **Recomendación:** Prueba esta aplicación en una **instalación de WordPress de pruebas** antes de usarla en producción. Así podrás validar rutas, permisos, compatibilidad de temas y plugins, y la correcta gestión de nonces y autenticación.

## Autor

**A. Cambronero (Blogpocket)**

