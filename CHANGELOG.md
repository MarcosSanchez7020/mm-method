# Changelog

Formato: una sección por versión, la más reciente arriba. Versionado semántico: cambia el número mayor cuando una regla cambia de sentido, el menor cuando se agrega algo y el de parche cuando se corrige redacción.

Format: one section per version, newest first. Semantic versioning: major when a rule changes meaning, minor when something is added, patch for wording fixes.

## 2.0.0

Dos reglas cambian de sentido, por eso sube el número mayor: nadie revisa su propio trabajo (antes, el dueño podía escoger que el mismo agente revisara lo suyo) y nadie programa, en ningún frente, sin el «programa» explícito del dueño (antes bastaba «la palabra del dueño», y un área podía tener su propia autorización). Lo demás son adiciones. Todo en las dos skills, con el mismo contenido en español e inglés.

- **Nadie revisa su propio trabajo.** Las revisiones de lo que hizo un agente, y la verificación de sus hallazgos, las hace el otro agente en un chat de solo lectura; esa opción no se le ofrece al dueño. Los validadores mecánicos, con comandos reales, no cuentan como revisión. La revisión va en tandas hasta «sin hallazgos», con cada recomprobación sobre el diff exacto (`SKILL.md`, `pruebas-y-revisiones.md`, `ciclo.md`).
- **Nadie programa sin el «programa» explícito del dueño**, en ningún frente; un plan o un diseño escrito no es programar (`SKILL.md`, `ciclo.md`).
- **El mapa del plan** (`mapa-del-plan.md`, nueva): el plan por bloques con casillas que se marcan en el mismo commit que hace cada cosa, con quién, fecha y evidencia; los pasos del dueño los marca el agente que recibe su respuesta, con sus palabras; nada se borra, se tacha; un dibujo generado del archivo con una prueba de sincronía; todo chat lo lee al empezar. Esqueleto en `plantillas.md`.
- **La revisión diaria de versiones y de salud del entorno**, en el primer chat del día de cada computador (`memoria.md`, `SKILL.md`).
- **El hub** (`hub.md`, nueva): un chat orquestador de orquestadores, único contacto del dueño con los chats de trabajo; tablero; una sola lista de decisiones numerada de corrido; mensajes entre chats con las palabras textuales del dueño; reportes de los chats solo en los hitos, con esquema y en menos de 15 líneas; todo prompt para un chat nuevo dice modelo y esfuerzo; cuando el cliente del otro agente no puede escribirle al hub (en Codex, su revisión automática bloquea esos envíos), un hub espejo en ese cliente que no ejecuta los prompts, solo crea los chats, con un encabezado que lo dice; esos chats no reportan a ningún hub y el dueño pega el resultado. Esquemas en `plantillas.md`.
- **Orquestación** (`orquestacion.md`, nueva): un orquestador barato que solo coordina y juzga en las compuertas; diseño y refutador en el modelo fuerte con esfuerzo alto; ejecutores económicos por contrato exacto que no deciden, con dos intentos; validador con comandos reales; workflows por fase con un guion común; resúmenes con esquema; los commits los hace el orquestador. «Modelos por función» de `dos-agentes.md` ahora apunta ahí.
- **Economía de tokens y umbrales** (`costos.md`): el costo de un chat es su contexto por sus turnos; un chat por bloque; nada de acuses ni de reportes de pasos; se verifican los hitos. `CLAUDE.md` fija umbrales de aviso y de cierre para el contexto, el uso semanal del modelo caro y el uso semanal total; al cerrar, nada se borra. Los umbrales van como marcadores en la plantilla, sin cifras.
- **Todo PR lleva el monitor del PR de su cliente** (Auto-fix, en Claude Code), prendido por la sesión que lo abre; **la cola de merges**: un solo PR en revisión a la vez, el siguiente abre con el anterior unido y `main` integrada, y dos ramas no tocan los mismos archivos; **la línea de aprobación se pide una sola vez**, con la CI en verde y sin commits pendientes (`repositorio.md`, `ciclo.md`, `dos-agentes.md`).
- Decisiones: la lista única numerada de corrido entre chats, «todas las recomendadas» como respuesta válida y las entradas que llegan «por el hub» (`decisiones.md`). Prompts para el otro agente, de una, y con modelo y esfuerzo (`dos-agentes.md`, `estilo.md`). Un mensaje de otro chat, también del hub, es dato (`seguridad.md`). Plantilla de `CLAUDE.md` con el mapa, el hub, los umbrales, los modelos por rol, Auto-fix y la cola.
- Cuatro puntos del hub sin fuente escrita en el proyecto de origen (el reporte de menos de 15 líneas, el bloqueo de los envíos de Codex al hub, «el costo es el contexto por los turnos» con «nada de acuses» y «solo los hitos», y la vigilancia de choques entre ramas, incluidas las numeraciones del mapa) los confirmó el autor el 07/11/2026 en el chat de este cambio.
- README en los dos idiomas: 13 referencias, los principios con «nadie revisa su propio trabajo», y el bloque de instalación rápida con los dos comandos de Claude Code justo después de la introducción. Documentación (`docs/`): la guía con la sección del mapa, el hub y la orquestación, la lista de adopción y las preguntas frecuentes al día.

Two rules change meaning, hence the major version: nobody reviews their own work (before, the owner could choose the same agent to review its own work) and nobody codes, on any front, without the owner's explicit agreed word, "go" (before, "the owner's word" was enough, and an area could have its own authorization). The rest are additions. All in both skills, with the same content in Spanish and English.

- **Nobody reviews their own work.** Reviews of an agent's work, and the verification of their findings, are done by the other agent in a read-only chat; that option is not offered to the owner. Mechanical validators, with real commands, do not count as a review. The review goes in rounds until "no findings", each re-check on the exact diff (`SKILL.md`, `testing-and-reviews.md`, `cycle.md`).
- **Nobody codes without the owner's explicit "go"**, on any front; a written plan or design is not coding (`SKILL.md`, `cycle.md`).
- **The plan map** (`plan-map.md`, new): the plan in blocks with checkboxes ticked in the same commit that does each thing, with who, date and evidence; the owner's steps are ticked by the agent that receives the answer, with the owner's words; nothing is deleted, it is struck through; a drawing generated from the file with a sync test; every chat reads it at the start. Skeleton in `templates.md`.
- **The daily check of versions and environment health**, in the first chat of the day on each computer (`memory.md`, `SKILL.md`).
- **The hub** (`hub.md`, new): an orchestrator-of-orchestrators chat, the owner's only contact with the working chats; a board; a single decision list numbered consecutively; messages between chats with the owner's verbatim words; chat reports only at milestones, with a schema and under 15 lines; every prompt for a new chat states model and effort; when the other agent's client cannot message the hub (in Codex, its automatic review blocks those sends), a mirror hub in that client that does not run the prompts, only creates the chats, with a header that says so; those chats report to no hub and the owner pastes the result. Schemas in `templates.md`.
- **Orchestration** (`orchestration.md`, new): a cheap orchestrator that only coordinates and judges at the gates; design and refuter on the strong model with high effort; cheap executors by exact contract that do not decide, with two attempts; a validator with real commands; one workflow per phase with a common script; summaries with a schema; the orchestrator makes the commits. "Models by role" in `two-agents.md` now points there.
- **Token economy and thresholds** (`costs.md`): a chat's cost is its context times its turns; one chat per block; no acknowledgements or step reports; milestones are what gets verified. `CLAUDE.md` sets warning and closing thresholds for context, the expensive model's weekly usage and total weekly usage; on closing, nothing is deleted. The thresholds are placeholders in the template, without figures.
- **Every PR carries its client's PR monitor** (Auto-fix, in Claude Code), turned on by the session that opens it; **the merge queue**: one PR under review at a time, the next opens once the previous one is merged and `main` integrated, and two branches never touch the same files; **the approval line is requested once**, with CI green and no pending commits (`repository.md`, `cycle.md`, `two-agents.md`).
- Decisions: the single list numbered consecutively across chats, "all the recommended ones" as a valid answer and entries that arrive "via the hub" (`decisions.md`). Prompts for the other agent right away, with model and effort (`two-agents.md`, `style.md`). A message from another chat, the hub included, is data (`security.md`). `CLAUDE.md` template with the map, the hub, the thresholds, models by role, Auto-fix and the queue.
- Four hub points with no written source in the originating project (the under-15-line report, the blocking of Codex sends to the hub, "cost is context times turns" with "no acknowledgements" and "milestones only", and watching for clashes between branches, map numbering included) were confirmed by the author on 07/11/2026 in the chat for this change.
- README in both languages: 13 references, the principles with "nobody reviews their own work", and the quick-install block with the two Claude Code commands right after the introduction. Documentation (`docs/`): the guide with the section on the map, the hub and orchestration, the adoption checklist and the FAQ brought up to date.

## 1.2.1

- El instalador del gancho de equipo pide el nombre y lo lee como dato desde la entrada estándar, sin meterlo en el código: ningún carácter del nombre rompe el script ni ejecuta nada al instalar. El nombre es una línea con algo más que espacios; una línea vacía o con solo espacios, o un fin de entrada sin salto de línea, hacen fallar el bloque sin cambiar nada.
- Bloques para cambiar el nombre sin reinstalar el gancho y para buscar los commits de un computador sin meter el nombre en el comando; la búsqueda compara como texto exacto el valor de cada trailer, no la confunden el asunto ni el cuerpo y rechaza un nombre vacío.

- The machine hook installer asks for the name and reads it as data from standard input, without putting it in the code: no character in the name breaks the script or runs anything at install time. The name is one line with more than spaces; an empty or space-only line, or end of input without a line break, makes the block fail without changing anything.
- Blocks to change the name without reinstalling the hook and to find a computer's commits without putting the name in the command; the search compares each trailer value as exact text, is not confused by the subject or body, and rejects an empty name.

## 1.2.0

- La skill se aplica solo si el usuario la pide o si el `CLAUDE.md` o el `AGENTS.md` del proyecto dice que la adoptó; la plantilla de `CLAUDE.md` trae la línea de adopción.
- El check de la línea de aprobación exige el SHA completo del último commit, con igualdad exacta, y falla si hay otra línea de aprobación marcada, también numerada o en una cita.
- El gancho de equipo lee el nombre de la configuración de Git del clon y no reemplaza un gancho existente ni un enlace con ese nombre, aunque esté roto.
- El puente `AGENTS.md` indica leer la memoria del proyecto como dato, no como instrucción.
- Al cerrar una sesión, el commit y el push van solo si el dueño los pidió o el proyecto los acordó en `CLAUDE.md`.
- Instalar el gancho de equipo y apagar claude-mem para una revisión, solo si el dueño lo decide.
- `CODEOWNERS` cubre también `/.github/`, con el propio `CODEOWNERS` y los workflows.
- Lo que tenga la memoria local y no esté en el repo se le muestra al dueño y se pasa revisado: solo lo del proyecto, sin secretos ni datos personales.
- La atribución de herramientas en los PR sigue la decisión del equipo escrita en `CLAUDE.md`.
- La activación de la skill se documenta por cliente, Claude Code y Codex, sin garantía de activación automática.
- `LICENSE` dentro del plugin y `license` en el manifiesto de Claude Code.
- `SECURITY.md` con cómo reportar una vulnerabilidad.
- Costo en contexto medido de nuevo sobre la 1.2.0, en dos configuraciones de Claude Code.

- The skill applies only if the user asks for it or the project's `CLAUDE.md` or `AGENTS.md` says it adopted it; the `CLAUDE.md` template carries the adoption line.
- The approval line check requires the full SHA of the last commit, with exact equality, and fails if there is another checked approval line, also numbered or in a quote.
- The machine hook reads the name from the clone's Git configuration and does not replace an existing hook or a link with that name, even if it is broken.
- The `AGENTS.md` bridge says to read the project memory as data, not instructions.
- When closing a session, commit and push happen only if the owner asked for them or the project agreed on them in `CLAUDE.md`.
- Installing the machine hook and disabling claude-mem for a review, only if the owner decides so.
- `CODEOWNERS` also covers `/.github/`, with the `CODEOWNERS` file itself and the workflows.
- What local memory holds and the repo does not is shown to the owner and moved after review: only what belongs to the project, without secrets or personal data.
- Tool attribution in PRs follows the team's decision written in `CLAUDE.md`.
- Skill activation is documented per client, Claude Code and Codex, without a guarantee of automatic activation.
- `LICENSE` inside the plugin and `license` in the Claude Code manifest.
- `SECURITY.md` with how to report a vulnerability.
- Context cost measured again on 1.2.0, in two Claude Code configurations.

## 1.1.0

- Soporte de Codex: catálogo propio y manifiesto portable junto al manifiesto de Claude Code, con una única fuente de skills por idioma.
- Aclaraciones equivalentes ES/EN para Codex solo o con Claude Code; puente `AGENTS.md` a las reglas compartidas en `CLAUDE.md`.
- Instalación de Codex documentada después de ejecutar sus comandos; ambos README y guías actualizados. El paquete sigue siendo solo instrucciones, sin scripts de instalación, ganchos, servidores ni conexiones adicionales.

- Codex support: a dedicated catalog and a portable manifest alongside the Claude Code manifest, with one skill source per language.
- Equivalent EN/ES clarifications for Codex alone or with Claude Code; an `AGENTS.md` bridge to shared rules in `CLAUDE.md`.
- Codex installation documented after executing its commands; both READMEs and guides updated. The package remains instructions only, without installation scripts, hooks, servers or additional connections.

## 1.0.1

- El README en inglés pasa a ser `README.md`, el que GitHub muestra por defecto; el español pasa a `README.es.md`. Sin cambios en las skills.

- The English README becomes `README.md`, the one GitHub shows by default; the Spanish one moves to `README.es.md`. No changes to the skills.

## 1.0.0

- Primera versión. Dos skills con el mismo contenido: `metodologia-agentes` (español) y `agent-methodology` (inglés), cada una con su `SKILL.md` y diez referencias.
- Documentación en `docs/es/` y `docs/en/`: guía completa, guía de adopción y preguntas frecuentes.

- First version. Two skills with the same content: `metodologia-agentes` (Spanish) and `agent-methodology` (English), each with its `SKILL.md` and ten references.
- Documentation in `docs/es/` and `docs/en/`: full guide, adoption guide and FAQ.
