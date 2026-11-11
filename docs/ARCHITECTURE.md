# Arquitectura — parrasanlabs.com

> **Estado:** Fase 0 · Borrador para aprobación. **No se ha escrito código ni desplegado nada.**

## 1. Decisiones principales

| Decisión | Elección | Motivo |
|----------|----------|--------|
| Framework | **Astro 5** (salida estática + un endpoint de servidor) | HTML estático por defecto, cero JS innecesario, i18n nativo, excelente SEO |
| Lenguaje | **TypeScript** (`strict`) | Seguridad de tipos en contenido, formularios y Worker |
| Estilos | **Tailwind CSS v4** (`@tailwindcss/vite`) | Tokens del design system como variables CSS; sin CSS muerto |
| Hosting / runtime | **Cloudflare Workers** con *Static Assets* + `@astrojs/cloudflare` | Borde global, sin servidores que mantener, un solo despliegue para estáticos + API |
| Anti-spam | **Cloudflare Turnstile** + honeypot + Rate Limiting binding | Sin CAPTCHA molestos, sin cookies de terceros de seguimiento |
| Analítica | **Cloudflare Web Analytics** (sin cookies) — *opcional, por confirmar* | Evita banner de cookies y minimiza datos personales |
| Contenido | **Astro Content Collections** (Markdown/MDX + esquemas Zod) | Contenido separado del código, validado en build, listo para traducción |
| Pruebas | Vitest (unidad), Playwright (e2e), axe-core (a11y), Lighthouse CI | Ver §9 |
| CI | GitHub Actions | Checks obligatorios antes de *merge* |

### Independencia de `parrasan-assist`
- Repositorio propio: `parrasan-labs/parrasanlabs-web`.
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
                    │        │ 5. Envío de notificación ────────────┼──▶ Proveedor de correo (§6.4)
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
│   ├── workflows/preview.yml         # despliegue a entorno preview por PR (Fase 1+)
│   ├── pull_request_template.md
│   └── CODEOWNERS
├── docs/                             # Esta documentación (Fase 0)
├── public/
│   ├── fonts/                        # WOFF2 autoalojadas
│   ├── favicon.svg
│   └── robots.txt                    # generado o estático por entorno
├── src/
│   ├── components/                   # Button, Header, Footer, SolutionCard, ContactForm…
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
│   │   └── mailer.ts                 # adaptador de envío (interfaz + implementación)
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
   5. Envía la notificación (§6.4). `Reply-To` = correo del prospecto.
   6. Responde `303 See Other` → `/contacto/gracias/` (o JSON si fue `fetch`).
4. Errores: 400 (validación, con mensajes por campo), 429 (rate limit), 502 (fallo del proveedor; mostrar correo alternativo).

### 6.2 Esquema (resumen)
`name` (2–100), `email` (email válido, ≤ 254), `company` (2–120), `phone?` (≤ 30), `teamSize?` (enum), `area` (enum), `message` (20–2000), `timeline?` (enum), `consent` (literal `true`), `website` (honeypot, debe estar vacío), `cf-turnstile-response`.

### 6.3 Secretos y variables
| Nombre | Tipo | Dónde |
|--------|------|-------|
| `TURNSTILE_SECRET_KEY` | secreto | `wrangler secret put` |
| `PUBLIC_TURNSTILE_SITE_KEY` | público | variable de entorno de build |
| `MAIL_API_KEY` | secreto | `wrangler secret put` |
| `CONTACT_TO` | var | `cparra@parrasanlabs.com` |
Ningún secreto en el repositorio. `.dev.vars` en `.gitignore`.

### 6.4 Envío de correo — **decisión pendiente, sin tocar DNS**

> Restricción del proyecto: **no se alteran DNS ni configuraciones de correo**. Varias opciones comunes requieren registros DNS (MX, SPF, DKIM), por lo que se documentan con su impacto:

| Opción | ¿Requiere cambios DNS/correo? | Comentario |
|--------|-------------------------------|------------|
| Cloudflare Email Routing + `send_email` binding | **Sí** (habilita Email Routing → MX) | ❌ Descartada sin aprobación explícita |
| Proveedor transaccional con **dominio** verificado (Resend, Postmark, SES) | **Sí** (DKIM/SPF/Return-Path) | ❌ Descartada sin aprobación explícita |
| Proveedor transaccional con **remitente único verificado por correo** (ej. Postmark *Sender Signature*) | No (verificación por enlace en el buzón) | ✅ Candidata v1; entregabilidad aceptable para notificaciones internas |
| Guardar envíos en **Cloudflare D1** + notificación por el proveedor anterior | No | ✅ Respaldo si falla el correo; implica política de retención (ver §8) |

**Recomendación:** v1 con remitente único verificado; evaluar en v2 (con aprobación separada del responsable de DNS) la verificación de dominio para mejor entregabilidad. Cualquier cambio de DNS se gestiona fuera de este repositorio y fuera del alcance de los PRs.

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
| Todas | `Organization` (nombre, url, logo, `email`, `address` CR cuando se confirme) + `WebSite` |
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
| Retención | Si se usa D1: borrado automático tras plazo definido (ej. cron trigger mensual) `[POR CONFIRMAR plazo]`. Si solo correo: la retención la gestiona el buzón |
| Seguridad | HTTPS obligatorio (HSTS), secretos en Wrangler, sin logs con datos personales (se registra solo ID de petición y código de estado), cabeceras de seguridad (§10) |
| Sin rastreo | Sin cookies de terceros, sin píxeles publicitarios, sin Google Fonts remotas; analítica sin cookies o ninguna → no se requiere banner de cookies (validar con asesoría legal) |
| Terceros embebidos | Solo Turnstile; se menciona en la política |
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
  - `Content-Security-Policy: default-src 'self'; script-src 'self' https://challenges.cloudflare.com; frame-src https://challenges.cloudflare.com; connect-src 'self'; img-src 'self' data:; style-src 'self'; font-src 'self'; form-action 'self'; frame-ancestors 'none'; base-uri 'self'` (+ `static.cloudflareinsights.com` si se usa analítica)
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains` (sin `preload` hasta aprobación)
  - `X-Content-Type-Options: nosniff`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy: camera=(), microphone=(), geolocation=()`
- Dependencias: Dependabot/Renovate, `npm audit` en CI, versiones fijadas con lockfile.
- Escapado de todos los campos en el correo de notificación (texto plano preferido).

## 11. Entornos y despliegue (planificado, **no ejecutado**)

| Entorno | URL | Disparador | Indexable |
|---------|-----|-----------|-----------|
| Local | `localhost:4321` / `wrangler dev` | Manual | No |
| Preview | `<rama>-parrasanlabs-web.<cuenta>.workers.dev` | PR (Fase 1+) | No |
| Producción | `parrasanlabs.com` | Merge a `main` + aprobación manual (environment protection) | Sí |

- La asignación del dominio de producción al Worker (*Custom Domain / Route*) se hace **solo** con aprobación explícita del responsable de la zona, en el PR de lanzamiento. Si implica crear o modificar registros DNS, se coordina fuera de este repositorio.
- Rollback: `wrangler rollback` a la versión anterior.
