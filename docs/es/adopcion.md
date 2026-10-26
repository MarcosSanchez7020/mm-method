# Guía de adopción

De cero a un proyecto que trabaja con la metodología. Los pasos 2 a 6 los puede hacer el agente; los pasos 7 y 8 son configuración de GitHub y los hace una persona con permisos de administración del repositorio.

## Antes de empezar

- Claude Code o Codex con soporte de plugins (`claude plugin --help` o `codex plugin --help` lista los comandos).
- Git, y el repositorio del proyecto en GitHub.
- Opcional: la CLI de GitHub (`gh`), para que el agente abra los PR.
- Opcional: un segundo agente (por ejemplo, Codex) si van a trabajar dos.

## 1. Instalar la skill

Elige tu cliente; si usas ambos, instala el plugin en cada uno.

Claude Code:

```bash
claude plugin marketplace add MarcosSanchez7020/mm-method
claude plugin install metodologia-agentes@mm-method
```

Codex:

```bash
codex plugin marketplace add MarcosSanchez7020/mm-method
codex plugin add metodologia-agentes@mm-method
```

Abre una sesión nueva. Consulta la [guía de Codex](codex.md) para las versiones probadas y los límites.

## 2. Pedirle al agente que monte el proyecto

En una sesión nueva, dentro del repositorio del proyecto:

```text
Vamos a trabajar este repo con la metodología. Antes de crear archivos, pásame en lista las decisiones que necesitas para el CLAUDE.md: quién decide qué, la palabra para programar, idioma y trato, quién revisa por defecto, qué archivos exigen mi revisión técnica, qué agentes y modelos usamos y el presupuesto de CI.
```

El agente responde con una lista numerada. Con tus respuestas escribe los archivos de los pasos 3 a 6 y anota tus respuestas en el registro.

## 3. `CLAUDE.md`

El esqueleto está en `references/plantillas.md` de la skill. Las secciones que no pueden faltar:

| Sección | Qué lleva |
|---|---|
| Fuente de verdad | Qué documentos mandan y dónde está la fuente técnica original |
| Lo que no se negocia | Los principios de la metodología y las reglas propias del producto |
| El equipo | Quién es dueño de qué y quién decide qué |
| Cómo trabajar con el equipo | Palabra para programar, decisiones en lista, idioma, revisiones, modelos |
| Cómo se trabaja cada módulo | Los seis pasos del ciclo, ajustados al proyecto |
| Memoria en el repo | Qué va en `MEMORIA/` y cuándo se anota |
| Agentes | Carpetas, ramas, mensajes, archivos compartidos, PR (solo si hay dos) |
| Repositorio | `main` protegida, revisión técnica, changelog, CI, rutina después del merge |

Si el proyecto ya tiene un `CLAUDE.md`, el agente propone dónde encaja cada sección y te pasa en lista lo que choca.

## 4. La carpeta de memoria

```bash
mkdir MEMORIA
```

Con cuatro archivos: `README.md`, `bitacora_equipos.md`, `entorno_equipos.md` y `registro_respuestas.md`. Los esqueletos están en `references/plantillas.md`. Nunca van ahí secretos ni datos personales de terceros.

## 5. `CHANGELOG.md`

Con una sección "[Sin publicar]" por rama. Cada cambio, por pequeño que sea, lleva fecha, archivo, qué cambió y quién.

## 6. `AGENTS.md` siempre que se use Codex

Incluso si Codex es el único agente, crea un `AGENTS.md` que indique leer `CLAUDE.md` y `MEMORIA/`. El esqueleto está en `references/plantillas.md`. Las reglas compartidas permanecen en `CLAUDE.md`; no se duplican. Si ya existe `AGENTS.md`, integra el puente conservando sus instrucciones.

Solo si trabajan dos agentes, crea el worktree del segundo desde la carpeta principal:

```bash
git fetch --prune
git worktree add --no-track -b <rama-del-agente> .worktrees/<agente> origin/main
```

Y `.worktrees/` en el `.gitignore`.

## 7. Proteger `main`

En GitHub, como administrador del repositorio:

1. **Settings → Branches** (o **Settings → Rules → Rulesets**): una regla para `main` que exija PR antes de unir, exija los checks de la CI en verde y bloquee los force-push.
2. **Settings → General → Pull Requests:** dejar activo "Allow merge commits" y desactivar "Allow squash merging" y "Allow rebase merging", para que cada merge conserve los commits de la rama.
3. **La aprobación que GitHub autentica:** un archivo `.github/CODEOWNERS` que ponga los archivos técnicos y `/.github/` (el propio `CODEOWNERS` y los workflows) a nombre del dueño y, en la regla de `main`, "Require review from Code Owners" y "Dismiss stale pull request approvals when new commits are pushed". La línea de la descripción del PR no autentica a nadie. GitHub no deja que el autor de un PR lo apruebe: si el agente abre los PR con la cuenta del dueño, el agente necesita su propia cuenta.

## 8. La plantilla del PR y la CI

`.github/pull_request_template.md`, con al menos:

```markdown
## Qué cambia

## Pruebas
- [ ] Suite local: <resultado>
- [ ] CI: <resultado>

## Revisión técnica
- [ ] Revisión técnica aprobada por <dueño> (fecha: DD/MM/AAAA, commit: <SHA completo del último commit>)
```

La línea de aprobación la marca solo el dueño.

En los workflows de la CI que corren en cada PR, esto cancela la corrida anterior cuando entra un commit nuevo, para no pagar dos veces:

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
  cancel-in-progress: true
```

La lista de archivos que exigen revisión técnica (por ejemplo, `.github/technical-paths.txt`) la puede usar un check aparte que compruebe la línea de aprobación y su commit. Ese check va en su propio workflow, que escucha también `edited`, para reevaluarse cuando el dueño marca la línea y cuando entra un commit nuevo; la suite de pruebas no escucha `edited`. El ejemplo está en `references/plantillas.md` de la skill, "Check de la línea de aprobación". La parte que limita el check a los PR que tocan archivos de la lista depende de cada repositorio y la skill no la trae.

## 9. Varios computadores (opcional)

En cada computador que no sea el principal, un gancho local marca los commits con el nombre del equipo. Se instala una vez por computador, desde la carpeta principal del clon, porque los worktrees comparten los ganchos del clon; el agente del worktree no lo instala. El script pide el nombre y lo guarda, como dato, en la configuración de Git del clon, que el gancho lee en cada commit; resuelve la ruta real del gancho y no pisa uno que ya exista, ni un enlace con ese nombre aunque esté roto. Se corre el bloque completo y, cuando pide el nombre, se escribe en una línea, sin comillas:

```sh
(
printf '%s' 'Nombre del equipo (una línea): '
if IFS= read -r nombre; then
  nombre=${nombre%"$(printf '\r')"}
else
  nombre=
fi
case $nombre in
  *[![:space:]]*) ;;
  *) printf '%s\n' 'Escribe un nombre que no esté vacío ni tenga solo espacios, y termina con Enter.' >&2; exit 1 ;;
esac
git config --local equipo.nombre "$nombre" || exit 1
hook=$(git rev-parse --git-path hooks/prepare-commit-msg) || exit 1
if [ -e "$hook" ] || [ -L "$hook" ]; then
  echo "Ya existe $hook: agrégale a mano las dos últimas líneas del gancho de abajo."
else
  mkdir -p "$(dirname "$hook")"
  cat > "$hook" <<'EOF'
#!/bin/sh
case "$2" in merge|squash) exit 0 ;; esac
nombre=$(git config --get equipo.nombre) || exit 0
git interpret-trailers --in-place --if-exists doNothing --trailer "Equipo: $nombre" "$1"
EOF
  chmod +x "$hook"
fi
)
```

Los detalles (worktrees, `core.hooksPath`, qué hacer si ya hay un gancho, cómo cambiar el nombre y cómo buscar lo que se hizo desde ese computador) están en `references/plantillas.md` de la skill, "Gancho de equipo".

## 10. La primera sesión de trabajo

```text
Lee CLAUDE.md y MEMORIA/. Después: git fetch --prune, git worktree list y git status. Vamos a planear <el módulo>: lee la fuente original y pásame lo que dice, lo que hace hoy el código y las decisiones en lista. No programes hasta que te diga "<palabra>".
```

## Lista de comprobación

- [ ] La skill instalada en el cliente elegido (`claude plugin list` o `codex plugin list --marketplace mm-method --json`).
- [ ] `CLAUDE.md` con sus ocho secciones y las respuestas del dueño.
- [ ] `MEMORIA/` con sus cuatro archivos.
- [ ] `CHANGELOG.md` con "[Sin publicar]".
- [ ] `AGENTS.md` si se usa Codex, incluso solo; worktree separado si hay un segundo agente.
- [ ] `main` protegida y solo merge commits.
- [ ] La plantilla del PR con la línea de aprobación.
- [ ] `CODEOWNERS` y revisión obligatoria de los dueños de código, si la aprobación tiene que ser autenticada.
- [ ] La CI corriendo en cada PR, con cancelación de la corrida anterior.
- [ ] El gancho de equipo en los computadores que no son el principal.
