### Caso de uso #19: Empezar liga

#### Descripción
Permite al creador de una liga confirmar y dar inicio formal a los partidos una vez alcanzadas las condiciones de equipos requeridos o lo hará el sistema llegada la fecha establecida.

#### Atributos
* **Actor Primario:** Usuario (Creador de la liga)
* **Actor Secundario:** Sistema
* **Precondición:** 
  1. El Usuario debe estar autenticado y estar posicionado en el menú/gestión de la liga.
  2. La liga debe tener registrada la cantidad mínima de equipos requerida.
  3. Cada equipo participante debe contar con su plantilla reglamentaria (3 jugadores titulares y 3 suplentes), independientemente de si participan o no en otras ligas activas.

#### Inputs
1. Confirmación de inicio ('Empezar liga').

#### Escenario Exitoso
1. El Usuario selecciona la liga creada en estado "Inscripción".
2. El Usuario presiona el botón 'Empezar Liga' para iniciar los partidos.
3. El Sistema valida que la liga cumpla con el número mínimo de equipos inscritos y que cada equipo tenga asignados sus jugadores.
4. El Sistema cambia el estado de la liga a "En Curso".
5. El Sistema genera el fixture de la liga.
6. El Sistema muestra un mensaje de confirmación notificando el inicio exitoso de la liga y redirige al panel de visualización de partidos.

#### Escenarios Excepcionales

* **3a. La liga no cumple con la cantidad mínima de equipos registrados:**
  1. El Sistema detecta que la cantidad de equipos inscritos es menor al mínimo configurado.
  2. El Sistema muestra un mensaje de error indicando que no se pueden empezar los partidos hasta completar el cupo mínimo.
  3. La liga permanece en su estado anterior.

* **3b. Uno o más equipos no cuentan con la plantilla de jugadores requerida:**
  1. El Sistema detecta que un equipo no posee exactamente 6 jugadores registrados (3 titulares y 3 suplentes).
  2. El Sistema muestra un mensaje de error notificando la inconsistencia en las plantillas.
  3. La liga permanece en su estado anterior.