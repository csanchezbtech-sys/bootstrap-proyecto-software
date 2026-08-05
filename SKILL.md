---
name: bootstrap-proyecto-software
description: Configura desde cero el contexto de Claude Code para un proyecto de software nuevo, sea cual sea el stack o el dominio (ERP, API, librería, CLI, app web, lo que sea) — hace una entrevista general sobre el proyecto, arma el CLAUDE.md con esas respuestas, instala automáticamente el set base de skills ya probado (control de versiones, calidad de CLAUDE.md, specs, documentación de decisiones, etc.), suma frontend/verificación visual solo si el proyecto tiene interfaz de usuario, y cierra revisando la calidad del CLAUDE.md resultante. Actívala cuando el usuario diga que va a empezar un proyecto de software nuevo, que quiere "montar esto en Claude Code", "configurar el contexto como hicimos con [otro proyecto]", o pida ayuda para arrancar un repositorio desde cero. No asume ningún dominio, lenguaje ni framework específico — se adapta preguntando.
---

# Bootstrap de proyecto de software + Claude Code

## Por qué existe

Es la versión general de `bootstrap-tienda-shopify`, que nació de hacer este
mismo proceso a mano para una tienda de e-commerce (Symmetry sobre Shopify).
El proceso —entrevista → armar `CLAUDE.md` → set base de skills → completar
lo que falte → cerrar con auditoría— generaliza sin problema a cualquier
proyecto de software; lo único específico de e-commerce era el catálogo de
skills, no el proceso en sí.

**Lo que es fijo:** el set base de skills del Paso 2. Es infraestructura de
Claude Code y buenas prácticas de ingeniería (git, specs, documentación de
decisiones, revisión de `CLAUDE.md`...) que aplica a cualquier proyecto de
software. Eso se instala siempre, sin volver a decidirlo desde cero.
`find-skills` (Paso 4) queda para lo que ese set base no cubre: algo
específico del stack, el dominio, o el framework de este proyecto en
particular.

## Paso 0 — Entrevista antes de tocar nada

No empieces a instalar ni a escribir `CLAUDE.md` sin estas respuestas.
Pregúntalas de corrido, no una por una en turnos separados:

1. **¿Qué es este proyecto?** (propósito y dominio — ERP, API interna,
   librería, app web, CLI, automatización, lo que sea) y **¿en qué etapa
   está** (arrancando de cero, o ya tiene código y equipo)?
2. **¿Qué stack usa o va a usar** (lenguaje, framework, base de datos,
   infraestructura)? Determina si vale la pena buscar con `find-skills`
   (Paso 4) una skill oficial o de comunidad específica de ese stack.
3. **¿Tiene o va a tener interfaz de usuario** (web, desktop, móvil), o es
   backend/librería/CLI puro? Esto decide si se instala el bloque
   condicional de frontend + verificación visual (Paso 2.4) — no lo
   instales si la respuesta es "no tiene UI".
4. **¿El proyecto ya tiene `CLAUDE.md`**, o arranca de cero? Si ya existe,
   léelo completo antes de proponer nada — no lo sobrescribas a ciegas.
5. **¿En qué idioma se documenta y se comitea** este proyecto? Alimenta la
   convención de commits (Paso 2.3).
6. **¿Hay un historial de trabajo previo** (otro repo, otro intento
   abandonado) del que se pueda aprender qué NO volver a hacer? La regla más
   importante de un proyecto casi siempre sale de un fracaso anterior, no de
   una decisión en frío.
7. **¿Dónde o cómo se va a desplegar/publicar**, si ya se sabe? Alimenta
   `shipping-and-launch` (Paso 2.1).

Con las respuestas, sigue los pasos 1-6 **ejecutando directamente** — no te
detengas a proponer un plan y esperar aprobación en cada paso, salvo antes de
cualquier acción irreversible (borrar algo, publicar, forzar un push).

## Paso 1 — Arma (o actualiza) `CLAUDE.md` con esas respuestas

Con lo que salió del Paso 0, escribe o actualiza `CLAUDE.md`: qué es el
proyecto, qué stack usa, las reglas de comportamiento no negociables que ya
existan, y qué queda explícitamente pendiente de definir (nunca inventes
identidad, arquitectura o decisiones que el usuario no dio — anótalo como
pendiente, igual que se hace con la marca en un proyecto de e-commerce). Este
es el documento que se revisa al cierre (Paso 6) con `claude-md-improver` —
no hace falta que quede perfecto ahora, sí que exista y sea honesto sobre lo
que falta.

## Paso 2 — Instala el set base de skills

Este set salió de evaluar decenas de skills candidatas una por una en un
proyecto real — no se vuelve a decidir en cada proyecto nuevo. Se instala
completo, siempre (salvo el bloque 2.4, que es condicional), sin pasar por
`find-skills` ni pedir confirmación: son skills de comportamiento/
infraestructura, no acciones irreversibles.

**Regla dura: todo esto se instala a nivel de proyecto, nunca a nivel de
usuario.** Ningún comando `npx skills add` de este paso lleva `-g` — ese flag
instala a nivel de usuario (`~/.claude/skills/`, compartido entre *todos* los
proyectos de la máquina), y este set base es específico del proyecto que se
está bootstrapeando. Instalarlo globalmente mezclaría el catálogo de skills
de proyectos distintos en la misma máquina.

### 2.1 — Del ecosistema (instalación directa, sin adaptar)

Corre las 11 en cualquier orden — pueden ir en paralelo. Si algún comando
falla porque el repo de origen cambió de estructura, no te detengas: sigue
con las demás e infórmalo al final del paso.

| Skill | Para qué | Instalación |
|---|---|---|
| `claude-md-improver` | Audita y mejora el `CLAUDE.md` resultante (se usa en el Paso 6) | `npx skills add https://github.com/anthropics/claude-plugins-official/tree/main/plugins/claude-md-management/skills/claude-md-improver` |
| `skill-creator` | Crear skills a medida cuando de verdad no existe una externa que sirva | `npx skills add https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator/skills/skill-creator` |
| `find-skills` | Buscar e instalar skills nuevas del ecosistema — la que se usa en el Paso 4 | `npx skills add https://github.com/vercel-labs/skills/tree/main/skills/find-skills` |
| `git-workflow-and-versioning` | Referencia de convenciones de commit/branching/versionado | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/git-workflow-and-versioning` |
| `context-engineering` | Cómo estructurar `CLAUDE.md` y el contexto del proyecto en general | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/context-engineering` |
| `documentation-and-adrs` | Registrar decisiones de arquitectura/proyecto con contexto | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/documentation-and-adrs` |
| `incremental-implementation` | Dividir cambios grandes en pasos chicos y verificables | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/incremental-implementation` |
| `spec-driven-development` | Crear specs antes de construir cuando algo todavía no está definido | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/spec-driven-development` |
| `source-driven-development` | Formaliza basarse en documentación oficial antes de inventar | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/source-driven-development` |
| `shipping-and-launch` | Checklist de lanzamiento, rollback y monitoreo | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/shipping-and-launch` |
| `agent-development` | Diseñar subagentes, si en algún momento hace falta aislar una tarea con memoria propia | `npx skills add https://github.com/anthropics/claude-code/tree/main/plugins/plugin-dev/skills/agent-development` |

Fuentes verificadas contra la API de GitHub (rama y ruta exacta de cada
`SKILL.md`) el 2026-08-05 — si algún comando devuelve 404, el repo de origen
cambió de estructura; busca el nombre de la skill con `find-skills` como
alternativa puntual.

### 2.2 — Ya empaquetada aquí, sin dependencia externa

| Skill | Para qué | Instalación |
|---|---|---|
| `gestion-tareas` | Mantiene `TAREAS.md` — checklist vivo de pendientes entre sesiones | Copia tal cual [references/listas-para-usar/gestion-tareas/SKILL.md](references/listas-para-usar/gestion-tareas/SKILL.md) a `.claude/skills/gestion-tareas/SKILL.md` del proyecto nuevo — no tiene nada específico de ningún proyecto, no necesita adaptación |

### 2.3 — Se construyen a medida, siempre (plantilla + adaptación por entrevista)

No dependen del stack, pero sí de este proyecto puntual (nombre, idioma) —
se generan con `skill-creator` a partir de la plantilla correspondiente,
usando las respuestas del Paso 0:

| Skill a construir | Plantilla base |
|---|---|
| `commits-atomicos` | [references/plantillas/commits-atomicos.md](references/plantillas/commits-atomicos.md) |
| `auditoria-repo` | [references/plantillas/auditoria-repo.md](references/plantillas/auditoria-repo.md) |

### 2.4 — Condicional: SOLO si el proyecto tiene interfaz de usuario (respuesta 3 del Paso 0)

Si el proyecto es backend/librería/CLI puro, **no instales nada de este
bloque** — no aporta nada y ensucia el catálogo de skills con cosas que
nunca se van a usar.

| Skill | Para qué | Instalación |
|---|---|---|
| `frontend-design` | Dirección estética al construir o rehacer UI | `npx skills add https://github.com/anthropics/skills/tree/main/skills/frontend-design` |
| `frontend-ui-engineering` | UI de producción, accesible, responsive | `npx skills add https://github.com/addyosmani/agent-skills/tree/main/skills/frontend-ui-engineering` |
| `playwright-cli` | Automatización de navegador — base técnica de `verificacion-visual` | `npx skills add https://github.com/microsoft/playwright-cli/tree/main/skills/playwright-cli` |
| `verificacion-visual` | Confirma con capturas reales que un cambio de UI se ve como se pidió | Construir a medida desde [references/plantillas/verificacion-visual.md](references/plantillas/verificacion-visual.md) |

## Paso 3 — Taxonomía de contexto (aplica siempre, sea cual sea el proyecto)

Ver [references/taxonomia.md](references/taxonomia.md) para el detalle
completo. Resumen:

| Capa | Vive en | Regla |
|---|---|---|
| Reglas permanentes del proyecto | `CLAUDE.md` | Nunca duplica el procedimiento de una skill — si algo cambia, se edita en un solo lugar |
| Procedimientos con activación automática | `.claude/skills/*/SKILL.md` | Se activan solas por su `description`, nunca hace falta invocarlas a mano |
| Memoria de un subagente puntual | `.claude/agent-memory/<agente>/` con `memory: project` | Solo si hay algo concreto que acumular — no de entrada, sin evidencia |
| Contexto entre sesiones de Claude | Memoria automática nativa | No se construye, ya existe |
| Qué cambió / historial | `git log` | Nunca se documenta a mano en ningún archivo |
| Catálogo humano de skills instaladas | `.claude/SKILLS.md` | Para que el usuario audite de un vistazo, no lo lee el sistema |

## Paso 4 — Buscar lo que el set base no cubre

Para necesidades que **no** estén ya resueltas en el Paso 2 — casi siempre
algo específico del stack (una skill oficial de React, Django, Rails, lo que
sea), del dominio (contabilidad, logística, lo que resuelva el ERP), o del
framework exacto que se eligió:

1. Usa `find-skills` — ya quedó instalada en el Paso 2.1.
2. Verifica calidad real: instalaciones (prefiere >1K), reputación de la
   fuente (oficial > comunidad conocida > desconocido), estrellas del repo si
   hace falta más certeza.
3. Solo si de verdad no existe nada bueno, construye a medida con
   `skill-creator`.

## Paso 5 — Identificar procesos repetitivos propios de este proyecto

Si el proyecto ya tiene historial de git, léelo (`git log --oneline`) para
detectar patrones reales en vez de adivinar. Si arranca de cero, pregúntale
al usuario qué procesos ya sabe que va a repetir mucho — más allá de los que
ya cubre el set base del Paso 2.

Para cada proceso nuevo que aparezca, decide con este criterio (detalle en
[references/decision-skill-subagente.md](references/decision-skill-subagente.md)):
- ¿Genera output ruidoso? → aislar (skill con `context: fork`, o subagente).
- ¿Necesita acumular conocimiento propio entre sesiones, con evidencia
  concreta de que hace falta (no especulativo)? → subagente con
  `memory: project`.
- Si no a ambas → skill simple.

No conviertas todo en subagente por defecto — la mayoría de los procesos de
un proyecto de software terminan siendo skills simples.

## Paso 6 — Cerrar

1. Corre `claude-md-improver` (ya instalada en el Paso 2.1) sobre el
   `CLAUDE.md` armado en el Paso 1 — esta es la revisión de calidad que
   confirma que el documento quedó bien estructurado, no solo que existe.
2. Corre una auditoría de consistencia con el `auditoria-repo` construido en
   el Paso 2.3, para confirmar que todo lo documentado coincide con lo
   realmente instalado antes de dar el trabajo por terminado.
3. Crea o actualiza `.claude/SKILLS.md` con el catálogo final — distingue en
   la tabla qué vino del set base (Paso 2) de lo que se buscó o construyó
   puntual para este proyecto (Pasos 4-5).

## Qué no hacer

- No copies el `CLAUDE.md` de otro proyecto y le cambies el nombre — la
  entrevista existe para no arrastrar decisiones que no aplican.
- No instales el bloque 2.4 (frontend/verificación visual) si el proyecto no
  tiene interfaz de usuario — no todo software tiene UI, y da igual que "por
  si acaso" se vea más completo.
- No uses las plantillas de `bootstrap-tienda-shopify` (`editor-antes-que-
  código`, `sync-tema-shopify`, `shopify-liquid`, prompts de imagen) en un
  proyecto de software general — son específicas de plantillas de pago de
  Shopify, no generalizan.
- No instales una skill del Paso 4 sin verificar su calidad real solo porque
  el nombre suena bien.
- No agregues `-g` a ningún `npx skills add` — ni en el set base ni en lo
  que se busque en el Paso 4. Todo va a nivel de proyecto.
- No documentes algo como instalado antes de instalarlo de verdad — verifica
  en disco al final, no confíes en la memoria de la conversación.
