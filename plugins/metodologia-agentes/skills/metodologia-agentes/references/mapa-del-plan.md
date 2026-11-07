# El mapa del plan

Cuando el trabajo pasa de un módulo a un tramo largo, con varios frentes, varios chats y varios agentes, el plan vive en un archivo del repositorio: el mapa del plan, por bloques, con casillas. Sirve para que cualquier chat sepa en qué va el proyecto y qué le toca, y para que nada se dé por hecho sin evidencia.

## Qué tiene

- Un resumen general: de dónde viene el proyecto, qué se decidió y cuándo.
- Los bloques de un vistazo: una tabla con cada bloque, su estado («pendiente», «en proceso», «hecho» o «aplazado»), de qué bloques depende y quién lo hace.
- Qué corre en paralelo y qué va en serie.
- Lo que el mapa no cubre: lo que falta del proyecto y no está en el mapa, con el documento donde sí está.
- Cada bloque, uno por uno: sus pasos como casillas, con quién los hace, y lo que «queda abierto».
- Un registro de chequeo, por fecha.

## Reglas

1. **Cada cosa que se haga se marca en el mapa en el mismo commit que la hace.** `- [ ]` es un paso pendiente; `- [x]` es un paso hecho, seguido de quién, la fecha y el commit o la evidencia (un PR, un enlace, una respuesta del dueño entre comillas). Nunca se marca algo antes de que esté hecho ni algo que no se verificó.
2. **Los pasos del dueño** (aprobar, unir, responder, pegar un prompt) los marca el agente que recibe su respuesta, con sus palabras tal cual y la fecha.
3. **Cada agente marca lo suyo en su rama.** Al unir `main` se conservan las marcas de los demás: los conflictos en el mapa se resuelven conservando los dos lados.
4. **El estado de un bloque se cambia cuando cambia de verdad.** Lo que un bloque deja abierto se escribe en su «queda abierto».
5. **Nada se borra.** Si el plan cambia (una decisión nueva, un bloque que se parte o se aplaza), se cambia en el mapa con la fecha y la frase del dueño, y en el registro de respuestas; lo que deja de aplicar se tacha con `~~` y se dice por qué.
6. **Todo chat lee el mapa al empezar**, después de `CLAUDE.md` y de la memoria, para saber en qué bloque va el proyecto y qué le toca.
7. **El trabajo nuevo pasa por el dueño.** El trabajo nuevo, o algo de la lista de lo que el mapa no cubre cuyo momento llegó, se le pasa primero al dueño como decisión y, si lo aprueba, entra como bloque nuevo o como pasos de un bloque.
8. **Los números los asigna un solo chat.** Los bloques se numeran de corrido; si varios chats agregan al mapa a la vez, los números los pone el que coordina (el hub, si lo hay: `hub.md`), para que dos ramas no creen el mismo bloque.

## El dibujo

- Un script del repositorio, sin dependencias, lee el mapa y genera el dibujo (bloques, dependencias, serie y paralelo), por ejemplo un SVG y un diagrama en texto. El dibujo no se edita a mano ni se toca el mapa para que el dibujo salga bien.
- Quien marque una casilla regenera el dibujo y lo sube en el mismo commit.
- Una prueba de la suite comprueba que el dibujo coincide con el archivo (el script tiene un modo de comprobación), para que no se desalineen.
- Si el dibujo choca al unir `main`, no se resuelve a mano: se conservan los dos lados del mapa y se regenera.
- Si el proyecto tiene una página de estado, el dibujo se muestra ahí, debajo del estado.

## Mapa, memoria y changelog

- El mapa dice en qué va el plan; la bitácora, qué se hizo desde cada equipo; el changelog, qué cambió en cada archivo. Los tres se anotan en el mismo commit.

El esqueleto está en `plantillas.md`, «El mapa del plan».
