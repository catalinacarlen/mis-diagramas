# Mis Diagramas

> Herramienta de diagramación visual en un único archivo HTML: un editor tipo LucidChart pensado para trabajar rápido, sin instalación, sin servidor y con control total del dato.

Aplicación cliente (HTML + SVG + JavaScript, sin dependencias) para crear, editar, versionar, presentar, importar y exportar diagramas técnicos y de negocio. Todo reside en un único archivo: se abre con doble clic en cualquier navegador moderno y el documento se guarda como `.json` portable, con autoguardado en el navegador.

**Demo en vivo:** https://catalinacarlen.github.io/mis-diagramas/

**Estado:** versión **1.0** — completa, auditada y publicada.

---

## Características

**Modelado y creación**

- Lienzo con arrastrar y soltar desde un panel categorizado y buscable; también dibujar-para-dimensionar.
- Mover, redimensionar (incluso rotada), rotar, duplicar, colorear, bloquear, alinear/distribuir y borrar figuras, con guías inteligentes de alineación (snap a bordes y centros).
- Edición de texto in-place sobre la figura y, en figuras estructuradas (Clase/Entidad), edición por compartimentos.
- Conectores que siguen a las figuras, con cabeceras configurables por extremo (flecha, triángulo, diamante, círculo), estilo de línea (sólida, guiones, punteada), color propio y trazado recto, curvo (Bézier) u ortogonal.
- Selección múltiple, menú contextual, copiar/cortar/pegar, agrupar, ordenar por capas, badges de estado y *quick-connect* (crear una figura ya conectada arrastrando desde el borde).

**Bibliotecas de formas** (94+ figuras, con íconos vectoriales propios, sin recursos con derechos de autor)

- Redes y cloud: AWS, Azure, GCP y Cisco.
- UML: clases, casos de uso, secuencias, estados y actividades.
- ERD: entidad-relación, con importación de esquema SQL → ERD y exportación ERD → `CREATE TABLE`.
- Flujo y procesos: diagramas de flujo y BPMN.
- UI/UX: wireframes y mockups web y móvil.

**Generación automática.** Organigramas y mapas mentales a partir de texto indentado, con disposición automática sin solapamientos.

**Trazabilidad.** Deshacer/rehacer, historial de revisiones con marca de tiempo, comparación entre versiones y restauración a un punto anterior.

**Presentación.** Conversión de secciones del lienzo (frames) en diapositivas navegables a pantalla completa, sin exportar a PowerPoint.

**Persistencia e import/export.** Autoguardado local, guardar/abrir `.json` validado, exportación a PNG y SVG, color de fondo del lienzo y compatibilidad hacia atrás del formato.

---

## Uso

No requiere instalación ni dependencias.

- En línea: https://catalinacarlen.github.io/mis-diagramas/
- En local:

```bash
git clone https://github.com/catalinacarlen/mis-diagramas.git
cd mis-diagramas
```

Luego abrir `index.html` con doble clic (o arrastrarlo al navegador).

---

## Arquitectura

El diseño se apoya en la separación de incumbencias: el motor no conoce las formas concretas, sino que las consume desde un registro; la paleta se genera a partir de ese mismo registro y reutiliza el render del lienzo.

```
index.html (un único archivo)
│
├── REGISTRO (SHAPES) ──define──▶ MOTOR
│         │                       · estado serializable
│         │                       · render declarativo
│         │                       · selección / conexión
│         │                       · historial (undo/redo)
│         │                       · persistencia / exportación
│         ▼
└── PALETA (panel drag & drop) ──consume──▶ motor
```

Sumar una figura nueva equivale a agregar una entrada en el registro `SHAPES`. La especificación de requisitos, los criterios de aceptación y la matriz de trazabilidad están en [`SRS-hoja-de-ruta.md`](SRS-hoja-de-ruta.md).

---

## Principios de desarrollo

El proyecto se rige por los **principios de ingeniería de software** (en la línea de Ghezzi, Jazayeri y Mandrioli, *Fundamentals of Software Engineering*):

- **Rigor y formalidad:** requisitos numerados con criterios de aceptación medibles y verificación en cada entrega.
- **Separación de incumbencias:** motor, registro de formas e interfaz desacoplados.
- **Modularidad:** alta cohesión y bajo acoplamiento; cada figura es una entrada autónoma del registro.
- **Abstracción:** identificar lo esencial e ignorar lo accesorio (los previews del panel reutilizan el render real).
- **Anticipación al cambio:** formato de documento versionado e historial de revisiones.
- **Generalización:** resolver el problema general (un mismo parser alimenta organigramas, mapas mentales y, a futuro, SQL → ERD).
- **Incrementalidad:** cada iteración entrega un subconjunto funcional verificado (pruebas con jsdom + muestrario visual) y su reporte (`REPORTE-iteracion-NN.md`).

---

## Estado del proyecto

**Versión 1.0 — cerrada.**

- SRS original (iteraciones 01–12): **100% cumplido** (45 requisitos funcionales y no funcionales).
- Fase 2 — calidad y experiencia (iteraciones 13–16): **completa** (10 requisitos: edición in-place, compartimentos en figuras estructuradas, dibujar-para-dimensionar, estilo de borde, íconos SVG, drag&drop visual, paletas de color, badges de estado y *quick-connect*).
- **Auditoría final aprobada:** verificación funcional integral y comprobaciones de seguridad (sin XSS, sin ejecución dinámica ni acceso a red, saneamiento de la entrada). Detalle en [`REPORTE-auditoria-v1.md`](REPORTE-auditoria-v1.md).

**Backlog (Fase 3, opcional).** Pruebas en navegador real a gran escala, ruteo ortogonal que esquive figuras, codos de conector arrastrables, carriles BPMN múltiples, exportación a PDF, autoguardado al archivo en disco, paleta de comandos y múltiples páginas por documento.

---

## Estructura del repositorio

| Archivo | Contenido |
|---|---|
| `index.html` | La aplicación completa. |
| `SRS-hoja-de-ruta.md` | Especificación de requisitos, hoja de ruta y matriz de trazabilidad. |
| `REPORTE-iteracion-NN.md` | Reporte de cada iteración (qué se hizo, verificación, próximos pasos). |
| `REPORTE-auditoria-v1.md` | Auditoría funcional y de seguridad de la versión 1.0. |
| `muestrario-*.png` | Capturas de verificación visual por iteración. |
| `mis-diagramas-v0-prototipo.html` | Prototipo inicial, conservado como referencia. |

---

## Alcance

Fuera de alcance en esta versión: colaboración en tiempo real, backend y cuentas de usuario, integraciones externas (Jira, Confluence) y exportación nativa a `.pptx` / `.docx`.

---

Desarrollado por Cata · proyecto de uso propio.
