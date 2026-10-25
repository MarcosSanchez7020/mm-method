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
- Una revisión corta de esos textos: dos revisores a los que se les da el registro de respuestas. Quién revisa se le pregunta antes al dueño, salvo que haya una regla escrita (`pruebas-y-revisiones.md`).
- Se revisan las cuentas de los ejemplos que queden escritos.
- El agente le confirma al dueño lo que entendió. El commit, solo cuando el dueño lo pida.

## 3. Programar, solo con la palabra del dueño

- El agente escribe el núcleo y lo verifica a mano: los números de los documentos y de los libros, y un caso completo por la cadena real.
- Después lanza, sin preguntar, porque ya hay autorización para programar:
  - agentes de pruebas independientes, cada uno en su propio archivo nuevo;
  - mutación en una copia;
  - un agente para la página de prueba (el laboratorio).
- Las revisiones adversariales, en cambio, se preguntan (`pruebas-y-revisiones.md`).
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
- Los documentos pasan a "programado", una revisión corta solo de lo que cambió y mutación repetida sobre el código final.

## 6. Subir y verificar en paralelo

1. Changelog y README al día; commit.
2. Unir `origin/main` en la rama (merge, nunca rebase) y resolver los conflictos conservando los dos lados.
3. Al arrancar la suite completa local, push y PR, para que la suite local y la CI corran al mismo tiempo.
4. El PR declara la validación en curso y no marca las pruebas como aprobadas hasta tener el resultado. Después se actualiza su descripción con los dos resultados (`repositorio.md`).
5. La aprobación técnica y el merge son solo del dueño.

## Por qué este orden

Planear antes de programar evita escribir código para una regla que el dueño iba a decidir distinto. Escribir lo decidido antes de programar deja una fuente de verdad contra la que se prueba. Y las pruebas independientes, la mutación y las revisiones atacan el código desde tres lados distintos: lo que debería dar, si las pruebas detectan errores y si alguien que no lo escribió encuentra fallas.
