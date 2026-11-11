# Plan de implementación — parrasanlabs.com

> **Estado:** Fase 0 aprobada conceptualmente (revisión 2, 2026-11-11).
> **Siguiente paso:** crear `parrasan-labs/parrasanlabs-web` (privado) → PR-00 → PR-01 a PR-03 → **aprobación visual de Inicio**.
> No se despliega en producción hasta autorización explícita.

## 1. Reglas de trabajo

1. **Un entregable = un PR** con objetivo claro. Se aceptan PRs más grandes que en la revisión 1, pero cada uno debe poder revisarse en una sesión (orientativo: < 800 líneas de diff sin contar lockfiles ni assets).
2. Cada PR incluye **pruebas** de lo que cambia y CI en verde.
3. **Revisión y autorización de Charlie Parra** en cada PR. La autorización es el merge manual (ver ARCHITECTURE §12.2). **Sin auto-merge.**
4. PRs abiertos como **borrador**; pasan a "Ready for review" con CI en verde, capturas (escritorio 1440 px y móvil 375 px si hay UI) y checklist completo.
5. Todo ocurre en `parrasan-labs/parrasanlabs-web`. **Nada** se fusiona ni se despliega desde `scr-website`.
6. **Prohibido en cualquier PR:** cambios de DNS, dominio, MX/SPF/DKIM/DMARC o correo corporativo; contenido inventado (clientes, testimonios, métricas, casos de éxito); datos legales o del equipo no confirmados; recrear el logo en código.
7. Despliegue a producción: solo en PR-11 y solo con autorización explícita.

## 2. Resumen de entregables (12)

| PR | Entregable | Puerta de aprobación |
|----|------------|----------------------|
| PR-00 | Documentación de Fase 0 (traslado verificado) | Aprobación de docs |
| PR-01 | Fundaciones: proyecto, calidad y CI | — |
| PR-02 | Sistema de diseño: tokens, fuentes, ranuras del logo, guía de estilo | — |
| PR-03 | Estructura: layout, SEO base, i18n, encabezado y footer | — |
| **PR-04** | **Prototipo visual de Inicio (escritorio + móvil)** | **⏸ Aprobación visual antes de continuar** |
| PR-05 | Soluciones y Cómo trabajamos | — |
| PR-06 | Aplicaciones, Nosotros y 404 | — |
| PR-07 | Contacto: interfaz y endpoint seguro (con envío simulado) | — |
| PR-08 | Postmark + validación de entrega real | ⏸ Entrega validada |
| PR-09 | Privacidad, Términos y activación del formulario | ⏸ Política revisada + datos legales |
| PR-10 | SEO final, seguridad y Web Analytics | — |
| PR-11 | Preparación de lanzamiento y despliegue a producción | ⏸ Autorización de producción y de dominio |
| (futuro) | Versión en inglés | Decisión aparte |

Cambios respecto a la revisión 1 (20 PRs): se agruparon herramientas + CI (PR-01), componentes con sus páginas (PR-05/06), formulario UI + endpoint (PR-07), y SEO + seguridad + analítica (PR-10). Las pruebas y controles de calidad se mantienen íntegros.

## 3. Detalle

### PR-00 · Documentación de Fase 0
- Contenido: los seis documentos + `docs/CHECKSUMS.sha256`.
- Verificación: `sha256sum -c docs/CHECKSUMS.sha256` → todos `OK`; salida pegada en el PR.
- ✅ Aceptación: documentos idénticos a los aprobados; aprobados por Charlie Parra. Después, eliminar la rama temporal de `scr-website` (con autorización).

### PR-01 · Fundaciones: proyecto, calidad y CI
- Astro 5 + TypeScript `strict`, `@astrojs/cloudflare`, `wrangler.jsonc` (sin dominio de producción), `.nvmrc`, `.editorconfig`, `.gitignore` (incluye `.dev.vars`), README.
- ESLint (astro, ts, jsx-a11y), Prettier, Vitest, Playwright + `@axe-core/playwright`, Lighthouse CI, comprobador de enlaces.
- *Content guard*: script que falla ante palabras vetadas ("garantizado", "100 %", "el mejor", "testimonio", "clientes satisfechos") y ante escenarios sin `illustrative: true`.
- GitHub Actions: install → lint → typecheck → unit → build → e2e + axe → Lighthouse → enlaces → content guard.
- `.github/pull_request_template.md` (incluye casilla "Revisado y autorizado por Charlie Parra"), `CODEOWNERS` (`* @<usuario de Charlie>`), Dependabot.
- README documenta la configuración de protección de `main` (ARCHITECTURE §12.2) y la limitación de plan para repos privados.
- ✅ Aceptación: `npm ci && npm run build && npm test && npm run e2e` funcionan desde cero; una violación a11y intencional hace fallar CI (verificado y revertido); auto-merge desactivado en el repositorio.

### PR-02 · Sistema de diseño
- Tailwind v4 con `@theme`: paleta aprobada, tonos accesibles, `gold-500` (provisional y rotulado hasta recibir el valor oficial), temas por sección `data-theme="dark|light"`.
- Fuentes autoalojadas: Manrope 600/700, Inter 400/500 (WOFF2, subset latin + latin-ext).
- Ranuras de marca: `src/assets/brand/brand.ts` + componente `Logo` con sustituto de texto. Sin dibujar el logo.
- Componentes base: `Button`, `SectionHeading`, `SkipLink`.
- Guía de estilo `/_styleguide/` (no se genera en producción).
- Pruebas: test de contraste sobre todos los pares texto/fondo declarados (incluye dorado); test de que `Logo` funciona con y sin archivos; axe sobre la guía.
- ✅ Aceptación: cambiar los archivos del logo y `brand.ts` no requiere tocar componentes (demostrado en un test con archivos de ejemplo neutros, no con un logo inventado).

### PR-03 · Estructura
- `BaseLayout` (lang, meta, canonical, Open Graph, JSON-LD `Organization`/`WebSite` **sin** `address` ni razón social), `src/i18n/{config,routes,es}.ts`, `localizedPath`, bandera EN desactivada.
- `Header` oscuro (escritorio y menú móvil accesible) y `Footer` (nombre comercial, "Costa Rica", correo).
- Cloudflare Web Analytics preparado detrás de `PUBLIC_CF_ANALYTICS_TOKEN` (vacío = no se carga).
- Pruebas: unit de SEO e i18n; e2e de teclado (Tab, Esc, foco de retorno), `aria-expanded`, `aria-current`, un solo H1, 320 px y zoom 400 %.

### PR-04 · Prototipo visual de Inicio ⏸
- Página de Inicio completa según CONTENT_MAP §1 y wireframes §10.1–10.2, identidad A: hero, "¿Te suena familiar?", Qué hacemos, Paso a paso, Cómo pensamos la IA, CTA final.
- `FlowDiagram` (entrada → agente → revisión humana en dorado → resultado), `SolutionCard`, `ProcessTimeline`, `PrincipleItem`, `CTASection`.
- Entrega para revisión: capturas a 1440 px, 1024 px, 768 px, 375 px y 320 px; capturas con `prefers-reduced-motion`; informe Lighthouse y axe. *Preview* en `workers.dev` solo si se autoriza conectar la cuenta de Cloudflare.
- ✅ Aceptación: aprobación explícita de la experiencia visual en escritorio y móvil. **No se inicia PR-05 sin esta aprobación.** Los ajustes pedidos se hacen en este mismo PR.

### PR-05 · Soluciones y Cómo trabajamos
- `SolutionBlock`, sub-navegación por anclas, diagramas por solución, `FAQ` (`FAQPage` solo con preguntas visibles), bloque "Lo que la IA no debería hacer sola".
- Preguntas con respuesta pendiente (p. ej. "¿fuera de Costa Rica?") no se publican.
- Pruebas: e2e de anclas y navegación, axe, Lighthouse, JSON-LD válido.

### PR-06 · Aplicaciones, Nosotros y 404
- `ScenarioCard` con rótulo visible "Escenario ilustrativo"; filtro progresivo (sin JS se ven todas).
- Nosotros **sin sección de equipo** hasta tener datos confirmados.
- Pruebas: test de esquema (escenario sin `illustrative: true` = build fallido); e2e del filtro con y sin JS; axe.

### PR-07 · Contacto: interfaz y endpoint seguro
- Página en tema claro con encabezado oscuro; formulario accesible; aviso de datos sensibles; consentimiento no premarcado.
- `/api/contacto`: comprobación de origen y tamaño, rate limiting, Zod, honeypot, Turnstile, `Mailer` (en este PR solo `MockMailer`), `SubmissionStore` = `NoopStore`.
- Bandera `PUBLIC_CONTACT_FORM_ENABLED=false`: la página muestra solo el correo.
- Pruebas: unit del esquema y del handler (303/400/429/502, honeypot, Turnstile inválido, origen inválido, cuerpo excedido); e2e con y sin JS usando claves de prueba de Turnstile; axe en estados de error.

### PR-08 · Postmark y validación de entrega ⏸
- `PostmarkMailer` (texto plano, `ReplyTo`, sin seguimiento), secretos vía `wrangler secret`. Sender Signature verificada por correo, **sin DNS**.
- Pruebas: test de contrato con respuestas simuladas de Postmark (éxito, 401, 422, 5xx); sin datos personales en logs.
- Validación: ≥ 3 envíos reales desde *preview* recibidos en bandeja de entrada de `cparra@parrasanlabs.com`, documentados en el PR. Si llegan a spam o se rechazan, se detiene y se presenta la opción de verificación de dominio como decisión aparte (requiere autorización DNS).

### PR-09 · Legales y activación del formulario ⏸
- `/privacidad/` y `/terminos/` en tema claro.
- **Bloqueante:** razón social, cédula jurídica y domicilio legal confirmados; política revisada. Sin esos datos, el PR queda abierto y el formulario sigue desactivado.
- Al aprobarse: `PUBLIC_CONTACT_FORM_ENABLED=true` (ARCHITECTURE §6.5).

### PR-10 · SEO final, seguridad y Web Analytics
- Sitemap, robots por entorno, `X-Robots-Tag: noindex` en *previews*, imágenes Open Graph con el logo oficial (o sin logo si aún no llega), `Service`/`BreadcrumbList`, redirecciones 301, verificación de Search Console por archivo (no DNS).
- Cabeceras: CSP (incluye Turnstile y Web Analytics), HSTS sin `preload`, etc.
- Web Analytics activado con su token.
- Pruebas: e2e de JSON-LD, canonical, sitemap; test de cabeceras.

### PR-11 · Lanzamiento ⏸
- Auditoría manual: VoiceOver iOS, NVDA + Firefox, solo teclado, zoom 400 %, alto contraste de Windows.
- Checklist de lanzamiento (§5) completo.
- Despliegue a producción y conexión del dominio **solo con autorización explícita y específica**. Si requiere crear o cambiar registros DNS, lo hace la persona responsable fuera de este repositorio. `cparra@parrasanlabs.com` debe seguir funcionando (verificación posterior incluida).
- Plan de rollback probado (`wrangler rollback`).

### Futuro · Versión en inglés
- `en.ts`, rutas EN, selector de idioma, `hreflang`, traducción revisada por una persona. Se planifica en su momento.

## 4. Criterios de aceptación globales (Definition of Done)

### Contenido y marca
- [ ] Sin clientes, logotipos de terceros, testimonios, métricas ni casos de éxito inventados.
- [ ] Sin razón social, cédula, domicilio ni integrantes no confirmados.
- [ ] Todo escenario lleva el rótulo visible "Escenario ilustrativo".
- [ ] Sin promesas absolutas ("garantizado", "100 %", "sin errores", "reemplaza").
- [ ] CTA principal con el texto exacto "Hablemos de tu proyecto" hacia `/contacto/`.
- [ ] `cparra@parrasanlabs.com` visible en Contacto y footer.
- [ ] Logo solo desde archivos oficiales (o sustituto de texto); nunca recreado en código.
- [ ] Sin robots genéricos ni imágenes de "IA futurista".
- [ ] Identidad A: sitio predominantemente oscuro; claro solo en Contacto y legales.

### Accesibilidad (WCAG 2.2 AA)
- [ ] 0 violaciones axe *serious/critical*.
- [ ] Contraste ≥ 4.5:1 en texto (≥ 3:1 texto grande y componentes), incluido el dorado.
- [ ] Todo operable con teclado; foco visible (≥ 2 px, ≥ 3:1).
- [ ] Objetivos táctiles ≥ 24×24 px (botones principales ≥ 44 px).
- [ ] Un H1 por página; landmarks; skip link.
- [ ] Formularios con etiquetas visibles y errores textuales asociados.
- [ ] Usable a 320 px y con zoom 400 % sin scroll horizontal.
- [ ] `prefers-reduced-motion` respetado.

### Rendimiento y SEO
- [ ] Lighthouse móvil: Performance ≥ 90, Accessibility = 100, Best Practices ≥ 95, SEO = 100.
- [ ] LCP ≤ 2.5 s, CLS ≤ 0.1, INP ≤ 200 ms (laboratorio).
- [ ] Title/description únicos, canonical, Open Graph, sitemap, robots por entorno.
- [ ] JSON-LD válido, sin reseñas ni datos legales no confirmados.

### Privacidad y seguridad
- [ ] Formulario desactivado hasta cumplir ARCHITECTURE §6.5.
- [ ] Consentimiento no premarcado y validado en servidor.
- [ ] Sin almacenamiento de envíos en el MVP.
- [ ] Sin cookies de terceros; analítica solo Cloudflare Web Analytics.
- [ ] Secretos fuera del repositorio; sin datos personales en logs.

### Proceso
- [ ] CI en verde; autorizado y fusionado manualmente por Charlie Parra.
- [ ] Ningún cambio de DNS, dominio ni correo corporativo.

## 5. Checklist de lanzamiento
- [ ] Datos pendientes de PRODUCT_BRIEF §9 resueltos o con decisión explícita de omitir.
- [ ] Archivos oficiales del logo integrados (o decisión explícita de lanzar con el sustituto).
- [ ] Entrega real del formulario validada (PR-08) y formulario activado (PR-09).
- [ ] 404, redirecciones 301 y `noindex` de previews verificados.
- [ ] Search Console verificado (por archivo/meta) y sitemap enviado.
- [ ] Prueba de comprensión con 3–5 personas de la audiencia.
- [ ] Autorización escrita de producción y, aparte, del dominio.
- [ ] Tras el lanzamiento: `cparra@parrasanlabs.com` recibe correo con normalidad.

## 6. Lo que se necesita para avanzar

| Para | Necesitamos | De quién |
|------|-------------|----------|
| PR-00 | Crear `parrasan-labs/parrasanlabs-web` (privado, vacío) y dar acceso a la sesión de Claude | Charlie Parra |
| PR-01 | Confirmar el plan de GitHub de la organización (para las reglas de protección en repo privado) y el usuario de GitHub para `CODEOWNERS` | Charlie Parra |
| PR-02 / PR-04 | Archivos oficiales del logo (SVG) y valor exacto del dorado. Si aún no están, el prototipo usa el sustituto de texto y un dorado provisional rotulado | Equipo de marca |
| *Preview* (opcional) | Autorización para conectar una cuenta de Cloudflare y desplegar en `workers.dev` | Charlie Parra |
| PR-08 | Cuenta de Postmark y dirección a verificar | Charlie Parra |
| PR-09 | Razón social, cédula jurídica, domicilio legal; revisión de la política | Parrasan Labs |
