# 🐸 SapoGritón

**Un rastreador SEO de código abierto con la estética de Screaming Frog.**

SapoGritón es un fork de [LibreCrawl](https://github.com/PhialsBasement/LibreCrawl) que mantiene todo su motor de rastreo y le cambia la cara: una interfaz clara, densa y de aspecto "escritorio", pensada para quien está acostumbrado a trabajar con tablas largas y quiere ver muchos datos a la vez.

![Licencia MIT](https://img.shields.io/badge/licencia-MIT-green.svg)
![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue.svg)

<!-- Sustituye esta imagen por una captura real de tu instalación -->
![Captura de SapoGritón](docs/screenshot.png)

---

## ¿Qué es esto?

[LibreCrawl](https://github.com/PhialsBasement/LibreCrawl) es una aplicación web (Python + Flask) que rastrea sitios y extrae información SEO: títulos, metadescripciones, encabezados, enlaces, códigos de estado, rendimiento, etc. Funciona muy bien, pero su interfaz original es un tema oscuro con degradados morados.

**SapoGritón conserva el funcionamiento y cambia la presentación.** El objetivo es que la herramienta se sienta como un SEO spider de escritorio clásico.

### Qué cambia respecto al original

| | LibreCrawl | SapoGritón |
|---|---|---|
| Tema | Oscuro, degradados morados | Claro, gris de escritorio con acento verde |
| Tipografía | Inter, 14 px | Segoe UI / Tahoma, 12 px |
| Cabecera | Botones grandes con color | Barra tipo menú, botones planos |
| Barra de URL | Campo grande | Barra compacta con **Start** en verde |
| Panel izquierdo | Tarjetas | Árbol de filtros con contadores alineados |
| Pestañas | Subrayadas | Pestañas clásicas, la activa con borde verde |
| Tablas | Filas amplias con cebra | Rejilla densa, cabecera fija, fila seleccionada en azul |
| Códigos de estado | Insignias de color | Texto coloreado (200 verde, 3xx naranja, 4xx/5xx rojo) |
| Diálogos | Tarjetas redondeadas oscuras | Ventanas planas con barra de título gris |
| Scrollbars | Moradas | Estilo escritorio |

El cambio visual está aislado en **un solo archivo**, [`web/static/css/sf-theme.css`](web/static/css/sf-theme.css), que se carga después de la hoja de estilos original. No se modificó la lógica del rastreador.

> La interfaz sigue en inglés, igual que el proyecto original.

---

## Funciones

Todo lo siguiente viene heredado de LibreCrawl:

- 🚀 **Multiusuario**: varios usuarios pueden rastrear a la vez con sesiones aisladas.
- 🔄 **Renderizado de JavaScript** para contenido dinámico (React, Vue, Angular…) mediante Playwright.
- 📊 **Análisis SEO**: títulos, metadescripciones, encabezados y más.
- 🔗 **Análisis de enlaces** internos y externos con mapa de relaciones.
- 🔍 **Detección automática de incidencias SEO**.
- 📈 **PageSpeed Insights** y Core Web Vitals.
- 💾 **Exportación** a CSV, Excel (XLSX), JSON o XML.
- ⚡ **Progreso y estadísticas en tiempo real**.
- 🎨 **CSS personalizado** desde los ajustes, por navegador.
- 🔌 **Sistema de plugins** (ver más abajo).

---

## Inicio rápido

### Con el script de arranque (lo más fácil)

**Windows**

```batch
start-librecrawl.bat
```

**Linux / macOS**

```bash
chmod +x start-librecrawl.sh
./start-librecrawl.sh
```

El script usa Docker si está instalado; si no, instala las dependencias de Python y arranca en modo local. Después abre `http://localhost:5000`.

> Los scripts conservan su nombre original (`start-librecrawl.*`) para que sigan funcionando igual que en el proyecto base.

### Docker

```bash
git clone https://github.com/TU_USUARIO/SapoGriton.git
cd SapoGriton

cp .env.example .env
docker compose up -d
```

Abre `http://localhost:5000`.

### Python (instalación manual)

Requisitos: Python 3.8 o superior y un navegador moderno.

```bash
git clone https://github.com/TU_USUARIO/SapoGriton.git
cd SapoGriton

pip install -r requirements.txt

# Opcional, para el renderizado de JavaScript
playwright install chromium

# Modo local: sin autenticación, sin límites
python main.py --local

# Modo estándar: con autenticación y sistema de niveles
python main.py
```

---

## Configuración

El archivo `.env` controla el modo de ejecución.

**Uso personal (por defecto):**

```bash
LOCAL_MODE=true
HOST_BINDING=127.0.0.1
REGISTRATION_DISABLED=false
```

**Despliegue con usuarios y autenticación:**

```bash
LOCAL_MODE=false
HOST_BINDING=0.0.0.0
REGISTRATION_DISABLED=false
# Genera una clave con: python -c "import secrets; print(secrets.token_hex(32))"
SECRET_KEY=pon-aqui-una-cadena-larga-y-aleatoria
```

| Modo | Comportamiento |
|---|---|
| **Local** (`--local`) | Todos los usuarios tienen nivel admin, sin límites. Ideal para uso personal. |
| **Estándar** | Login y registro, niveles de acceso (Guest, User, Extra, Admin). Los invitados tienen un límite de 3 rastreos cada 24 horas por IP. |

Desde la propia interfaz (**Settings**) puedes ajustar profundidad y velocidad del rastreo, user agent, proxy, robots.txt, renderizado de JavaScript, filtros de URL, formatos de exportación y exclusión de incidencias.

---

## Plugins

Los plugins del proyecto original funcionan sin cambios. Cada archivo `.js` que pongas en `web/static/plugins/` crea una pestaña nueva en la interfaz.

La API conserva su nombre original, `LibreCrawlPlugin`, para no romper plugins existentes:

```javascript
LibreCrawlPlugin.register({
    id: 'mi-plugin',
    name: 'Mi plugin',
    // ...
});
```

Consulta [`web/static/plugins/README.md`](web/static/plugins/README.md) para la referencia completa.

---

## Volver al tema original

Si prefieres la apariencia de LibreCrawl, quita esta línea de `web/templates/index.html`:

```html
<link rel="stylesheet" href="/static/css/sf-theme.css">
```

---

## Mantener el fork al día con LibreCrawl

Como los cambios están concentrados, es fácil incorporar las novedades del proyecto original:

```bash
git remote add upstream https://github.com/PhialsBasement/LibreCrawl.git
git fetch upstream
git merge upstream/main
```

Los únicos conflictos esperables estarán en los textos donde se cambió el nombre de la marca.

---

## Aviso

SapoGritón es un proyecto independiente de la comunidad. **No está afiliado, patrocinado ni respaldado por Screaming Frog Ltd.** "Screaming Frog" y "SEO Spider" son marcas de sus respectivos propietarios y se mencionan aquí únicamente para describir la inspiración estética de la interfaz.

La hoja de estilos se escribió desde cero: este repositorio no incluye código, iconos, logotipos ni otros recursos de Screaming Frog.

---

## Créditos y licencia

- Motor de rastreo y aplicación base: **[LibreCrawl](https://github.com/PhialsBasement/LibreCrawl)**, de Phiality y colaboradores. Si te resulta útil, considera apoyar el proyecto original.
- Rediseño visual y rebautizado: SapoGritón.

Distribuido bajo la licencia **MIT**, la misma del proyecto original. Se conserva el aviso de copyright original en [`LICENSE`](LICENSE), como exige la licencia.
