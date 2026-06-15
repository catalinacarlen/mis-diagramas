# SRS & Hoja de Ruta — quetedenLucid

**Documento de Especificación de Requisitos de Software (SRS) y plan incremental**
**Producto:** quetedenLucid — herramienta de diagramación (clon mejorado de LucidChart, uso propio)
**Versión del documento:** 1.0
**Fecha:** 12/06/2026
**Estado:** vivo (se actualiza al cerrar cada iteración)

> Propósito: este documento es la **fuente única de verdad** de qué tiene que hacer la app y cómo lo verificamos. Cada requisito tiene un ID y un criterio de aceptación medible. La **§9 Matriz de trazabilidad** es la checklist para confirmar que se cumplen todos los requerimientos.

---

## 1. Introducción

### 1.1 Objetivo
Construir una herramienta de modelado visual de diagramas que iguale las funciones centrales de LucidChart y las supere en lo que importe para uso propio: rapidez, cero fricción (un solo archivo, sin instalación) y control total del dato.

### 1.2 Alcance
Aplicación cliente (HTML/SVG/JS) que permite crear, editar, versionar, presentar, importar y exportar diagramas técnicos y de negocio. Sin backend obligatorio: el documento se guarda como archivo `.json` portable y se autoguarda en el navegador.

### 1.3 Definiciones
- **Forma / figura:** elemento gráfico del lienzo (nodo).
- **Conector:** vínculo dirigido entre dos formas.
- **Registro de formas (`SHAPES`):** objeto que define todas las figuras disponibles.
- **Figura estructurada:** figura con compartimentos de texto (Clase UML, Entidad ERD).
- **Frame:** región del lienzo marcada para usarse como diapositiva en modo presentación.
- **Documento:** estado completo del diagrama (formas + conectores + metadatos).

### 1.4 Usuaria y contexto
Usuaria única (estudiante de ciberseguridad). Casos de uso: arquitecturas de red/cloud, modelado UML/ERD para cursada, procesos BPMN, wireframes rápidos, organigramas y mapas mentales.

---

## 2. Principios de desarrollo (rectores del diseño)

Tomados de la consigna del proyecto; cada decisión técnica debe poder justificarse contra estos:

1. **Rigor y formalidad** — complemento a la creatividad.
2. **Separación de incumbencias** — el motor no conoce las formas; las formas no conocen la UI.
3. **Modularidad** — alta cohesión, bajo acoplamiento. Sumar una figura = una entrada en el registro.
4. **Abstracción** — identificar lo importante e ignorar lo irrelevante (los previews reutilizan el render real).
5. **Anticipación al cambio** — control de versiones y formato de documento versionado (`version`).
6. **Generalización** — resolver el problema general (un parser texto→diagrama sirve para organigrama, mapa mental y futuro SQL→ERD).
7. **Incrementalidad** — entregar subconjuntos funcionales y reportar cada iteración.

---

## 3. Requisitos funcionales (RF)

> Convención de prioridad (MoSCoW): **M** = imprescindible, **S** = debería, **C** = podría.
> Estado: 🟢 hecho · 🟡 parcial · 🔴 pendiente.

### RF-1 Modelado y creación de diagramas (arrastrar y soltar)

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-1.1 | Crear formas en el lienzo mediante **arrastrar y soltar** desde un panel categorizado. | M | 🟢 |
| RF-1.2 | Mover, redimensionar, duplicar, borrar y dar color a las formas. | M | 🟢 |
| RF-1.3 | Conectar formas con flechas; los conectores siguen a las formas al moverlas. | M | 🟢 |
| RF-1.4 | Editar texto de formas y conectores (inline). | M | 🟢 |
| RF-1.5 | Buscador de formas y categorías colapsables en el panel. | S | 🟢 |
| RF-1.6 | Zoom, paneo y "ajustar al contenido". | M | 🟢 |
| RF-1.7 | **Selección múltiple:** cuadro (marquee), shift/ctrl-clic, mover en bloque. | M | 🟢 |
| RF-1.8 | **Menú contextual** (clic derecho): duplicar, ordenar por capas, agrupar/desagrupar, borrar. | M | 🟢 |
| RF-1.9 | **Copiar / Cortar / Pegar** y Seleccionar todo. | S | 🟢 |
| RF-1.10 | **Bloquear / desbloquear** elementos (no mover/editar/borrar). | M | 🟢 |
| RF-1.11 | **Conectores configurables:** cabecera por extremo (flecha/triángulo/diamante/círculo/ninguna) y estilo de línea (sólida/guiones/punteada). | M | 🟢 |
| RF-1.12 | **Rotar figuras** (tirador, pasos de 15°, 90°, persistencia). | S | 🟢 |
| RF-1.13 | **Trazado de conectores:** recto, curvo (Bézier) y ortogonal (codos). | S | 🟢 |
| RF-1.16 | **Guías inteligentes (snap)** al arrastrar: alinea a bordes/centros con líneas guía. | S | 🟢 |
| RF-1.17 | **No editar texto** en figuras sin texto (Barra nav, Imagen, etc.). | S | 🟢 |
| RF-1.18 | **Estilo de borde de figura** (sólido / guiones / punteado), como los conectores. | S | 🟢 |
| RF-1.19 | **Dibujar para dimensionar:** trazar la figura en el lienzo en vez de aparecer en tamaño fijo. | S | 🟢 |
| RF-1.20 | **Edición de texto in-place:** escribir sobre la figura viendo el resultado (sin cuadro que la tape). | S | 🟢 |
| RF-1.21 | **Edición por compartimentos** en figuras estructuradas (ERD/Clase): título y filas por separado. | S | 🟢 |
| RF-1.22 | **Drag&drop visual:** figura fantasma siguiendo el cursor + indicador de caída. | C | 🟢 |
| RF-1.23 | **Íconos SVG** (no emojis) junto a las opciones de barra y panel. | C | 🟢 |
| RF-1.24 | **Paletas de colores guardables** y reutilizables. | C | 🟢 |
| RF-1.25 | **Badges / íconos de estado** sobre las figuras (semáforo, check, alerta…). | C | 🟢 |
| RF-1.26 | **Quick-connect:** flechas al pasar el mouse para crear una figura ya conectada. | C | 🟢 |
| RF-1.14 | **Alinear y distribuir** figuras (6 alineaciones + distribuir H/V). | S | 🟢 |
| RF-1.15 | **Color propio de conector** (línea y cabeceras). | S | 🟢 |

**Bibliotecas de formas (RF-1.A … RF-1.E):**

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-1.A | **Redes y sistemas:** topologías de red, infra cloud y flujos de datos, con bibliotecas para **AWS, Azure, GCP y Cisco**. | M | 🟢 |
| RF-1.B | **UML completo:** clases, casos de uso, **secuencias**, estados y **actividades**. | M | 🟢 |
| RF-1.C | **ERD:** diagramas entidad-relación. | M | 🟢 |
| RF-1.C.1 | **Importar esquema SQL** (SQL/Oracle/PostgreSQL) → generar ERD automáticamente. | S | 🟢 |
| RF-1.C.2 | **Exportar ERD → código SQL** (`CREATE TABLE`). | S | 🟢 |
| RF-1.D | **Flujo y procesos:** flowchart, **BPMN**, flujos de usuario, mapas de valor. | M | 🟢 |
| RF-1.E | **UI/UX:** wireframes y mockups web y móvil. | S | 🟢 |

**Criterios de aceptación (RF-1):**
- AC-1.1: arrastrar cualquier figura del panel y soltarla en el lienzo la crea en esa posición.
- AC-1.A: existen categorías AWS, Azure, GCP y Cisco con figuras reconocibles; cada una se puede colocar, mover y conectar.
- AC-1.C.1: dado un script `CREATE TABLE`, la app genera entidades con sus atributos y las relaciones por claves foráneas.
- AC-1.C.2: dado un ERD, la app produce un `.sql` válido que recrea las tablas.

### RF-2 Generación automática

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-2.1 | Generar **organigramas** a partir de texto (listas indentadas) o datos importados. | M | 🟢 (texto) |
| RF-2.2 | Generar **mapas mentales** a partir de texto. | M | 🟢 |
| RF-2.3 | Auto-layout: distribuir los nodos generados sin solapamientos. | M | 🟢 |

**Criterio de aceptación (RF-2):** pegar un texto indentado produce un diagrama jerárquico legible, conectado y sin solapamientos, editable como cualquier otro.

### RF-3 Trazabilidad — Control de versiones

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-3.1 | Deshacer/rehacer durante la edición. | M | 🟢 |
| RF-3.2 | **Historial de revisiones** con marca de tiempo (y etiqueta opcional). | M | 🟢 |
| RF-3.3 | **Comparar** dos versiones (qué cambió). | S | 🟢 |
| RF-3.4 | **Restaurar** el documento a un punto anterior. | M | 🟢 |

**Criterio de aceptación (RF-3):** se puede listar versiones guardadas con fecha, ver diferencias entre dos y volver a una anterior sin perder la posibilidad de rehacer.

### RF-4 Presentación

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-4.1 | **Modo presentación:** convertir secciones del lienzo (frames) en diapositivas. | M | 🟢 |
| RF-4.2 | Navegar diapositivas (pantalla completa, flechas) sin exportar a PowerPoint. | M | 🟢 |

**Criterio de aceptación (RF-4):** definir ≥2 frames y recorrerlos a pantalla completa con teclado.

### RF-5 Persistencia, import/export y portabilidad

| ID | Requisito | Prioridad | Estado |
|---|---|---|---|
| RF-5.1 | Autoguardado local (navegador). | M | 🟢 |
| RF-5.2 | Guardar/Abrir documento como `.json` portable y validado. | M | 🟢 |
| RF-5.3 | Exportar a **PNG** y **SVG**. | M | 🟢 |
| RF-5.4 | Compatibilidad hacia atrás del formato de documento. | M | 🟢 |
| RF-5.5 | **Color de fondo del lienzo** (en pantalla y en export). | S | 🟢 |

---

## 4. Requisitos no funcionales (RNF)

| ID | Requisito | Criterio de aceptación |
|---|---|---|
| RNF-1 Portabilidad | Un solo archivo HTML, sin instalación ni servidor. | Abre con doble clic en cualquier navegador moderno. |
| RNF-2 Rendimiento | Edición fluida. | Render agrupado por frame (rAF); sin trabarse con ~200 formas. |
| RNF-3 Seguridad | El texto de la usuaria nunca se interpreta como markup. | Inserción vía `textContent`; documentos externos pasan por `sanitizeDoc`. |
| RNF-4 Robustez | Un archivo corrupto no rompe la app. | `sanitizeDoc` descarta datos inválidos y arranca limpio. |
| RNF-5 Mantenibilidad | Sumar una figura = una entrada en `SHAPES`. | Verificable en revisión de código. |
| RNF-6 Accesibilidad | Roles/labels ARIA en controles; atajos de teclado; foco visible; mover con flechas. | Navegable con teclado; foco visible; etiquetas presentes. |
| RNF-7 Sin assets con copyright | Íconos vectoriales propios. | Revisión: glifos dibujados a mano, sin SVGs oficiales. |
| RNF-8 i18n | Interfaz en español. | Toda la UI en español. |

---

## 5. Arquitectura (vista de alto nivel)

```
┌─────────────────────────────────────────────────────────┐
│  mis-diagramas.html  (un archivo)                         │
│                                                           │
│  ┌────────────┐   define    ┌──────────────────────────┐ │
│  │  REGISTRO  │ ───────────▶│  MOTOR                    │ │
│  │  SHAPES    │             │  · estado serializable    │ │
│  │  (figuras) │             │  · render declarativo     │ │
│  └────────────┘             │  · selección / conexión   │ │
│        │ mismo render        │  · historial (undo/redo)  │ │
│        ▼                     │  · persistencia / export  │ │
│  ┌────────────┐  consume     └──────────────────────────┘ │
│  │  PALETA    │ ◀──────────────────────┘                  │
│  │ (panel DnD)│                                            │
│  └────────────┘                                            │
└─────────────────────────────────────────────────────────┘
```

**Separación de incumbencias:** el *motor* no conoce las formas concretas; consume el *registro*. La *paleta* se genera desde el registro y reutiliza el mismo render que el lienzo. Esto materializa modularidad, abstracción y mantenibilidad (RNF-5).

**Módulos internos previstos (a medida que crezcan):** `core/` (estado, render, historial), `shapes/` (registro y glifos), `ui/` (paleta, props, atajos), `io/` (persistencia, import/export), `features/` (generación automática, versiones, presentación). Hoy conviven en el archivo único, separados por secciones comentadas; se pueden extraer a archivos si el tamaño lo justifica (anticipación al cambio).

---

## 6. Hoja de ruta incremental

> Cada iteración entrega un subconjunto funcional + su reporte (principio de incrementalidad).

| Iteración | Foco | Requisitos que cubre | Estado |
|---|---|---|---|
| **01** | Fundaciones + bibliotecas de formas | RF-1.1–1.6, RF-1.A/B/C/D (base), RF-5.* | ✅ Cerrada |
| **02** | Generación automática (texto → diagrama) | RF-2.1, RF-2.2, RF-2.3 | ✅ Cerrada |
| **03** | Usabilidad: selección múltiple + menú contextual | RF-1.7, RF-1.8, RF-1.9 | ✅ Cerrada |
| **04** | Control de versiones granular | RF-3.2, RF-3.3, RF-3.4 | ✅ Cerrada |
| **05** | Usabilidad: bloqueo de elementos + opciones de conector | RF-1.10, RF-1.11 | ✅ Cerrada |
| **06** | Modo presentación | RF-4.1, RF-4.2 | ✅ Cerrada |
| **07** | UML secuencia/actividad + wireframes UI/UX | RF-1.B (resto), RF-1.E | ✅ Cerrada |
| **08** | SQL ↔ ERD + rotar figuras + trazado de conectores + color de fondo | RF-1.C.1, RF-1.C.2, RF-1.12, RF-1.13, RF-5.5 | ✅ Cerrada |
| **09** | Pulido: alinear/distribuir + color propio de conector | RF-1.14, RF-1.15 | ✅ Cerrada |
| **10** | Fix + limpieza: íconos de alinear/distribuir, sin emojis, bug de distribuir | — (calidad) | ✅ Cerrada |
| **11** | Ampliar bibliotecas: más cloud (AWS/Azure/GCP/Cisco) + BPMN avanzado | RF-1.A, RF-1.D | ✅ Cerrada |
| **12** | Guías inteligentes (snap a bordes/centros) + accesibilidad (RNF-6) | RNF-6, deuda técnica | ✅ Cerrada |

**Fase 2 — Mejoras de calidad y experiencia (alcance ampliado, post-cierre del SRS original):**

| Iteración | Foco | Requisitos | Estado |
|---|---|---|---|
| **13** | Quick-wins de edición: no editar texto en figuras sin texto, estilo de borde, dibujar-para-dimensionar | RF-1.17, RF-1.18, RF-1.19 | ✅ Cerrada |
| **14** | Texto de verdad: edición in-place + compartimentos en ERD/Clase (+ fix resize rotado) | RF-1.20, RF-1.21 | ✅ Cerrada |
| **15** | Estética y color: íconos SVG, drag&drop visual, paletas guardables | RF-1.23, RF-1.22, RF-1.24 | ✅ Cerrada |
| **16** | Diferenciadores: badges de estado + quick-connect | RF-1.25, RF-1.26 | ✅ Cerrada |

El orden prioriza requisitos imprescindibles (M) pendientes y aprovecha la **generalización**: el parser de la iteración 02 se reutiliza para SQL→ERD en la 07. La iteración 03 se insertó por **feedback temprano** de uso (principio de incrementalidad).

---

## 7. Fuera de alcance (por ahora)
- Colaboración en tiempo real / multiusuario.
- Backend, cuentas y permisos.
- Integraciones externas (Jira, Confluence, etc.).
- Exportación nativa a `.pptx`/`.docx` (el modo presentación es interno).

---

## 8. Riesgos y mitigaciones

| Riesgo | Impacto | Mitigación |
|---|---|---|
| Crecimiento del archivo único dificulta el mantenimiento | Medio | Secciones modulares; extraer a archivos si supera un umbral. |
| Parseo de SQL real es complejo (dialectos) | Medio | Empezar por un subconjunto `CREATE TABLE` estándar; ampliar por dialecto. |
| Auto-layout de mapas mentales puede quedar feo | Bajo | Layout por niveles simple primero; mejorar luego. |
| `localStorage` lleno o bloqueado | Bajo | Guardado a `.json` siempre disponible como respaldo. |

---

## 9. Matriz de trazabilidad (checklist de verificación)

> Esta tabla es la **prueba de cumplimiento**. Un requisito se marca 🟢 solo cuando pasa su criterio de aceptación.

| Requisito | Prioridad | Iteración | Verificación | Estado |
|---|---|---|---|---|
| RF-1.1 Drag & drop | M | 01 | Test jsdom + uso manual | 🟢 |
| RF-1.2 Editar formas | M | 01 | Uso manual + test | 🟢 |
| RF-1.3 Conectores | M | 01 | Uso manual | 🟢 |
| RF-1.4 Texto inline | M | 01 | Uso manual | 🟢 |
| RF-1.5 Buscador/categorías | S | 01 | Test jsdom | 🟢 |
| RF-1.6 Zoom/paneo/fit | M | 01 | Uso manual | 🟢 |
| RF-1.7 Selección múltiple | M | 03 | Test jsdom + muestrario | 🟢 |
| RF-1.8 Menú contextual | M | 03 | Test jsdom | 🟢 |
| RF-1.9 Copiar/Cortar/Pegar | S | 03 | Test jsdom | 🟢 |
| RF-1.10 Bloquear elementos | M | 05 | Test jsdom + muestrario | 🟢 |
| RF-1.11 Conectores configurables | M | 05 | Test jsdom + muestrario | 🟢 |
| RF-1.12 Rotar figuras | S | 08 | Test jsdom + muestrario | 🟢 |
| RF-1.13 Trazado de conectores | S | 08 | Test jsdom + muestrario | 🟢 |
| RF-1.16 Guías inteligentes (snap) | S | 12 | Test jsdom + muestrario | 🟢 |
| RF-1.17 No editar texto sin texto | S | 13 | Test jsdom | 🟢 |
| RF-1.18 Estilo de borde de figura | S | 13 | Test jsdom + muestrario | 🟢 |
| RF-1.19 Dibujar para dimensionar | S | 13 | Test jsdom (pointer) | 🟢 |
| RF-1.20 Edición de texto in-place | S | 14 | Test jsdom + uso manual | 🟢 |
| RF-1.21 Compartimentos ERD/Clase | S | 14 | Test jsdom | 🟢 |
| RF-1.22 Drag&drop visual | C | 15 | Test jsdom (ghost) | 🟢 |
| RF-1.23 Íconos SVG en barra/panel | C | 15 | Test jsdom | 🟢 |
| RF-1.24 Paletas de colores guardables | C | 15 | Test jsdom | 🟢 |
| RF-1.25 Badges de estado | C | 16 | Test jsdom + muestrario | 🟢 |
| RF-1.26 Quick-connect | C | 16 | Test jsdom + muestrario | 🟢 |
| RF-1.14 Alinear / distribuir | S | 09 | Test jsdom + muestrario | 🟢 |
| RF-1.15 Color propio de conector | S | 09 | Test jsdom + muestrario | 🟢 |
| RF-1.A Redes AWS/Azure/GCP/Cisco | M | 01 / 11 | Test jsdom + muestrario | 🟢 |
| RF-1.B UML completo | M | 01 / 07 | Muestrario visual | 🟢 |
| RF-1.C ERD | M | 01 | Muestrario visual | 🟢 |
| RF-1.C.1 Import SQL→ERD | S | 08 | Test jsdom + muestrario | 🟢 |
| RF-1.C.2 Export ERD→SQL | S | 08 | Test jsdom (round-trip) | 🟢 |
| RF-1.D Flujo/BPMN | M | 01 / 11 | Test jsdom + muestrario | 🟢 |
| RF-1.E UI/UX wireframes | S | 07 | Test jsdom + muestrario | 🟢 |
| RF-2.1 Organigrama auto | M | 02 | Test jsdom + muestrario | 🟢 |
| RF-2.2 Mapa mental auto | M | 02 | Test jsdom + muestrario | 🟢 |
| RF-2.3 Auto-layout | M | 02 | Test: 0 solapamientos | 🟢 |
| RF-3.1 Undo/redo | M | 01 | Test jsdom | 🟢 |
| RF-3.2 Historial con timestamp | M | 04 | Test jsdom | 🟢 |
| RF-3.3 Comparar versiones | S | 04 | Test jsdom (diff) | 🟢 |
| RF-3.4 Restaurar versión | M | 04 | Test jsdom (deshacible) | 🟢 |
| RF-4.1 Frames→diapositivas | M | 06 | Test jsdom + muestrario | 🟢 |
| RF-4.2 Navegar presentación | M | 06 | Test jsdom (nav) + uso manual | 🟢 |
| RF-5.1 Autoguardado | M | 01 | Uso manual | 🟢 |
| RF-5.2 Guardar/Abrir JSON | M | 01 | Test jsdom (sanitize) | 🟢 |
| RF-5.3 Export PNG/SVG | M | 01 | Test + uso manual | 🟢 |
| RF-5.4 Compatibilidad formato | M | 01 | Test jsdom | 🟢 |
| RF-5.5 Color de fondo | S | 08 | Test jsdom | 🟢 |
| RNF-1 Portabilidad | — | 01 | Abre con doble clic | 🟢 |
| RNF-2 Rendimiento | — | 01 | Prueba con ~200 formas | 🟢 |
| RNF-3 Seguridad (textContent) | — | 01 | Revisión de código | 🟢 |
| RNF-4 Robustez (sanitize) | — | 01 | Test jsdom | 🟢 |
| RNF-5 Mantenibilidad | — | 01 | Revisión de código | 🟢 |
| RNF-6 Accesibilidad | — | 01 / 12 | Test jsdom (nudge) + revisión | 🟢 |
| RNF-7 Sin copyright | — | 01 | Revisión | 🟢 |
| RNF-8 i18n español | — | 01 | Revisión | 🟢 |

**Resumen de avance:**
- **SRS original (iteraciones 01–12): 100% cumplido** (45 requisitos en verde, funcionales y no funcionales).
- **Fase 2 (mejoras de calidad/experiencia): 10/10 cerrados** (RF-1.17 a RF-1.26, iteraciones 13–16) + fix de redimensionado con rotación. **Todo el SRS — original + Fase 2 — está en verde.**
- El archivo principal pasó a llamarse **`index.html`** (para publicación). Es la misma app.
- Los 4 requisitos centrales del proyecto y todas las funciones pedidas están implementados y verificados.
- Lo 🟡 son ampliaciones, no bloqueos: RF-1.A (más íconos cloud), RF-1.D (BPMN avanzado), RNF-6 (accesibilidad, mejora continua). Todo usable hoy.

---

## 10. Control de cambios del documento

| Versión | Fecha | Cambios |
|---|---|---|
| 1.0 | 12/06/2026 | Versión inicial del SRS + hoja de ruta, tras cerrar la iteración 01. |
| 1.1 | 12/06/2026 | Cierre de iteración 02 (generación automática): RF-2.1/2.2/2.3 → 🟢; roadmap y matriz actualizados. |
| 1.2 | 13/06/2026 | Cierre de iteración 03 (usabilidad): nuevos RF-1.7/1.8/1.9 → 🟢 (selección múltiple, menú contextual, copiar/pegar). Roadmap reordenado por feedback: control de versiones pasa a iter. 04. |
| 1.3 | 13/06/2026 | Cierre de iteración 04 (control de versiones): RF-3.2/3.3/3.4 → 🟢. **Requisito 3 (Trazabilidad) completo.** Esquema de documento → version 3 (incluye `versions`). |
| 1.4 | 13/06/2026 | Cierre de iteración 05 (usabilidad): nuevos RF-1.10 (bloqueo) y RF-1.11 (conectores configurables) → 🟢. Modo presentación pasa a iter. 06. |
| 1.5 | 13/06/2026 | Cierre de iteración 06 (modo presentación): RF-4.1/4.2 → 🟢. **Requisito 4 completo.** Esquema de documento ahora incluye `frames`. |
| 1.6 | 13/06/2026 | Cierre de iteración 07: RF-1.B (UML secuencia/actividad) y RF-1.E (wireframes UI/UX) → 🟢. Nueva categoría "Wireframe" y formas UML de secuencia/actividad. |
| 1.7 | 13/06/2026 | Cierre de iteración 08: RF-1.C.1/1.C.2 (SQL ↔ ERD) → 🟢 y nuevos RF-1.12 (rotar), RF-1.13 (trazado de conectores), RF-5.5 (color de fondo). **Sin requisitos en rojo: 39 🟢 · 3 🟡 · 0 🔴.** Documento incluye `bg`; figuras `rot`; conectores `route`. |
| 1.8 | 13/06/2026 | Cierre de iteración 09 (pulido): nuevos RF-1.14 (alinear/distribuir) y RF-1.15 (color propio de conector). **41 🟢 · 3 🟡 · 0 🔴.** Conectores con `color`. |
| 1.9 | 13/06/2026 | Iteración 10 (fix + limpieza): íconos de alinear/distribuir a SVG (se veían como cuadraditos), **emojis eliminados de toda la UI**, regresión completa (70 formas + todas las funciones, 0 errores). Mejora de RNF-6. Sin cambios en la matriz. |
| 1.9.1 | 13/06/2026 | Parche 10.1: bug real en **distribuir** — figuras colineales en el eje (ej. columna apilada + distribuir horizontal) colapsaban en el mismo punto y "desaparecían". `distributeSelected` reescrito con caso degenerado (separación fija). Verificado con test del caso exacto. |
| 1.9.2 | 13/06/2026 | Parche 10.2: la opción **Distribuir no se veía** — los 8 botones no entraban en el panel (190 px) y los 2 de distribuir quedaban cortados fuera. Rediseño en dos secciones etiquetadas y apiladas ("Alinear" / "Distribuir"); panel a 200 px. Verificado (existen, se habilitan con 3+, el clic reparte). |
| 2.0 | 13/06/2026 | Cierre de iteración 11: ampliación de bibliotecas (18 servicios cloud + Cisco, BPMN avanzado) → RF-1.A y RF-1.D a 🟢. Fix estético del label "Fondo". **43 🟢 · 1 🟡 · 0 🔴 — todos los RF en verde.** 94 figuras en total. |
| 2.1 | 13/06/2026 | Cierre de iteración 12: guías inteligentes (snap) → nuevo RF-1.16; accesibilidad (foco visible + mover con flechas) → RNF-6 a 🟢. **45 🟢 · 0 🟡 · 0 🔴 — SRS completo.** |
| 2.2 | 13/06/2026 | Apertura de **Fase 2** (mejoras de calidad/experiencia): nuevos RF-1.17 a RF-1.26 a partir del feedback de uso; roadmap iteraciones 13–16. |
| 2.3 | 13/06/2026 | Cierre de iteración 13: RF-1.17 (no editar texto sin texto), RF-1.18 (estilo de borde de figura), RF-1.19 (dibujar-para-dimensionar) → 🟢. |
| 2.4 | 14/06/2026 | Cierre de iteración 14: RF-1.20 (edición in-place) y RF-1.21 (compartimentos ERD/Clase) → 🟢; **fix de redimensionado con rotación** (manijas + esquina ancla fija). Archivo renombrado a `index.html`. |
| 2.5 | 14/06/2026 | Cierre de iteración 15: RF-1.22 (drag&drop visual), RF-1.23 (íconos SVG sin emojis), RF-1.24 (paletas de colores) → 🟢. **Fase 2: 8/10.** |
| 2.6 | 14/06/2026 | Cierre de iteración 16: RF-1.25 (badges de estado) y RF-1.26 (quick-connect) → 🟢. **Fase 2 completa (10/10). SRS íntegro en verde.** |
| **1.0** | 14/06/2026 | **Auditoría final + cierre de v1.0.** Verificación integral (94 figuras + todas las funciones, 0 errores), comprobaciones de seguridad (XSS, sin red/eval, entrada maliciosa). **Fix de seguridad:** rechazo de `type` heredados (`__proto__`) en `sanitizeDoc`/`drop` (sin prototype pollution). UI sin emojis, sin restos de debug. Ver `REPORTE-auditoria-v1.md`. **Apta para uso y publicación.** |
