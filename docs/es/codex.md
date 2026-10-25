# Soporte de Codex

mm-method 1.1.0 empaqueta las mismas skills en español e inglés para Codex y Claude Code. Solo contiene instrucciones. No agrega scripts de instalación, ganchos, servidores, conectores ni requisitos de autenticación.

## Instalar e invocar

En una terminal con Codex CLI y soporte de plugins:

```bash
codex plugin marketplace add MarcosSanchez7020/mm-method
codex plugin add metodologia-agentes@mm-method
codex plugin list --marketplace mm-method --json
```

Abre una sesión nueva. Usa los nombres completos que muestra el plugin:

- Inglés: `$metodologia-agentes:agent-methodology`.
- Español: `$metodologia-agentes:metodologia-agentes`.

Por ejemplo: "Usa $metodologia-agentes:metodologia-agentes para planear la adopción en este repositorio. Antes de escribir archivos, pásame las decisiones en una lista numerada."

El nombre corto sin prefijo no inyectó la skill completa en la sesión no interactiva de Codex probada. Los nombres completos sí. El modelo también puede seleccionar una skill por su descripción, pero la selección automática no está garantizada.

## Una fuente, dos clientes

El catálogo de Codex es `.agents/plugins/marketplace.json`; apunta a `plugins/metodologia-agentes/`. Esa carpeta contiene el `plugin.json` portable y el `.claude-plugin/plugin.json` existente. Claude conserva su `.claude-plugin/marketplace.json` en la raíz del repositorio. Los dos clientes usan las dos carpetas `skills/` existentes y sus diez referencias por idioma. No hacen falta enlaces simbólicos, copias generadas ni scripts de sincronización.

Codex instaló el paquete en `~/.codex/plugins/cache/mm-method/metodologia-agentes/1.1.0/`. Es una caché de instalación, no otra fuente para editar. Cambia el repositorio y reinstala el paquete.

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

Las pruebas del 25/10/2026 usaron Windows, Codex CLI 0.162.1 y Claude Code 2.1.296. Los dos comandos exactos de instalación pública de Codex funcionaron sobre `main` 1.0.1 antes de documentarlos. La 1.1.0 se instaló aparte desde la rama de trabajo; abrir un PR no publica una versión de rama. Los comandos de instalación por defecto siguen instalando la rama predeterminada hasta que el dueño una el PR.

La instalación local de 1.1.0 pasó en ambos clientes. Codex no tenía catálogo, registro de plugin ni caché de mm-method antes de la primera prueba; su configuración de usuario ajena al plugin no era un perfil nuevo. La instalación de Claude usó una carpeta de configuración separada y vacía. Las dos skills cargaron en ES y EN en sesiones nuevas: Codex usó los nombres completos ignorando la configuración del usuario; Claude usó la carpeta del plugin instalado en limpio, con herramientas restringidas y sin la configuración MCP externa. Los 22 archivos Markdown instalados coincidieron con la fuente compartida en cada cliente. La validación estricta de Claude pasó para el catálogo y el plugin, y el validador de skills pasó para ambos idiomas.

Son pruebas de instalación y carga explícita, no una garantía de todos los flujos ni de activación automática. No se probaron la instalación por interfaz de escritorio/IDE, macOS, Linux, otras versiones de los clientes ni el costo en contexto de Codex. El PR registra los resultados de instalación desde Git y la revisión exacta probada; el agente no publica una release ni une el PR.

## Fuentes

Documentación oficial consultada el 25/10/2026:

- [Empaquetar el plugin](https://developers.openai.com/plugins/build/plugins): manifiestos portables, catálogos y almacenamiento de instalación.
- [Crear skills](https://learn.chatgpt.com/docs/build-skills): estructura, descubrimiento local y distribución.
- [Instrucciones del proyecto](https://learn.chatgpt.com/docs/agent-configuration/agents-md): Codex descubre `AGENTS.md`.

El comando `codex plugin add` se obtuvo de la ayuda local de la CLI y después se ejecutó con éxito; no se dedujo del comando del catálogo ni se presentó como cita del sitio web.
