# Changelog

Formato: una sección por versión, la más reciente arriba. Versionado semántico: cambia el número mayor cuando una regla cambia de sentido, el menor cuando se agrega algo y el de parche cuando se corrige redacción.

Format: one section per version, newest first. Semantic versioning: major when a rule changes meaning, minor when something is added, patch for wording fixes.

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
