# El ciclo de cada módulo

Un módulo es una pieza que el dueño decide como unidad: una revisión, una regla, una función del producto. Cada uno pasa por estos seis pasos, en orden.

## 1. Planear

- Leer la fuente técnica en su original, al pie de la letra (el PDF de la norma, la especificación), no de memoria ni de un resumen. Si la fuente cita otra, se cita como referencia y se marca como interpretación nuestra.
- Pasarle al dueño, en un solo mensaje:
  - lo que dice la fuente, con la referencia;
  - lo que hace hoy el producto;
  - a qué casos aplica;
  - cuánto pesa la decisión; si sirve, un barrido con el código real, con casos que escoge el agente y que son informativos;
  - las decisiones, en lista (`decisiones.md`);
  - aparte, las interpretaciones: casos borde, qué se compara con qué, qué pasa si no hay datos.

## 2. Escribir lo decidido

- El dueño decide. El agente escribe las decisiones en los documentos, con sus palabras; lo que precise el agente va aparte, por confirmar.
- Una revisión corta de esos textos, por el otro agente, en un chat de solo lectura al que se le da el registro de respuestas y que verifica él mismo cada hallazgo; quien los escribió no los revisa (`pruebas-y-revisiones.md`).
- Se revisan las cuentas de los ejemplos que queden escritos.
- El agente le confirma al dueño lo que entendió. El commit, solo cuando el dueño lo pida.

## 3. Programar, solo con el «programa» explícito del dueño

- Nadie programa nada, en ningún frente, sin la palabra del dueño para esa tarea. Un plan o un diseño escrito no es programar, y tampoco autoriza a programar.
- El agente escribe el núcleo y lo verifica a mano: los números de los documentos y de los libros, y un caso completo por la cadena real. Si el proyecto trabaja con orquestador y ejecutores (`orquestacion.md`), el chat principal no escribe el código: lo escriben los ejecutores por contrato y lo verifica a mano un trabajador que piensa.
- Después lanza, sin preguntar, porque ya hay autorización para programar:
  - agentes de pruebas independientes, cada uno en su propio archivo nuevo;
  - mutación en una copia;
  - un agente para la página de prueba (el laboratorio).
- Las revisiones adversariales las hace el otro agente (`pruebas-y-revisiones.md`).
- Los agentes formatean solo sus propios archivos y no dejan archivos temporales de configuración.

## 4. El laboratorio, apenas exista

- Una página local que llama a las funciones públicas del núcleo, para que el dueño vea casos reales sin esperar a los revisores.
- Se sirve en el puerto del agente, se le pasa al dueño el link local y se corren todos los ejemplos en el navegador.
- El entorno publicado (el que ven otros) se construye solo desde `main`.

## 5. Corregir y cerrar

- Se corrigen los hallazgos confirmados.
- Si una prueba independiente contradice una corrección, gana la decisión escrita, y la fuente técnica gana sobre todo. Si la decisión no dice nada, se ajusta la prueba sin debilitarla y se dice. Lo que sea decisión del dueño se le pregunta en lista.
- Si una prueba compara el texto de un mensaje, el formato es parte de la decisión.
- Antes de quitar un `xfail` que pasó a XPASS, se comprueba que la prueba pruebe de verdad.
- Los documentos pasan a "programado", una revisión corta solo de lo que cambió, por el otro agente, y mutación repetida sobre el código final.
- La revisión va en tandas hasta «sin hallazgos»: cada recomprobación, sobre el diff exacto desde la tanda anterior (`orquestacion.md`).

## 6. Subir y verificar en paralelo

1. Changelog, README, memoria y mapa al día; commit.
2. Unir `origin/main` en la rama (merge, nunca rebase) y resolver los conflictos conservando los dos lados.
3. Si hay cola de merges, avisar «listo para abrir el PR» con el HEAD y esperar el turno; al llegar, volver a unir `origin/main` (`repositorio.md`).
4. Al arrancar la suite completa local, push y PR, para que la suite local y la CI corran al mismo tiempo. Al abrir el PR, prender el monitor del PR del cliente (Auto-fix, en Claude Code).
5. El PR declara la validación en curso y no marca las pruebas como aprobadas hasta tener el resultado. Después se actualiza su descripción con los dos resultados (`repositorio.md`).
6. Con la suite y los demás checks en verde (salvo el de la línea de aprobación) y sin commits pendientes, se le pide al dueño la línea de aprobación, una sola vez. La aprobación técnica y el merge son solo del dueño.

## Por qué este orden

Planear antes de programar evita escribir código para una regla que el dueño iba a decidir distinto. Escribir lo decidido antes de programar deja una fuente de verdad contra la que se prueba. Y las pruebas independientes, la mutación y las revisiones atacan el código desde tres lados distintos: lo que debería dar, si las pruebas detectan errores y si alguien que no lo escribió encuentra fallas.
