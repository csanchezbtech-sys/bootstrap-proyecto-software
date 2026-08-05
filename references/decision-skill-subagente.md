# Criterio: skill simple vs. skill con fork vs. subagente con memoria

Tres preguntas, en orden, para cada proceso repetitivo identificado:

1. **¿Genera output ruidoso** que ensuciaría el hilo principal (capturas,
   logs largos, resultados de exploración extensos)? Si no, es una skill
   simple — para ahí.
2. Si sí genera ruido: **¿ese aislamiento necesita persistir conocimiento
   propio entre ejecuciones**, con algo concreto y ya conocido que valga la
   pena no redescubrir cada vez (no hipotético)? Si no, alcanza con una skill
   con `context: fork` — más simple de mantener que un subagente completo.
3. Si sí hay algo concreto que acumular: ahí sí, **subagente dedicado con
   `memory: project`** (o `user` si el conocimiento cruza proyectos, o
   `local` si es personal y no debe compartirse).

## Ejemplos ya vividos (Avela Swim, plantilla Symmetry)

- **Verificación visual con capturas de pantalla** → skill con `context: fork`,
  sin memoria. Genera ruido (capturas, comandos repetidos) pero no había
  ningún gotcha concreto de ese proyecto que acumular de entrada — se
  resolvió con configuración (ruta de salida fija), no con memoria.
- **Commits, sincronización pull/push, "verificar settings antes de
  código"** → skills simples, sin fork. Son disciplina/convención, no
  generan output pesado.
- **Investigar referencias de e-commerce reales para decisiones de diseño**
  → se consideró subagente con `memory: project` (para acumular qué sitios
  ya se evaluaron y los links que las clientas dan directamente) — pero al
  final se descartó por completo: lo que el usuario realmente necesitaba
  (planeación grande antes de ejecutar un lote largo de cambios) ya lo
  resuelve el **Plan Mode nativo** de Claude Code, no un subagente custom.
  Antes de construir un subagente, pregúntate si lo que se describe no es en
  realidad Plan Mode.

## Error a evitar

No conviertas todo en subagente por default "para que quede bien hecho". La
mayoría de los procesos de un proyecto de este tamaño terminan siendo skills
simples — el subagente es la excepción que se justifica con evidencia
concreta, no la opción por defecto.
