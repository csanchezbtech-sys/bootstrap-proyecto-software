Plantilla genérica — confirma primero con el usuario si quiere esta
convención (basada en midu.dev) o ya tiene otra. Guardar como
`.claude/skills/commits-atomicos/SKILL.md`.

---
name: commits-atomicos
description: Define la estructura exacta que debe tener cada commit de git en este repositorio ({{NOMBRE_PROYECTO}}). Actívala cada vez que estés a punto de ejecutar "git commit" — sin excepción, incluso para cambios que parezcan menores. También aplica si el usuario pregunta cómo debería verse un commit, o menciona "commit", "mensaje de commit", "historial de git".
---

# Commits atómicos

## Formato

```
tipo(scope): descripción en presente, en {{IDIOMA_DEL_PROYECTO}}
```

- **`tipo`** — en inglés (convención reconocible): `feat`, `fix`, `docs`,
  `refactor`, `style`, `perf`, `build`, `ci`, `test`.
- **`(scope)`** — opcional, el módulo o parte del proyecto que toca (ej.
  `(auth)`, `(api)`, `(cli)`).
- Máximo ~50 caracteres en el título. Si no cabe, probablemente son dos
  commits, no uno — divide en vez de alargar.
- Cuerpo del commit para contexto extra cuando el título no baste.

## Un cambio = un commit

Si necesitas "y" para describir el commit, probablemente son dos commits.
