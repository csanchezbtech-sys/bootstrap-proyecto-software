# Taxonomía de contexto para un proyecto de software

Basada en la investigación hecha originalmente para un proyecto de e-commerce
(Avela Swim): se leyó la documentación oficial de Claude Code sobre memoria,
skills y subagentes (frontmatter real, no supuesto), y se cruzó con un repo
comunitario de buenas prácticas (`shanraisshan/claude-code-best-practice`)
para confirmarla. No tiene nada específico de e-commerce — aplica igual a
cualquier tipo de proyecto de software.

## Los 6 niveles

### 1. `CLAUDE.md` — reglas permanentes

Se carga siempre, en cada sesión. Va aquí: qué es el proyecto, qué stack usa,
qué se abandonó y por qué (si aplica), las reglas de comportamiento no
negociables, y el catálogo de comandos reales (build, test, dev, etc.).
**Nunca duplica el procedimiento de una skill** — si una skill ya explica
cómo hacer algo paso a paso, `CLAUDE.md` solo la referencia por nombre.
Duplicar contenido entre los dos es la forma más común de que `CLAUDE.md` se
desactualice sin que nadie lo note.

Existe también `CLAUDE.local.md` (gitignored) para preferencias personales no
compartidas con el equipo, y un `CLAUDE.md` global en `~/.claude/CLAUDE.md`
que aplica a todos los proyectos — no confundir los tres.

### 2. Skills de proyecto — el "cómo"

`.claude/skills/*/SKILL.md`. Se activan solas: el sistema le muestra a Claude
la lista completa de skills instaladas (nombre + `description`) en cada
turno, y Claude decide cuál aplica comparando la tarea actual contra esas
descripciones. **Nunca hace falta que el usuario escriba "usa la skill X".**
La única palanca real es qué tan bien escrita está la `description` — debe
ser "un poco insistente" (explícita sobre cuándo activarse, con ejemplos),
no vaga.

Un campo de frontmatter útil que se descubre tarde: `context: fork` — corre
la skill en un subagente aislado automáticamente, sin tener que definir un
subagente completo aparte. Úsalo para procesos que generan mucho output
(capturas, logs) que no vale la pena meter en el hilo principal.

### 3. Subagentes — solo si hay evidencia concreta

`.claude/agents/*.md`. A diferencia de las skills, no se auto-descubren por
descripción de la misma manera — Claude decide cuándo delegarle algo a un
subagente, así que si algo *siempre* debe pasar por un subagente, hace falta
una regla explícita en `CLAUDE.md` que lo diga.

No construyas un subagente "por si acaso" — el costo real es la memoria que
puede tener asociada (ver punto 4), que hay que mantener. Antes de crear uno,
pregúntate: ¿el output es tan ruidoso que ensuciaría el hilo principal
(capturas, exploración larga)? ¿Hay conocimiento concreto y específico del
proyecto que valga la pena que persista entre ejecuciones, no solo teórico?
Si las dos respuestas son no, una skill simple (con o sin `context: fork`)
alcanza.

### 4. Memoria de subagente — `memory: project`/`user`/`local`

Campo de frontmatter en un subagente. Cada scope vive en un directorio
distinto (`.claude/agent-memory/<agente>/` para `project`, versionado y
compartido con el equipo; `~/.claude/agent-memory/<agente>/` para `user`,
cruzado entre proyectos; `.claude/agent-memory-local/` para `local`,
gitignored). Las primeras ~200 líneas del `MEMORY.md` del subagente se
inyectan en su prompt cada vez que arranca — por eso no conviene llenarlo de
especulación: cuesta contexto y, si queda desactualizado, confunde en vez de
ayudar.

### 5. Memoria automática de Claude — ya existe, no se construye

Persiste entre conversaciones, la escribe y lee solo Claude (no el usuario
directamente). Sirve para gotchas de herramientas, contexto del proyecto que
no está en el código, y feedback repetido del usuario. No hay que
"activarla" ni diseñarla — ya está ahí.

### 6. `git log` — el historial, no un documento

Nunca crear un archivo de "qué cambió" a mano — el historial de commits ya
es eso, siempre que los commits sean atómicos y descriptivos (ver la skill
de convención de commits que le corresponda a este proyecto).

## Catálogo humano — `.claude/SKILLS.md`

No es un mecanismo del sistema, es documentación para que el usuario audite
de un vistazo qué hay instalado, de dónde viene, y para qué sirve. Créalo
siempre al final de un bootstrap, con una tabla por cada skill: nombre,
fuente, propósito, y si es a medida o externa.
