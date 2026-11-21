# Bitácora de IA

Esta bitácora registra cómo trabajaste con el agente durante el laboratorio: qué le pediste, qué propuso, qué decidiste y cómo lo comprobaste. No se evalúa cuánto código escribió la IA, sino tu criterio: por qué aceptaste, rechazaste o ajustaste cada propuesta. Agrega una entrada por cada paso del laboratorio y otra cada vez que deshagas un cambio del agente.

## Plantilla

Copia este bloque para cada entrada nueva:

```markdown
### Entrada N

- **Fecha:** AAAA-MM-DD
- **Paso del laboratorio:** (por ejemplo: memoria del proyecto, plan, RED de completar, GREEN de completar, refactor)
- **Prompt clave (literal):**
  > Pega aquí el prompt exacto que usaste.
- **Qué propuso el agente:** resumen en una o dos líneas.
- **Decisión:** aceptado / rechazado / ajustado. Por qué: ...
- **Cómo lo verifiqué:** tests (`python -m pytest`), navegador (`python servidor.py`), lectura del cambio...
```

## Ejemplo

### Entrada 1

- **Fecha:** el día del laboratorio
- **Paso del laboratorio:** memoria del proyecto (`/init`)
- **Prompt clave (literal):**
  > /init
- **Qué propuso el agente:** creó `AGENTS.md` con un resumen del proyecto, cómo ejecutarlo y una descripción de los archivos.
- **Decisión:** ajustado. Quité la descripción archivo por archivo, porque el agente la puede deducir leyendo el código. Agregué lo que no puede adivinar: que `servidor.py` y `almacen.py` no se modifican, que los mensajes de error deben ser exactamente los del docstring y que nunca se cambia un test solo para que pase.
- **Cómo lo verifiqué:** leí el archivo completo antes de guardarlo y comprobé que los comandos que menciona funcionan: `python -m pytest` pasa los tests de `agregar` y `python servidor.py` abre la página.

## Mis entradas

### Entrada 1

- **Fecha:** 2026-11-21
- **Paso del laboratorio:** Git y memoria del proyecto (`/init`)
- **Prompt clave (literal):**
  > /init

  Antes de `/init` le di al agente dos reglas, que luego quedaron en la memoria:
  > antes de correr o hacer test recuerda tu no puedes modificar los test para que no salga error en los test

  > todo cambio que hagas en los codigos me los consultas y explicas para que, ventajas y desventajas
- **Qué propuso el agente:** usé Claude Code en lugar de OpenCode, así que creó `CLAUDE.md` (su equivalente de `AGENTS.md`) con los comandos, las reglas del proyecto, el flujo TDD y el contrato entre `tareas.py` y `servidor.py`. No incluyó una descripción archivo por archivo.
- **Decisión:** aceptado sin cambios. Por qué: cubre los cinco puntos que pide el README (los comandos `python -m pytest` y `python servidor.py`, no tocar `servidor.py` ni `almacen.py`, sin dependencias nuevas, mensajes de error exactos y no cambiar un test para que pase) y además incluye mi regla de que me consulte cada cambio de código. [CONFIRMAR] Mantuve la sección del contrato con `servidor.py` porque explica cómo encajan los archivos, algo que no se ve leyendo uno solo.
- **Cómo lo verifiqué:** leí el archivo completo con la explicación de cada sección. [CONFIRMAR] Comprobé que `python -m pytest` pasa los 8 tests de `agregar` y que `python servidor.py` abre la página.
- **Nota sobre el orden:** hice `git init` después de crear `CLAUDE.md`, no antes como pide el README. Para que el historial respete el orden, el primer commit (`chore: proyecto base`) lleva solo los archivos originales y `CLAUDE.md` va en otro commit (`docs: CLAUDE.md revisado`).
