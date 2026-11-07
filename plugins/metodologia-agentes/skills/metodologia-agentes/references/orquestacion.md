# Orquestador, trabajadores que piensan y ejecutores por contrato

Cuando ya hay «programa», el chat principal no hace el trabajo: lo orquesta. Un orquestador barato coordina y juzga en las compuertas; los trabajadores que piensan usan el modelo fuerte con esfuerzo alto; los ejecutores, un modelo económico pero capaz, y no deciden nada. Qué modelo es cada cual se escribe en `CLAUDE.md`.

## Quién hace qué

- **El orquestador** (el chat principal) solo coordina y juzga en las compuertas entre fases. No escribe código, no corre suites a mano (solo comandos cortos de git y de estado) y no lee transcripciones de agentes: lee el resumen con esquema de cada uno. Lee el uso (contexto y semanal) en cada cambio de fase (`costos.md`). Puede ser un modelo barato: lo que pida pensar, también lo intermedio, va a un trabajador que piensa.
- **Los trabajadores que piensan:** diagnóstico, diseño, refutación, síntesis y verificación a mano. Modelo fuerte, esfuerzo alto.
- **Los ejecutores:** programan, escriben pruebas, corren mutación, escriben documentos. Modelo económico, suficiente para escribir bien y seguir instrucciones claras. Reciben contratos exactos (archivos, firmas, comportamiento caso por caso, qué función llamar, los casos de prueba con su valor esperado y cuándo está terminado) y no deciden: lo que pida criterio vuelve al orquestador, que lo pasa a un trabajador que piensa o al dueño.
- Si una tarea de ejecución es delicada, el orquestador le sube el modelo y lo dice.

## Las fases

- **A. Documentos.** Lo que el cambio autorizado toca en los documentos (un trabajador que piensa). Lo decidido del módulo ya quedó escrito y revisado antes del «programa», en el paso 2 de `ciclo.md`.
- **B. Diseño del cambio.** Un diseñador escribe el contrato exacto; un refutador intenta tumbarlo contra la fuente técnica y las decisiones escritas. Los dos con el modelo fuerte y esfuerzo alto. Sin refutador solo si la pieza es simple y el dueño lo decide.
- **C. Ejecución.** Ejecutores por contrato, sobre archivos disjuntos, dos intentos cada uno (el segundo retoma lo del primero sin reescribir); pruebas independientes con los valores a mano del contrato; mutación en una copia.
- **D. Verificación.** Un trabajador que piensa verifica a mano por la cadena real; un validador independiente, con comandos reales y conteos, que en toda corrección de código demuestra por mutación en una copia que las pruebas nuevas detectan la causa; y el laboratorio. Los validadores no cuentan como revisión (`pruebas-y-revisiones.md`).
- **E. Revisión.** Por el otro agente, en tandas hasta «sin hallazgos».
- **F. Cierre.** La suite completa una vez, en el commit final, en paralelo con el push y el PR (`repositorio.md`).

Las fases A a D se encadenan sin parar, con las compuertas del orquestador. Se para antes de la E solo por algo contra la fuente técnica, por una discrepancia del refutador que las decisiones escritas no cierren o por un ejecutor que no cierra en dos intentos; eso va al dueño, en lista.

## El guion común de los workflows

Cada fase es un workflow con un guion estándar:

- reglas comunes para todos los agentes: carpeta y rama exactas; nada de git que escriba (los commits los hace el orquestador); el intérprete y el entorno del proyecto; valores esperados nunca copiados del código; sin `xfail`, `skip` ni normalizaciones que escondan una falla; escrituras acotadas (por ejemplo, de a lo sumo 150 líneas por escritura); finales de línea respetados; el formateador solo sobre lo tocado; salida solo en el esquema;
- un esquema de salida por agente, de menos de 25 líneas;
- dos intentos por ejecutor.

## Los commits del orquestador

- El código y los documentos de una corrección van en un commit. Las decisiones y precisiones documentales, en commits aparte. Un merge de `main`, solo con la resolución de conflictos y los ajustes que pida.
- Cada commit con el changelog, la memoria y el mapa al día (`mapa-del-plan.md`).

## La revisión en tandas

Hallazgo → workflow de corrección → commit → prompt de recomprobación, de una, con la numeración del revisor, el HEAD, el diff exacto y lo que no se corrió. El PR, y el push que lo abre, solo con «sin hallazgos», y entonces la suite completa local en paralelo con la CI.

## Antes del «programa»

En lo que no es programar (planear, inventariar, leer documentos, armar listas de decisiones), el chat principal lee y consolida por sí mismo, con pocos agentes (`costos.md`, «Tamaño de los workflows»).
