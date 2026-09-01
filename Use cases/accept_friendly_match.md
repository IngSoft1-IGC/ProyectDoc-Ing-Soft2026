### Caso de uso #21: Aceptar partido amistoso

#### Descripción
Permite a un usuario que ha recibido una invitación a un partido amistoso aceptarla para dar inicio al partido de forma inmediata.

#### Atributos
* ** Actor Primario:** Usuario (Receptor de la invitación)
* ** Actor Secundario:** Sistema
* ** Precondición:** 
  1. El Usuario debe estar autenticado.
  2. El Usuario debe tener una invitación a partido amistoso en estado "Pendiente".
  3. Ambos usuarios deben contar con al menos 3 jugadores titulares disponibles (sin importar en cuántas ligas estén inscritos sus equipos).

#### Inputs
1. Confirmación de aceptación de la solicitud.

#### Escenario Exitoso
1. El Usuario accede a su lista de solicitudes de amistosos.
2. El Usuario selecciona la invitación pendiente y presiona el botón 'Aceptar'.
3. El Sistema verifica que ambos equipos dispongan de 3 jugadores titulares y no se encuentren disputando un partido en vivo en ese instante.
4. El Sistema actualiza el estado de la invitación a "Aceptada" en la base de datos.
5. El Sistema instancia la sala del partido amistoso (práctica 3v3).
6. El Sistema notifica a ambos usuarios y redirige la pantalla hacia la visualización del partido en tiempo real.

#### Escenarios Excepcionales

* ** 3a. Uno de los usuarios ingresó a un partido en curso previamente:**
  1. El Sistema detecta que uno de los dos rivales ya se encuentra disputando otro encuentro en tiempo real.
  2. El Sistema cancela la acción y notifica al usuario que el partido no puede iniciar en este momento.
  3. La invitación permanece pendiente.