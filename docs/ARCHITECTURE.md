# Arquitectura — parrasanlabs.com

> **Estado:** Fase 0 · Aprobado conceptualmente (revisión 2, 2026-11-11). **No se ha escrito código ni desplegado nada.**

## 1. Decisiones principales

| Decisión | Elección | Motivo |
|----------|----------|--------|
| Framework | **Astro 5** (salida estática + un endpoint de servidor) | HTML estático por defecto, cero JS innecesario, i18n nativo, excelente SEO |
| Lenguaje | **TypeScript** (`strict`) | Seguridad de tipos en contenido, formularios y Worker |
| Estilos | **Tailwind CSS v4** (`@tailwindcss/vite`) | Tokens del design system como variables CSS; sin CSS muerto |
| Hosting / runtime | **Cloudflare Workers** con *Static Assets* + `@astrojs/cloudflare` | Borde global, sin servidores que mantener, un solo despliegue para estáticos + API |
| Anti-spam | **Cloudflare Turnstile** + honeypot + Rate Limiting binding | Sin CAPTCHA molestos, sin cookies de terceros de seguimiento |
| Analítica | **Cloudflare Web Analytics** (sin cookies) — **aprobada** | Evita banner de cookies y minimiza datos personales |
| Envío del formulario | **Postmark**, remitente verificado individualmente — **aprobado** | No requiere cambios DNS (D5, D11) |
| Almacenamiento de envíos | **Ninguno en el MVP**; interfaz preparada para D1 | Minimización de datos; se agrega después sin cambiar el endpoint |
| Contenido | **Astro Content Collections** (Markdown/MDX + esquemas Zod) | Contenido separado del código, validado en build, listo para traducción |
| Pruebas | Vitest (unidad), Playwright (e2e), axe-core (a11y), Lighthouse CI | Ver §9 |
| CI | GitHub Actions | Checks obligatorios antes de *merge* |

### Independencia de `parrasan-assist` y de `scr-website`
- Repositorio propio y **privado**: `parrasan-labs/parrasanlabs-web`. Es el único lugar donde vive el proyecto; nada se fusiona ni se despliega desde `scr-website` (la rama temporal de ese repo se elimina tras verificar PR-00).
- Worker propio (`parrasanlabs-web`), secretos propios, sin dependencias de código ni bases de datos compartidas.
- Ningún paquete interno de `parrasan-assist` se importa.

## 2. Diagrama de alto nivel

```
                    ┌───────────────────────────────────────────────┐
 Visitante ──HTTPS──▶  Cloudflare (zona parrasanlabs.com, existente) │
                    │                                               │
                    │  Worker "parrasanlabs-web"                    │
                    │  ├── Static Assets: HTML/CSS/JS/fuentes/img   │  ← 99 % de las peticiones
                    │  └── POST /api/contacto  (endpoint Astro SSR) │
                    │        │ 1. Rate limit (binding)              │
                    │        │ 2. Validación Zod                    │
                    │        │ 3. Honeypot                          │
                    │        │ 4. Turnstile siteverify ─────────────┼──▶ challenges.cloudflare.com
                    │        │ 5. Envío de notificación ────────────┼──▶ api.postmarkapp.com (§6.4)
                    │        │ (6. Guardado opcional, futuro: D1)    │
                    │        ▼                                      │
                    │     303 → /contacto/gracias/                  │
                    └───────────────────────────────────────────────┘
                                       │
                                       ▼
                            cparra@parrasanlabs.com (buzón existente, sin cambios)
```

## 3. Estructura del repositorio (propuesta)

```
parrasanlabs-web/
├── .github/
│   ├── workflows/ci.yml              # lint, typecheck, test, build, e2e, a11y, lighthouse
│   ├── workflows/preview.yml         # despliegue a preview, solo si se autoriza conectar Cloudflare
│   ├── pull_request_template.md
│   └── CODEOWNERS
├── docs/                             # Esta documentación (Fase 0)
├── public/
│   ├── fonts/                        # WOFF2 autoalojadas
│   ├── favicon.svg
│   └── robots.txt                    # generado o estático por entorno
├── src/
│   ├── assets/brand/                 # archivos oficiales del logo + brand.ts (DESIGN_SYSTEM §2.5)
│   ├── components/                   # Logo, Button, Header, Footer, SolutionCard, ContactForm…
│   ├── layouts/BaseLayout.astro      # <html lang>, meta SEO, skip link, header/footer
│   ├── content/
│   │   ├── config.ts                 # esquemas Zod
│   │   ├── pages/es/*.md             # contenido por página
│   │   ├── solutions/es/*.md
│   │   └── scenarios/es/*.md         # campo obligatorio illustrative: true
│   ├── i18n/
│   │   ├── config.ts                 # locales, defaultLocale
│   │   ├── routes.ts                 # mapa de slugs ES ↔ EN
│   │   ├── es.ts                     # microcopy UI
│   │   └── en.ts                     # (futuro)
│   ├── lib/
│   │   ├── seo.ts                    # helpers de meta, canonical, hreflang, JSON-LD
│   │   ├── contact-schema.ts         # esquema Zod compartido cliente/servidor
│   │   ├── mailer/                   # interfaz Mailer + PostmarkMailer + MockMailer
│   │   └── store/                    # interfaz SubmissionStore + NoopStore (D1 en el futuro)
│   ├── pages/
│   │   ├── index.astro
│   │   ├── soluciones.astro
│   │   ├── como-trabajamos.astro
│   │   ├── aplicaciones.astro
│   │   ├── nosotros.astro
│   │   ├── contacto/index.astro
│   │   ├── contacto/gracias.astro
│   │   ├── privacidad.astro
│   │   ├── terminos.astro
│   │   ├── 404.astro
│   │   └── api/contacto.ts           # export const prerender = false
│   └── styles/global.css             # @import "tailwindcss"; @theme { tokens }
├── tests/
│   ├── unit/                         # Vitest
│   └── e2e/                          # Playwright + @axe-core/playwright
├── astro.config.mjs
├── wrangler.jsonc
├── tsconfig.json
├── package.json
└── README.md
```

## 4. Renderizado

- `output: 'static'` con el adaptador de Cloudflare; **todas** las páginas se prerenderizan.
- Única ruta dinámica: `src/pages/api/contacto.ts` (`prerender = false`).
- JS en el cliente limitado a: menú móvil, mejora progresiva del formulario (validación en vivo, estado "Enviando…"), widget Turnstile y filtro de Aplicaciones. Todo funciona sin JS salvo Turnstile (si no hay JS, se muestra mensaje con el correo alternativo).
- Presupuesto: ≤ 30 KB JS (gzip) por página, ≤ 50 KB CSS, LCP ≤ 2.5 s en 4G simulado.

## 5. Internacionalización (preparada, EN no publicado)

```js
// astro.config.mjs (referencia)
i18n: {
  locales: ['es', 'en'],
  defaultLocale: 'es',
  routing: { prefixDefaultLocale: false },
}
```
- Mapa de rutas en `src/i18n/routes.ts`: `{ home: { es: '/', en: '/en/' }, solutions: { es: '/soluciones/', en: '/en/solutions/' }, … }`. Todos los enlaces internos se generan con `localizedPath('solutions', locale)` — nunca se escriben rutas a mano.
- Contenido por idioma en carpetas `es/` / `en/` dentro de cada colección.
- `<html lang="es-CR">` (o `es`) y `lang="en"` en EN.
- `hreflang` (`es`, `en`, `x-default` → ES) se emiten **solo cuando la página traducida existe**.
- Mientras EN no esté aprobado: las páginas `/en/*` no se generan (feature flag `PUBLIC_ENABLE_EN=false`).
- Test que falla si una clave de `es.ts` falta en `en.ts` cuando el flag está activo.

## 6. Formulario de contacto

### 6.1 Flujo
1. Formulario HTML nativo `method="post" action="/api/contacto"` (funciona sin JS).
2. JS opcional: validación en el cliente con el mismo esquema Zod y envío con `fetch`, conservando el contenido ante errores.
3. Endpoint:
   1. Verifica `Content-Type`, tamaño del cuerpo (≤ 16 KB) y origen (`Origin` = dominio propio).
   2. Rate limiting por IP (ej. 5 envíos / 10 min) con el binding `ratelimit` de Workers.
   3. Valida con Zod; rechaza si el honeypot tiene contenido (respuesta 200 silenciosa para no dar pistas).
   4. Verifica el token Turnstile en `https://challenges.cloudflare.com/turnstile/v0/siteverify`.
   5. Envía la notificación con `Mailer` (Postmark, §6.4). `Reply-To` = correo del prospecto.
   6. Llama a `SubmissionStore.save()` — en el MVP es `NoopStore` (no guarda nada).
   6. Responde `303 See Other` → `/contacto/gracias/` (o JSON si fue `fetch`).
4. Errores: 400 (validación, con mensajes por campo), 429 (rate limit), 502 (fallo del proveedor; mostrar correo alternativo).

### 6.2 Esquema (resumen)
`name` (2–100), `email` (email válido, ≤ 254), `company` (2–120), `phone?` (≤ 30), `teamSize?` (enum), `area` (enum), `message` (20–2000), `timeline?` (enum), `consent` (literal `true`), `website` (honeypot, debe estar vacío), `cf-turnstile-response`.

### 6.3 Secretos y variables
| Nombre | Tipo | Dónde |
|--------|------|-------|
| `TURNSTILE_SECRET_KEY` | secreto | `wrangler secret put` |
| `PUBLIC_TURNSTILE_SITE_KEY` | público | variable de entorno de build |
| `POSTMARK_SERVER_TOKEN` | secreto | `wrangler secret put` (token de *Server*, no de cuenta) |
| `POSTMARK_MESSAGE_STREAM` | var | `outbound` (transaccional) |
| `CONTACT_FROM` | var | dirección verificada como *Sender Signature* en Postmark |
| `CONTACT_TO` | var | `cparra@parrasanlabs.com` |
| `PUBLIC_CONTACT_FORM_ENABLED` | var de build | `false` hasta cumplir §6.5 |
| `PUBLIC_CF_ANALYTICS_TOKEN` | var de build | token público del *beacon* de Web Analytics |
Ningún secreto en el repositorio. `.dev.vars` en `.gitignore`.

### 6.4 Envío de correo — **decidido: Postmark con remitente verificado (D5)**

- Se crea en Postmark un *Server* exclusivo para este sitio y una **Sender Signature** para una dirección concreta (la que Parrasan Labs elija, p. ej. `cparra@parrasanlabs.com`). La verificación se hace con un **enlace enviado a ese buzón**: no se crean ni modifican registros DNS, MX, SPF, DKIM ni DMARC, y el buzón sigue funcionando igual.
- El Worker llama a `POST https://api.postmarkapp.com/email` con `X-Postmark-Server-Token`, `MessageStream: outbound`, cuerpo en **texto plano**, `From` = `CONTACT_FROM`, `To` = `CONTACT_TO`, `ReplyTo` = correo del prospecto.
- Sin seguimiento de aperturas ni de enlaces (`TrackOpens: false`, `TrackLinks: None`).
- **Riesgo conocido:** sin firma DKIM del dominio, si `parrasanlabs.com` publica una política DMARC estricta (`quarantine`/`reject`) los mensajes pueden ir a spam o rechazarse. Por eso la entrega real se valida antes de publicar (§6.5). Si falla, la verificación de dominio en Postmark se presentará como **decisión separada** que requiere autorización DNS específica (D11).
- Postmark debe aprobar la cuenta para envíos generales; para el MVP basta con enviar al propio buzón.

Opciones evaluadas en la revisión 1 (referencia):

> Restricción del proyecto: **no se alteran DNS ni configuraciones de correo**. Varias opciones comunes requieren registros DNS (MX, SPF, DKIM), por lo que se documentan con su impacto:

| Opción | ¿Requiere cambios DNS/correo? | Comentario |
|--------|-------------------------------|------------|
| Cloudflare Email Routing + `send_email` binding | **Sí** (habilita Email Routing → MX) | ❌ Descartada sin aprobación explícita |
| Proveedor transaccional con **dominio** verificado (Resend, Postmark, SES) | **Sí** (DKIM/SPF/Return-Path) | ❌ Descartada sin aprobación explícita |
| Proveedor transaccional con **remitente único verificado por correo** (ej. Postmark *Sender Signature*) | No (verificación por enlace en el buzón) | ✅ Candidata v1; entregabilidad aceptable para notificaciones internas |
| Guardar envíos en **Cloudflare D1** + notificación por el proveedor anterior | No | ✅ Respaldo si falla el correo; implica política de retención (ver §8) |

Cualquier cambio de DNS se gestiona fuera de este repositorio, con autorización específica, y fuera del alcance de los PRs.

### 6.5 Condiciones para activar el formulario (`PUBLIC_CONTACT_FORM_ENABLED=true`)
1. Política de privacidad revisada y publicada con los datos reales del responsable (D10).
2. Entrega real validada desde el entorno *preview*: al menos 3 envíos de prueba recibidos en la bandeja de entrada (no en spam) de `cparra@parrasanlabs.com`, con `Reply-To` correcto. Resultado documentado en el PR.
3. Turnstile con claves de producción configuradas.
4. Autorización explícita de Charlie Parra en el PR.

Mientras tanto, `/contacto/` muestra solo el enlace `mailto:`.

### 6.6 Preparación para D1 (fuera del MVP)
- `SubmissionStore` define `save(submission): Promise<void>` y `purgeOlderThan(days)`. El MVP usa `NoopStore`.
- Agregar D1 = nueva implementación `D1Store`, binding en `wrangler.jsonc`, migración SQL, *cron trigger* de purga y actualización de la política de privacidad. El endpoint y el formulario no cambian.

## 7. SEO

### 7.1 Técnico
- HTML semántico, un `H1` por página, jerarquía H2/H3 correcta.
- `<title>` y `meta description` únicos por página (ver CONTENT_MAP).
- `<link rel="canonical">` absoluto en todas las páginas.
- `@astrojs/sitemap` → `/sitemap-index.xml` (excluye `noindex`).
- `robots.txt`: producción permite todo y referencia el sitemap; *previews* y `*.workers.dev` responden `Disallow: /` + cabecera `X-Robots-Tag: noindex`.
- Open Graph + Twitter Card con imagen 1200×630 por página (plantilla generada en build, sin robots).
- `hreflang` cuando exista EN (§5).
- URLs limpias, barra final consistente, 301 de variantes.
- Core Web Vitals: LCP ≤ 2.5 s, CLS ≤ 0.1, INP ≤ 200 ms.

### 7.2 Datos estructurados (JSON-LD)
| Página | Tipo |
|--------|------|
| Todas | `Organization` (nombre comercial, url, logo oficial, `email`; **sin** `address` ni `legalName` hasta que se confirmen) + `WebSite` |
| Soluciones | `Service` por solución (`provider` = Organization, `areaServed`: CR `[POR CONFIRMAR]`) |
| Cómo trabajamos | `FAQPage` (solo con preguntas visibles en la página) |
| Todas internas | `BreadcrumbList` |
- **Sin** `Review`, `AggregateRating` ni testimonios (no existen).

### 7.3 Estrategia de contenido y palabras clave (hipótesis a validar con datos de búsqueda)
- **Temas núcleo:** agentes de IA para empresas · automatización de procesos Costa Rica · conciliación bancaria automática · automatización de atención al cliente WhatsApp · gestión documental con IA · digitalización de facturas.
- **Intención:** informativa-comercial (PYMES que investigan). Cada página de solución responde: qué es, qué necesita, qué no hace.
- **Local:** mención de Costa Rica en títulos clave; Google Business Profile **solo si** Parrasan Labs lo decide (fuera de este repositorio).
- **v2:** sección de recursos/guías (ej. "¿Qué procesos conviene automatizar primero?") para captar búsquedas informativas, sin contenido de relleno.
- Medición: Google Search Console (verificación por **archivo HTML** o meta tag, **no** por DNS).

## 8. Protección de datos y privacidad

Marco: **Ley N.º 8968** (Costa Rica) y su reglamento; buenas prácticas compatibles con GDPR para la futura audiencia internacional.

| Principio | Implementación |
|-----------|----------------|
| Minimización | Solo los campos del §6.2; teléfono y tamaño opcionales; no se piden datos sensibles; aviso explícito en el textarea |
| Consentimiento informado | Checkbox **no** premarcado, enlazado a la política; el envío sin consentimiento se rechaza en servidor |
| Transparencia | `/privacidad/` describe responsable, finalidad, encargados (Cloudflare, proveedor de correo), transferencias internacionales, plazos y derechos |
| Derechos ARCO | Canal: `cparra@parrasanlabs.com`; procedimiento interno documentado `[POR CONFIRMAR]` |
| Retención | MVP: el sitio **no almacena** envíos; solo existen en el buzón de destino y en el registro de mensajes de Postmark (configurar la retención mínima disponible en Postmark). Si en el futuro se agrega D1: purga automática con plazo aprobado |
| Seguridad | HTTPS obligatorio (HSTS), secretos en Wrangler, sin logs con datos personales (se registra solo ID de petición y código de estado), cabeceras de seguridad (§10) |
| Sin rastreo | Sin cookies de terceros, sin píxeles publicitarios, sin Google Fonts remotas; Cloudflare Web Analytics sin cookies → no se prevé banner de cookies (confirmar en la revisión de la política) |
| Terceros | Turnstile y el *beacon* de Web Analytics (Cloudflare) en el navegador; Postmark en el servidor. Todos se mencionan en la política |
| Revisión legal | La política de privacidad y términos **deben** ser revisados por asesoría legal costarricense antes del lanzamiento |

## 9. Calidad y pruebas

| Tipo | Herramienta | Qué valida | Momento |
|------|-------------|-----------|---------|
| Formato / lint | Prettier, ESLint (`eslint-plugin-astro`, `jsx-a11y`) | Estilo y errores comunes | CI, pre-commit opcional |
| Tipos | `astro check`, `tsc --noEmit` | Tipos y esquemas de contenido | CI |
| Unidad | Vitest | Esquema Zod del formulario, helpers SEO/i18n, handler del endpoint (con mocks de Turnstile y mailer) | CI |
| E2E | Playwright (Chromium + WebKit móvil) | Navegación, menú móvil, formulario (éxito, errores, sin JS), 404 | CI |
| Accesibilidad | `@axe-core/playwright` en cada página + pruebas de teclado | 0 violaciones *serious/critical* | CI |
| Rendimiento/SEO | Lighthouse CI | Perf ≥ 90, A11y = 100, Best Practices ≥ 95, SEO = 100 (móvil) | CI |
| Enlaces | `lychee` o similar | Sin enlaces rotos | CI |
| Contenido | Script propio | Falla si aparecen palabras vetadas ("garantizado", "100 %", "el mejor", "testimonio") o escenarios sin `illustrative: true` | CI |
| Manual | Revisión humana | Lectores de pantalla (VoiceOver iOS, NVDA), zoom 200 %/400 %, contenido | Por PR |

## 10. Seguridad

- Cabeceras (vía `_headers` de Static Assets o middleware del Worker):
  - `Content-Security-Policy: default-src 'self'; script-src 'self' https://challenges.cloudflare.com; frame-src https://challenges.cloudflare.com; connect-src 'self'; img-src 'self' data:; style-src 'self'; font-src 'self'; form-action 'self'; frame-ancestors 'none'; base-uri 'self'` + para Web Analytics: `script-src https://static.cloudflareinsights.com` y `connect-src https://cloudflareinsights.com`
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains` (sin `preload` hasta aprobación)
  - `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy: camera=(), microphone=(), geolocation=()`
- Dependencias: Dependabot/Renovate, `npm audit` en CI, versiones fijadas con lockfile.
- Escapado de todos los campos en el correo de notificación (texto plano preferido).

## 11. Entornos y despliegue (planificado, **no ejecutado**)

| Entorno | URL | Disparador | Indexable |
|---------|-----|-----------|-----------|
| Local | `localhost:4321` / `wrangler dev` | Manual | No |
| Preview | `<rama>-parrasanlabs-web.<cuenta>.workers.dev` (protegido con Cloudflare Access si se desea) | Manual o por PR, **solo con autorización** para conectar la cuenta de Cloudflare | No |
| Producción | `parrasanlabs.com` | **No se despliega** hasta autorización explícita (D12). Después: *workflow* manual con aprobación del entorno `production` | Sí |

- El prototipo de Inicio se revisa con capturas de escritorio y móvil adjuntas al PR y, si se autoriza, en *preview*. No en producción.

- La asignación del dominio de producción al Worker (*Custom Domain / Route*) se hace **solo** con aprobación explícita del responsable de la zona, en el PR de lanzamiento. Si implica crear o modificar registros DNS, se coordina fuera de este repositorio.
- Rollback: `wrangler rollback` a la versión anterior.

## 12. Repositorio, revisión y control de cambios (D7, D8)

### 12.1 Repositorio
- `parrasan-labs/parrasanlabs-web`, **privado**, rama por defecto `main`.
- En la configuración del repositorio: **desactivar "Allow auto-merge"**, permitir solo *squash merge*, borrar ramas tras el merge.
- Secretos de GitHub Actions solo para CI (ninguno de producción hasta autorización).

### 12.2 Flujo de revisión cuando los PRs salen de la cuenta del revisor
GitHub **no permite aprobar un PR propio**. Como los PRs se crearán con la cuenta de Charlie Parra (que también es quien autoriza), exigir "1 aprobación" bloquearía todos los merges. Configuración propuesta (ruleset o protección de rama sobre `main`):

| Regla | Valor | Motivo |
|-------|-------|--------|
| Requerir pull request antes de fusionar | Sí | Nada entra a `main` sin PR |
| Aprobaciones requeridas | **0** | Evita el bloqueo por autoría propia |
| Requerir resolución de conversaciones | Sí | Los comentarios de revisión deben cerrarse |
| *Status checks* requeridos | Todos los jobs de CI | Calidad, seguridad y accesibilidad obligatorias |
| Requerir rama actualizada | Sí | — |
| Bloquear *force push* y borrado de `main` | Sí | — |
| Auto-merge | Desactivado en el repositorio | D8 |

- **La autorización es el acto de fusionar**: solo Charlie Parra pulsa *Squash and merge*, después de revisar. Los PRs se abren como **borrador** y pasan a "Ready for review" cuando CI está en verde; la plantilla de PR incluye una casilla "Revisado y autorizado por Charlie Parra".
- Despliegues a producción (cuando se autoricen): entorno `production` en GitHub con Charlie Parra como *required reviewer*. En entornos, la auto-aprobación está permitida salvo que se active "Prevent self-review", que **no** se activará.
- **Limitación de plan:** las reglas de protección y los *rulesets* en repositorios **privados** requieren GitHub Team (organizaciones) o Pro. Si la organización está en el plan Free, las reglas no se aplican; en ese caso el acuerdo se mantiene por proceso (PR obligatorio, CI en verde, merge manual solo por el revisor) y se documenta en el README. Verificar el plan antes de PR-01.

### 12.3 Integridad del traslado (PR-00)
- `docs/CHECKSUMS.sha256` contiene el SHA-256 de cada documento. Tras copiarlos al repositorio nuevo se ejecuta `sha256sum -c docs/CHECKSUMS.sha256` y el resultado se pega en la descripción de PR-00.
- Después de fusionar PR-00, se elimina la rama temporal de `scr-website` (con autorización).

