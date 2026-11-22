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

### Entrada 2

- **Fecha:** 2026-11-21
- **Paso del laboratorio:** plan (paso 2)
- **Prompt clave (literal):**
  > Sí, paso 2

  El agente siguió el prompt del README: un plan de tests para `completar`, `eliminar` y `editar`, uno por cada regla del docstring, sin escribir código.
- **Qué propuso el agente:** 13 tests (3 de `completar`, 3 de `eliminar`, 7 de `editar`), en el orden completar → eliminar → editar, con el ciclo RED → GREEN → commit por función y el REFACTOR de la validación del título en `editar`. Señaló un caso que el docstring no define (título vacío + id inexistente en `editar`) y propuso no testearlo.
- **Decisión:** aceptado sin cambios. El plan pasaba las tres preguntas del README: cubre cada regla del docstring, empieza por los tests y no toca `servidor.py` ni agrega dependencias.
- **Cómo lo verifiqué:** comparé cada test del plan con las reglas de los docstrings de `tareas.py`.

### Entrada 3

- **Fecha:** 2026-11-21
- **Paso del laboratorio:** RED de `completar` (paso 3)
- **Prompt clave (literal):**
  > Acepto el plan

  > Sí, aplícalo
- **Qué propuso el agente:** cambiar el import a `from tareas import agregar, completar` (solo `completar`, para que cada commit lleve lo de su función) y 3 tests: marca como hecha y devuelve la tarea (comprobando que la otra no cambia), id inexistente da `ValueError("No existe la tarea 7")` sin modificar la lista, y completar una tarea ya hecha la deja hecha. Ofreció dos formas de comprobar el mensaje: `match=` como en `agregar` u otra más estricta con `^...$`.
- **Decisión:** aceptado con `match=` igual que en `agregar`, porque el README pide el mismo estilo que esos tests.
- **Cómo lo verifiqué:** le pedí al agente que ejecutara `python -m pytest -v`: 8 passed y 3 failed, los tres con `NotImplementedError: completar() aún no está implementada`. Ningún test nuevo pasó ni falló por otro motivo.

### Entrada 4

- **Fecha:** 2026-11-21
- **Paso del laboratorio:** GREEN de `completar` (paso 4)
- **Prompt clave (literal):**
  > Sí, aplícala
- **Qué propuso el agente:** un `for` que busca la tarea por id, pone `hecha = True` y la devuelve; si no la encuentra, lanza `ValueError(f"No existe la tarea {id_tarea}")`. Descartó por ahora una función auxiliar `buscar_tarea` (adelantarse antes de que haya duplicación) y `next(...)` (menos legible).
- **Decisión:** aceptado. Asigna `True` en vez de invertir el valor, así que cumple la regla de la tarea ya hecha.
- **Cómo lo verifiqué:** `python -m pytest -v` dio 11 passed. Con `git diff` comprobé que solo cambió el cuerpo de `completar` y ningún test. [CONFIRMAR] En el navegador, al pulsar Completar la tarea aparece tachada en verde.

### Entrada 5

- **Fecha:** 2026-11-21
- **Paso del laboratorio:** deshacer (paso 5)
- **Prompt clave (literal):**
  > Paso 5: deshacer

  > Sí, aplícala

  El agente usó el prompt del README («Traduce al inglés todos los mensajes de error de tareas.py») y me consultó antes de aplicarlo.
- **Qué propuso el agente:** traducir los 5 mensajes de los `raise` de `tareas.py` (no los docstrings). Explicó que Claude Code no tiene `/undo` como OpenCode y que hay dos formas de deshacer: `git restore tareas.py` o el rewind (puntos de restauración).
- **Decisión:** aplicado a propósito para practicar, y deshecho con `git restore tareas.py`, que ejecutó el agente a mi pedido. Elegí `git restore` porque funciona sin importar quién hizo el cambio y no había otros cambios sin commit que se pudieran perder.
- **Cómo lo verifiqué:** `git diff --stat` mostró 5 líneas cambiadas, solo en `tareas.py`. [CONFIRMAR] Revisé el diff completo con `git diff`. Con la traducción, `python -m pytest` dio 3 failed y 8 passed: fallaron los tests de título vacío, título de 81 caracteres y `completar` con id inexistente, por «Regex pattern did not match». Después de restaurar, `git status` dijo «nothing to commit, working tree clean» y los tests volvieron a 11 passed.
- **Nota:** los mensajes de error son parte del contrato: traducirlos rompe los tests, y la solución es deshacer el código, no cambiar los tests. Límites: ni `/undo` ni el rewind revierten lo que cambie un comando de terminal, y `git restore` solo deshace cambios sin commit (para un cambio ya commiteado haría falta `git revert`).
