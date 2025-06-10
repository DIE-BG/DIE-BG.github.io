# Cómo crear y modificar el sitio web del equipo?

## Conocimiento básicos

### Referencias de documentación

- [Overview](https://quarto.org/docs/websites/#overview): Guía rápida para crear un sitio web.
- [Publicación en GH-Pages](https://quarto.org/docs/publishing/github-pages.html)
- [Documentation](https://quarto.org/docs/reference/projects/websites.html)

Usar la extensión de VSCode para inicializar un sitio web.

### Crear el repositorio del sitio web

- Crear un repositorio en Github con dos ramas independientes.
  - `main`: Contiene los archivos de Quarto.
  - `gh-pages`: Exclusiva para publicación.

```bash
# Clone the repo first and switch to main branch.
git checkout --orphan gh-pages # creates the new branch
git reset --hard # make sure all changes are committed before running this!
git commit --allow-empty -m "Initialising gh-pages branch"
git push origin gh-pages
```

### Verificar la rama de publicación en GitHub

Esta configuración se encuentre en el repositorio del sitio web en GitHub.
Verificar que la rama de publicación es `gh-pages`.

### Publicación del sitio web

En la rama donde se están modificando los archivos de Quarto, commit a los cambios que se desean publicar.

En la terminal ejecutar:

```bash
quarto publish gh-pages
```

### Cómo acceder al sitio web?

```
repositorio -> sidebar derecho -> Deployments -> link al sitio
```

### Configuración del sitio web

En el archivo `_quato.yml` está la configuración de los diferentes elementos del sitio web, incluyendo formato, contenido, y comportamiento.

```yaml
project:
  type: website

website:
  title: "DIE-BG"
  navbar:
    pinned: true # siempre mostrar el Navbar
    tools: # íconos con links a otros sitios web
      - icon: github
        menu:
          - text: Organization
            href: https://github.com/DIE-BG
    left: # en esta parte empieza a configurarse el contenido del navbar 
      - text: Home # nombre que se muestra en el navbar
        href: index.qmd # dirección al archivo
      - sidebar:hefm # sidebar que solo se verá al ingresar a esa pestaña, identificado con el id

  sidebar: # a partir de aquí se configurar cada uno de los sidebars
    - id: hefm # id para referenciarlo al navbar
      title: "HEFM"
      style: "docked" # siempre visible
      search: true
      contents: 
        - href: hefm/index.qmd
          text: "HEFM"
        - section: "Results"
          contents:
            - section: "SVAR50_4B"
              contents:
                - text: "Index"
                  href: hefm/results/SVAR50_4B/index.qmd
                - text: "Variable Final Estimation Date"
                  href: hefm/results/SVAR50_4B/SVAR50_4B_forecast_OS_finalEstPeriod/index.qmd
                - text: "Variable Initial Estimation Date (IS)"
                  href: hefm/results/SVAR50_4B/SVAR50_forecast_IS_initialEstPeriod/index.qmd
                - text: "Variable Initial Estimation Date (OS)"
                  href: hefm/results/SVAR50_4B/SVAR50_forecast_OS_initialEstPeriod/index.qmd
                - text: "Variable Sample Size"
                  href: hefm/results/SVAR50_4B/SVAR50_4B_HV/index.qmd

format: # estilo y diseño
  html:
    theme:
      - cosmo
      - brand
    css: assets/css/styles.css
    toc: true
```

### Cómo embeber presentaciones?

Esto se hace dentro de un archivo de quarto

```markdown

<!-- En un archivo de Quarto es posible incluir metas o variables. -->
---
frame_height: "400px"
frame_width: "100%"

SVAR50_4B: 
    dir: "/hefm/results/SVAR50_4B"
    name: "SVAR50-4B"
    decks: 
        - dir: "SVAR50_4B_forecast_OS_finalEstPeriod"
          filename: "SVAR50_4B_forecast_OS_finalEstPeriod"
          title: "Variable Final Estimation Date"
---

# {{< meta SVAR50_4B.decks.1.title >}}

<!-- link para hacer fullscreen. Es un link normal de markdown -->
[**Fullscreen ⛶**]({{< meta SVAR50_4B.dir >}}/{{< meta SVAR50_4B.decks.1.dir >}}/{{< meta SVAR50_4B.decks.1.filename >}}.qmd)


<!-- iframe para agregar el reveal.js y que sea interactivo -->

` ``{=html}
<iframe 
    src="{{< meta SVAR50_4B.dir >}}/{{< meta SVAR50_4B.decks.1.dir >}}/{{< meta SVAR50_4B.decks.1.filename >}}.html"
    height={{< meta frame_height >}}
    width={{< meta frame_width >}}
    style="border: none;"
></iframe>
` ``
```