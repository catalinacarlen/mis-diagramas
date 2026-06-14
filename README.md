# quetedenLucid — Mis Diagramas

> Herramienta de diagramación visual en un solo archivo HTML. Un clon de LucidChart, pensado para ir más rápido: cero instalación, cero servidor, control total del dato.

Aplicación cliente (HTML + SVG + JavaScript, sin dependencias) para crear, editar, versionar, presentar, importar y exportar diagramas técnicos y de negocio. Todo vive en un único archivo: se abre con doble clic en cualquier navegador moderno y el documento se guarda como `.json` portable (con autoguardado en el navegador).

---

## ✨ Características

**Modelado y creación**

- Lienzo con **arrastrar y soltar** desde un panel categorizado y buscable.
- Mover, redimensionar, rotar, duplicar, colorear, bloquear y borrar figuras.
- Conectores que siguen a las figuras, con cabeceras configurables (flecha, triángulo, diamante, círculo), estilo de línea (sólida/guiones/punteada), color propio y trazado recto, curvo (Bézier) u ortogonal.
- Selección múltiple (marquee + shift/ctrl), menú contextual, copiar/cortar/pegar, agrupar, ordenar por capas.
- Alinear y distribuir, guías inteligentes (snap a bordes y centros), zoom, paneo y ajustar al contenido.
- Dibujar-para-dimensionar y estilo de borde por figura.

**Bibliotecas de formas**

- **Redes y cloud:** AWS, Azure, GCP y Cisco.
- **UML completo:** clases, casos de uso, secuencias, estados y actividades.
- **ERD:** entidad-relación, con import de esquema SQL → ERD y export ERD → `CREATE TABLE`.
- **Flujo y procesos:** flowchart, BPMN, flujos de usuario, mapas de valor.
- **UI/UX:** wireframes y mockups web y móvil.

(94+ figuras en total, todas con íconos vectoriales propios, sin assets con copyright.)

**Generación automática**

- Organigramas y mapas mentales a partir de texto indentado, con auto-layout sin solapamientos.

**Trazabilidad (control de versiones)**

- Deshacer/rehacer, historial de revisiones con marca de tiempo, comparación entre versiones y restauración a un punto anterior.

**Presentación**

- Modo presentación: convierte secciones del lienzo (frames) en diapositivas navegables a pantalla completa, sin exportar a PowerPoint.

**Persistencia e import/export**

- Autoguardado local, guardar/abrir `.json` validado, export a **PNG** y **SVG**, color de fondo del lienzo y compatibilidad hacia atrás del formato.

---

## 🚀 Uso

No requiere instalación ni dependencias.

```bash
git clone https://github.com/<tu-usuario>/quetedenLucid.git
cd quetedenLucid
```

Luego abrí **`index.html`** con doble clic (o arrastralo al navegador). Listo.

---

## 🧱 Arquitectura

Diseño guiado por **separación de incumbencias**: el motor no conoce las formas concretas, las consume desde un registro; la paleta se genera desde ese mismo registro y reutiliza el render del lienzo.

```
index.html (un solo archivo)
│
├── REGISTRO (SHAPES) ──define──▶ MOTOR
│         │                       · estado serializable
│         │                       · render declarativo
│         │                       · selección / conexión
│         │                       · historial (undo/redo)
│         │                       · persistencia / export
│         ▼
└── PALETA (panel drag & drop) ──consume──▶ motor
```

Sumar una figura nueva = una entrada en el registro `SHAPES`. La especificación completa de requisitos, criterios de aceptación y matriz de trazabilidad está en [`SRS-hoja-de-ruta.md`](SRS-hoja-de-ruta.md).

---

## 🧭 Principios de desarrollo

Rigor y formalidad · separación de incumbencias · modularidad (alta cohesión, bajo acoplamiento) · abstracción · anticipación al cambio · generalización · incrementalidad.

Cada iteración entrega un subconjunto funcional verificado (tests jsdom + muestrario visual) y su reporte (`REPORTE-iteracion-NN.md`).

---

## 📋 Estado del proyecto

SRS original (iteraciones 01–12): **100% cumplido** — 45 requisitos funcionales y no funcionales verificados.

En curso (**Fase 2**, mejoras de calidad y experiencia, iteraciones 13–16): edición de texto in-place, compartimentos en figuras estructuradas, íconos SVG, paletas guardables, badges de estado y quick-connect.

---

## 📁 Estructura del repo

| Archivo | Contenido |
|---|---|
| `index.html` | La aplicación completa. |
| `SRS-hoja-de-ruta.md` | Especificación de requisitos, roadmap y matriz de trazabilidad. |
| `REPORTE-iteracion-NN.md` | Reporte de cada iteración (qué se hizo, verificación, próximos pasos). |
| `muestrario-*.png` | Capturas de verificación visual por iteración. |

---

## 🔒 Alcance

Fuera de alcance por ahora: colaboración en tiempo real, backend/cuentas, integraciones externas (Jira, Confluence) y export nativo a `.pptx`/`.docx`.

---

Hecho por Cata · proyecto de uso propio.
