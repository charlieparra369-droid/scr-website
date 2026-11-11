# Design System — parrasanlabs.com

> **Estado:** Fase 0 · Aprobado conceptualmente (revisión 2, 2026-11-11)
> **Identidad elegida: Propuesta A, "Nocturna precisa"** (§3). Secciones claras solo en Contacto y páginas legales. Tipografía: Manrope (títulos) + Inter (textos). Dorado del logo como acento secundario. Wireframes en §10.

## 1. Principios de diseño

1. **Claridad antes que espectáculo.** Cada sección responde una pregunta del visitante.
2. **Silencio visual.** Mucho espacio en blanco, una sola acción destacada por pantalla.
3. **Tecnología sin clichés.** Nada de robots, cerebros, circuitos brillantes ni manos humanoides. La tecnología se sugiere con geometría, ritmo, retícula y diagramas de flujo simples.
4. **Movimiento con propósito.** Transiciones ≤ 200 ms, sin parallax, sin animaciones en bucle. Todo se desactiva con `prefers-reduced-motion`.
5. **Accesible por defecto.** WCAG 2.2 AA es el mínimo, no una tarea posterior.

## 2. Paleta

### 2.1 Colores base (preliminares, proporcionados por Parrasan Labs)

| Token | Hex | Rol |
|-------|-----|-----|
| `ink-950` | `#0B1020` | Fondo oscuro principal / texto principal sobre claro |
| `ink-800` | `#17243C` | Superficies oscuras elevadas, tarjetas sobre oscuro |
| `cyan-400` | `#23C7D9` | Acento de marca, CTA sobre oscuro, foco |
| `mist-50` | `#F4F6FA` | Fondo claro principal |
| `slate-500` | `#7B879B` | Texto secundario **solo sobre oscuro**, bordes, iconos decorativos |

### 2.2 Colores derivados (propuestos para cumplir contraste)

| Token | Hex | Rol |
|-------|-----|-----|
| `cyan-700` | `#0A6B77` | Enlaces y acento **sobre fondo claro** |
| `slate-700` | `#4A5568` | Texto secundario **sobre fondo claro** |
| `slate-300` | `#B4BDCB` | Texto secundario alternativo sobre oscuro (más legible) |
| `white` | `#FFFFFF` | Tarjetas sobre `mist-50` |
| `danger` | `#B42318` | Errores de formulario (validar contraste en implementación) |
| `success` | `#067647` | Confirmaciones (validar contraste en implementación) |

### 2.2.1 Acento secundario: dorado del logo (D3)

| Token | Hex | Rol |
|-------|-----|-----|
| `gold-500` | **`[PENDIENTE]`** — tomar el valor exacto de los archivos oficiales del logo (no se estima a ojo) | Acento secundario sobre oscuro |
| `gold-700` | Derivado de `gold-500` cuando se conozca, oscurecido hasta lograr ≥ 4.5:1 sobre `mist-50` | Acento secundario sobre claro (solo si hace falta) |

**Reglas de uso del dorado:**
- Es **secundario**: el cyan sigue siendo el acento principal y el único color de los CTA. El dorado nunca se usa en botones ni enlaces.
- Usos previstos: marca del logo, el nodo de "revisión humana" en los diagramas de flujo (el punto donde decide una persona), viñetas y separadores finos, numeración de etapas en *Cómo trabajamos*.
- Superficie limitada: detalles pequeños, nunca fondos de sección ni bloques grandes.
- Dorado y cyan no se colocan juntos en el mismo elemento.
- Como texto, solo sobre `ink-950`/`ink-800` y solo si el valor oficial cumple ≥ 4.5:1 (o ≥ 3:1 en texto grande). La prueba automática de contraste (PR-02) se ejecuta en cuanto se registra el valor.
- Hasta tener el valor oficial, el prototipo usa un dorado **provisional** rotulado como tal en la guía de estilo y en el PR; no se publica en producción con ese valor.

### 2.3 Contraste verificado (WCAG 2.x, ratio)

| Primer plano / Fondo | Ratio | Resultado |
|----------------------|-------|-----------|
| `#F4F6FA` / `#0B1020` | 17.50 | ✅ AAA |
| `#23C7D9` / `#0B1020` | 9.23 | ✅ AAA (texto y CTA) |
| `#0B1020` / `#23C7D9` (texto del botón) | 9.23 | ✅ AAA |
| `#23C7D9` / `#17243C` | 7.56 | ✅ AAA |
| `#B4BDCB` / `#17243C` | 8.18 | ✅ AAA |
| `#7B879B` / `#0B1020` | 5.21 | ✅ AA texto normal |
| `#7B879B` / `#17243C` | 4.27 | ⚠️ Solo texto grande (≥ 24 px o 19 px bold) |
| `#7B879B` / `#F4F6FA` | 3.36 | ❌ No usar para texto sobre claro |
| `#23C7D9` / `#F4F6FA` | 1.89 | ❌ **Nunca** usar cyan como texto sobre claro |
| `#0A6B77` / `#F4F6FA` | 5.74 | ✅ AA |
| `#4A5568` / `#F4F6FA` | 6.96 | ✅ AA |
| `#0B1020` / `#F4F6FA` | 17.50 | ✅ AAA |

**Reglas derivadas:**
- Sobre claro, el acento es `cyan-700`; `cyan-400` solo como relleno decorativo (líneas, puntos, fondos de icono) junto a texto oscuro.
- `slate-500` nunca para texto de cuerpo sobre claro ni sobre `ink-800` en tamaño normal.
- El anillo de foco usa `cyan-400` sobre oscuro y `cyan-700` sobre claro, 2 px + 2 px de separación (cumple 2.4.13 / 1.4.11 ≥ 3:1).

### 2.4 Mapa de temas por página (D1)

| Página / zona | Tema |
|---------------|------|
| Encabezado y footer (todas las páginas) | Oscuro (`ink-950`) |
| Inicio | Oscuro completo, alternando `ink-950` y `ink-800` |
| Soluciones, Cómo trabajamos, Aplicaciones, Nosotros, 404 | Oscuro |
| Contacto | Hero oscuro; formulario y columna lateral en **claro** (`mist-50`, tarjeta `white`) |
| Privacidad y Términos | Título oscuro; cuerpo del texto en **claro** (`mist-50`) |

Los temas se aplican por sección con un atributo (`data-theme="dark" | "light"`) que redefine las variables CSS; los componentes no tienen colores fijos y funcionan en ambos temas.

### 2.5 Logo y archivos de marca (D4)

El logo oficial existe y hay una versión modernizada en desarrollo. **No se dibuja, vectoriza, recrea ni modifica el logo en código.** El sistema solo reserva lugares para los archivos oficiales.

**Ubicación y nombres (contrato estable):**

```
src/assets/brand/
├── brand.ts                     # manifiesto: rutas, proporciones, texto alternativo, versión
├── logo-horizontal-on-dark.svg  # principal (sobre fondos oscuros)
├── logo-horizontal-on-light.svg # sobre secciones claras
├── logo-mark-on-dark.svg        # isotipo (favicon, móvil compacto)
├── logo-mark-on-light.svg
public/
├── favicon.svg / favicon.ico / apple-touch-icon.png (180×180)
└── og/default.png               # 1200×630, compuesto con el logo oficial
```

**Componente `Logo`:** `<Logo variant="horizontal" | "mark" tone="onDark" | "onLight" height={32} />`.
- Lee rutas y proporciones desde `brand.ts`. Cambiar al logo modernizado = reemplazar los archivos (mismos nombres) y actualizar `brand.ts` (`version`, proporción). **Ningún componente cambia.**
- Alturas definidas por tokens (`--logo-h-header: 32px` escritorio / `28px` móvil; `--logo-h-footer: 28px`); el ancho se deriva de la proporción para evitar saltos de diseño.
- Envuelve el logo en un enlace a `/` con `aria-label="Parrasan Labs, inicio"`; la imagen lleva `alt=""` dentro del enlace para no duplicar la lectura.
- **Sustituto provisional** mientras no estén los archivos: el texto "Parrasan Labs" en Manrope 600. Es solo texto; no intenta imitar el logo. Se rotula "Logo provisional" en la guía de estilo.
- Requisitos de los archivos: SVG con `viewBox`, sin imágenes rasterizadas incrustadas, texto convertido a trazos, optimizado (SVGO) sin alterar la forma; área de protección indicada por el equipo de marca.
- El valor exacto del dorado (`gold-500`) se toma de estos archivos.

## 3. Identidad visual

### Elegida — Propuesta A, "Nocturna precisa" (oscuro dominante)

**Concepto:** El sitio se siente como un instrumento de precisión: fondo `ink-950`, tipografía clara, el cyan aparece como una línea de luz que guía la lectura. Transmite tecnología y seriedad.

- **Fondo principal:** `ink-950`; secciones alternas en `ink-800`.
- **Texto:** `mist-50` (títulos), `slate-300` (cuerpo).
- **Acento principal:** `cyan-400` en CTA, enlaces, foco y líneas finas de 1 px.
- **Acento secundario:** `gold-500` (dorado del logo) en el nodo de revisión humana, numeración de etapas y detalles pequeños (§2.2.1).
- **Motivo gráfico:** retícula de puntos muy sutil (opacidad 6–8 %) y diagramas de flujo minimalistas (nodos + conectores) que ilustran cada solución: *entrada → agente → revisión humana (nodo dorado) → resultado*. SVG, sin glow.
- **Tarjetas:** `ink-800`, borde `1px rgba(123,135,155,.25)`, radio 12 px; hover: borde cyan.
- **Ventajas:** Diferenciación fuerte, look "tech" sin clichés, el cyan luce al máximo.
- **Riesgos:** Lectura larga más cansada; páginas legales y formularios deben aclararse; puede parecer "frío" para PYMES tradicionales.
- **Mitigación (aprobada):** Contacto y legales en tema claro (§2.4); cuerpo de texto ≥ 18 px en escritorio; párrafos cortos y ancho de línea ≤ 68 caracteres.

```
┌──────────────────────────────────────────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ink-950 ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓  Parrasan Labs          Soluciones ...   [■ Hablemos ...]   ▓│
│▓                                                             ▓│
│▓  AGENTES DE IA · AUTOMATIZACIÓN        · · · · · · · ·      ▓│
│▓  Automatiza el trabajo                 ○──▶◇──▶◉──▶□        ▓│
│▓  repetitivo.  ─────── (línea cyan)     · · · · · · · ·      ▓│
│▓  [■ Hablemos de tu proyecto]  Ver cómo trabajamos →         ▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│░░░░░░░░░░░░░░░░░░░░░░░░░ ink-800 ░░░░░░░░░░░░░░░░░░░░░░░░░░░░│
│░  [tarjeta]  [tarjeta]  [tarjeta]  [tarjeta]                 ░│
└──────────────────────────────────────────────────────────────┘
```

### Descartada — Propuesta B, "Claridad editorial" (claro dominante)

> Se conserva como referencia. **No** se implementa: Parrasan Labs no quiere un sitio predominantemente blanco. Sus reglas de tema claro se reutilizan solo en Contacto y legales.

**Concepto:** Un sitio que se lee como una buena publicación: fondo `mist-50`, tipografía oscura generosa, y bloques oscuros (`ink-950`) en momentos clave (hero, CTA final, footer). Transmite cercanía, orden y confianza.

- **Fondo principal:** `mist-50`; tarjetas `white`.
- **Texto:** `ink-950` (títulos y cuerpo), `slate-700` (secundario).
- **Acento:** `cyan-700` en enlaces; `cyan-400` como relleno en bloques oscuros y en detalles gráficos.
- **Hero y CTA final:** bandas `ink-950` con texto `mist-50` y CTA `cyan-400`.
- **Motivo gráfico:** mismos diagramas de flujo de la Propuesta A, en trazo `ink-950` con nodo de "revisión humana" resaltado en cyan.
- **Tarjetas:** `white`, borde `1px #E3E7EF`, sombra mínima `0 1px 2px rgba(11,16,32,.06)`, radio 12 px.
- **Ventajas:** Máxima legibilidad, más amigable para PYMES y equipos no técnicos, formularios naturales.
- **Riesgos:** Menos distintiva; el cyan pierde protagonismo sobre claro.
- **Mitigación:** Bandas oscuras con ritmo intencional (hero, mitad de página, cierre).

```
┌──────────────────────────────────────────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ink-950 (hero) ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓  Parrasan Labs          Soluciones ...   [■ Hablemos ...]   ▓│
│▓  Automatiza el trabajo repetitivo.        ○──▶◇──▶◉──▶□     ▓│
│▓  [■ Hablemos de tu proyecto]                                ▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│                         mist-50                              │
│   ¿Te suena familiar?                                        │
│   ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐  (white)       │
│   └────────┘ └────────┘ └────────┘ └────────┘                │
└──────────────────────────────────────────────────────────────┘
```

### Decisión

Se adopta **A**. La recomendación inicial (B) quedó superada por la decisión D1. La legibilidad que motivaba B se resuelve con el mapa de temas (§2.4) y las reglas de tipografía (§4).

## 4. Tipografía

| Uso | Familia | Alternativa | Notas |
|-----|---------|-------------|-------|
| Títulos | **Manrope** (aprobada) | system-ui | Geométrica, moderna, buena en español (acentos, ñ); pesos 600 y 700 |
| Cuerpo | **Inter** (aprobada) | system-ui, sans-serif | Alta legibilidad en pantalla; pesos 400 y 500 |
| Datos / código | **JetBrains Mono** (opcional) | ui-monospace | Solo en diagramas o etiquetas técnicas |

- Fuentes **autoalojadas** (WOFF2, subset latin + latin-ext), `font-display: swap`. Sin Google Fonts en tiempo de ejecución (privacidad + rendimiento).
- Máximo 2 familias, 4 archivos de fuente: Inter 400/500, Manrope 600/700 (o las versiones variables de ambas si pesan menos). Ambas con licencia SIL Open Font License.
- Sobre fondo oscuro, el cuerpo usa `slate-300` con interlineado 1.65 para compensar el brillo del texto claro.

### Escala (fluida con `clamp`, base 16 px)

| Token | Móvil | Escritorio | Uso |
|-------|-------|-----------|-----|
| `display` | 40 px | 64 px | H1 de Inicio |
| `h1` | 34 px | 48 px | H1 de páginas internas |
| `h2` | 28 px | 36 px | Secciones |
| `h3` | 22 px | 24 px | Tarjetas |
| `body-lg` | 18 px | 20 px | Intro / lead |
| `body` | 16 px | 18 px | Cuerpo |
| `small` | 14 px | 14 px | Notas, rótulos |
| `eyebrow` | 13 px, 600, tracking 0.08em, mayúsculas | | Sobre-títulos |

- Interlineado: 1.1–1.2 títulos, 1.6 cuerpo. Ancho de línea: 60–72 caracteres (`max-w-prose`).

## 5. Espaciado, retícula y layout

- Escala de espaciado base 4 px (la de Tailwind): 4, 8, 12, 16, 24, 32, 48, 64, 96, 128.
- Contenedor: `max-width: 1200px`, padding lateral 20 px (móvil) / 32 px (tablet) / 48 px (escritorio).
- Retícula: 4 columnas (móvil), 8 (tablet), 12 (escritorio), gutter 24 px.
- Espacio vertical entre secciones: 64 px (móvil) / 112 px (escritorio).
- Breakpoints (Tailwind): `sm 640`, `md 768`, `lg 1024`, `xl 1280`.

## 6. Bordes, radios, sombras

- Radios: `sm 6px` (inputs), `md 12px` (tarjetas, botones), `full` (chips).
- Bordes: 1 px. Sombras mínimas (solo tema claro); nada de glows.

## 7. Iconografía e imágenes

- Iconos lineales 1.5 px, 24 px, estilo consistente (ej. **Lucide**, licencia ISC), importados como SVG en build (sin fuentes de iconos).
- Ilustraciones: diagramas de flujo propios en SVG (nodos, flechas, nodo humano resaltado). Un diagrama por solución.
- Fotografía: solo fotos **reales** del equipo o espacios de trabajo, con autorización. Sin stock de "personas felices frente a pantallas".
- Prohibido: robots, cerebros, androides, manos de robot, hologramas, imágenes generadas que simulen personas reales o clientes.
- Formatos: AVIF/WebP vía `astro:assets`, `width`/`height` siempre definidos, `alt` significativo o `alt=""` si es decorativa.

## 8. Componentes

| Componente | Variantes | Notas de accesibilidad |
|------------|-----------|------------------------|
| `Button` | `primary` (cyan-400 / texto ink-950), `secondary` (borde), `link` | Altura mín. 44 px; foco visible; `<a>` si navega, `<button>` si acciona |
| `Logo` | horizontal / isotipo · sobre oscuro / sobre claro | Ver §2.5; enlace con `aria-label`, imagen `alt=""` |
| `Header` | siempre oscuro, sticky simple | `<nav aria-label="Principal">`, menú móvil con `aria-expanded`, trampa de foco y `Esc` |
| `Footer` | — | `<nav aria-label="Pie de página">` |
| `SkipLink` | — | Primer elemento enfocable |
| `Hero` | home / interno | Un solo H1 |
| `SectionHeading` | con eyebrow | Jerarquía H2/H3 correcta |
| `SolutionCard` | — | Toda la tarjeta clicable vía enlace en el título (no `div` con onclick) |
| `SolutionBlock` | — | Ver CONTENT_MAP §2 |
| `ProcessTimeline` | horizontal (lg) / vertical (móvil) | `<ol>` semántico |
| `ScenarioCard` | con rótulo "Escenario ilustrativo" | Rótulo visible, no solo color |
| `PrincipleItem` | — | — |
| `FAQ` | — | `<details>/<summary>` nativo |
| `CTASection` | oscuro / claro | — |
| `FlowDiagram` | por solución | SVG con `<title>` y `<desc>`, o `aria-hidden` + texto equivalente |
| `ContactForm` | — | `<label>` visibles, errores con `aria-describedby` + `aria-invalid`, resumen de errores enfocable, sin validación solo por color |
| `LanguageSwitcher` | oculto en v1 | `hreflang`, `lang` en enlaces |

### Estados de formulario
`default` · `hover` · `focus` (anillo 2 px) · `error` (borde danger + icono + texto) · `disabled` · `submitting` (botón con texto "Enviando…" y `aria-busy`) · `success` (redirección a `/contacto/gracias/`).

## 9. Movimiento

- Duraciones: 120 ms (hover), 200 ms (aparición). Easing `cubic-bezier(.2,.0,.0,1)`.
- Permitido: fade/translate de 8 px al entrar en vista (una vez), subrayado animado en enlaces.
- No permitido: parallax, autoplay de video, contadores animados, texto que se escribe solo, partículas, cursores personalizados.
- `@media (prefers-reduced-motion: reduce)` → sin transiciones de entrada.

## 10. Wireframes

> Los wireframes aplican a la Propuesta A: todas las bandas son oscuras salvo Contacto (formulario) y legales. `◆ Parrasan Labs` representa el componente `Logo`, no un diseño del logo.

Leyenda: `[■ CTA]` botón primario · `[□ …]` botón secundario · `(img)` diagrama/imagen · `▓` banda oscura.

### 10.1 Inicio — Escritorio (≥ 1024 px)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│ Saltar al contenido principal (visible al foco)                              │
├──────────────────────────────────────────────────────────────────────────────┤
│▓ ◆ Parrasan Labs     Soluciones  Cómo trabajamos  Aplicaciones  Nosotros  [■ Hablemos de tu proyecto] ▓│
│▓                                                                            ▓│
│▓  AGENTES DE IA · AUTOMATIZACIÓN EMPRESARIAL      ┌───────────────────────┐ ▓│
│▓                                                   │  (FlowDiagram)        │ ▓│
│▓  Automatiza el trabajo repetitivo.                │  Entrada → Agente →   │ ▓│
│▓  Tu equipo se queda con lo importante.            │  Revisión ◉ → Result. │ ▓│
│▓                                                   └───────────────────────┘ ▓│
│▓  Diseñamos agentes de IA y automatizaciones…                               ▓│
│▓  [■ Hablemos de tu proyecto]   Ver cómo trabajamos →                       ▓│
├──────────────────────────────────────────────────────────────────────────────┤
│  ¿Te suena familiar?                                                         │
│  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐ ┌──────────────┐         │
│  │ ⌁ Preguntas  │ │ ⌁ Conciliar  │ │ ⌁ Documentos │ │ ⌁ Copiar y   │         │
│  │   repetidas  │ │   a mano     │ │   perdidos   │ │   pegar      │         │
│  └──────────────┘ └──────────────┘ └──────────────┘ └──────────────┘         │
├──────────────────────────────────────────────────────────────────────────────┤
│  QUÉ HACEMOS                                                                 │
│  ┌───────────────────────────┐ ┌───────────────────────────┐                 │
│  │ Atención al cliente     → │ │ Conciliación financiera → │                 │
│  │ texto 2 líneas            │ │ texto 2 líneas            │                 │
│  └───────────────────────────┘ └───────────────────────────┘                 │
│  ┌───────────────────────────┐ ┌───────────────────────────┐                 │
│  │ Gestión documental      → │ │ Automatización procesos → │                 │
│  └───────────────────────────┘ └───────────────────────────┘                 │
├──────────────────────────────────────────────────────────────────────────────┤
│  Paso a paso, sin saltos al vacío                                            │
│   ① Diagnóstico ──── ② Piloto ──── ③ Implementación ──── ④ Acompañamiento    │
│   texto corto        texto corto    texto corto           texto corto        │
│                                              Conoce el detalle →             │
├──────────────────────────────────────────────────────────────────────────────┤
│  Cómo pensamos la IA                                                         │
│  Las personas deciden │ Tus datos son tuyos │ Sin promesas mágicas           │
├──────────────────────────────────────────────────────────────────────────────┤
│▓  ¿Tienes un proceso que quieres simplificar?                               ▓│
│▓  Cuéntanos cómo funciona hoy…              [■ Hablemos de tu proyecto]     ▓│
├──────────────────────────────────────────────────────────────────────────────┤
│▓  Parrasan Labs     Sitio        Legal        Contacto                      ▓│
│▓  tagline           enlaces      Privacidad   cparra@parrasanlabs.com        ▓│
│▓  © 2026                                                                    ▓│
└──────────────────────────────────────────────────────────────────────────────┘
```

### 10.2 Inicio — Móvil (360–414 px)

```
┌──────────────────────────────┐
│▓ ◆ Parrasan Labs     [☰]    ▓│
│▓                            ▓│
│▓ AGENTES DE IA ·            ▓│
│▓ AUTOMATIZACIÓN             ▓│
│▓                            ▓│
│▓ Automatiza el trabajo      ▓│
│▓ repetitivo. Tu equipo se   ▓│
│▓ queda con lo importante.   ▓│
│▓                            ▓│
│▓ Diseñamos agentes de IA…   ▓│
│▓                            ▓│
│▓ [■ Hablemos de tu proyecto]▓│  ← ancho completo, 48 px alto
│▓ Ver cómo trabajamos →      ▓│
│▓ ┌────────────────────────┐ ▓│
│▓ │ (FlowDiagram vertical) │ ▓│
│▓ └────────────────────────┘ ▓│
├──────────────────────────────┤
│ ¿Te suena familiar?          │
│ ┌──────────────────────────┐ │
│ │ ⌁ Preguntas repetidas    │ │
│ └──────────────────────────┘ │
│ ┌──────────────────────────┐ │
│ │ ⌁ Conciliar a mano       │ │
│ └──────────────────────────┘ │
│ … (apiladas)                 │
├──────────────────────────────┤
│ QUÉ HACEMOS                  │
│ ┌──────────────────────────┐ │
│ │ Atención al cliente    → │ │
│ └──────────────────────────┘ │
│ … 4 tarjetas apiladas        │
├──────────────────────────────┤
│ Paso a paso                  │
│ ①  Diagnóstico               │
│ │  texto                     │
│ ②  Piloto                    │
│ │  texto                     │
│ ③  Implementación            │
│ ④  Acompañamiento            │
├──────────────────────────────┤
│ Cómo pensamos la IA          │
│ (3 bloques apilados)         │
├──────────────────────────────┤
│▓ ¿Tienes un proceso…?       ▓│
│▓ [■ Hablemos de tu proyecto]▓│
├──────────────────────────────┤
│▓ Footer apilado             ▓│
└──────────────────────────────┘

Menú móvil abierto:
┌──────────────────────────────┐
│▓ ◆ Parrasan Labs     [✕]    ▓│
│▓                            ▓│
│▓ Soluciones                 ▓│
│▓ Cómo trabajamos            ▓│
│▓ Aplicaciones               ▓│
│▓ Nosotros                   ▓│
│▓                            ▓│
│▓ [■ Hablemos de tu proyecto]▓│
│▓ cparra@parrasanlabs.com    ▓│
└──────────────────────────────┘
```

### 10.3 Soluciones — Escritorio / Móvil

```
ESCRITORIO                                               MÓVIL
┌───────────────────────────────────────────────┐        ┌───────────────────┐
│ H1 Soluciones                                 │        │ H1 Soluciones     │
│ intro                                         │        │ intro             │
│ [Atención] [Conciliación] [Documental] [Proc.]│ ← sub- │ [chips scroll ⇢]  │
├───────────────────────────────────────────────┤  nav   ├───────────────────┤
│ Atención al cliente          ┌──────────────┐ │ ancla  │ Atención al       │
│ Qué es…                      │ FlowDiagram  │ │        │ cliente           │
│ Qué puede hacer  • • •       └──────────────┘ │        │ (diagrama)        │
│ Dónde sigue tu equipo …                       │        │ Qué es…           │
│ Qué necesitamos de ti …                       │        │ Qué puede hacer…  │
│ [■ Hablemos de tu proyecto]                   │        │ Dónde sigue tu    │
├───────────────────────────────────────────────┤        │ equipo…           │
│ (diagrama a la izq.) Conciliación financiera  │        │ [■ Hablemos…]     │
│ … alternando lados                            │        ├───────────────────┤
├───────────────────────────────────────────────┤        │ … siguiente       │
│ Lo que la IA no debería hacer sola            │        │                   │
└───────────────────────────────────────────────┘        └───────────────────┘
```

### 10.4 Cómo trabajamos — Escritorio / Móvil

```
ESCRITORIO                                               MÓVIL
┌───────────────────────────────────────────────┐        ┌───────────────────┐
│ H1 Cómo trabajamos · intro                    │        │ H1 · intro        │
├───────────────────────────────────────────────┤        ├───────────────────┤
│ ①──────②──────③──────④──────⑤                 │        │ ① Conversación    │
│ ┌────────────────────────────────────────────┐│        │ │ Qué pasa        │
│ │ Etapa  │ Qué pasa │ Qué recibes │ Tu parte ││        │ │ Qué recibes     │
│ │ (fila por etapa, <ol> con <dl> internos)   ││        │ │ Tu parte        │
│ └────────────────────────────────────────────┘│        │ ② Diagnóstico     │
├───────────────────────────────────────────────┤        │ │ …               │
│ Lo que puedes esperar de nosotros (2×2)       │        ├───────────────────┤
├───────────────────────────────────────────────┤        │ Compromisos       │
│ Preguntas frecuentes  ▸ ▸ ▸ ▸                 │        │ FAQ ▸ ▸ ▸         │
│ [■ Hablemos de tu proyecto]                   │        │ [■ Hablemos…]     │
└───────────────────────────────────────────────┘        └───────────────────┘
```

### 10.5 Aplicaciones — Escritorio / Móvil

```
ESCRITORIO                                               MÓVIL
┌───────────────────────────────────────────────┐        ┌───────────────────┐
│ H1 Aplicaciones                               │        │ H1 Aplicaciones   │
│ Son ejemplos ilustrativos, no casos de        │        │ Aviso ilustrativo │
│ clientes.                                     │        ├───────────────────┤
│ Filtro: [Todas][Finanzas][Servicio][Adm.]…    │        │ [ESCENARIO ILUS.] │
├───────────────────────────────────────────────┤        │ Finanzas          │
│ ┌─────────────┐ ┌─────────────┐ ┌───────────┐ │        │ Situación…        │
│ │ESCENARIO    │ │ESCENARIO    │ │ESCENARIO  │ │        │ Cómo ayuda…       │
│ │ILUSTRATIVO  │ │ILUSTRATIVO  │ │ILUSTRATIVO│ │        │ Revisión humana…  │
│ │Finanzas     │ │Servicio     │ │Administr. │ │        ├───────────────────┤
│ │Situación    │ │…            │ │…          │ │        │ … apiladas        │
│ │Cómo ayuda   │ │             │ │           │ │        │                   │
│ │Revisión ◉   │ │             │ │           │ │        │                   │
│ └─────────────┘ └─────────────┘ └───────────┘ │        │                   │
│ (3×2)                                         │        ├───────────────────┤
│ ¿Tu caso se parece? [■ Hablemos…]             │        │ [■ Hablemos…]     │
└───────────────────────────────────────────────┘        └───────────────────┘
```
> El filtro es progresivo: sin JS se ven todas las tarjetas.

### 10.6 Nosotros — Escritorio / Móvil

```
ESCRITORIO                                               MÓVIL
┌───────────────────────────────────────────────┐        ┌───────────────────┐
│ H1 Nosotros                                   │        │ H1 Nosotros       │
│ Intro (2 col: texto │ foto real o diagrama)   │        │ Intro             │
├───────────────────────────────────────────────┤        ├───────────────────┤
│ Por qué existimos                             │        │ Por qué existimos │
├───────────────────────────────────────────────┤        ├───────────────────┤
│ Equipo  ┌──────┐ ┌──────┐ ┌──────┐            │        │ ┌──────┐ Nombre   │
│         │ foto │ │ foto │ │ foto │            │        │ │ foto │ Cargo    │
│         │Nombre│ │Nombre│ │Nombre│            │        │ └──────┘ Bio      │
│         │Cargo │ │Cargo │ │Cargo │            │        │ …                 │
├───────────────────────────────────────────────┤        ├───────────────────┤
│ Valores (4 col)                               │        │ Valores (apilados)│
│ [■ Hablemos de tu proyecto]                   │        │ [■ Hablemos…]     │
└───────────────────────────────────────────────┘        └───────────────────┘
```

### 10.7 Contacto — Escritorio / Móvil

```
ESCRITORIO                                               MÓVIL
┌───────────────────────────────────────────────┐        ┌───────────────────┐
│ H1 Hablemos de tu proyecto                    │        │ H1 Hablemos de    │
├──────────────────────────┬────────────────────┤        │ tu proyecto       │
│ Nombre*      [_______]   │ ¿Qué pasa después? │        │ Intro             │
│ Correo*      [_______]   │ 1. Leemos tu caso  │        ├───────────────────┤
│ Empresa*     [_______]   │ 2. Te respondemos  │        │ Nombre*           │
│ Teléfono     [_______]   │ 3. Si tiene        │        │ [_______________] │
│ Tamaño       [▼ ____ ]   │    sentido, una    │        │ Correo*           │
│ Área*        [▼ ____ ]   │    conversación    │        │ [_______________] │
│ Proceso*                 │                    │        │ … (1 columna)     │
│ [                     ]  │ ¿Prefieres correo? │        │ ⚠ No incluyas     │
│ [                     ]  │ cparra@            │        │ datos sensibles   │
│ ⚠ No incluyas datos      │ parrasanlabs.com   │        │ ☐ Acepto…*        │
│   sensibles…             │                    │        │ [Turnstile]       │
│ ¿Qué tan pronto? ○ ○ ○   │                    │        │ [■ Enviar mensaje]│
│ ☐ Acepto … Privacidad*   │                    │        ├───────────────────┤
│ [Turnstile]              │                    │        │ ¿Qué pasa después?│
│ [■ Enviar mensaje]       │                    │        │ ¿Prefieres correo?│
└──────────────────────────┴────────────────────┘        └───────────────────┘
* = requerido (indicado con texto "(obligatorio)" para lectores de pantalla)
```

## 11. Tokens para Tailwind (referencia, no código definitivo)

```
colors: {
  ink:   { 950: '#0B1020', 800: '#17243C' },
  cyan:  { 400: '#23C7D9', 700: '#0A6B77' },
  gold:  { 500: '<PENDIENTE: valor oficial del logo>', 700: '<derivado>' },
  mist:  { 50:  '#F4F6FA' },
  slate: { 300: '#B4BDCB', 500: '#7B879B', 700: '#4A5568' },
}
fontFamily: {
  display: ['Manrope', 'system-ui', 'sans-serif'],
  sans:    ['Inter', 'system-ui', 'sans-serif'],
}
```
Se definirán como variables CSS (`--color-*`) en Tailwind v4 (`@theme`) para permitir temas por sección.
