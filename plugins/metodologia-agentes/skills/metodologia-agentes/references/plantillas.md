# Esqueletos para adoptar la metodología

Lo que va entre `< >` lo llena el equipo. Nada de esto se rellena con datos inventados: si un dato no se sabe, se le pregunta al dueño.

## `CLAUDE.md`

```markdown
# <Nombre del proyecto>

<Qué es el producto, en dos líneas. Quién es el equipo.>

## Fuente de verdad
- `<documento de alcance y decisiones>`: alcance, decisiones, pendientes.
- `<documentos técnicos>`: requisitos, modelo de datos, diagramas.
- `<fuente técnica original>`: la norma o especificación, en su archivo original.
- `CHANGELOG.md`: todo cambio.
- `MEMORIA/`: bitácora por equipo, entorno de cada equipo y registro textual de las respuestas del dueño. Se lee al empezar cada sesión.
- `AGENTS.md`: para los agentes que no son Claude Code.

## Lo que no se negocia
- La fuente técnica prima sobre cualquier decisión (<dueño>, <fecha>).
- Supuestos y limitaciones a la vista, hacia afuera y hacia adentro.
- <Reglas propias del producto.>
- Sin relleno.

## El equipo
- <Persona>: dueño de <área>. Decide <qué>.
- <Persona>: <área>. Decide <qué>.

## Cómo trabajar con el equipo
- Idioma y trato: <…>.
- Primero se planea, después se programa. La palabra que autoriza programar: "<palabra>".
- Decisiones en lista numerada, con opciones con letra y la recomendada en negrilla.
- Los prompts, en el chat, en bloques de código.
- Revisiones: <quién por defecto; preguntar antes de cada una>.
- Modelos: <cuál para planear, cuál para ejecutar>.

## Cómo se trabaja cada módulo
<Los seis pasos del ciclo, ajustados al proyecto.>

## Reglas duraderas: siempre aquí
Toda regla del equipo que deba valer siempre se escribe en este archivo, con su entrada en el changelog, en la rama en la que se está trabajando.

## Memoria en el repo
<Qué va en `MEMORIA/` y cuándo se anota.>

## Agentes: cómo se trabaja con los dos
<Carpetas, ramas, mensajes por el dueño, archivos compartidos, puertos, PR y merge, revisiones. Las excepciones, con fecha y respuesta del dueño.>

## Repositorio
- Nunca directo en `main`; `main` protegida; merge, no squash.
- Revisión técnica del dueño: archivos en `<lista>`; la línea de aprobación la escribe solo él.
- Changelog para todo; commits con `git commit -F`.
- Suite local y CI en paralelo; el resultado local, en la descripción del PR.
- La rutina después de cada merge.
```

## `MEMORIA/README.md`

```markdown
# Memoria del proyecto (en el repo)

- `bitacora_equipos.md`: lo que se hizo desde cada equipo, por fecha.
- `entorno_equipos.md`: cómo está montado cada equipo.
- `registro_respuestas.md`: las respuestas del dueño, textuales.

Las reglas están en `CLAUDE.md`. Aquí no van secretos ni datos personales de terceros.
```

## Entrada del registro

```markdown
### <DD/MM/AAAA> — <Tema> (<dueño>, <agente>)

<Contexto en una o dos líneas.> El agente le pasó, con todas sus opciones (la recomendada, en negrilla):
1. <pregunta>: **a) <recomendada>**; b) <…>.

Dio por decidido: <…>. <Dueño>:

> <sus palabras, tal cual>

(<Cómo se lee. Lo que no dijo.>)
```

## Entrada de la bitácora

```markdown
- <DD/MM/AAAA> (<agente>, sesión `<id>`): COMMIT `<hash>` (<qué>) y PUSH de `<rama>`, <con o sin PR>. <Lo demás que pasó.> <Lo que queda sin commit, si queda.>
```

## Sección del changelog

```markdown
## [Sin publicar] — <DD/MM/AAAA>: <tema> (rama `<rama>`)

### Decidido
- **<Decisión>** (<dueño>, <fecha>: "<respuesta>"): <qué quedó>. (<dueño>, con propuesta de <agente>)

### Cambiado
- `<archivo>`: <qué cambió>. (<persona>, <agente>)
```

## Línea de aprobación en la plantilla del PR

```markdown
- [ ] Revisión técnica aprobada por <dueño> (fecha: DD/MM/AAAA, commit: abc1234)
```

## Prompt para el chat siguiente

```text
Chat nuevo de <agente> en <carpeta>, <equipo>.

ANTES DE NADA: lee CLAUDE.md y MEMORIA/ de origin/main y de origin/<rama>. Después: git fetch --prune, git worktree list y git status.

DÓNDE QUEDÓ
- <Lo hecho, con commits y PR.>
- <Lo que está en curso y en qué rama.>
- <Lo que corre en otro agente.>

LO QUE SIGUE
1. <Paso.>
2. <Paso.>

PENDIENTES
- <Lo abierto, con su fuente.>
```


## Check de la línea de aprobación

Un workflow aparte de la suite de pruebas. Escucha también `edited`, para reevaluarse cuando el dueño marca la línea, y `synchronize`, para exigir una aprobación nueva cuando entra otro commit. La suite no escucha `edited`, así que editar la descripción no la vuelve a correr.

```yaml
name: Revisión técnica
on:
  pull_request:
    types: [opened, edited, synchronize, reopened]
permissions:
  contents: read
jobs:
  aprobacion:
    runs-on: ubuntu-latest
    steps:
      - name: Comprobar la línea de aprobación y su commit
        shell: bash
        env:
          BODY: ${{ github.event.pull_request.body }}
          HEAD_SHA: ${{ github.event.pull_request.head.sha }}
        run: |
          pattern='^- \[x\] Revisión técnica aprobada por [^()]+ \(fecha: [0-9]{2}/[0-9]{2}/[0-9]{4}, commit: ([0-9a-f]{7,40})\)$'
          lines=$(printf '%s\n' "$BODY" | tr -d '\r' | grep -E "$pattern" || true)
          count=$(printf '%s' "$lines" | grep -c . || true)
          if [ "$count" -ne 1 ]; then echo "Se espera exactamente una línea de aprobación marcada y bien formada; hay $count."; exit 1; fi
          [[ $lines =~ $pattern ]]
          sha=${BASH_REMATCH[1]}
          case "$HEAD_SHA" in
            "$sha"*) echo "Formato válido, aprobado sobre $sha." ;;
            *) echo "La aprobación es de $sha y el último commit es $HEAD_SHA."; exit 1 ;;
          esac
```

- **Este check no autentica a nadie.** Comprueba el formato de la línea y que su commit sea el último del PR, pero cualquiera que pueda editar la descripción, también el autor del PR, puede escribirla. La aprobación que GitHub sí verifica es una revisión: un archivo `.github/CODEOWNERS` con los archivos técnicos a nombre del dueño y, en la protección de `main`, revisión obligatoria de los dueños de código y descarte de las aprobaciones cuando entra un commit nuevo (`repositorio.md`). Si el equipo se queda solo con la línea, la declara como registro, no como control de acceso.
- Acepta exactamente una línea marcada y bien formada, comparada completa de principio a fin (también con finales de línea CRLF), y toma el commit de esa misma coincidencia. Un texto agregado después de la línea, otra línea marcada o un nombre con paréntesis la hacen fallar.
- La descripción entra por una variable de entorno y no dentro del script, para que un texto del PR no pueda ejecutar comandos.
- Así como está, aplica a todos los PR. Limitarlo a los que tocan archivos de la lista técnica exige consultar los archivos del PR en la API de GitHub; esa parte depende de cada repositorio y no está aquí.
- Probado en una terminal con `bash --noprofile --norc -eo pipefail`, como corre GitHub Actions un paso con `shell: bash`, en diez casos: sin marcar, el último commit abreviado y completo, otro commit, un texto agregado con otro commit después de la línea, un nombre con paréntesis, dos líneas marcadas, finales de línea CRLF, un intento de inyección y la descripción vacía. El workflow completo no se ha corrido en GitHub.
## Gancho de equipo

Marca los commits hechos desde un computador que no es el principal. Los ganchos de Git son de todo el clon: los comparten la carpeta principal y sus worktrees. Por eso se instala una sola vez por computador, desde la carpeta principal del clon, y lo instala la persona o el agente de esa carpeta, nunca el agente del worktree, que no escribe en la carpeta del otro.

```sh
hook=$(git rev-parse --git-path hooks/prepare-commit-msg)
if [ -e "$hook" ]; then
  echo "Ya existe $hook: agrégale a mano la línea de git interpret-trailers de abajo."
else
  mkdir -p "$(dirname "$hook")"
  cat > "$hook" <<'EOF'
#!/bin/sh
case "$2" in merge|squash) exit 0 ;; esac
git interpret-trailers --in-place --if-exists doNothing --trailer "Equipo: <nombre-del-equipo>" "$1"
EOF
  chmod +x "$hook"
fi
```

- `git rev-parse --git-path` da la ruta real del gancho. En un worktree, `.git` es un archivo y no una carpeta, así que `.git/hooks/` no sirve como ruta fija.
- Si el clon usa `core.hooksPath`, la ruta sale de esa configuración. Si es relativa, Git la resuelve en la carpeta de trabajo donde corre el gancho, así que cada worktree busca su propia copia; en ese caso conviene versionar el gancho en esa carpeta o usar una ruta absoluta.
- Si ya hay un gancho `prepare-commit-msg`, no se reemplaza: se le agrega la línea de `git interpret-trailers`.
- El gancho agrega `Equipo: <nombre-del-equipo>` como trailer del commit, y `git log --grep "Equipo: <nombre-del-equipo>"` encuentra lo que se hizo desde ese computador.
- Probado con Git 2.55 en un clon con un worktree: la ruta vista desde el worktree es la de los ganchos del clon, un commit hecho en el worktree lleva el trailer, una segunda instalación no pisa la primera y `core.hooksPath` cambia la ruta.
