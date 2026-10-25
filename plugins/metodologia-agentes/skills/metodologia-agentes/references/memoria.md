# La memoria vive en el repositorio

La memoria local de un agente se queda en un computador. El repositorio viaja con Git a todos los equipos, a la nube y al otro agente. Por eso todo lo que haga falta saber para seguir trabajando se escribe en el repo.

## Qué va dónde

| Archivo | Qué guarda |
|---|---|
| `CLAUDE.md` | Las reglas y preferencias duraderas del equipo. Se carga solo en cada sesión. |
| `AGENTS.md` | Para los agentes que no son Claude Code: apunta a `CLAUDE.md` y a la memoria, con las reglas propias de esos agentes. |
| `MEMORIA/README.md` | Qué hay en la carpeta y cómo se usa. |
| `MEMORIA/bitacora_equipos.md` | Por equipo y por fecha: cada commit, push, PR, rama, cambio de estado y el trabajo que queda sin commit. |
| `MEMORIA/entorno_equipos.md` | Cómo está montado cada equipo: versiones, rutas, puertos y los trucos que hacen falta. |
| `MEMORIA/registro_respuestas.md` | Las respuestas del dueño, textuales (`decisiones.md`). |
| `CHANGELOG.md` | Todo cambio, por pequeño que sea. |
| Los documentos de fuente de verdad | Alcance y decisiones, requisitos, modelo de datos, diagramas. |

Nunca van en el repo secretos, claves, contraseñas, tokens, archivos `.env` ni datos personales de terceros.

## Reglas

- **Se anota en el momento**: cada respuesta del dueño al registro, cada commit, push o PR a la bitácora, lo nuevo de un equipo al entorno. A más tardar, antes de cada commit.
- **La memoria va con cada rama.** Al empezar, el agente hace `git fetch` y lee la memoria de `main` y la de su rama. Lo que pasa después del último commit de una rama (el push, el PR, el merge) se anota en la rama siguiente.
- **La memoria local solo apunta al repo.** Si un computador tiene algo en su memoria local que no está en el repo, se pasa al repo en la primera sesión en ese equipo, sin dar por confirmado nada que no lo esté.
- **Las herramientas de memoria son apoyo.** Si un plugin de memoria choca con lo escrito en el repo, gana el repo.

## Complemento opcional: claude-mem

claude-mem es un plugin de terceros (`thedotmack/claude-mem`, en GitHub) que guarda en cada computador observaciones de las sesiones y se las vuelve a dar al agente al empezar. La metodología no lo necesita: usarlo es una decisión de cada usuario. Si se usa, estos ajustes evitan que choque con ella:

- **El repo manda.** Lo que recuerde claude-mem es apoyo; si choca con `CLAUDE.md`, con la memoria del repo o con los documentos, gana el repo.
- **No reemplaza el registro.** Las decisiones del dueño se citan del registro textual del repo, no de un recuerdo.
- **Revisiones independientes.** Un chat revisor que no deba arrastrar lo que ya se sabe se abre con el plugin apagado (`claude plugin disable claude-mem@thedotmack`) y se vuelve a prender después (`claude plugin enable claude-mem@thedotmack`).
- **Uso semanal.** Su configuración (`~/.claude-mem/settings.json`) trae umbrales de uso semanal (`CLAUDE_MEM_QUOTA_THRESHOLD_SEVEN_DAY` y `CLAUDE_MEM_QUOTA_THRESHOLD_SEVEN_DAY_OPUS`) a partir de los cuales pausa su captura; conviene ajustarlos según cuánto margen se quiera dejar.
- **Datos: dónde se guardan y a dónde van.** Guarda en el computador (en `~/.claude-mem`) lo que pasa en las sesiones y, según su propia documentación, envía las observaciones al proveedor de IA que se le configure para comprimirlas (Claude, OpenRouter o Gemini); una instalación local contacta también a cmem.ai una vez, al registrarse. Antes de prenderlo se revisa a qué proveedor van los datos y cómo los trata, y no se usa en sesiones que lean secretos o datos personales que no deban salir del computador.

Si se decide usarlo: `claude plugin marketplace add thedotmack/claude-mem` y `claude plugin install claude-mem@thedotmack`. Sus requisitos y su documentación están en su repositorio.

## El changelog

- Cada entrada: fecha, archivo, qué cambió y quién ("(Persona, Agente)").
- Va en la sección "[Sin publicar]" de la rama hasta que se suba.
- Nunca se hace un commit sin su entrada en el changelog.
- Cuando el PR se une, la rama siguiente del agente que lo abrió le pone el título a la sección: "DD/MM/AAAA — <tema> (`primer commit` a `último`, unido a `main` con el PR #N, `commit del merge`)".

## Varias personas y varios equipos

- **Quién está trabajando:** el nombre de Git del computador (`git config user.name`). Si no está claro, por ejemplo en una sesión en la nube, se pregunta antes de anotar o hacer un commit. Si la cuenta del agente es compartida, cuenta la persona y el equipo, no la cuenta.
- **Desde qué equipo:** lo que se hace desde un computador distinto del principal se marca con su nombre corto:
  - en los commits, un trailer `Equipo: <nombre>`, puesto por un gancho local `prepare-commit-msg` que no se sube (el gancho está en `plantillas.md`, "Gancho de equipo");
  - en el changelog, "(Persona, Agente; <equipo>)";
  - en los PR, "Hecho desde: <equipo>".
- En un equipo nuevo, el agente pregunta su nombre corto antes del primer commit e instala el gancho en ese clon.
- Cada equipo tiene su sección en la bitácora y en el entorno.

## Cuando un chat se cierra

Por límite de uso, por contexto lleno o porque se terminó la tarea:

1. Memoria al día en la rama, con su entrada en el changelog.
2. Commit y push.
3. Un prompt para el chat siguiente, en un bloque de código, que empiece por leer `CLAUDE.md` y la memoria y diga dónde quedó el trabajo.

Nunca se deja trabajo sin commit sin anotarlo en la bitácora.
