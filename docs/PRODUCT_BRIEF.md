# Product Brief — parrasanlabs.com

> **Estado:** Fase 0 · Borrador para aprobación
> **Fecha:** 2026-11-11
> **Responsable de aprobación:** Dirección de Parrasan Labs
> **Repositorio destino:** `parrasan-labs/parrasanlabs-web` (por crear; independiente de `parrasan-assist`)

---

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

> Las siguientes personas son **hipótesis de diseño**, no clientes reales.

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
- Páginas legales: Política de privacidad, Términos de uso, (Aviso de cookies si se usa analítica con cookies).
- Página 404.
- Formulario de contacto con protección anti-spam y envío a `cparra@parrasanlabs.com`.
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

## 7. Llamado a la acción (CTA)

- **CTA principal (único y consistente):** `Hablemos de tu proyecto` → `/contacto`.
- **CTA secundario:** `Ver soluciones` / `Cómo trabajamos` (navegación, nunca compite visualmente con el principal).
- **Canal alternativo:** enlace `mailto:cparra@parrasanlabs.com` visible en Contacto y footer.

## 8. Riesgos y mitigaciones

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| Falta de prueba social (sin clientes publicables) | Menor confianza | Compensar con transparencia de proceso, equipo real con nombre y foto, metodología, política de datos clara. Añadir casos reales solo con autorización escrita. |
| Lenguaje demasiado técnico | Baja comprensión | Pruebas de lectura con audiencia; revisión editorial en cada PR de contenido. |
| Spam en formulario | Ruido operativo | Cloudflare Turnstile + honeypot + rate limiting. |
| Recolección de datos personales | Riesgo legal (Ley 8968) | Minimización, consentimiento explícito, política de privacidad, sin almacenar más de lo necesario. |
| Expectativas infladas sobre IA | Pérdida de credibilidad | Sección "Lo que la IA no hace" / límites en cada solución. |
| Mezcla con `parrasan-assist` | Acoplamiento, fugas de config | Repositorio, cuenta de Workers y secretos separados. |

## 9. Supuestos y preguntas abiertas

Requieren respuesta de Parrasan Labs antes o durante la Fase 1:

1. ¿Nombre legal, cédula jurídica y dirección para el footer y la política de privacidad?
2. ¿Quiénes aparecen en *Nosotros* (nombres, cargos, fotos reales autorizadas)?
3. ¿Hay algún proyecto real que pueda describirse con autorización? (Si no, se usan "escenarios ilustrativos" claramente rotulados.)
4. ¿Se ofrece agendar llamada (ej. Cal.com) o solo formulario + correo?
5. ¿Analítica: Cloudflare Web Analytics (sin cookies, recomendado) o ninguna?
6. ¿Logotipo final existe? Si no, ¿se diseña un wordmark tipográfico provisional?
7. ¿WhatsApp Business como canal de contacto? (Implica número público.)
8. ¿Quién es el revisor humano de cada PR y quién aprueba contenido?
9. ¿Dominio `parrasanlabs.com` ya está en Cloudflare? (Solo lectura; no se modifica DNS en este proyecto.)

## 10. Definición de éxito de Fase 0

- [ ] Los 6 documentos en `docs/` están revisados y aprobados.
- [ ] Se eligió una de las dos propuestas visuales (A o B) o una combinación explícita.
- [ ] Se respondieron las preguntas abiertas críticas (1, 2, 5, 8).
- [ ] Se aprobó el plan de PRs de `IMPLEMENTATION_PLAN.md`.
