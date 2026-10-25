# CLAUDE.md — Southern Colored Rooster

Este repo tiene dos cosas: la **web de la banda** (scrband.com) y el **sistema de contenido para redes** del lanzamiento de *Campfire Stories* (27 nov 2026).

## Estructura

```
.
├── index.html                  # Web completa (una sola página, bilingüe EN/ES, con CSS y JS inline)
├── CNAME                       # Dominio scrband.com (GitHub Pages)
├── *.jpg / *.png               # Portadas (álbum y sencillos) y logo
├── *-snippet.mp3               # Fragmentos de audio de los sencillos que usa la web
├── brand/
│   └── marca.md                # Guía de marca: bio, tono, do's y don'ts, hashtags, canciones y créditos
├── content/
│   ├── calendario.md           # Calendario de contenido del 25 oct al 11 dic 2026, con estado por entrada
│   └── borradores/             # Posts redactados por el CM, pendientes de aprobación
└── .claude/
    └── agents/
        └── community-manager.md  # Subagente community manager
```

- **`brand/marca.md` es la fuente de verdad** de la banda: integrantes, créditos, qué canciones ya salieron, tono y handles. Si un dato cambia (un link de pre-save, una fecha nueva o un integrante), se actualiza ahí primero.
- **`content/calendario.md`** manda en el qué y el cuándo. Estados: `Pendiente` → `Borrador` → `Aprobado` → `Publicado`.

## Cuándo usar el subagente `community-manager`

Usalo para **todo lo que sea contenido de redes**:
- Redactar posts, carruseles, guiones de reels, stories, captions para TikTok o títulos de Shorts
- Responder comentarios o mensajes con la voz de la banda
- Proponer ideas nuevas o ajustar entradas del calendario

**No lo uses** para cambios en la web (`index.html`) ni para tareas técnicas del repo. Esas se hacen en la conversación principal.

## Reglas para Claude en este repo

- **Nada se publica sin la aprobación de Charlie.** El subagente solo deja borradores con el estado `PENDIENTE DE APROBACIÓN`. Cuando Charlie diga explícitamente que aprueba un borrador, la conversación principal cambia el estado a `Aprobado` en el borrador y en el calendario. Charlie publica a mano y después se marca como `Publicado`.
- No hay integraciones con redes sociales en este proyecto, y no se deben agregar sin que Charlie lo pida.
- La web todavía enlaza al handle viejo de Instagram (`southern_colored_rooster`). El handle actual es **@scrband**.
- Los textos de la web son bilingües: cada bloque tiene su versión `class="lang-en"` y `class="lang-es"`. Si cambiás uno, cambiá los dos.
