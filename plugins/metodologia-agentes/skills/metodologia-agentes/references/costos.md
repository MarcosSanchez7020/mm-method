# Contexto, uso y tamaño de los workflows

El contexto de un chat y el uso semanal de una cuenta son finitos. Un proyecto largo los gasta sin que nadie lo note, y cuando se acaban, el trabajo queda a medias.

## Contexto

- Los resultados de un workflow o de muchos agentes no se leen completos: se resumen con un script del scratchpad que imprime solo lo necesario.
- A los agentes se les piden reportes cortos (menos de 25 líneas) y con esquema fijo.
- Las correcciones de texto repartidas en muchos archivos se delegan a un agente con instrucciones exactas.
- No se vuelven a leer archivos enteros que ya se leyeron.
- Si el contexto se está llenando, el agente avisa y cierra con un prompt para un chat nuevo, con la memoria al día (`memoria.md`).

## Tamaño de los workflows

- **En tareas que no son programar** (planear, inventariar, leer documentos, armar listas de decisiones), el agente lee y consolida por sí mismo. Si hace falta una revisión independiente, usa pocos agentes (de 2 a 4) y verifica solo lo delicado. Antes de lanzar más de 6, dice cuántos y para qué, y espera el sí.
- **Cuando se trata de programar**, ya con la palabra del dueño, el agente lanza los agentes y workflows que necesite: pruebas independientes, mutación, laboratorio. Las revisiones siguen la regla de preguntar quién las hace.
- No se lanzan agentes para temas que el dueño no pidió: se pregunta antes.

## Uso semanal

- Cuando la cuenta va cerca de su límite, el agente lo dice y propone opciones: pasar el diseño o las revisiones al otro agente, trabajar sin subagentes o esperar el reinicio.
- Antes de que se acabe, todo queda en commit y con su nota en la bitácora.
- Si la cuenta es compartida entre personas, se avisa antes de lanzar muchos agentes, porque el límite es de todos.

## Modelos

El modelo más capaz para planear, diseñar y juzgar; uno más económico para ejecutar contratos cerrados (`dos-agentes.md`). Un ejecutor caro haciendo trabajo mecánico gasta el uso que después falta para decidir bien.
