### Caso de uso #22: Rechazar partido amistoso

#### Descripción
Permite a un usuario rechazar una invitación a un partido amistoso recibida de otro jugador.

#### Atributos
* **Actor Primario:** Usuario (Receptor de la invitación)
* **Actor Secundario:** Sistema
* **Precondición:** 
  1. El Usuario debe estar autenticado.
  2. Debe existir una solicitud de partido amistoso dirigida a él en estado "Pendiente".

#### Inputs
1. Confirmación de rechazo de la solicitud.

#### Escenario Exitoso
1. El Usuario selecciona la invitación de amistoso pendiente.
2. El Usuario presiona el botón 'Rechazar'.
3. El Sistema solicita confirmación para rechazar la invitación.
4. El Usuario confirma la acción.
5. El Sistema actualiza el estado de la invitación a "Rechazada" (la elimina en la base de datos).
6. El Sistema notifica al Usuario desafiante sobre el rechazo y muestra un mensaje indicando que la invitación fue rechazada.

#### Escenarios Excepcionales

* **4a. El Usuario Cancela la confirmación de rechazo:**
  1. El Sistema cancela la operación sin modificar los datos.
  2. La invitación permanece en la lista en estado "Pendiente".