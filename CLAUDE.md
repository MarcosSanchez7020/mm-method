# mm-method: shared repository rules / reglas compartidas

- Maintain one source per language under `plugins/metodologia-agentes/skills/`. Claude Code and Codex use those same folders. Keep ES/EN equivalent.
- Keep the Claude manifests and the Codex catalog compatible with their respective clients. Keep manifest versions aligned with the changelog.
- The package contains instructions only: no installation scripts, hooks, servers or additional connections. Code examples in references are not installed executables.
- Never add secrets, private project information or third-party personal data. Preserve the MIT license and authorship.
- Plan before implementation and wait for the owner's agreed authorization. Work on a branch, never directly on `main`; never force-push. The owner chooses the final reviewer and merges the PR.
- Before a PR, validate the Claude catalog and plugin with `claude plugin validate --strict`, test installation and both languages in both clients, and report failures and anything not tested explicitly.

- Mantén una fuente por idioma en `plugins/metodologia-agentes/skills/`. Claude Code y Codex usan esas mismas carpetas. Conserva la equivalencia ES/EN.
- Mantén los manifiestos de Claude y el catálogo de Codex compatibles con sus clientes. Alinea las versiones de los manifiestos con el changelog.
- El paquete solo contiene instrucciones: sin scripts de instalación, ganchos, servidores ni conexiones adicionales. Los ejemplos de código de las referencias no son ejecutables instalados.
- Nunca agregues secretos, información privada de proyectos ni datos personales de terceros. Conserva la licencia MIT y la autoría.
- Planea antes de implementar y espera la autorización acordada con el dueño. Trabaja en una rama, nunca directamente en `main`; nunca hagas force-push. El dueño escoge al revisor final y une el PR.
- Antes de un PR, valida el catálogo y el plugin de Claude con `claude plugin validate --strict`, prueba la instalación y ambos idiomas en ambos clientes, y declara los fallos y lo no probado.
