# Sitemap y arquitectura de información — parrasanlabs.com

> **Estado:** Fase 0 · Borrador para aprobación

## 1. Mapa del sitio

```
parrasanlabs.com/                         (ES, idioma por defecto)
├── /                                     Inicio
├── /soluciones/                          Soluciones (resumen de las 4 líneas)
│   ├── #atencion-al-cliente              (anclas en v1; páginas propias en v2 si hay contenido)
│   ├── #conciliacion-financiera
│   ├── #gestion-documental
│   └── #automatizacion-de-procesos
├── /como-trabajamos/                     Cómo trabajamos (metodología por etapas)
├── /aplicaciones/                        Aplicaciones (escenarios por área / industria)
├── /nosotros/                            Nosotros
├── /contacto/                            Contacto (formulario + correo)
│   └── /contacto/gracias/                Confirmación post-envío (noindex)
├── /privacidad/                          Política de privacidad
├── /terminos/                            Términos de uso
└── /404                                  Página no encontrada

parrasanlabs.com/en/                      (FUTURO — no publicado en v1)
├── /en/
├── /en/solutions/
├── /en/how-we-work/
├── /en/use-cases/
├── /en/about/
├── /en/contact/
│   └── /en/contact/thank-you/
├── /en/privacy/
└── /en/terms/
```

## 2. Convenciones de URL

- Minúsculas, sin acentos, palabras separadas por guion: `/como-trabajamos/`.
- Barra final consistente (`trailingSlash: 'always'` en Astro).
- Español sin prefijo (`/`), inglés con prefijo `/en/` (estrategia *prefix-other-locales*).
- Slugs **traducidos** en inglés (`/en/how-we-work/`), mapeados en un diccionario de rutas central (ver `ARCHITECTURE.md §5`).
- Redirecciones 301 desde variantes comunes: `/contact` → `/contacto/`, `/servicios/` → `/soluciones/`, `/about` → `/nosotros/`.

## 3. Navegación

### 3.1 Encabezado (desktop)

```
[Parrasan Labs]   Soluciones  Cómo trabajamos  Aplicaciones  Nosotros   [ES|EN*]  [Hablemos de tu proyecto]
```
`*` El selector de idioma permanece oculto hasta que exista la versión EN.

- "Contacto" no aparece como enlace de texto: el botón CTA cumple esa función (evita duplicidad).
- Página activa marcada con `aria-current="page"` y subrayado.

### 3.2 Encabezado (móvil)

```
[Parrasan Labs]                                   [☰ Menú]
```
Menú desplegable a pantalla completa con los 4 enlaces + CTA destacado al final + correo.

### 3.3 Footer

```
Parrasan Labs                 Sitio              Legal                Contacto
Agentes de IA y              Soluciones          Privacidad           cparra@parrasanlabs.com
automatización               Cómo trabajamos     Términos             San José, Costa Rica*
empresarial.                 Aplicaciones
Hecho en Costa Rica.         Nosotros
                             Contacto
──────────────────────────────────────────────────────────────────────────────
© 2026 Parrasan Labs*. Todos los derechos reservados.
```
`*` Razón social y ubicación exactas por confirmar.

## 4. Flujos principales

### Flujo A — Visitante con un problema concreto (ej. conciliación)
`Inicio` → tarjeta *Conciliación financiera* → `/soluciones/#conciliacion-financiera` → *Cómo trabajamos* (confianza) → `Hablemos de tu proyecto` → `/contacto/` (campo "proceso" preseleccionado vía `?solucion=conciliacion`) → `/contacto/gracias/`

### Flujo B — Visitante explorando ("¿qué puede hacer la IA por mi empresa?")
`Inicio` → `/aplicaciones/` (escenarios por área) → `/soluciones/` → `/contacto/`

### Flujo C — Visitante evaluando confianza
`Inicio` → `/nosotros/` → `/como-trabajamos/` → `/privacidad/` → `/contacto/`

### Flujo D — Contacto directo
Cualquier página → botón CTA del encabezado → `/contacto/` → formulario **o** `mailto:`

## 5. Prioridad de páginas (para orden de implementación)

| Prioridad | Página | Razón |
|-----------|--------|-------|
| P0 | Inicio, Contacto, Privacidad | Mínimo viable para captar prospectos legalmente |
| P1 | Soluciones, Cómo trabajamos | Explican y generan confianza |
| P2 | Aplicaciones, Nosotros, Términos | Profundizan; dependen de insumos del cliente (fotos, bios) |
| P3 | Versión EN | Futuro |

## 6. Páginas indexables vs. no indexables

| Indexable | No indexable (`noindex`) |
|-----------|---------------------------|
| `/`, `/soluciones/`, `/como-trabajamos/`, `/aplicaciones/`, `/nosotros/`, `/contacto/`, `/privacidad/`, `/terminos/` | `/contacto/gracias/`, `/404`, entornos de *preview* (`*.workers.dev`, ramas) |
