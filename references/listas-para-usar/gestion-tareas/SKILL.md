---
name: gestion-tareas
description: Mantiene actualizado TAREAS.md, el checklist vivo de pendientes del proyecto — para que nada se pierda entre sesiones distintas. Actívala cuando el usuario diga que algo queda pendiente, para después, o lo posponga explícitamente; cuando termines una sesión de trabajo dejando algo a medias o sin completar; cuando se arme un plan o checklist de varios pasos que no se va a ejecutar todo de una vez; o cuando el usuario pregunte "qué me quedó pendiente", "en qué íbamos", "qué falta por hacer". No es para tareas efímeras de un solo turno (para eso ya existe el todo list nativo de la sesión) — es para lo que sobrevive al cierre de la conversación.
---

# Gestión de tareas pendientes

## Por qué existe

El todo list nativo de una sesión desaparece cuando la conversación termina.
La memoria automática de Claude la controla Claude, no es un checklist que
el usuario pueda leer y tachar. `git log` mira hacia atrás, no hacia
adelante. Nada cubría "qué quedó a medias y hay que retomar la próxima vez"
— por eso existe `TAREAS.md`.

## Cómo mantenerlo

- **Agregar un pendiente**: una línea `- [ ] descripción corta`, con
  suficiente contexto para entenderla sin recordar la conversación completa
  (igual que un commit — que se entienda en frío).
- **Algo pasa a medias, no terminado**: muévelo a la sección "En progreso"
  con una nota de en qué quedó exactamente, no solo que "está en proceso".
- **Se termina algo**: bórralo de la lista. No lo archives ni lo marques
  como hecho y lo dejes ahí — el historial de qué se hizo ya lo tiene
  `git log`, este archivo es solo lo que falta, no un registro de lo que ya
  pasó. Si la línea se vuelve irrelevante (ya no aplica, se descartó la
  idea), bórrala igual, con un commit que lo explique si hace falta.
- **Al empezar a trabajar en algo nuevo**, si `TAREAS.md` tiene contenido,
  revísalo primero — puede haber algo relacionado con lo que se va a hacer.

## Formato de una línea

```
- [ ] Qué hay que hacer, con contexto suficiente para entenderlo en frío
```

No hace falta fecha ni autor — para eso está `git blame` sobre este mismo
archivo si algún día hace falta saber cuándo se agregó algo.
