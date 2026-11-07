# Contexto, uso, umbrales y economía de tokens

El contexto de un chat y el uso semanal de una cuenta son finitos. Un proyecto largo los gasta sin que nadie lo note, y cuando se acaban, el trabajo queda a medias.

## Economía de tokens

- **El costo de un chat es su contexto por sus turnos.** Cada turno vuelve a pagar todo lo que el chat lleva leído; por eso un chat largo con muchos turnos cortos cuesta más que varios chats cortos.
- **Un chat por bloque** del mapa del plan. Cuando el bloque termina, el chat cierra con su prompt para el siguiente (`memoria.md`).
- **Nada de acuses de recibo ni de reportes de pasos.** Un turno solo cuando aporta: una decisión, un hito, un cierre. El hub y el dueño verifican en los hitos, no en cada paso (`hub.md`); lo que cada chat verifica por su cuenta no cambia.
- Los resultados de un workflow o de muchos agentes no se leen completos: se resumen con un script del scratchpad que imprime solo lo necesario, o cada agente devuelve un resumen con esquema de menos de 25 líneas. El orquestador no lee transcripciones (`orquestacion.md`).
- Las correcciones de texto repartidas en muchos archivos se delegan a un agente con instrucciones exactas.
- No se vuelven a leer archivos enteros que ya se leyeron.
- Sin bajar la calidad: la economía recorta turnos y lecturas, no verificaciones.

## Umbrales

`CLAUDE.md` fija, para el proyecto, un umbral de aviso y uno de cierre para cada uno de estos tres valores: el contexto del chat que orquesta, el uso semanal del modelo caro y el uso semanal de todos los modelos. Basta con que uno se acerque o se alcance.

- **Al acercarse** al umbral de aviso, el agente lo dice.
- **Al alcanzar** el umbral de cierre, se cierra todo sin borrar nada: memoria, mapa y changelog al día, con commit si el dueño lo pidió o el proyecto lo acordó. Por contexto, se le pasa al dueño un prompt para seguir en un chat nuevo. Por uso semanal, se le dice qué falta y cuánto se estima gastar en eso, y él decide si se sigue.
- Se leen en cada cambio de fase y al cerrar cada hito. Un agente que vigile los valores esperando gasta tokens en la espera; si se quiere un aviso automático, es un script sin modelo, o el monitor del cliente.

## Tamaño de los workflows

- **En tareas que no son programar** (planear, inventariar, leer documentos, armar listas de decisiones), el agente lee y consolida por sí mismo. Si hace falta una revisión independiente, usa pocos agentes (de 2 a 4) y verifica solo lo delicado. Antes de lanzar más de 6, dice cuántos y para qué, y espera el sí.
- **Cuando se trata de programar**, ya con el «programa» del dueño, el agente lanza los agentes y workflows que necesite: pruebas independientes, mutación, laboratorio, validadores (`orquestacion.md`). Las revisiones las hace el otro agente (`pruebas-y-revisiones.md`).
- No se lanzan agentes para temas que el dueño no pidió: se pregunta antes.

## Uso semanal

- Cuando la cuenta va cerca de su límite, el agente lo dice y propone opciones: pasar el diseño o las revisiones al otro agente, trabajar sin subagentes o esperar el reinicio.
- Antes de que se acabe, todo queda con su nota en la bitácora y, si el dueño lo pidió o el proyecto lo acordó en `CLAUDE.md`, en commit.
- Si la cuenta es compartida entre personas, se avisa antes de lanzar muchos agentes, porque el límite es de todos.

## Modelos

Un orquestador barato que coordina y juzga; el modelo fuerte, con esfuerzo alto, para diseñar, refutar y verificar; uno económico para ejecutar contratos cerrados (`orquestacion.md`). Un ejecutor caro haciendo trabajo mecánico gasta el uso que después falta para decidir bien.
