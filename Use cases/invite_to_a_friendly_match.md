### Caso de uso #20: Invitar a partido amistoso

#### Descripción
Permite a un usuario enviar una solicitud a otro usuario para jugar un partido amistoso sin requerir pertenecer a una liga ni afectar su participación en ellas.

#### Atributos
* **Actor Primario:** Usuario (Desafiante)
* **Actor Secundario:** Usuario (Rival) / Sistema
* **Precondición:** 
  1. El Usuario debe estar autenticado.
  2. El Usuario debe contar con al menos 3 jugadores titulares configurados en su equipo.
  3. El usuario rival debe estar registrado en la plataforma y poseer equipos válidos.

#### Inputs
1. Usuario a desafiar.

#### Escenario Exitoso
1. El Usuario selecciona al rival de la lista de usuarios o busca su perfil.
2. El Usuario presiona el botón 'Invitar a Amistoso'.
3. El Sistema solicita confirmación para enviar la invitación.
4. El Usuario confirma el envío.
5. El Sistema registra la invitación en estado "Pendiente" en la base de datos.
6. El Sistema envía una notificación al Usuario rival y muestra un mensaje de éxito al Usuario desafiante.

#### Escenarios Excepcionales

* **4a. El Usuario Cancela el envío:**
  1. El Sistema cancela la operación sin modificar los datos.
  2. El Sistema regresa al perfil del rival o lista de usuarios.

* **5a. El Usuario rival no tiene los jugadores mínimos o está disputando un partido en curso:**
  1. El Sistema detecta que el rival no cumple los requisitos o se encuentra en medio de un partido en tiempo real.
  2. El Sistema muestra un mensaje de error indicando que el rival no está disponible en este momento.
  3. No se genera la solicitud de invitación.