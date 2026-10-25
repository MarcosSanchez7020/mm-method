# Dos agentes en el mismo repositorio

Cuando trabajan dos agentes, por ejemplo Claude Code y Codex, el riesgo no es técnico sino de coordinación: que uno pise el trabajo del otro, que uno dé por hecho lo que el otro hizo o que las reglas de un chat no lleguen al otro. Estas reglas valen para los dos y para cualquier chat nuevo de cualquiera de los dos. Se escriben en `CLAUDE.md` (y `AGENTS.md` apunta ahí).

## Carpetas y ramas

1. **Cada agente en su carpeta.** Uno en la carpeta principal del repo; el otro en un worktree de Git (`.worktrees/<agente>`). Solo la carpeta principal tiene `main`: el otro agente nunca se cambia a esa rama. Ninguno abre, edita, cambia de rama ni corre comandos que escriban en la carpeta del otro, ni borra el worktree o las ramas del otro.
2. **Cada tarea, de un solo agente, en su propia rama.** El dueño asigna cada tarea. Las que tocan el mismo módulo van en serie; solo van en paralelo tareas de módulos distintos.
3. **Cada rama nace de `main` al día:**

   ```bash
   git fetch --prune
   git switch --no-track -c <rama> origin/main
   git push -u origin <rama>   # el primer push
   ```

   Sin `--no-track`, la rama queda siguiendo a `main` y un push equivocado puede ir a donde no es.
4. **Excepción: uno diseña y el otro programa.** La rama del que programa nace de la del que diseñó (no de `main`), para traer el diseño. El que diseñó deja de escribir en su rama apenas el otro la tome. El que programa abre el PR, que lleva los commits de los dos. Cada excepción se escribe en `CLAUDE.md`.

## Comunicación

- **Los agentes no se hablan directo: el dueño pasa los mensajes.** Si uno necesita algo del otro, escribe el mensaje listo para pegar, en un bloque de código. La única excepción es el revisor de solo lectura.
- **Ninguno da por hecho lo que el otro hizo:** lo verifica en Git y en la memoria.
- **Las decisiones son solo del dueño.** Cada agente le pasa las suyas y anota las respuestas en el registro de su rama. Si una decisión afecta la tarea del otro, lo dice y le prepara el mensaje.
- **A un agente que está trabajando se le puede sumar una tarea sin interrumpirlo**, con un mensaje de "steer" que agrega lo nuevo a lo que ya hace.

## Archivos compartidos

- El changelog, la memoria y los documentos los tocan las dos ramas.
- Antes de abrir un PR: `git fetch` y unir `origin/main` en la rama (merge, nunca rebase ni force-push). Los conflictos se resuelven conservando los dos lados, nunca borrando lo del otro, y se corre la suite completa después de unir.
- El título de la sección del changelog de un PR unido lo pone la rama siguiente del agente que abrió ese PR. El otro lo deja como esté.

## Servidores y entorno publicado

- Cada agente tiene su puerto para probar su rama y apaga su servidor al terminar.
- El entorno publicado se construye desde `main` y lo reconstruye un solo agente, en su rutina después de cada merge.

## PR y merge

- Cada agente abre su PR y le pasa al dueño la línea de aprobación técnica con su último commit. El dueño la escribe y hace el merge. Ninguno toca el PR del otro.
- Después de un merge del otro agente, ninguno abandona su tarea activa:
  - el agente de la carpeta principal verifica el merge, hace `git fetch --prune` y revisa la CI de `main`; el cambio a `main` y la reconstrucción del entorno esperan a que su carpeta esté en un commit;
  - el agente que tiene una tarea en curso conserva su rama, hace `git fetch --prune` y une `origin/main` en ella (merge, nunca rebase) antes de seguir.
- Después del merge de su propio PR, cada agente crea su rama siguiente desde `origin/main` cuando le asignen la tarea siguiente. El agente del worktree nunca se cambia a `main` (Git no deja tener la misma rama en dos worktrees); si necesita ver `main`, usa `origin/main` o `git switch --detach origin/main`.
- Después de cada PR en el que se usaron chats del otro agente, el agente principal le entrega al dueño, completos y en bloques de código, el prompt de cierre de cada chat que se usó y el del chat siguiente. El del chat siguiente es autocontenido y empieza por leer `AGENTS.md`, `CLAUDE.md` y la memoria.

## Modelos por función

- Planear, diseñar, coordinar y juzgar: el modelo más capaz.
- Ejecutar (leer, programar tareas acotadas, pruebas independientes, mutación): un modelo más económico, pero suficiente.
- **Los ejecutores no deciden.** Reciben contratos exactos: archivos, firmas, comportamiento caso por caso, qué función llamar, los casos de prueba con su valor esperado y cuándo está terminado. Lo que pida criterio vuelve al que coordina o al dueño.
- Si una tarea de ejecución es delicada, el que coordina puede subirle el modelo y lo dice.
