# Plan de implementación — parrasanlabs.com

> **Estado:** Fase 0 completada en borrador · **En espera de aprobación.**
> No se implementa ni despliega nada hasta recibir aprobación explícita.

## 1. Reglas de trabajo

1. **PRs pequeños** (idealmente < 400 líneas de diff, sin contar lockfiles ni assets), un objetivo por PR.
2. Cada PR incluye **pruebas** de lo que cambia y pasa CI en verde.
3. **Revisión humana obligatoria** (mín. 1 aprobación de CODEOWNER) antes de *merge*. Nada se fusiona automáticamente.
4. Rama protegida `main`: sin *push* directo, *squash merge*, checks requeridos.
5. Cada PR describe: objetivo, capturas móvil + escritorio (si hay UI), cómo probar, checklist de criterios.
6. **Prohibido en cualquier PR:** cambios de DNS, MX/SPF/DKIM/DMARC, configuración de correo, contenido inventado (clientes, testimonios, métricas, casos de éxito).
7. Contenido marcado `[POR CONFIRMAR]` no se publica en producción hasta tener el dato real (o se omite la sección).

## 2. Fases

```
Fase 0  Documentación ............................ ✅ (este PR) → ⏸ aprobación
Fase 1  Fundaciones (repo, tooling, CI, tokens) ... PR-01 a PR-04
Fase 2  Estructura y componentes base ............. PR-05 a PR-07
Fase 3  Páginas .................................... PR-08 a PR-13
Fase 4  Formulario de contacto ..................... PR-14 a PR-16
Fase 5  SEO, legal, calidad y lanzamiento .......... PR-17 a PR-20
Fase 6  Versión en inglés (futuro) ................. PR-21+
```

## 3. Detalle de PRs

### Fase 0 — Documentación
**PR-00 · Documentación de Fase 0**
- Archivos: `docs/PRODUCT_BRIEF.md`, `SITEMAP.md`, `CONTENT_MAP.md`, `DESIGN_SYSTEM.md`, `ARCHITECTURE.md`, `IMPLEMENTATION_PLAN.md`.
- Aceptación: revisión y aprobación de Parrasan Labs; elección de propuesta visual; respuestas a preguntas abiertas críticas.

### Fase 1 — Fundaciones
**PR-01 · Scaffolding Astro + TypeScript**
- Crear repo `parrasan-labs/parrasanlabs-web` (por una persona con permisos en la organización), Astro 5, TS `strict`, `.editorconfig`, `.nvmrc`, README, licencia `[POR CONFIRMAR]`, `.gitignore` (incluye `.dev.vars`).
- Pruebas: `astro check` y `astro build` en verde.
- ✅ Criterios: `npm ci && npm run build` funciona desde cero; página placeholder `noindex`.

**PR-02 · Lint, formato y pruebas base**
- ESLint (astro, ts, jsx-a11y), Prettier, Vitest, Playwright + axe con una prueba de humo.
- ✅ Criterios: `npm run lint`, `npm test`, `npm run e2e` pasan; una violación a11y intencional hace fallar la prueba (verificado localmente y revertido).

**PR-03 · CI en GitHub Actions + plantilla de PR + CODEOWNERS**
- Jobs: install (cache) → lint → typecheck → unit → build → e2e+axe → Lighthouse CI → link check → *content guard*.
- Protección de rama documentada en README (aplicada manualmente por admin).
- ✅ Criterios: CI corre en PR y bloquea si falla cualquier job.

**PR-04 · Tailwind v4 + tokens del design system + fuentes autoalojadas**
- `global.css` con `@theme` (colores, tipografía, espaciados), fuentes WOFF2 subset, página `/_styleguide` (`noindex`, excluida del sitemap y de producción).
- Pruebas: snapshot visual de la guía; test de contraste automatizado sobre pares de tokens definidos.
- ✅ Criterios: todos los pares texto/fondo usados cumplen AA (tabla DESIGN_SYSTEM §2.3).

### Fase 2 — Estructura y componentes
**PR-05 · BaseLayout, SEO helper e i18n base**
- `BaseLayout` (lang, meta, canonical, OG, JSON-LD `Organization`/`WebSite`), `SkipLink`, `src/i18n/{config,routes,es}.ts`, helper `localizedPath`.
- Pruebas: unit de `seo.ts` y `routes.ts`; e2e verifica `<html lang>`, un solo H1 y canonical.
- ✅ Criterios: ningún enlace interno escrito a mano; EN desactivado por flag.

**PR-06 · Header (desktop + menú móvil) y Footer**
- Pruebas: e2e de teclado (Tab, Shift+Tab, Esc cierra menú, foco vuelve al botón), `aria-expanded`, `aria-current`.
- ✅ Criterios: usable a 320 px y con zoom 400 %; CTA visible en el header en todos los tamaños.

**PR-07 · Componentes de contenido**
- `Button`, `SectionHeading`, `SolutionCard`, `ProcessTimeline`, `ScenarioCard`, `PrincipleItem`, `FAQ`, `CTASection`, `FlowDiagram`.
- Esquemas de Content Collections (con `illustrative: true` obligatorio en escenarios).
- Pruebas: render de cada componente en `/_styleguide` + axe; test de esquema que falla si un escenario no es ilustrativo.

### Fase 3 — Páginas (una por PR, contenido de CONTENT_MAP)
| PR | Página | Dependencias de contenido |
|----|--------|---------------------------|
| PR-08 | Inicio | — |
| PR-09 | Soluciones | Diagramas de flujo por solución |
| PR-10 | Cómo trabajamos (+ `FAQPage`) | Respuesta "¿fuera de CR?" |
| PR-11 | Aplicaciones (+ filtro progresivo) | — |
| PR-12 | Nosotros | **Fotos y bios reales autorizadas** — si no llegan, se publica sin sección de equipo |
| PR-13 | 404 | — |

Cada PR de página:
- Pruebas: e2e de navegación, axe sin violaciones *serious/critical*, Lighthouse móvil en umbrales, snapshot de título/description.
- Capturas a 375 px y 1440 px en la descripción del PR.
- Revisión editorial (lenguaje sencillo, sin promesas) por Parrasan Labs.

### Fase 4 — Formulario
**PR-14 · UI del formulario (sin envío real)**
- Formulario accesible, validación cliente con esquema Zod compartido, estados, aviso de datos sensibles, consentimiento.
- Pruebas: unit del esquema; e2e de errores (resumen enfocable, `aria-invalid`, mensajes por campo), funcionamiento sin JS.

**PR-15 · Endpoint `/api/contacto` con Turnstile, honeypot y rate limit**
- Mailer detrás de interfaz con implementación *mock* en tests y en preview.
- Pruebas: unit del handler (400/429/502/303, honeypot, Turnstile inválido, tamaño excedido, origen inválido); e2e contra `wrangler dev` con claves de prueba de Turnstile.

**PR-16 · Integración del proveedor de correo (decisión de ARCHITECTURE §6.4)**
- **Requiere aprobación previa de la opción elegida.** Sin cambios de DNS.
- Pruebas: test de contrato del adaptador; prueba manual documentada desde preview hacia `cparra@parrasanlabs.com`.
- ✅ Criterios: correo en texto plano, `Reply-To` correcto, campos escapados, sin datos personales en logs.

### Fase 5 — SEO, legal, calidad, lanzamiento
**PR-17 · Privacidad y Términos**
- Texto **revisado por asesoría legal** (bloqueante). Datos de responsable confirmados.

**PR-18 · SEO final**
- Sitemap, robots por entorno, OG images, `Service`/`BreadcrumbList`, 301s, `X-Robots-Tag` en previews, verificación de Search Console por archivo.
- Pruebas: e2e valida JSON-LD parseable, canonical absolutos, sitemap sin `noindex`.

**PR-19 · Cabeceras de seguridad, analítica opcional, auditoría final**
- CSP, HSTS, etc.; Cloudflare Web Analytics si se aprobó.
- Auditoría manual: VoiceOver iOS, NVDA + Firefox, solo teclado, zoom 400 %, modo alto contraste de Windows.
- ✅ Criterios: securityheaders.com ≥ A; checklist de lanzamiento (§5) completo.

**PR-20 · Lanzamiento**
- Configuración de Custom Domain/Route del Worker **solo con aprobación explícita** del responsable de la zona. Si requiere registros DNS, se ejecuta fuera de este repo por la persona responsable.
- Plan de rollback probado en preview.

### Fase 6 — Inglés (futuro)
- PR-21: `en.ts` + rutas EN + selector de idioma + `hreflang`, detrás de flag.
- PR-22+: una página traducida por PR, con revisión de traductor humano.
- Activación del flag solo con todas las páginas P0–P1 traducidas.

## 4. Criterios de aceptación globales (Definition of Done)

### Contenido
- [ ] Sin clientes, logotipos, testimonios, métricas ni casos de éxito inventados.
- [ ] Todo escenario lleva el rótulo visible "Escenario ilustrativo".
- [ ] Sin promesas absolutas ("garantizado", "100 %", "sin errores", "reemplaza").
- [ ] CTA principal con el texto exacto "Hablemos de tu proyecto" y apuntando a `/contacto/`.
- [ ] `cparra@parrasanlabs.com` visible en Contacto y Footer.
- [ ] Sin imágenes de robots genéricos ni stock de "IA futurista".

### Accesibilidad (WCAG 2.2 AA)
- [ ] 0 violaciones axe *serious/critical* en todas las páginas.
- [ ] Contraste de texto ≥ 4.5:1 (≥ 3:1 texto grande y componentes UI).
- [ ] Todo operable con teclado; foco visible (≥ 2 px, ≥ 3:1); sin trampas de foco salvo el menú modal (con salida por Esc).
- [ ] Objetivos táctiles ≥ 24×24 px (botones principales ≥ 44 px).
- [ ] Un H1 por página; landmarks (`header`, `nav`, `main`, `footer`); skip link.
- [ ] Formularios con `<label>` visibles, errores textuales asociados, sin depender solo del color.
- [ ] Usable a 320 px de ancho y con zoom 400 % sin scroll horizontal.
- [ ] `prefers-reduced-motion` respetado.
- [ ] Imágenes con `alt` adecuado; diagramas con alternativa textual.

### Rendimiento y SEO
- [ ] Lighthouse móvil: Performance ≥ 90, Accessibility = 100, Best Practices ≥ 95, SEO = 100.
- [ ] LCP ≤ 2.5 s, CLS ≤ 0.1, INP ≤ 200 ms (laboratorio).
- [ ] Title y description únicos; canonical; OG; sitemap; robots por entorno.
- [ ] JSON-LD válido y sin tipos de reseñas.

### Privacidad y seguridad
- [ ] Consentimiento no premarcado y validado en servidor.
- [ ] Política de privacidad revisada por asesoría legal y publicada antes de habilitar el formulario.
- [ ] Sin cookies de terceros ni rastreadores publicitarios.
- [ ] Secretos fuera del repo; sin datos personales en logs.
- [ ] Cabeceras de seguridad configuradas.

### Proceso
- [ ] CI en verde; revisión humana aprobada; capturas móvil y escritorio adjuntas.
- [ ] Ningún cambio de DNS ni configuración de correo.

## 5. Checklist de lanzamiento
- [ ] Todas las preguntas abiertas de PRODUCT_BRIEF §9 respondidas o con decisión explícita de omitir.
- [ ] Prueba de envío real del formulario recibida en `cparra@parrasanlabs.com`.
- [ ] 404, redirecciones 301 y `noindex` de previews verificados.
- [ ] Search Console verificado (por archivo/meta) y sitemap enviado.
- [ ] Prueba de comprensión con 3–5 personas de la audiencia realizada y hallazgos atendidos.
- [ ] Aprobación final por escrito de Parrasan Labs.

## 6. Decisiones que necesitan aprobación ahora (bloquean Fase 1)

1. **Propuesta visual:** A (Nocturna precisa), B (Claridad editorial, recomendada) o combinación.
2. **Tipografía:** Inter + Inter Tight (recomendado) o Inter + Manrope.
3. **Envío de correo** sin cambios DNS: remitente único verificado (recomendado) ± respaldo en D1.
4. **Analítica:** Cloudflare Web Analytics sin cookies (recomendado) o ninguna.
5. **Creación del repositorio** `parrasan-labs/parrasanlabs-web` por alguien con permisos en la organización (esta documentación se trasladará allí como PR-00).
6. **Revisor(es) humano(s)** / CODEOWNERS.
7. Datos legales y del equipo (PRODUCT_BRIEF §9).
