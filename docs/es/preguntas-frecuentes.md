# Preguntas frecuentes

## ¿Sirve sin un segundo agente?

Sí. Todo lo de la metodología funciona con Claude Code o Codex como único agente. Con Codex se incluye `AGENTS.md` aunque no haya segundo agente. La referencia `dos-agentes.md` solo se lee cuando hay dos agentes en el mismo repositorio.

## ¿Sirve si no hay un "dueño técnico" aparte?

Sí: el dueño es quien decide, aunque sea la misma persona que pide el trabajo. Lo que cambia es que las decisiones quedan escritas en lista y registradas, y eso sirve igual cuando uno vuelve al proyecto un mes después.

## ¿No es demasiado proceso?

Para un script de una tarde, sí. La metodología está pensada para trabajo que dura más que un chat. La [guía, sección 12](guia.md#12-proyectos-chicos-y-equipos-grandes), dice qué se puede recortar en un proyecto chico sin perder lo esencial y qué no conviene recortar.

## ¿Por qué las decisiones en lista y no conversando?

Porque una conversación larga deja dudas sobre qué se decidió al final. Con "1a, 2b" no hay que releer nada, y el registro textual guarda qué se preguntó y qué se respondió. La recomendación va en negrilla porque el agente tiene el contexto técnico fresco; la decisión sigue siendo de la persona.

## ¿Por qué la memoria en el repositorio y no en la memoria de Claude?

La memoria local del agente se queda en un computador y no la ven otros chats en otros equipos ni otro agente. El repositorio viaja con Git a todos. Las herramientas de memoria pueden ayudar, pero si chocan con lo escrito en el repo, gana el repo.

## ¿Necesito claude-mem u otra herramienta de memoria?

No. La memoria de la metodología vive en el repositorio. claude-mem se sugiere como complemento opcional y es de terceros. Antes de prenderlo, ten en cuenta que guarda en el computador datos de las sesiones y que, según su documentación, envía las observaciones al proveedor de IA que se le configure (Claude, OpenRouter o Gemini): revisa a qué proveedor van y cómo los trata, y no lo uses en sesiones con secretos o datos personales que no deban salir del computador. Los demás ajustes sugeridos (que el repo mande, apagarlo para revisiones independientes y sus umbrales de uso) están en la referencia `memoria.md` de la skill.

## ¿Cómo convive con un `CLAUDE.md` que ya existe?

El `CLAUDE.md` del proyecto manda. Al adoptar la metodología, el agente propone dónde encaja cada sección y pasa en lista lo que choca. Las reglas propias del proyecto se quedan.

## ¿Cómo la invoco si no se activa sola?

Pidiéndola: "trabajemos con la metodología", o nombrando la skill (`metodologia-agentes` o `agent-methodology`). Se activa sola cuando la conversación trata de lo que cubre su descripción: organizar un proyecto con agentes, `CLAUDE.md`, la memoria del proyecto, decisiones, el ciclo de un módulo, PR, dos agentes.

## ¿Por qué hay dos skills con el mismo contenido?

Los dos clientes usan las mismas carpetas del paquete; no hay una copia para Codex y otra para Claude. Las cifras siguientes son históricas, de mm-method 1.0.0 en Claude Code, no mediciones de 1.1.0 ni de Codex.

Una en español y otra en inglés. Cada una tiene su descripción en su idioma, así que el agente usa la que corresponde a la conversación. Según dos corridas de `claude plugin details` con Claude Code 2.1.296, el 25/10/2026, juntas cuestan de ~456 a ~643 tokens de contexto por sesión, y cada una de ~1.300 a ~1.900 cuando se activa; la causa de la diferencia entre las dos corridas no se ha determinado (el comando y el detalle están en el [README](../../README.es.md#costo-en-contexto)).

## ¿Ejecuta algo por su cuenta?

No. No trae hooks, comandos ni servidores. Son instrucciones para el agente.

## ¿Qué pasa con secretos y datos personales?

La metodología prohíbe ponerlos en el repositorio, en la memoria y en el changelog, y prohíbe que el agente maneje tokens o contraseñas: si hay que tocar un panel con credenciales, lo hace una persona.

## ¿Cómo se actualiza?

En Codex, sigue la [guía de instalación y actualización](codex.md). En Claude Code:

```bash
claude plugin marketplace update mm-method
claude plugin update metodologia-agentes@mm-method
```

Y reiniciar Claude Code. Los cambios de cada versión están en el [CHANGELOG](../../CHANGELOG.md).

## ¿Puedo adaptarla?

Sí, en el `CLAUDE.md` de cada proyecto: ahí se fija la palabra para programar, el idioma, quién revisa, qué modelos se usan y las excepciones. La skill da el método; el proyecto, sus valores.
