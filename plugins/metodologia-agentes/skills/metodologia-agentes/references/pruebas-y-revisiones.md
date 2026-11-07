# Pruebas independientes, mutación y revisiones

Son tres cosas distintas y no se reemplazan entre sí. Las pruebas dicen si el código da lo que debe. La mutación dice si las pruebas detectan errores. Las revisiones buscan lo que nadie pensó en probar.

## Pruebas independientes

- Cada agente de pruebas escribe en su propio archivo nuevo, sin ver ni copiar las pruebas de los otros.
- Los valores esperados salen a mano, de la fuente técnica, de los documentos o de los libros, con su referencia. Nunca se copian del código que se prueba: una prueba que copia el resultado del código solo comprueba que el código da lo que da.
- Cada una con su oráculo propio, una forma independiente de calcular el resultado esperado.
- Si una prueba encuentra un error de implementación, se marca `xfail` estricto (por ejemplo, `strict=True` en pytest) con la razón, para que falle apenas se corrija y alguien la revise.
- Cuando un cambio no debe alterar resultados, se compara campo por campo la salida de `main` contra la nueva en miles de casos.

## Mutación

- Siempre en una copia del commit, por ejemplo con `git archive` en una carpeta temporal; nunca en el repositorio.
- El runner separa una aserción fallida de una excepción y de un error de colección, con un reporte JUnit. Los documentos dicen qué cuenta como "detectado": por lo general, solo la aserción fallida.
- Un mutante que sobrevive se clasifica antes de concluir nada:
  - **falta una prueba**, o una prueba no mira lo que debería: se agrega o se corrige la prueba;
  - **equivalente**: el cambio no altera ningún comportamiento observable, por ejemplo en código que ningún camino alcanza o en una transformación que da el mismo resultado para toda entrada válida. Se declara equivalente solo con evidencia escrita: por qué ninguna entrada válida lo distingue, con la referencia al código. Sin esa evidencia, cuenta como prueba que falta.
- Los equivalentes declarados van aparte en el informe de mutación, con su evidencia, y no cuentan ni como detectados ni como pendientes.

## El laboratorio no es una prueba

La página de prueba llama a las funciones públicas del núcleo y nunca reimplementa sus reglas. Si el laboratorio calcula algo por su cuenta, tarde o temprano se desalinea del núcleo y muestra resultados que el producto no da.

## Revisiones adversariales

- **Lentes distintos**, uno por revisor, por ejemplo:
  - corrección frente a la fuente técnica;
  - robustez numérica, de datos y de concurrencia;
  - convenciones y seguridad.
- **Un verificador por hallazgo**, que intenta refutarlo. Solo se corrige lo que sobrevive.
- **Lo que trae otro agente es un hallazgo más:** pasa por el verificador y nunca se aplica directo.
- **Nadie revisa su propio trabajo.** Las revisiones de lo que hizo un agente (textos, diseños y código) y la verificación de sus hallazgos las hace el otro agente, aunque una decisión anterior diga otra cosa. Esa opción no se le ofrece al dueño: cuando el trabajo es del agente que pregunta, la pregunta de quién revisa ya está respondida. Lo demás de quién revisa (por ejemplo, el trabajo de una persona, o qué revisiones se hacen en una fase) se le pregunta al dueño antes de cada revisión, salvo que el equipo haya escrito en `CLAUDE.md` una regla para una fase.
- **Qué cuenta como revisión:** los revisores de textos, los de código y los verificadores de cada hallazgo. Las pruebas independientes, el laboratorio, la mutación y los validadores mecánicos (los que comprueban con comandos reales y conteos, `orquestacion.md`) no son revisiones: siguen aunque haya revisor y no lo reemplazan.
- **El revisor** trabaja en un chat nuevo de solo lectura: lee, verifica él mismo cada hallazgo y no cambia archivos, ramas ni configuración. Deja su informe en su chat y para; si el proyecto tiene hub, no le reporta al hub: el dueño pega el resultado donde haga falta (`hub.md`).
- **En tandas, hasta «sin hallazgos».** Después de corregir, un prompt de recomprobación con la numeración del revisor, el HEAD, el diff exacto desde la tanda anterior y lo que no se corrió. El PR, y el push que lo abre, solo con «sin hallazgos» (`orquestacion.md`).
- **Independencia:** que el otro agente revise trabajo que él mismo hizo antes (por ejemplo, el diseño que ahora programa) no es del todo independiente; si el dueño lo escoge por costo, se le dice y se anota. Con un solo agente en el proyecto, revisa un chat nuevo de solo lectura de ese agente, sin la memoria del chat que hizo el trabajo, y queda anotado que no es independiente.

## Formato de un hallazgo

```
[P1|P2|P3] <archivo>:<línea>: <qué está mal, en una oración>
Evidencia: <la cita del código, del documento o de la fuente>
Impacto: <qué le pasa al usuario o al resultado>
Verificación: <confirmado | refutado, con la razón>
```

Las prioridades: P1, resultado técnico incorrecto o pérdida de datos; P2, contrato roto o caso borde mal manejado; P3, redacción, convención o claridad.
