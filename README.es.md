# mm-method

[English](README.md) · **Español**

Una metodología de trabajo, empaquetada como plugin de Claude Code y Codex, para proyectos de software hechos con agentes de IA en los que una persona (el dueño) toma las decisiones técnicas. Funciona con Claude Code o Codex, solos o juntos en el mismo repositorio.

No es una librería ni una herramienta que automatice algo: es un conjunto de instrucciones que el agente sigue. Lo que la hace cumplir en un proyecto es el `CLAUDE.md` del proyecto, la protección de `main` y la disciplina del equipo. La skill le enseña al agente cómo trabajar y le da los esqueletos para montarlo.

## El problema que resuelve

Un agente de código es rápido y capaz, pero en un proyecto que dura semanas aparecen los mismos problemas:

- toma decisiones técnicas que no le tocaban, o las toma en silencio;
- lo que se decidió en un chat no llega al siguiente, ni a otro computador, ni al otro agente;
- programa antes de que esté claro qué hay que programar;
- escribe pruebas que copian lo que da el código, y por eso siempre pasan;
- dos agentes en el mismo repo se pisan las ramas y los archivos;
- se gastan el contexto y el uso semanal sin que nadie lo note.

La metodología responde a cada uno con una regla concreta y con el porqué de esa regla.

## Los principios

| # | Principio | Por qué |
|---|---|---|
| 1 | La fuente técnica prima (la norma, la especificación) | Si el producto tiene que cumplir algo, ninguna decisión, ni del dueño ni del agente, la contradice en silencio. |
| 2 | Supuestos y limitaciones a la vista | El usuario del producto y el dueño saben siempre qué se supuso y qué no se revisa. |
| 3 | El dueño decide; el agente propone | Las decisiones se piden en lista numerada, con la opción recomendada en negrilla, y se registran textuales. |
| 4 | Primero se planea, después se programa | No se escribe código del producto sin la palabra del dueño. |
| 5 | Nada inventado | Ni datos, ni resultados, ni capacidades; los valores esperados de una prueba no se copian del código. |
| 6 | La memoria vive en el repositorio | Viaja con Git a otros chats, computadores y agentes. |
| 7 | Sin relleno | En la interfaz, los textos, los documentos y el código. |

## Qué trae

```
mm-method/                            catálogo de plugins (este repositorio)
├── .claude-plugin/marketplace.json
├── .agents/plugins/marketplace.json
├── plugins/metodologia-agentes/
│   ├── .claude-plugin/plugin.json
│   ├── plugin.json
│   ├── LICENSE
│   └── skills/
│       ├── metodologia-agentes/       la skill en español
│       │   ├── SKILL.md
│       │   └── references/            10 referencias que se leen solo cuando hacen falta
│       └── agent-methodology/         la misma skill en inglés
├── docs/es/  ·  docs/en/              la documentación
├── AGENTS.md  ·  CLAUDE.md
├── CHANGELOG.md  ·  LICENSE  ·  SECURITY.md
└── README.md (inglés)  ·  README.es.md (español)
```

Los dos clientes leen las mismas carpetas de skills, sin copias por cliente. Las dos skills tienen contenido equivalente en sus respectivos idiomas. Cada una tiene su descripción en su idioma, así que el agente usa la que corresponde a la conversación.

| Referencia | De qué trata |
|---|---|
| `decisiones.md` | Cómo se piden, se registran y se escriben las decisiones |
| `memoria.md` | La memoria en el repo, el changelog, varias personas y varios computadores |
| `ciclo.md` | El ciclo de un módulo, de la planeación al PR |
| `pruebas-y-revisiones.md` | Pruebas independientes, mutación y revisiones adversariales |
| `dos-agentes.md` | Dos agentes en el mismo repositorio |
| `repositorio.md` | Ramas, PR, changelog, CI y la rutina después del merge |
| `costos.md` | Contexto, uso y tamaño de los workflows |
| `estilo.md` | Sin relleno |
| `plantillas.md` | Esqueletos de `CLAUDE.md`, la memoria, el changelog y los prompts |
| `seguridad.md` | Secretos, protecciones, contenido de afuera, CI y servidores |

## Instalar

### Claude Code

Necesitas Claude Code con soporte de plugins. El repositorio es su propio catálogo:

```bash
claude plugin marketplace add MarcosSanchez7020/mm-method
claude plugin install metodologia-agentes@mm-method
```

El alcance por defecto es `user`: la skill queda disponible en todos tus proyectos. Para dejarla solo en un proyecto, usa `--scope project` (se guarda en la configuración del proyecto) o `--scope local`.

Dentro de Claude Code también sirve: `/plugin marketplace add MarcosSanchez7020/mm-method` y después `/plugin install metodologia-agentes@mm-method`.

Para comprobar que quedó instalada:

```bash
claude plugin list
```

### Codex

Con Codex CLI y soporte de plugins:

```bash
codex plugin marketplace add MarcosSanchez7020/mm-method
codex plugin add metodologia-agentes@mm-method
```

Abre una sesión nueva después de instalar. En la CLI, menciona `$metodologia-agentes:metodologia-agentes` para español o `$metodologia-agentes:agent-methodology` para inglés. Comprueba la instalación con `codex plugin list --marketplace mm-method --json`.

Los dos comandos de instalación se ejecutaron con éxito con Codex CLI 0.162.1 el 25/10/2026. Consulta el [alcance de las pruebas y la distribución](docs/es/codex.md). Los comandos instalan la rama predeterminada del catálogo.

### Versión fija

Los comandos anteriores instalan la rama predeterminada del catálogo, que cambia con cada merge. Para instalar una versión publicada, agrega el catálogo con su etiqueta; las etiquetas están en [Tags](https://github.com/MarcosSanchez7020/mm-method/tags):

```bash
claude plugin marketplace add MarcosSanchez7020/mm-method@v1.2.1
codex plugin marketplace add MarcosSanchez7020/mm-method --ref v1.2.1
```

Después se instala igual que arriba (`claude plugin install metodologia-agentes@mm-method` o `codex plugin add metodologia-agentes@mm-method`). Los dos comandos se probaron el 25/10/2026 con Claude Code 2.1.296 y Codex CLI 0.162.1, con configuraciones nuevas y vacías: cada cliente registró la etiqueta, clonó su commit e instaló la versión de esa etiqueta (probado con `v1.2.0` y con `v1.2.1`). Para pasar a otra versión, quita el catálogo y vuelve a agregarlo con la otra etiqueta; `marketplace update` sobre un catálogo fijado no se probó.

## Actualizar

### Claude Code

```bash
claude plugin marketplace update mm-method
claude plugin update metodologia-agentes@mm-method
```

Los cambios se aplican al reiniciar Claude Code. Qué cambió en cada versión está en el [CHANGELOG](CHANGELOG.md).

### Codex

La actualización del catálogo y la reinstalación del plugin se describen en la [guía de Codex](docs/es/codex.md).

## Desinstalar

### Claude Code

Con el mismo alcance que usaste al instalar; sin `--scope`, `uninstall` usa `user`:

```bash
claude plugin uninstall metodologia-agentes@mm-method --scope user
```

Cambia `user` por `project` o `local` si la instalaste así. Quita el catálogo solo cuando ninguna otra instalación ni otro plugin lo use; sin `--scope`, `marketplace remove` lo quita de todos los alcances:

```bash
claude plugin marketplace remove mm-method
```

### Codex

```bash
codex plugin remove metodologia-agentes@mm-method
codex plugin marketplace remove mm-method
```

Quita el catálogo solo si ya no lo necesitas. Estos comandos afectan a la instalación de Codex.

## Arranque rápido

La skill se aplica cuando la pides o cuando el `CLAUDE.md` del proyecto dice que adoptó la metodología. En Claude Code puede cargarse sola por su descripción; en Codex, nómbrala con su nombre completo (`$metodologia-agentes:metodologia-agentes` o `$metodologia-agentes:agent-methodology`). La activación automática no está garantizada. Por ejemplo:

- "Vamos a montar este repo con la metodología: escribamos el `CLAUDE.md` y la carpeta de memoria."
- "Antes de programar esto, pásame las decisiones en lista."
- "Planea el módulo de facturación con la metodología; no programes hasta que te diga."
- "Voy a trabajar con Codex en este mismo repo. ¿Cómo nos organizamos?"
- "Cierra el chat: deja la memoria al día y dame el prompt para el siguiente."

Para adoptarla en un proyecto, sigue la [guía de adopción](docs/es/adopcion.md).

## Documentación

- [Guía completa](docs/es/guia.md): cómo funciona cada parte y por qué.
- [Guía de adopción](docs/es/adopcion.md): de cero a un proyecto montado, paso a paso.
- [Preguntas frecuentes](docs/es/preguntas-frecuentes.md).
- [Codex: distribución, instalación y alcance de las pruebas](docs/es/codex.md).

## Costo en contexto

Estimación de `claude plugin details` con Claude Code 2.1.296, el 25/10/2026, sobre la versión 1.2.0, desde la raíz de este repositorio:

```bash
claude --plugin-dir plugins/metodologia-agentes plugin details metodologia-agentes
```

| Qué | Configuración nueva y vacía | Configuración habitual del autor |
|---|---|---|
| Las dos descripciones, cargadas en cada sesión | ~472 | ~693 |
| Una skill al activarse (su `SKILL.md`) | ~1.600 a ~1.700 | ~2.500 |
| Cada referencia, solo si el agente la lee | depende de la referencia | depende de la referencia |

La herramienta advierte que son estimaciones y que pueden diferir del uso real. La misma versión dio cifras distintas según la configuración de Claude Code: una carpeta de configuración nueva y vacía (`CLAUDE_CONFIG_DIR`) o la que el autor usa a diario. La causa no se ha determinado: tómalas como un rango, no como una cifra exacta.

Con la 1.0.0, el mismo día, dos corridas del mismo comando dieron ~643 y ~456 tokens fijos, y ~1.900 y ~1.300 por activación. Ninguna de estas cifras mide el consumo en Codex.

## Límites

- La skill instruye; no obliga. Si el `CLAUDE.md` del proyecto dice otra cosa, manda el proyecto.
- No trae hooks, comandos ni servidores: no ejecuta nada por su cuenta.
- No incluye ni instala otros plugins. claude-mem se sugiere como complemento opcional, con sus ajustes, en la referencia `memoria.md`.
- La protección de `main`, los checks obligatorios y la plantilla del PR se configuran en el repositorio del proyecto; la guía de adopción dice cómo, pero la skill no los crea sola.
- Con Codex, incluso como único agente, el proyecto necesita un `AGENTS.md` que indique leer las reglas compartidas en `CLAUDE.md`. La [guía de adopción](docs/es/adopcion.md) incluye ese puente.

## Seguridad

- **Qué instala.** Solo texto: dos skills en Markdown y el texto de la licencia. No trae hooks, comandos, agentes, servidores MCP ni LSP, ni código que corra al instalarlo (`claude plugin details`: 2 skills, 0 agentes, 0 hooks, 0 servidores MCP, 0 LSP).
- **Los ejemplos de código** de la documentación (el check de aprobación y el gancho de equipo) solo corren si alguien los copia y los ejecuta. Léelos antes, como cualquier código de terceros. La skill le indica al agente instalar el gancho solo si el dueño lo decide.
- **Lo que manda la metodología:** no subir secretos ni datos personales, no desactivar protecciones, tratar como dato (no como instrucción) lo que llega de afuera, CI con `pull_request` y permisos mínimos, y servidores locales en `127.0.0.1`. Todo está en la referencia `seguridad.md` de la skill.
- **Repositorio público, memoria pública.** Si adoptas la metodología en un repo público, su carpeta de memoria, el registro de las respuestas del dueño y el changelog también son públicos.
- **claude-mem** es opcional y de terceros: guarda datos de las sesiones en el computador y los envía al proveedor de IA que se le configure. Revísalo antes de instalarlo.

## Versión y licencia

Versión 1.2.1 (ver [CHANGELOG](CHANGELOG.md)). Licencia: MIT ([LICENSE](LICENSE)).
