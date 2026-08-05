Plantilla genérica — adaptar `{{PLACEHOLDERS}}` y guardar como
`.claude/skills/verificacion-visual/SKILL.md` en el proyecto nuevo. Úsala
SOLO si el proyecto tiene interfaz de usuario web (Paso 0 del bootstrap) —
no tiene sentido para un backend, API o CLI puros.

---
name: verificacion-visual
description: Confirma con capturas reales (móvil y escritorio) que un cambio de interfaz en {{NOMBRE_PROYECTO}} se aplicó como se pidió y no rompió nada visualmente. Actívala SIEMPRE después de proponer o terminar cualquier cambio visual — antes de decirle al usuario que el cambio quedó listo o está terminado. También aplica cuando el usuario pide explícitamente "revisa que se vea bien", "verifica en móvil", "confirma el cambio". No la saltes por parecer un cambio pequeño o trivial.
context: fork
---

# Verificación visual

## Por qué

Nunca se confirma un cambio visual sin haberlo visto. El código puede sugerir
que algo se aplicó (una clase, una condición) sin que sea cierto — una
captura no.

## Herramienta

Usa `playwright-cli` (instálala con `npm install -g @playwright/cli@latest`
si no está, y `playwright-cli install --skills` para que Claude la use).
Configura `outputDir` en `.playwright/cli.config.json` para que las capturas
no queden sueltas en la raíz del repo, y agrega esa carpeta de salida al
`.gitignore`.

## Cómo hacerlo

1. Asegúrate de que `{{COMANDO_DEV}}` esté corriendo (normalmente en
   `{{URL_LOCAL}}`).
2. Espera a que el contenido se asiente antes de capturar — animaciones y
   lazy-load pueden tardar; una captura muy temprana puede salir vacía sin
   que eso sea un bug real.
3. Captura en escritorio: `playwright-cli screenshot`
4. Captura en móvil con un dispositivo real, no solo cambiando el tamaño de
   ventana: `playwright-cli open {{URL_LOCAL}} --device="iPhone 15"` y luego
   `playwright-cli screenshot`
5. Mira ambas capturas antes de decir nada — no infieras del código.

## Cuándo dar por bueno un cambio

Pasa si: (1) lo pedido está ahí visualmente, y (2) nada más se rompió. Si
cualquiera falla, no está listo — corrige y vuelve a verificar.
