# Soporte de Codex

mm-method 1.2.0 empaqueta las mismas skills en español e inglés para Codex y Claude Code. Solo contiene instrucciones. No agrega scripts de instalación, ganchos, servidores, conectores ni requisitos de autenticación. El catálogo declara `policy.authentication` porque el formato lo exige; el plugin no tiene conectores, así que no hay nada que autenticar.

## Instalar e invocar

En una terminal con Codex CLI y soporte de plugins:

```bash
codex plugin marketplace add MarcosSanchez7020/mm-method
codex plugin add metodologia-agentes@mm-method
codex plugin list --marketplace mm-method --json
```

Para instalar una versión publicada en lugar de la rama predeterminada, agrega el catálogo con su etiqueta: `codex plugin marketplace add MarcosSanchez7020/mm-method --ref v1.2.0`.

Abre una sesión nueva. Usa los nombres completos que muestra el plugin:

- Inglés: `$metodologia-agentes:agent-methodology`.
- Español: `$metodologia-agentes:metodologia-agentes`.

Por ejemplo: "Usa $metodologia-agentes:metodologia-agentes para planear la adopción en este repositorio. Antes de escribir archivos, pásame las decisiones en una lista numerada."

El nombre corto sin prefijo no inyectó la skill completa en la sesión no interactiva de Codex probada. Los nombres completos sí. El modelo también puede seleccionar una skill por su descripción, pero la selección automática no está garantizada.

## Una fuente, dos clientes

El catálogo de Codex es `.agents/plugins/marketplace.json`; apunta a `plugins/metodologia-agentes/`. Esa carpeta contiene el `plugin.json` portable y el `.claude-plugin/plugin.json` existente. Claude conserva su `.claude-plugin/marketplace.json` en la raíz del repositorio. Los dos clientes usan las dos carpetas `skills/` existentes y sus diez referencias por idioma. No hacen falta enlaces simbólicos, copias generadas ni scripts de sincronización.

Codex instala el paquete en `plugins/cache/mm-method/metodologia-agentes/<versión>/`, dentro de su carpeta de configuración (`~/.codex` por defecto). Es una caché de instalación, no otra fuente para editar. Cambia el repositorio y reinstala el paquete.

Al adoptar la metodología en un proyecto, mantén las reglas compartidas en `CLAUDE.md`. Incluye el puente `AGENTS.md` de la [guía de adopción](adopcion.md#6-agentsmd-siempre-que-se-use-codex), aunque Codex sea el único agente. Instalar el plugin no crea esos archivos del proyecto ni configura las protecciones de GitHub.

## Actualizar y quitar

Para un catálogo alojado en Git, actualízalo y vuelve a instalar la versión seleccionada:

```bash
codex plugin marketplace upgrade mm-method
codex plugin add metodologia-agentes@mm-method
```

Abre una sesión nueva. Para quitar este plugin y después el catálogo si ya no lo necesitas:

```bash
codex plugin remove metodologia-agentes@mm-method
codex plugin marketplace remove mm-method
```

Estos comandos solo afectan a Codex. Claude Code mantiene su propia instalación y sus comandos de actualización en el [README](../../README.es.md#claude-code).

## Alcance de las pruebas

Las pruebas del 25/10/2026 usaron Windows, Codex CLI 0.162.1 y Claude Code 2.1.296. Los dos comandos exactos de instalación pública de Codex funcionaron sobre `main` 1.0.1 antes de documentarlos. La 1.1.0 se instaló aparte desde la rama de trabajo. Los comandos instalan la rama predeterminada del catálogo.

La instalación local de 1.1.0 pasó en ambos clientes. Codex no tenía catálogo, registro de plugin ni caché de mm-method antes de la primera prueba; su configuración de usuario ajena al plugin no era un perfil nuevo. La instalación de Claude usó una carpeta de configuración separada y vacía. Las dos skills cargaron en ES y EN en sesiones nuevas: Codex usó los nombres completos ignorando la configuración del usuario; Claude usó la carpeta del plugin instalado en limpio, con herramientas restringidas y sin la configuración MCP externa. Los 22 archivos Markdown instalados coincidieron con la fuente compartida en cada cliente. La validación estricta de Claude pasó para el catálogo y el plugin, y el validador de skills pasó para ambos idiomas.

Son pruebas de instalación y carga explícita, no una garantía de todos los flujos ni de activación automática. No se probaron la instalación por interfaz de escritorio/IDE, macOS, Linux, otras versiones de los clientes ni el costo en contexto de Codex.

Las pruebas de la 1.2.0, el 25/10/2026, usaron Windows, Codex CLI 0.162.1 y Claude Code 2.1.296, con el catálogo local de la rama de trabajo. La instalación pasó en los dos clientes con carpetas de configuración nuevas y vacías (`CLAUDE_CONFIG_DIR` y `CODEX_HOME`): los 25 archivos instalados, `LICENSE` incluido, coincidieron byte a byte con la fuente. En Claude, las dos skills cargaron en ES y EN en sesiones nuevas con `--plugin-dir`, `--restricted` y `--strict-mcp-config`.

Después del merge, Codex CLI 0.162.1 instaló la 1.2.0 desde `main` (`5c76fc4`) con los dos comandos públicos, sin catálogo, registro ni caché previos de mm-method: los 25 archivos instalados, `LICENSE` incluida, coinciden con `main` normalizando los finales de línea, y las dos skills cargaron completas en ES y EN en sesiones nuevas de solo lectura, con `--ignore-user-config` y los nombres completos.

La instalación fijada a `v1.2.0` se probó el 25/10/2026 desde GitHub, con Claude Code 2.1.296 (`marketplace add MarcosSanchez7020/mm-method@v1.2.0`) y Codex CLI 0.162.1 (`marketplace add MarcosSanchez7020/mm-method --ref v1.2.0`), cada uno con una carpeta de configuración nueva y vacía. Los dos registraron la referencia `v1.2.0`, clonaron el commit `5c76fc4` e instalaron la 1.2.0. Los 25 archivos instalados coinciden con los de la etiqueta, salvo los finales de línea, que Git para Windows convirtió a CRLF. No se probó `marketplace update` sobre un catálogo fijado.

## Fuentes

Documentación oficial consultada el 25/10/2026:

- [Empaquetar el plugin](https://developers.openai.com/plugins/build/plugins): manifiestos portables, catálogos y almacenamiento de instalación.
- [Crear skills](https://learn.chatgpt.com/docs/build-skills): estructura, descubrimiento local y distribución.
- [Instrucciones del proyecto](https://learn.chatgpt.com/docs/agent-configuration/agents-md): Codex descubre `AGENTS.md`.

El comando `codex plugin add` se obtuvo de la ayuda local de la CLI y después se ejecutó con éxito; no se dedujo del comando del catálogo ni se presentó como cita del sitio web.
