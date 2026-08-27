<div align="center">

# 🔎 RASTRO-GH

### Rastreo OSINT de cuentas de GitHub → emails → pivote de identidad

`sin backend` · `sin API keys` · `100% navegador` · `un solo fichero HTML`

![status](https://img.shields.io/badge/estado-activo-4a9d8f?style=flat-square)
![stack](https://img.shields.io/badge/stack-HTML%20%2B%20JS%20vanilla-1a1a16?style=flat-square)
![privacy](https://img.shields.io/badge/datos-solo%20local-d99a3d?style=flat-square)
![license](https://img.shields.io/badge/uso-investigaci%C3%B3n%20autorizada-b8482f?style=flat-square)

</div>

---
## Enlace a la herramienta: https://s3gad3.github.io/rastro-gh/
## ¿Qué es esto?

**RASTRO-GH** parte de un dato mínimo —un **nombre de usuario de GitHub**— y reconstruye alrededor de él una pequeña ficha de inteligencia: quién es la cuenta, qué correos ha dejado expuestos sin darse cuenta, y por dónde seguir tirando del hilo.

Todo corre **en tu navegador**. No hay servidor, no hay base de datos, no se guarda nada: cada consulta se hace en el momento, directamente contra la API pública de GitHub y contra los servicios de terceros a los que decides pivotar.

> Pensado para trabajo de **ciberinteligencia / OSINT**: una primera pasada rápida sobre un alias antes de decidir si merece una investigación más profunda.

---

## 🕵️ Qué puedes investigar con esto

**1. De username a persona**
La ficha de perfil no se queda en el avatar: empresa, ubicación, web/blog, cuenta de X, fecha de creación de la cuenta (útil para detectar cuentas recién creadas / desechables), última actividad, seguidores, repos, gists, y si la cuenta es de tipo `Organization` o pertenece a *staff* de GitHub — señales todas ellas relevantes para valorar la fiabilidad del perfil.

**2. De username a email real**
La mayoría de la gente no sabe que **GitHub filtra su email real en el historial de commits**, aunque lo oculte en el perfil. RASTRO-GH rastrea tres fuentes en cascada:
- el email público del perfil,
- los eventos públicos recientes (`push` a repos),
- los commits de sus repositorios más activos.

Cada email hallado se etiqueta con **de dónde salió** (qué repo, qué evento), y se distingue automáticamente el correo real del típico `@users.noreply.github.com`, que no sirve para pivotar.

**3. De email a identidad real**
Sobre cada email encontrado se abre un panel de **pivote OSINT** con un clic:

| Pivote | Para qué sirve |
|---|---|
| Google / DuckDuckGo | Buscar el email citado literalmente en la web indexada |
| GitHub código / commits | Ver en qué otros repos aparece ese mismo email |
| **Gravatar** | Se calcula el hash MD5 del email **en tu propio navegador** y se comprueba si existe una foto de perfil pública asociada — a veces revela nombre real y cara aunque la cuenta de GitHub sea anónima |
| Hunter.io | Verificación del formato/existencia del email |
| Epieos / Have I Been Pwned | Reverse lookup y brechas de datos conocidas |

Si Gravatar encuentra imagen, aparece **la miniatura al instante**, con opción de **verla en grande y descargarla** directamente.

**4. De username a huella en el resto de internet**
Aunque la cuenta exista en GitHub, casi nadie usa el mismo alias en un solo sitio. Por eso, además, se generan **enlaces directos al mismo username** en más de 30 plataformas —desarrollo, redes sociales, contenido y comunidades— agrupadas y listas para abrir en pestaña nueva y comprobar a golpe de vista si el alias está reutilizado en otro lugar.

---

## ⚙️ Cómo funciona por dentro

```
usuario ──▶ [1] Perfil público  ──▶ ficha + email de perfil (si lo hay)
        ──▶ [2] Eventos públicos ──▶ commits recientes vía PushEvent
        ──▶ [3] Repos + commits ──▶ commits de los 5 repos más activos (no forks)
                    │
                    ▼
            emails únicos deduplicados, con fuente de cada uno
                    │
                    ▼
        pivote OSINT (email) + pivote multiplataforma (username)
```

Solo usa `fetch()` contra `api.github.com` (endpoints públicos, sin autenticación) y enlaces de salida a servicios externos que el propio investigador decide abrir. No hay scraping automatizado de terceros: el navegador no permite comprobar por su cuenta si una cuenta existe en Instagram, LinkedIn, etc. — por eso esos pivotes son enlaces manuales, no resultados automáticos.

---

## 🚀 Uso

1. Descarga `index.html`.
2. Ábrelo con cualquier navegador (doble clic, sin instalar nada).
3. Escribe un username de GitHub y pulsa **Buscar**.
4. Explora la ficha, despliega los emails encontrados y pulsa **Pivotar** para investigar cada uno.

No requiere token de GitHub, pero si vas a hacer muchas consultas seguidas, el *rate limit* público (60 req/hora sin autenticar) puede saltarte — se muestra siempre abajo a la derecha cuánto te queda.

---

## ⚠️ Uso responsable

Esta herramienta solo consulta **información pública**: lo que cualquier persona puede ver visitando GitHub.com, y enlaces a buscadores/servicios públicos de terceros. No hace *scraping* masivo, no evade autenticaciones ni límites, y no automatiza consultas a plataformas que no lo permiten.

Está pensada para **investigación autorizada** (ciberinteligencia, respuesta a incidentes, verificación de identidad en el marco de una investigación). El uso que se le dé a la información obtenida es responsabilidad de quien la utiliza.

---

## 🛠️ Stack

Un único archivo `index.html`. Sin dependencias, sin `npm install`, sin build:

- HTML + CSS puro
- JavaScript vanilla (incluye una implementación propia de MD5 para el pivote Gravatar, sin librerías externas)
- `fetch()` nativo contra la API REST de GitHub

---

<div align="center">

*Parte del kit de herramientas OSINT internas · uso interno autorizado*

</div>
