# Content Map — parrasanlabs.com

> **Estado:** Fase 0 · Contenido **preliminar** en español, sujeto a revisión editorial.
> Los textos marcados con `[POR CONFIRMAR]` requieren información de Parrasan Labs.
> Los textos marcados con `[ILUSTRATIVO]` son escenarios de ejemplo y **deben mostrarse rotulados como tales** en el sitio.

## 0. Reglas editoriales

- **Tono:** cercano, claro, seguro sin ser arrogante. Tuteo (tú).
- **Frases:** máx. ~20 palabras. Párrafos de 1–3 frases.
- **Vocabulario:** preferir "tarea", "proceso", "equipo", "revisión" sobre jerga ("pipeline", "LLM", "orquestación"). Si se usa un término técnico, explicarlo una vez.
- **Prohibido:** cifras de ahorro, nombres o logos de clientes, testimonios, "líder del mercado", "el mejor", "100 % automático", "sin errores", "reemplaza a tu equipo".
- **Verbos permitidos para beneficios:** ayuda a, permite, reduce el trabajo manual en, puede encargarse de.
- **CTA:** siempre el texto exacto `Hablemos de tu proyecto`.
- **Cada página** tiene: `title` (≤ 60 caracteres), `meta description` (≤ 155), un único `H1`.

---

## 1. Inicio (`/`)

**SEO**
- Title: `Parrasan Labs · Agentes de IA y automatización para empresas`
- Description: `Diseñamos agentes de IA y automatizaciones para atención al cliente, conciliación financiera y gestión documental. Equipo en Costa Rica.`

### 1.1 Hero
- **Eyebrow:** Agentes de IA · Automatización empresarial
- **H1:** Automatiza el trabajo repetitivo. Tu equipo se queda con lo importante.
- **Subtítulo:** Diseñamos agentes de IA y automatizaciones a la medida para pequeñas y medianas empresas. Empezamos por un proceso, lo probamos contigo y crecemos solo si funciona.
- **CTA principal:** Hablemos de tu proyecto
- **CTA secundario (enlace):** Ver cómo trabajamos →

### 1.2 Problema (reconocimiento)
- **H2:** ¿Te suena familiar?
- Lista (iconos lineales simples, sin robots):
  - Las mismas preguntas de clientes, todos los días, por WhatsApp y correo.
  - Horas cruzando pagos contra estados de cuenta en hojas de cálculo.
  - Documentos que llegan por todos lados y nadie sabe dónde quedaron.
  - Tareas de copiar y pegar entre sistemas que no se hablan.
- **Cierre:** Estas tareas son necesarias, pero no tienen por qué consumir el día de tu equipo.

### 1.3 Soluciones (resumen)
- **H2:** Qué hacemos
- **Intro:** Cuatro áreas donde la automatización suele aportar más valor desde el inicio.
- Tarjetas (enlace a `/soluciones/#…`):
  1. **Atención al cliente** — Agentes que responden preguntas frecuentes, recopilan datos y pasan el caso a una persona cuando hace falta.
  2. **Conciliación financiera** — Cruce automático de pagos, facturas y movimientos bancarios, con las diferencias señaladas para revisión.
  3. **Gestión documental** — Clasificación, extracción de datos y archivo ordenado de facturas, contratos y expedientes.
  4. **Automatización de procesos** — Conectamos tus herramientas para eliminar pasos manuales entre sistemas.

### 1.4 Cómo trabajamos (resumen)
- **H2:** Paso a paso, sin saltos al vacío
- 4 pasos numerados:
  1. **Diagnóstico** — Entendemos tu proceso y vemos si automatizarlo tiene sentido.
  2. **Piloto** — Construimos una versión acotada y la probamos con datos reales, bajo supervisión.
  3. **Implementación** — Si el piloto cumple, lo integramos a tu operación.
  4. **Acompañamiento** — Medimos, ajustamos y damos soporte.
- Enlace: Conoce el detalle →

### 1.5 Principios (confianza sin prueba social inventada)
- **H2:** Cómo pensamos la IA
- 3 columnas:
  - **Las personas deciden.** Diseñamos puntos de revisión humana en los pasos que importan.
  - **Tus datos son tuyos.** Usamos solo la información necesaria y te explicamos dónde vive y quién accede.
  - **Sin promesas mágicas.** Te decimos qué se puede automatizar, qué no y por qué.

### 1.6 CTA final
- **H2:** ¿Tienes un proceso que quieres simplificar?
- **Texto:** Cuéntanos cómo funciona hoy. Te respondemos con una primera opinión honesta, sin compromiso.
- **CTA:** Hablemos de tu proyecto

---

## 2. Soluciones (`/soluciones/`)

**SEO**
- Title: `Soluciones de IA y automatización · Parrasan Labs`
- Description: `Agentes de IA para atención al cliente, conciliación financiera, gestión documental y automatización de procesos operativos.`

- **H1:** Soluciones
- **Intro:** Cada empresa es distinta, pero muchos procesos comparten el mismo patrón: información que entra, reglas que se aplican y resultados que alguien revisa. Ahí es donde un agente de IA puede ayudar.

Cada solución sigue la misma estructura (componente `SolutionBlock`):

| Campo | Propósito |
|-------|-----------|
| Qué es | Una frase sin jerga |
| Qué puede hacer | 3–5 capacidades |
| Dónde sigue participando tu equipo | Honestidad sobre revisión humana |
| Qué necesitamos de ti | Accesos, ejemplos, una persona de contacto |
| Herramientas con las que suele integrarse | Genérico (correo, WhatsApp Business, hojas de cálculo, ERP) — **sin** logos de marcas salvo integraciones verificadas |
| CTA | Hablemos de tu proyecto (`/contacto/?solucion=<slug>`) |

### 2.1 Atención al cliente `#atencion-al-cliente`
- **Qué es:** Un agente que atiende consultas frecuentes en tus canales y entrega a tu equipo los casos que necesitan a una persona.
- **Qué puede hacer:**
  - Responder preguntas frecuentes con la información que tú apruebas.
  - Recopilar datos del cliente antes de pasar el caso.
  - Clasificar solicitudes por tipo y urgencia.
  - Funcionar fuera de horario y avisar al equipo al día siguiente.
- **Dónde sigue tu equipo:** Casos sensibles, quejas, excepciones y cualquier situación que el agente no pueda resolver con seguridad.
- **Qué necesitamos de ti:** Preguntas frecuentes actuales, tono de tu marca, reglas de escalamiento.

### 2.2 Conciliación financiera `#conciliacion-financiera`
- **Qué es:** Un proceso automático que cruza pagos, facturas y movimientos bancarios y te muestra solo lo que no coincide.
- **Qué puede hacer:**
  - Leer estados de cuenta y reportes en distintos formatos.
  - Emparejar transacciones según tus reglas.
  - Señalar diferencias, duplicados y pendientes.
  - Generar un resumen para revisión.
- **Dónde sigue tu equipo:** Aprobación de ajustes, decisiones contables y cierre.
- **Qué necesitamos de ti:** Ejemplos de archivos, reglas de conciliación actuales, acceso de solo lectura cuando aplique.

### 2.3 Gestión documental `#gestion-documental`
- **Qué es:** Un sistema que recibe documentos, extrae los datos clave y los archiva donde corresponde.
- **Qué puede hacer:**
  - Identificar el tipo de documento (factura, contrato, orden, etc.).
  - Extraer campos como fechas, montos, nombres o números de referencia.
  - Renombrar y archivar con una estructura consistente.
  - Señalar documentos incompletos o ilegibles.
- **Dónde sigue tu equipo:** Validación de documentos con baja confianza y decisiones sobre excepciones.
- **Qué necesitamos de ti:** Muestras representativas, estructura de archivo deseada, políticas de retención.

### 2.4 Automatización de procesos `#automatizacion-de-procesos`
- **Qué es:** Conexiones entre tus herramientas para que la información fluya sin copiar y pegar.
- **Qué puede hacer:**
  - Mover datos entre formularios, hojas de cálculo, correo y sistemas internos.
  - Disparar tareas y notificaciones según eventos.
  - Generar reportes periódicos.
- **Dónde sigue tu equipo:** Definición de reglas y supervisión de excepciones.
- **Qué necesitamos de ti:** Mapa del proceso actual (aunque sea a mano), accesos a las herramientas involucradas.

### 2.5 Bloque "Lo que no hacemos"
- **H2:** Lo que la IA no debería hacer sola
- Tomar decisiones legales, médicas o financieras finales sin revisión.
- Contestar con información que no ha sido aprobada por tu empresa.
- Operar sin registros que permitan revisar qué hizo y por qué.

---

## 3. Cómo trabajamos (`/como-trabajamos/`)

**SEO**
- Title: `Cómo trabajamos · Metodología por etapas · Parrasan Labs`
- Description: `Diagnóstico, piloto, implementación y acompañamiento. Una forma ordenada y de bajo riesgo de incorporar IA a tu operación.`

- **H1:** Cómo trabajamos
- **Intro:** Preferimos avanzar en pasos pequeños y comprobables. Así sabes qué estás obteniendo antes de invertir más.

### Etapas (componente `ProcessTimeline`)

| Etapa | Qué pasa | Qué recibes | Tu participación |
|-------|----------|-------------|------------------|
| **1. Conversación inicial** | Nos cuentas tu proceso y tus objetivos. | Una opinión honesta sobre si vale la pena automatizarlo. | Una reunión. |
| **2. Diagnóstico** | Revisamos el proceso a detalle con quien lo ejecuta. | Documento con alcance, riesgos, datos necesarios y propuesta de piloto. | Acceso a la persona que hace el trabajo y ejemplos reales. |
| **3. Piloto** | Construimos una versión acotada y la probamos en paralelo a tu proceso actual. | Un piloto funcionando y un informe de resultados observados. | Revisar resultados y dar retroalimentación. |
| **4. Implementación** | Integramos la solución a tu operación con controles y registros. | Solución en producción, documentación y capacitación. | Aprobación de puesta en marcha. |
| **5. Acompañamiento** | Medimos, ajustamos y damos soporte. | Ajustes periódicos y canal de soporte. | Reportar excepciones. |

> Duraciones y precios **no** se publican en v1 `[POR CONFIRMAR]`. Si se publican, deben ser rangos reales que Parrasan Labs pueda sostener.

### Compromisos
- **H2:** Lo que puedes esperar de nosotros
  - Explicaciones en lenguaje sencillo en cada etapa.
  - Revisión humana diseñada desde el inicio.
  - Uso mínimo de datos y acuerdos de confidencialidad cuando aplique.
  - Si algo no conviene automatizarlo, te lo decimos.

### Preguntas frecuentes (componente `FAQ`, marcado `FAQPage`)
- **¿Necesito tener un equipo técnico?** No. Trabajamos con quienes conocen el proceso; nosotros nos encargamos de la parte técnica.
- **¿Qué pasa si el piloto no funciona?** Revisamos juntos qué aprendimos y decidimos si ajustar, cambiar el enfoque o detenernos.
- **¿Dónde quedan mis datos?** Depende de la solución y de tus herramientas actuales. En el diagnóstico definimos dónde se almacena la información, quién accede y por cuánto tiempo.
- **¿La IA puede equivocarse?** Sí. Por eso diseñamos controles, registros y puntos de revisión humana.
- **¿Trabajan con empresas fuera de Costa Rica?** `[POR CONFIRMAR]`

---

## 4. Aplicaciones (`/aplicaciones/`)

**SEO**
- Title: `Aplicaciones de agentes de IA por área · Parrasan Labs`
- Description: `Ejemplos de cómo la automatización puede apoyar a finanzas, servicio al cliente, operaciones y administración.`

- **H1:** Aplicaciones
- **Intro:** Algunos escenarios típicos donde la automatización puede ayudar. Son **ejemplos ilustrativos**, no casos de clientes.
- **Rótulo visible en cada tarjeta:** `Escenario ilustrativo`

Estructura por tarjeta: *Área · Situación · Cómo podría ayudar un agente · Dónde revisa una persona*.

| Área | Situación `[ILUSTRATIVO]` | Cómo podría ayudar | Revisión humana |
|------|---------------------------|--------------------|-----------------|
| Finanzas | Un equipo concilia transferencias de clientes contra facturas emitidas cada semana. | Emparejar pagos y facturas y listar solo las diferencias. | Aprobar ajustes y cerrar el periodo. |
| Servicio al cliente | Un comercio recibe muchas consultas repetidas sobre horarios, precios y disponibilidad. | Responder con información aprobada y pasar a una persona lo demás. | Casos especiales y quejas. |
| Administración | Una oficina recibe facturas de proveedores por correo en PDF. | Extraer datos, renombrar y archivar en la carpeta correcta. | Validar documentos dudosos. |
| Operaciones | Un pedido se registra en un formulario y luego se copia a mano en varias hojas. | Mover los datos automáticamente y notificar al responsable. | Revisar excepciones. |
| Recursos humanos | Solicitudes internas (vacaciones, constancias) llegan por canales distintos. | Centralizar solicitudes y preparar borradores. | Aprobar y firmar. |
| Ventas | Prospectos llegan por web y redes y se pierden entre bandejas. | Registrar y clasificar prospectos y avisar al vendedor. | Seguimiento comercial. |

- **CTA final:** ¿Tu caso se parece a alguno? → Hablemos de tu proyecto

---

## 5. Nosotros (`/nosotros/`)

**SEO**
- Title: `Nosotros · Parrasan Labs, Costa Rica`
- Description: `Somos una empresa costarricense que diseña agentes de IA y automatizaciones con un enfoque práctico, transparente y humano.`

- **H1:** Nosotros
- **Intro:** Parrasan Labs es una empresa costarricense dedicada a diseñar agentes de IA y automatizaciones para empresas que quieren trabajar mejor, no solo más rápido.
- **Por qué existimos:** Creemos que la IA es útil cuando resuelve problemas concretos y las personas mantienen el control. `[POR CONFIRMAR: historia fundacional real]`
- **Equipo** `[POR CONFIRMAR]`: nombre, cargo, foto real autorizada, bio de 2 líneas, enlace a LinkedIn (opcional). **No** usar fotos de stock ni avatares generados.
- **Valores:** Claridad · Responsabilidad · Cercanía · Aprendizaje continuo.
- **Ubicación:** Costa Rica `[POR CONFIRMAR: ciudad]`.
- **Sin** logos de clientes ni de "partners" salvo acuerdos verificables y autorizados.

---

## 6. Contacto (`/contacto/`)

**SEO**
- Title: `Contacto · Hablemos de tu proyecto · Parrasan Labs`
- Description: `Cuéntanos qué proceso quieres simplificar. Te respondemos con una primera opinión honesta, sin compromiso.`

- **H1:** Hablemos de tu proyecto
- **Intro:** Cuéntanos cómo funciona hoy el proceso que quieres mejorar. Leemos cada mensaje y respondemos personalmente. `[POR CONFIRMAR: tiempo de respuesta real, ej. "en 2 días hábiles"]`

### Formulario (ver `ARCHITECTURE.md §6` y `DESIGN_SYSTEM.md §8`)

| Campo | Tipo | Requerido | Notas |
|-------|------|-----------|-------|
| Nombre | text | Sí | `autocomplete="name"` |
| Correo de trabajo | email | Sí | `autocomplete="email"` |
| Empresa | text | Sí | `autocomplete="organization"` |
| Teléfono | tel | No | `autocomplete="tel"` |
| Tamaño del equipo | select | No | 1–10 · 11–50 · 51–200 · Más de 200 |
| Área de interés | select | Sí | Atención al cliente · Conciliación financiera · Gestión documental · Automatización de procesos · No estoy seguro |
| Cuéntanos sobre el proceso | textarea | Sí | Placeholder: "¿Qué tarea quieres simplificar? ¿Quién la hace hoy y cuánto tiempo toma?" — máx. 2 000 caracteres. **Aviso:** "No incluyas datos sensibles de clientes, contraseñas ni información financiera." |
| ¿Qué tan pronto? | radio | No | Explorando · En los próximos 3 meses · Es urgente |
| Consentimiento | checkbox | Sí | "Acepto que Parrasan Labs use estos datos para responder a mi consulta, según la [Política de privacidad](/privacidad/)." — **no** premarcado |
| Honeypot | hidden | — | Anti-spam |
| Turnstile | widget | — | Anti-spam |

- **Botón:** Enviar mensaje
- **Alternativa:** ¿Prefieres escribirnos directamente? `cparra@parrasanlabs.com`
- **Éxito (`/contacto/gracias/`):** "Gracias, recibimos tu mensaje. Te escribiremos a [correo] pronto." + enlace a Cómo trabajamos.
- **Error:** "No pudimos enviar tu mensaje. Inténtalo de nuevo o escríbenos a cparra@parrasanlabs.com." (sin perder lo escrito).

---

## 7. Legales

### 7.1 Política de privacidad (`/privacidad/`) — estructura mínima
> Debe ser revisada por asesoría legal antes de publicar. Referencia: Ley N.º 8968 de Protección de la Persona frente al Tratamiento de sus Datos Personales (Costa Rica) y su reglamento.

1. Responsable del tratamiento `[POR CONFIRMAR: razón social, cédula jurídica, domicilio]`.
2. Datos que recopilamos (solo los del formulario; analítica sin cookies si se usa).
3. Finalidad (responder la consulta y dar seguimiento comercial relacionado).
4. Base: consentimiento informado.
5. Conservación `[POR CONFIRMAR: plazo, ej. 12 meses sin relación comercial]`.
6. Encargados / terceros (Cloudflare; proveedor de envío de correo).
7. Transferencias internacionales (servidores de Cloudflare / proveedor de correo fuera de CR).
8. Derechos ARCO: acceso, rectificación, cancelación, oposición → escribir a `cparra@parrasanlabs.com`.
9. Seguridad.
10. Cambios a la política y fecha de última actualización.

### 7.2 Términos de uso (`/terminos/`)
Uso del sitio, propiedad intelectual, sin garantía sobre contenido informativo, ley aplicable (Costa Rica), contacto.

### 7.3 404
- **H1:** No encontramos esta página
- Texto: Puede que el enlace haya cambiado. Prueba desde el inicio o escríbenos.
- Botones: Ir al inicio · Hablemos de tu proyecto

---

## 8. Microcopy global

| Elemento | Texto |
|----------|-------|
| Saltar al contenido | Saltar al contenido principal |
| Menú móvil | Abrir menú / Cerrar menú |
| Selector de idioma (futuro) | Español / English |
| Rótulo escenario | Escenario ilustrativo |
| Footer tagline | Agentes de IA y automatización empresarial. Hecho en Costa Rica. |

## 9. Preparación para inglés

- Todo texto vive en archivos de contenido (`src/content/` o `src/i18n/es.ts`), nunca embebido en componentes.
- Cada clave de contenido tendrá su par en `en.ts` cuando se traduzca; las claves faltantes **no** caen silenciosamente a español en producción (falla el build).
- Traducción profesional/revisada por humano; no publicar traducción automática sin revisión.
- Ajustar ejemplos y referencias legales (la Ley 8968 se menciona igualmente, pues aplica al responsable).
