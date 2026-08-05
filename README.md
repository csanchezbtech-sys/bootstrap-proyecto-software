# bootstrap-proyecto-software

Skill para [Claude Code](https://claude.com/claude-code) que configura desde cero el
contexto de un proyecto de software nuevo — **sin importar el stack, el lenguaje o el
dominio** (ERP, API, librería, CLI, app web, automatización, lo que sea).

Es la versión general de [`bootstrap-tienda-shopify`](https://github.com/csanchezbtech-sys/bootstrap-tienda-shopify),
que nació de hacer este mismo proceso a mano para una tienda de e-commerce sobre
Shopify. El proceso generaliza sin problema: entrevista → armar `CLAUDE.md` →
instalar el set base de skills ya probado → completar con lo que falte → cerrar
con auditoría. Lo único que era específico de e-commerce en la versión original
era el catálogo de skills, no el proceso.

## Qué hace al ejecutarse

1. **Entrevista corta** — qué es el proyecto, qué stack usa, si tiene interfaz de
   usuario o es backend/librería/CLI puro, si ya existe `CLAUDE.md`, idioma, historial
   previo, plan de despliegue.
2. **Arma `CLAUDE.md`** con esas respuestas — nunca inventa identidad o arquitectura
   que el usuario no dio.
3. **Instala directo (sin buscar) el set base de 11 skills** del ecosistema que
   aplican a cualquier proyecto de software: control de versiones, specs antes de
   construir, documentación de decisiones, implementación incremental, checklist de
   lanzamiento, creación de skills a medida, y más — cada una con su repo de origen
   documentado en `SKILL.md`.
4. Copia `gestion-tareas` tal cual (ya es 100% genérica) y construye a medida, con
   plantillas, la convención de commits y la auditoría de repo — las únicas dos que
   sí dependen del proyecto puntual (nombre, idioma).
5. **Solo si el proyecto tiene interfaz de usuario**, suma frontend + verificación
   visual con Playwright — nunca si es backend/librería/CLI puro.
6. Para lo que ese set base no cubre (algo específico del stack o el dominio), busca
   con `find-skills` en vez de asumir que hay que construir algo nuevo.
7. Cierra corriendo `claude-md-improver` sobre el `CLAUDE.md` resultante y una
   auditoría de consistencia antes de dar el trabajo por terminado.

## Instalación

Requiere Node.js (para `npx`) y estar corriendo dentro de un proyecto con Claude Code.

```bash
npx skills add csanchezbtech-sys/bootstrap-proyecto-software
```

Esto descarga el skill dentro de `.claude/skills/bootstrap-proyecto-software/` del
proyecto actual. No hace falta invocarlo a mano — Claude Code lo activa solo cuando
detecta que el usuario quiere arrancar un proyecto de software nuevo.

### Alternativa manual (sin `npx skills`)

```bash
git clone https://github.com/csanchezbtech-sys/bootstrap-proyecto-software .claude/skills/bootstrap-proyecto-software
rm -rf .claude/skills/bootstrap-proyecto-software/.git
```

## Uso

Abre Claude Code en el proyecto nuevo y dile, por ejemplo:

> "Vamos a montar un proyecto de software nuevo, es un ERP para gestión de inventario"

La skill arranca con la entrevista corta y, con esas respuestas, ejecuta el resto
del proceso sin pedir aprobación en cada paso — salvo antes de cualquier acción
irreversible.

## Qué NO instala este repo

A propósito, no incluye nada específico de e-commerce/Shopify (Liquid, temas,
CRO, prompts de imagen de producto) — para eso está el repo hermano
[`bootstrap-tienda-shopify`](https://github.com/csanchezbtech-sys/bootstrap-tienda-shopify).
Tampoco fuerza frontend en proyectos sin interfaz de usuario.

## Actualizar

```bash
npx skills update
```

## Licencia

MIT — ver [LICENSE](LICENSE).
