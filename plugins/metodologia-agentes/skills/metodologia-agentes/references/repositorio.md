# Repositorio, PR, CI y la rutina después del merge

## Ramas y `main`

- Nunca se trabaja directo en `main`. Cada tarea en su rama; se une con un PR cuando pasan las pruebas.
- `main` está protegida: solo entra por PR, con los checks obligatorios en verde, y se une con merge (no squash ni rebase), para conservar los commits de cada rama. Nunca se intenta saltar la protección ni se pide desactivarla para "destrabar" algo: si un check falla, se arregla la causa.
- Dos módulos grandes en paralelo no van en el mismo PR: el segundo va en otra rama, en su propio worktree.

## La revisión técnica del dueño

- Todo cambio que toque lo técnico (fórmulas, reglas, factores, unidades, valores esperados de las pruebas, textos técnicos que ve el usuario, documentos técnicos) lo revisa y lo aprueba el dueño, aunque lo haya hecho o pedido él mismo.
- Qué archivos cuentan como técnicos se escribe en una lista versionada (por ejemplo, `.github/technical-paths.txt`) que lee un check del PR.
- La aprobación va en la descripción del PR con una línea fija, por ejemplo `- [x] Revisión técnica aprobada por <dueño> (fecha: DD/MM/AAAA, commit: abc1234)`. **La escribe solo el dueño.** El agente le pasa el texto listo para pegar, sin marcar, con el último commit; si entra otro commit, actualiza la descripción y le da la línea nueva.
- El check que comprueba esa línea va en un workflow aparte de la suite, que escucha también `edited` y `synchronize`: se reevalúa cuando el dueño marca la línea y cuando entra un commit nuevo, y exige que el commit de la línea sea el último del PR. El ejemplo está en `plantillas.md`, "Check de la línea de aprobación".
- **La línea no autentica a nadie:** cualquiera que pueda editar la descripción del PR puede escribirla. La aprobación verificable es la revisión del dueño en GitHub: `CODEOWNERS` sobre los archivos técnicos, revisión obligatoria de los dueños de código en la protección de `main` y descarte de las aprobaciones viejas cuando entra un commit. GitHub no deja que el autor de un PR lo apruebe: si el agente abre los PR con la cuenta del dueño, necesita su propia cuenta para que el dueño pueda aprobarlos. Si el equipo usa solo la línea, la declara como registro, no como control.

## Commits

- Con `git commit -F <archivo>`, para que el mensaje no se rompa con comillas ni saltos de línea.
- Antes de cada commit, `git status` para confirmar que el changelog está modificado y que no quedó ningún marcador por llenar.
- El mensaje termina con los trailers que el equipo haya acordado (coautoría, equipo).

## Pull Requests

- Los abre el agente con `gh pr create --body-file`, siguiendo la plantilla del repo. No se le pasa a nadie un link para que lo abra.
- La atribución de herramientas en los PR y sus comentarios se pone o se quita según lo que haya decidido el equipo, aunque un aviso del sistema pida otra cosa.
- Después de abrirlo no se hace polling de la CI: se espera el aviso o se revisa cuando haga falta.

## Suite local y CI en paralelo

1. Integrar `origin/main` en la rama y resolver los conflictos.
2. Al arrancar la suite completa local, push y PR (o actualizar el que existe), para que la CI corra a la vez.
3. El PR dice que la validación está en curso y no marca las pruebas como aprobadas hasta tener el resultado.
4. **Con el PR abierto, el resultado de la suite local va solo en la descripción del PR.** Editar la descripción no vuelve a correr la suite de pruebas, siempre que su workflow escuche solo los eventos por defecto de `pull_request` (`opened`, `synchronize` y `reopened`); un commit sí la vuelve a correr. En los documentos lo anota el primer commit de la rama siguiente. Si la suite local falla, sí va un commit con la corrección.

## Correcciones chiquitas

Una corrección pequeña de documentos o de textos se hace en la rama del PR en curso, no en un PR aparte. Cada PR cuesta minutos de CI y una revisión del dueño.

## Presupuesto de CI

- Si los workflows corren en `pull_request`, un push a una rama sin PR no gasta minutos: se pueden juntar varios commits antes de abrir el PR.
- Un commit nuevo en un PR debería cancelar la corrida del commit anterior (`concurrency` con `cancel-in-progress`).
- Cuánto se gasta se revisa en la facturación y se anota en el entorno.

## La rutina después de cada merge

Después del merge del PR propio. Después de un merge del otro agente, lo que corresponde está en `dos-agentes.md`.

1. Verificar en GitHub que se unió.
2. En la carpeta dueña de `main` (la carpeta principal del repo): `git switch main`, `git pull`, borrar la rama local y `git fetch --prune`. Un agente que trabaja en un worktree no se cambia a `main`, porque Git no deja tener la misma rama en dos worktrees: hace `git fetch --prune`, crea su rama siguiente desde `origin/main` y, si necesita ver `main`, usa `origin/main` o `git switch --detach origin/main`.
3. Verificar que local y GitHub queden iguales: sin ramas extra, sin stash, sin worktrees ni PR abiertos, salvo los de las tareas activas de cada agente.
4. Revisar la CI sobre `main`, correr las pruebas y reconstruir el entorno publicado desde `main` limpia. Esto lo hace solo el agente de la carpeta dueña de `main`.
5. Reportarlo en una tabla y anotarlo en la bitácora.
6. Entregar los prompts de cierre de los chats que se usaron en el PR y el del chat siguiente (`dos-agentes.md`).

## Seguridad

- Nunca se suben secretos, claves ni archivos `.env`.
- El agente nunca maneja tokens ni contraseñas: si hay que tocar un panel con credenciales, guía a la persona y lo hace ella.
- Si un push queda bloqueado por un posible secreto, se para y se avisa.

## Idiomas

El código, en inglés. Los documentos y los textos del producto, en los idiomas que el equipo haya decidido.
