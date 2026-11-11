# Product Brief — parrasanlabs.com

> **Estado:** Fase 0 · **Aprobado conceptualmente con ajustes** (revisión 2, 2026-11-11)
> **Responsable de aprobación:** Charlie Parra, Parrasan Labs (revisa y autoriza todos los cambios)
> **Repositorio:** `parrasan-labs/parrasanlabs-web` · **privado** · exclusivo para este proyecto, independiente de `parrasan-assist`.
> Nada de este proyecto se fusiona ni se despliega desde `scr-website`.

---

## 0. Registro de decisiones (revisión 2)

| # | Tema | Decisión |
|---|------|----------|
| D1 | Identidad visual | **Propuesta A, "Nocturna precisa"**, como identidad principal. Secciones claras solo donde mejoran la legibilidad (Contacto y páginas legales). El sitio **no** será predominantemente blanco. |
| D2 | Tipografía | **Manrope** para títulos, **Inter** para textos. |
| D3 | Color | Se mantiene la paleta aprobada, se adoptan los tonos accesibles propuestos y se incorpora el **dorado del logo** como acento secundario (valor exacto a tomar de los archivos oficiales). |
| D4 | Logo | Existe un logo oficial y se prepara una versión modernizada. El sistema debe aceptar los archivos definitivos sin reconstruir componentes. **No se dibuja ni se recrea el logo en código.** |
| D5 | Formulario | **Postmark** con remitente verificado individualmente (sin cambios DNS). **Sin D1 en el MVP**; arquitectura preparada para añadirlo. Entrega real validada antes de publicar. |
| D6 | Analítica | **Cloudflare Web Analytics** (sin cookies). |
| D7 | Repositorio | Exclusivamente `parrasan-labs/parrasanlabs-web`, privado. Los seis documentos se trasladan como **PR-00** con verificación de integridad. La rama temporal de `scr-website` se elimina después. |
| D8 | Revisión | Charlie Parra revisa y autoriza. Flujo compatible con PRs creados desde su propia cuenta. **Sin merges automáticos.** |
| D9 | Plan | Se reduce a **12 entregables** (PR-00 a PR-11). Se aprueba la experiencia visual de Inicio (escritorio y móvil) antes de desarrollar las demás páginas. |
| D10 | Datos legales | "Parrasan Labs" es el **nombre comercial**. Razón social, cédula jurídica, domicilio legal e integrantes del equipo quedan **pendientes**; no se publican datos ficticios. La política de privacidad se revisa antes de activar el formulario. |
| D11 | DNS y correo | Sin cambios de DNS, dominio ni correo corporativo sin autorización específica. `cparra@parrasanlabs.com` debe seguir funcionando. |
| D12 | Producción | No se despliega en producción hasta autorización explícita. |

## 1. Resumen

Parrasan Labs es una empresa costarricense especializada en **agentes de IA y automatización empresarial**. `parrasanlabs.com` será su sitio comercial: el lugar donde una empresa entiende qué problema le podemos resolver, confía en que sabemos cómo hacerlo y da el primer paso para conversar.

El sitio **no** es una plataforma de producto ni un portal de clientes. Es un sitio de marketing estático, rápido, accesible y bilingüe a futuro, con un único formulario de contacto.

## 2. Objetivos

| # | Objetivo | Cómo se mide (sin inventar cifras base) |
|---|----------|------------------------------------------|
| O1 | **Generar confianza** | Tiempo en página de *Cómo trabajamos* y *Nosotros*; tasa de rebote en Inicio. Línea base se establece en las primeras 4–6 semanas tras el lanzamiento. |
| O2 | **Explicar con lenguaje sencillo** | Pruebas de comprensión con 3–5 personas de la audiencia antes del lanzamiento: deben poder explicar con sus palabras qué hace Parrasan Labs en < 30 s. |
| O3 | **Convertir en prospectos calificados** | Envíos del formulario que incluyen proceso, tamaño de equipo y urgencia. Proporción calificados / totales. |

> No se fijan metas numéricas hasta tener datos reales. Fijar objetivos inventados contradice el principio de honestidad de la marca.

## 3. Audiencia

### 3.1 Segmentos primarios

1. **Pequeñas y medianas empresas (PYMES)** — dueños, gerentes generales. Saben que pierden tiempo en tareas repetitivas; no necesariamente saben qué es un "agente de IA".
2. **Equipos operativos** — jefaturas de operaciones, finanzas, servicio al cliente, administración. Conocen el dolor del proceso al detalle; buscan alivio concreto y bajo riesgo.
3. **Negocios en exploración** — empresas que quieren "hacer algo con IA" pero no tienen claro por dónde empezar.

### 3.2 Proto-personas (hipótesis, a validar)

> Las siguientes personas son **hipótesis de diseño** (nombres ficticios solo para uso interno, nunca se publican), no clientes reales.

**Mariela — Gerente de operaciones, empresa de distribución (40 personas)**
- Dolor: su equipo concilia pagos de clientes contra estados bancarios en hojas de cálculo cada semana.
- Miedo: que la IA cometa errores con dinero; que el proyecto sea caro e interminable.
- Necesita ver: cómo se controla la calidad, quién revisa, cuánto dura una prueba piloto.

**Andrés — Dueño de una clínica / comercio de servicios (12 personas)**
- Dolor: WhatsApp y correo saturados de preguntas repetidas (horarios, precios, citas).
- Miedo: que un bot trate mal a sus clientes; perder el toque humano.
- Necesita ver: que el agente puede escalar a una persona y que él mantiene el control.

**Laura — Coordinadora administrativa, firma de servicios profesionales**
- Dolor: clasificar, nombrar y archivar documentos (facturas, contratos, expedientes).
- Miedo: privacidad de la información de clientes.
- Necesita ver: dónde viven los datos, quién tiene acceso, cumplimiento de la Ley 8968.

### 3.3 Trabajos por hacer (JTBD)

- *Cuando* mi equipo dedica horas a tareas repetitivas, *quiero* entender si se pueden automatizar *para* liberar tiempo sin arriesgar la operación.
- *Cuando* evalúo un proveedor de IA, *quiero* ver cómo trabaja y quién está detrás *para* decidir si confío en él.
- *Cuando* estoy listo para conversar, *quiero* un canal simple *para* explicar mi caso sin compromisos.

## 4. Propuesta de valor

**Frase principal (borrador):**
> Agentes de IA y automatización que trabajan con tu equipo, no en su lugar.

**Pilares:**
1. **Claridad** — Explicamos qué se puede automatizar y qué no, en lenguaje sencillo.
2. **Control** — Las personas supervisan; los agentes ejecutan tareas acotadas con revisión humana donde importa.
3. **Avance por etapas** — Diagnóstico, piloto acotado y escalamiento solo si el piloto funciona.
4. **Cercanía local** — Equipo en Costa Rica, atención en español, conocimiento del contexto regional.

## 5. Principios de contenido (no negociables)

1. **No inventar** clientes, logotipos, testimonios, métricas, porcentajes de ahorro ni casos de éxito. Si no hay evidencia, se omite la sección o se usa un ejemplo **explícitamente ilustrativo** ("Ejemplo ilustrativo", "Escenario típico").
2. **No prometer** resultados garantizados ("reduce costos 80 %", "reemplaza a tu equipo"). Usar verbos de capacidad: *puede*, *ayuda a*, *permite*.
3. **No usar** imágenes de robots genéricos, cerebros brillantes, manos humanoides ni stock de "IA futurista".
4. **Lenguaje sencillo**: frases cortas, segunda persona (tú), sin siglas sin explicar. Glosario breve si se usa "agente", "LLM", "integración".
5. **Honestidad sobre límites**: cada solución menciona qué requiere de parte del cliente y dónde se mantiene revisión humana.

## 6. Alcance

### 6.1 Dentro del alcance (v1)

- 6 páginas: Inicio, Soluciones, Cómo trabajamos, Aplicaciones, Nosotros, Contacto.
- Páginas legales: Política de privacidad, Términos de uso, (no se prevé aviso de cookies: Cloudflare Web Analytics no usa cookies).
- Página 404.
- Formulario de contacto con protección anti-spam y envío a `cparra@parrasanlabs.com` mediante Postmark (activado solo tras revisar la política de privacidad y validar la entrega real).
- SEO técnico, datos estructurados, sitemap, Open Graph.
- Accesibilidad WCAG 2.2 AA.
- Arquitectura preparada para i18n (ES por defecto, EN futuro).

### 6.2 Fuera del alcance (v1)

- Versión en inglés publicada (solo se prepara la estructura).
- Blog / recursos (se evalúa en v2).
- Portal de clientes, login, pagos.
- Chatbot en el sitio (se evalúa después; si se hace, debe ser una demostración honesta y con aviso).
- Cambios en DNS, MX, SPF, DKIM, DMARC o cualquier configuración de correo. **Prohibido en este proyecto sin aprobación explícita y separada.**
- Integración con `parrasan-assist` (repos y despliegues independientes).
- Respaldo de envíos en base de datos (D1): fuera del MVP, arquitectura preparada.
- Rediseño o recreación del logo (D4).

## 7. Llamado a la acción (CTA)

- **CTA principal (único y consistente):** `Hablemos de tu proyecto` → `/contacto`.
- **CTA secundario:** `Ver soluciones` / `Cómo trabajamos` (navegación, nunca compite visualmente con el principal).
- **Canal alternativo:** enlace `mailto:cparra@parrasanlabs.com` visible en Contacto y footer.

## 8. Riesgos y mitigaciones

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| Sitio oscuro con lecturas largas | Fatiga de lectura | Cuerpo ≥ 18 px en escritorio, texto `slate-300`/`mist-50`, Contacto y legales en tema claro (D1). |
| Correo enviado por Postmark sin firma de dominio | Puede llegar a spam o rechazarse si `parrasanlabs.com` tiene una política DMARC estricta | Validar la entrega real a `cparra@parrasanlabs.com` antes de publicar (D5); si falla, se presenta la opción de verificación de dominio como decisión aparte (requiere autorización DNS, D11). |
| Logo definitivo aún en desarrollo | Retrabajo | Componente `Logo` con ranuras de archivos y texto provisional; cambiar archivos no cambia componentes (D4). |
| Falta de prueba social (sin clientes publicables) | Menor confianza | Compensar con transparencia de proceso, equipo real con nombre y foto, metodología, política de datos clara. Añadir casos reales solo con autorización escrita. |
| Lenguaje demasiado técnico | Baja comprensión | Pruebas de lectura con audiencia; revisión editorial en cada PR de contenido. |
| Spam en formulario | Ruido operativo | Cloudflare Turnstile + honeypot + rate limiting. |
| Recolección de datos personales | Riesgo legal (Ley 8968) | Minimización, consentimiento explícito, política de privacidad, sin almacenar más de lo necesario. |
| Expectativas infladas sobre IA | Pérdida de credibilidad | Sección "Lo que la IA no hace" / límites en cada solución. |
| Mezcla con `parrasan-assist` | Acoplamiento, fugas de config | Repositorio, cuenta de Workers y secretos separados. |

## 9. Datos pendientes y preguntas abiertas

**Pendientes que no se asumen ni se publican con valores ficticios (D10):**

| Dato | Estado | Dónde se usa | Mientras tanto |
|------|--------|--------------|----------------|
| Razón social | Pendiente | Política de privacidad, términos | El responsable figura como "Parrasan Labs (nombre comercial)" solo en borradores; la política no se publica sin este dato |
| Cédula jurídica | Pendiente | Política de privacidad | Idem |
| Domicilio legal | Pendiente | Política de privacidad, JSON-LD `address` | Se omite `address`; el footer dice solo "Costa Rica" |
| Integrantes del equipo | Pendiente | Nosotros | La sección de equipo no se muestra |
| Archivos oficiales del logo (SVG) y valor exacto del dorado | Pendiente | Header, footer, favicon, Open Graph, tokens | Texto "Parrasan Labs" como sustituto provisional, sin imitar el logo |
| Tiempo de respuesta real a contactos | Pendiente | Contacto | Se omite la promesa de plazo |

**Preguntas abiertas (no bloquean el prototipo de Inicio):**
1. ¿Hay algún proyecto real que pueda describirse con autorización? (Si no, solo "escenarios ilustrativos".)
2. ¿Se ofrece agendar llamada o solo formulario y correo?
3. ¿WhatsApp Business como canal de contacto? (Implica número público.)
4. ¿Se atiende a empresas fuera de Costa Rica?
5. ¿`parrasanlabs.com` ya está en Cloudflare? (Solo lectura; no se modifica nada, D11.)

## 10. Definición de éxito de Fase 0

- [x] Los 6 documentos revisados y aprobados conceptualmente (revisión 1).
- [x] Propuesta visual elegida: A, "Nocturna precisa" (D1).
- [x] Revisor definido: Charlie Parra (D8).
- [x] Plan de entregables ajustado (D9).
- [ ] Repositorio `parrasan-labs/parrasanlabs-web` creado (privado).
- [ ] PR-00 abierto en el repositorio nuevo, con integridad verificada, y aprobado.
