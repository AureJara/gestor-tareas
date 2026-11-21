# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Comandos

- Tests: `python -m pytest`
- Un solo test: `python -m pytest test_tareas.py::test_agregar_titulo_vacio_da_error` (o `-k completar` para un grupo)
- Vista web: `python servidor.py` → http://127.0.0.1:8000 (otro puerto: `python servidor.py 8001`). Vuelve a cargar `tareas.py` en cada petición: basta recargar la página, sin reiniciar el servidor.

## Reglas del proyecto

- No modificar `servidor.py` ni `almacen.py`.
- Sin dependencias nuevas: solo la biblioteca estándar (pytest solo para los tests).
- Los mensajes de `ValueError` deben ser exactamente los de los docstrings de `tareas.py`: los tests los comparan.
- Nunca cambiar un test existente para que pase: si falla, se corrige el código. Si un test parece incorrecto, avisar al usuario y que él decida.
- Antes de cualquier cambio de código, consultar al usuario: explicar qué se cambia, para qué, ventajas y desventajas, y esperar su aprobación.

## Flujo de trabajo (TDD, una función a la vez)

Orden: `completar`, `eliminar`, `editar`.

1. RED: escribir en `test_tareas.py` un test por cada regla del docstring, con el mismo estilo que los tests de `agregar`. Deben fallar con `NotImplementedError`; si alguno pasa con la función sin implementar, no prueba lo que se cree.
2. GREEN: implementar la función en `tareas.py` sin tocar los tests.
3. Commit del test y el código juntos (p. ej. `feat: completar tareas`). Prefijos convencionales: `feat:`, `docs:`, `chore:`.

En `editar`, con los tests en verde, extraer la validación del título a una sola función que usen `agregar` y `editar` (REFACTOR), y comprobar que todos los tests siguen en verde antes del commit.

`ai-log.md` es la bitácora del laboratorio, donde se registran los prompts clave y las decisiones.

## Contrato entre `tareas.py` y `servidor.py`

- Cada función recibe la lista, la modifica directamente y devuelve el dict de la tarea afectada. Si no devuelve un dict con `"id"`, la página avisa de que falta el `return`.
- `ValueError` → aviso rojo con su mensaje; `NotImplementedError` → aviso amarillo de función pendiente; cualquier otra excepción → "Error inesperado". El servidor solo guarda en `tareas.json` si la función terminó sin error.
- Validar antes de modificar la lista: los tests comprueban que la lista queda igual cuando se lanza un error.
- El servidor convierte el `id` del formulario a `int` antes de llamar a la función.
