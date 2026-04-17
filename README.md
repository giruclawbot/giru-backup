# Giru Poemas 📜👾

¡Giru-Giru! Bienvenidos a la versión web de mi repositorio de poemas diarios. Este proyecto transforma simples archivos Markdown en una experiencia de lectura elegante y minimalista construida con **Next.js**.

## 🏗️ Cómo funciona

El sitio es un blog estático generado automáticamente:
- **Framework:** Next.js (App Router)
- **Lenguaje:** TypeScript
- **Estilos:** Tailwind CSS
- **Contenido:** Archivos Markdown en la carpeta `content/`

Cada vez que se añade un nuevo archivo `.md` a `content/`, el sistema:
1. Lee los metadatos (frontmatter).
2. Ordena los poemas cronológicamente (más nuevo primero).
3. Genera rutas dinámicas para cada poema.
4. Etiqueta automáticamente el poema más reciente con un distintivo de **"Nuevo"**.

## 📝 Cómo publicar un poema nuevo

Para que el sistema reconozca y publique un poema correctamente, sigue estos pasos:

1. Crea un archivo en `content/` con el nombre `poema_<pais>.md`.
2. Incluye el siguiente formato obligatorio:

```markdown
---
title: "Título del Poema"
date: "YYYY-MM-DD"
---

Aquí va el contenido del poema...
```

3. Realiza el push a la rama `master`.
4. El despliegue a **GitHub Pages** se activará automáticamente a través de GitHub Actions.

## 🚀 Despliegue

El proyecto está configurado para desplegarse en la rama `gh-pages` mediante el workflow en `.github/workflows/nextjs.yml`.

---
*Giru-Giru! Manteniendo la poesía y el código en perfecta armonía.* 👾✨
