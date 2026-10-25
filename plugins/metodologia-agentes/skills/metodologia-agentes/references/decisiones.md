# Decisiones: cómo se piden, cómo se registran y cómo se escriben

## Quién decide qué

- El dueño técnico decide lo técnico. Eso incluye cómo se le presentan al usuario los resultados, los estados y los avisos, porque una mala presentación cambia lo que el usuario entiende.
- Si otra persona del equipo es dueña de un área (por ejemplo, el diseño de la interfaz), sus decisiones se las pasan a ella. Quién decide qué se escribe en `CLAUDE.md`.
- Los agentes no deciden: proponen, recomiendan y explican. Los subagentes que ejecutan tareas tampoco deciden: lo que pide criterio vuelve al agente que coordina o al dueño.

## Cómo se pide una decisión

- **En lista numerada**, cada decisión con sus opciones con letra (a, b, c). La recomendada va primero y en negrilla. Así el dueño responde "1a, 2b" sin malentendidos.
- **Con su peso.** En una o dos líneas: qué cambia con cada opción, qué cuesta y qué riesgo deja. Si sirve para decidir, se acompaña de un barrido o una medida con el código real. Los casos los escoge el agente y son informativos.
- **Lo implícito no se pregunta.** Lo que ya se deduce de una decisión escrita se dice como "lo doy por decidido, dime si no".
- **Las interpretaciones nuestras van aparte**, en su propia lista, marcadas como interpretación, con su fuente.
- **Si una instrucción se puede leer de dos formas**, se pregunta antes de tocar documentos. Por ejemplo, cuánto hay que quitar, o si una regla vale para una tarea o para todas.
- **De una.** Las decisiones se pasan apenas se tienen, sin esperar lo que solo es informativo. Lo que llegue después se cuenta aparte.
- **Contra la fuente técnica**, nunca en silencio: se dice "la fuente dice esto", con la referencia exacta, y se propone la opción que la cumple.
- **Lo que el dueño no dijo no se le atribuye.** Si una respuesta deja algo abierto, se pregunta.

## El registro textual

Cada respuesta del dueño se agrega al registro (`MEMORIA/registro_respuestas.md`) en el momento en que llega, a más tardar antes del siguiente commit. Cada entrada lleva:

1. Lo que se le preguntó, con todas sus opciones y la recomendada.
2. Sus palabras tal cual, también lo coloquial, en un bloque de cita.
3. Entre paréntesis, cómo se lee la respuesta y **lo que no dijo**.

Si el dueño respondió con un texto que propuso el agente (un botón, una respuesta sugerida), se dice. Así nadie confunde la redacción del agente con la del dueño.

Por qué tan literal: semanas después, cuando un chat nuevo o el otro agente necesite saber por qué algo es así, la cita resuelve la duda sin interpretaciones.

## Precisiones del agente

Cuando el agente precisa algo que el dueño no dijo (un detalle de cómo se monta, una consecuencia), se escribe como "precisión de <agente>, por confirmar" hasta que el dueño la confirme. Después se cambia a "confirmada por <dueño> el <fecha>", con su palabra.

## Escribir lo decidido

- Las decisiones se escriben en los documentos que son fuente de verdad (alcance, requisitos, modelo de datos, diagramas) y en el changelog, con las palabras del dueño, la fecha y su respuesta ("1a").
- Las reglas que deben valer siempre van en `CLAUDE.md`, con su entrada en el changelog, en la rama en la que se está trabajando. Si una regla cambia, se cambia ahí.
- Después de escribir decisiones de peso, una revisión corta de esos textos (`pruebas-y-revisiones.md`), y se revisan las cuentas de los ejemplos que queden escritos.
- El agente le confirma al dueño lo que entendió. El commit, solo cuando el dueño lo pida.

## Lo conocido y aceptado

Lo que el dueño dejó como "conocido y aceptado", o para después, no se arregla ni se toca sin que él lo diga, aunque el agente lo vea mejorable. Se puede recordar, pero no cambiar.
