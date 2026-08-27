# RASTRO-GH

**OSINT S3GAD3** · Herramienta de localización de direcciones de correo asociadas a una cuenta de GitHub, usando únicamente endpoints públicos de la API de GitHub.

🔗 **Demo:** [https://s3gad3.github.io/rastro-gh/](https://s3gad3.github.io/rastro-gh/)

---

## ¿Qué hace?

Dado un nombre de usuario de GitHub, la herramienta busca y correla direcciones de correo electrónico visibles públicamente a través de tres fuentes:

1. **Perfil** — el campo `email` público del perfil, si el usuario lo ha configurado.
2. **Eventos públicos** — commits incluidos en eventos de tipo `PushEvent` de los últimos ~90 días.
3. **Commits en repositorios** — commits del propio autor en sus repositorios públicos no bifurcados (no forks).

Para cada correo encontrado se indica de qué fuente(s) procede, y se marcan por separado las direcciones de tipo `noreply.github.com` / `users.noreply.github.com`, que no son vinculables directamente a una persona.

## Cómo usarla

1. Abre la página.
2. Escribe un nombre de usuario de GitHub.
3. Pulsa **Buscar**.
4. Consulta los resultados junto con su fuente y el rate limit restante de la API.

No requiere instalación ni backend: es un único archivo HTML autocontenido que corre enteramente en el navegador.

## Límites de la API

Sin autenticación, la API de GitHub permite **60 peticiones/hora por IP**. Cada búsqueda consume varias peticiones (perfil + páginas de eventos + commits por repo), así que ese límite se agota rápido con un uso intensivo.

Si necesitas más volumen, se puede añadir un campo para introducir un **Personal Access Token** (sube el límite a 5000/h). No está incluido en esta versión — pídelo como mejora si hace falta.

## Stack

- HTML + CSS + JavaScript vanilla, sin dependencias ni build.
- Fuentes: IBM Plex Mono / IBM Plex Sans (Google Fonts).
- Consume directamente `api.github.com` mediante `fetch()`.

## Alcance y limitaciones

- Solo información **pública**: no accede a datos privados ni requiere autenticación por defecto.
- Los eventos públicos (`/events/public`) cubren únicamente los últimos ~90 días de actividad.
- Un usuario sin actividad reciente, sin email en el perfil y sin commits accesibles no arrojará resultados.
- Las direcciones `noreply` de GitHub no identifican a una persona real.

## Uso responsable

Esta herramienta consulta exclusivamente información que el propio usuario de GitHub ha hecho pública (perfil, eventos, historial de commits). Está pensada como apoyo a tareas de investigación OSINT. El uso que se le dé es responsabilidad de quien la ejecuta.

## Licencia

Este proyecto se distribuye bajo la licencia MIT.
