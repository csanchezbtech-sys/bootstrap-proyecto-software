Plantilla genérica, sin cambios de fondo necesarios entre proyectos. Guardar
como `.claude/skills/auditoria-repo/SKILL.md`.

---
name: auditoria-repo
description: Revisa todo el repositorio buscando referencias rotas o desactualizadas — documentos que mencionan archivos que ya no existen, skills documentadas que no están realmente instaladas (o viceversa), IDs/rutas desactualizados. Actívala SOLO cuando el usuario lo pida explícitamente. No es una skill de fondo — revisión bajo demanda, no en cada turno.
---

# Auditoría de repositorio

## Qué revisar

1. Referencias a archivos que ya no existen en `CLAUDE.md`, `.claude/SKILLS.md`,
   y cualquier `.md` en la raíz — confirma que cada archivo mencionado existe.
2. **Skills documentadas en `.claude/SKILLS.md` vs. las que realmente están en
   `.claude/skills/` en disco** — no confíes en lo que la conversación dice
   que se instaló, verifica en disco. (Esto encontró un error real en un
   proyecto anterior: varias skills se documentaron como instaladas sin
   haberse instalado de verdad.)
3. IDs/valores que podrían haber cambiado (ej. IDs de entorno, URLs de
   servicios externos, versiones de dependencias fijadas) — coteja contra lo
   verificable, no contra lo que dice la documentación.
4. Secciones "pendiente de definir" que ya se resolvieron y nadie actualizó.
5. `.gitignore` — patrones muertos o carpetas nuevas sin ignorar.

## Cómo reportar

No arregles nada sin confirmar — es una auditoría, no una skill de
escritura. Lista corta: qué encontraste, dónde, qué acción sugieres. Si no
hay hallazgos, dilo así de simple.
