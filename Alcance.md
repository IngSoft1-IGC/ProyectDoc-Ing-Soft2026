## Proyecto Ingenieria: 

## Alcance: 

## Introduccion: 

- FutBot es una plataforma web multiplayer cliente-servidor donde los usuarios asumen un rol de director tecnico que compiten por partidos de futbol los cuales los conformaran 3 jugadores titulares y 3 suplentes donde la intervencion de los mismos es la eleccion de los comportamientos de los jugadores seleccionados en el transcurso del partido. Los jugadores en cancha seran bots cuyas skills estaran sujetas a sus pacss los cuales seran diseñados por el usuario y tendran una limitacion en la suma de los puntos. Los partidos se desarrollan en un entorno 2D y pueden ser visualizados por los mismos usuarios o por usuarios externos. Cada usuario dispondra de un club el cual podra crear jugadores con sus respectivos atributos y nombres. Ademas podra crear una liga con su respectiva duracion y un limite de equipos que la conforman. Otra opcion disponible al usuario es que este se peuda unir a una liga ya creada. 

## Caracteristicas Principales: 

- Gestión de Cuentas y Autenticación: Los usuarios al momento de querer inicializarse en la plataforma deberan registrarse e iniciar sesion, para ello deberan brindar ciertos datos que la plataforma requerida. Esto les permitira poder gestionar su perfil (El cual tendra la opcion de poder ser personalizado a traves de un avatar), acceder a la administracion de su club al momento que este lo cree e interactuar con el juego.

- Gestion De Club y Jugadores: El usuario podra crear y administrar un club con la disponibilidad de creacion de jugadores. En caso que este se una a una liga debera seleccionar 6 jugadores en ese momento, los cuales 3 seran titulares y 3 suplentes, en caso que juegue partidos amistosos solo jugaran los 3 titulares. Estos estaran configurados a traves de sus PACSS (Power, Agility, Control, Speed, Strength) que seran fijos, cada atributo sera un entero entre 20 y 100, limitado a que cuya suma sea exactamente 300 por jugador. 

- Comportamientos de Jugadores: El sistema proveerá primitivas básicas y ejemplos. El usuario podrá crear sus propios comportamientos fuera del partido programando la lógica en código Python dentro de las reglas de la plataforma. Una vez iniciado el partido, el usuario no podrá crear ni editar código, pero sí tendrá permitido reasignar entre sus jugadores los comportamientos creados previamente.

- Desarrollo Del Partido: Los partidos se desarrollan 3v3, en los cuales no hay arbitro, faltas, fueras de juego, laterales ni corners, ya que en los bordes del campo la pelota rebota. Las disputas de pelota por jugadores a misma distancia se resuelven comparando sus respectivos atributos delimitados en sus PACSS. Ademas los partidos estan divididos en 4 tiempos con 3 pausas (2 cooling breaks y 1 entretiempo), durante cada pausa el usuario tiene permitido la sustitucion de un jugador, la cual no es acumulativa, es decir, si no realiza la sustitucion ese cambio se pierde. Ademas el usuario podra ver ademas de sus jugadores como sus comportamientos, los comportamientos del rival.

- Sistema De Ligas y Amistosos: El usuario dispondra de la opcion de creacion de ligas tanto publicas en la cual cualquier usuario con su club podra unirse respetando las condiciones de ingreso de la misma, como tambien privadas que estas para ingresar debera el usuario de disponer una contraseña. En el desarrollo de la liga habran fixtures que decidiran que clubes se enfrentan y en la liga habra un sistema de puntos que decidiran que club es el ganador de la liga y en caso de empate de puntos el ganador se decide a traves de goles a favor y en contra. Otro detalle es que el usuario ya sea en el transcurso de la liga o no, este puede jugar partidos amistosos con otros Usuarios con los jugadores que esten en disponibilidad en ese momento. 

- Visualizacion de partidos: Ademas la plataforma dispondra de la opcion de que los partidos que se jueguen en ese momento puedan ser visualizados por los usuarios que son enfrentados pero ademas podra tener espectadores que seran los demas usuarios.

- Ranking Global: Tabla general de posiciones que clasifica y ordena a todos los usuarios de la plataforma en función de los puntos sumados a lo largo de todas las ligas disputadas

## Requisitos Funcionales: 
 
- El sistema debe asociar un unico club a la cuenta de cada usuario registrado.
- El sistema debe rechazar el registro de un usuario si el email ingresado ya esta asociado a una cuenta existente.
- El sistema debe rechazar el inicio de sesion si el email o la contraseña ingresados no coinciden con los registrados.
- El sistema debe rechazar la creacion de un jugador si algun atributo individual (Power, Agility, Control, Speed, Strength) es menor a 20 o mayor a 100.
- El sistema debe abortar el guardado de un jugador si la sumatoria de sus 5 atributos no es exactamente igual a 300.
- El sistema debe asignar un comportamiento por defecto a todo jugador recien creado, el cual puede ser reemplazado posteriormente por el usuario.
- El sistema debe recibir y almacenar el codigo Python de los comportamientos asociado al usuario creador.
- El sistema debe validar tanto la sintaxis como las instrucciones del codigo python desarrollado por el usuario antes de incorporar el comportamiento
- El sistema debe rechazar cualquier comportamiento que el usuario intente agregar si el codigo python tiene errores de sintaxis o intenta modificar o intervenir el funcionamiento de la plataforma.
- El sistema debe bloquear cualquier intento de modificacion, creacion o eliminacion de codigo de comportamiento mientras el usuario tenga un partido en curso.
- El sistema debe verificar que la contraseña ingresada coincida de manera segura para autorizar el ingreso de un club a una liga privada.
- El sistema debe validar que un club cuente con exactamente 6 jugadores creados antes de permitir su inscripcion a una liga.
- El sistema debe rechazar la inscripcion de un club a una liga si la cantidad de equipos inscriptos ya alcanzo el maximo definido al crear la liga.
- El sistema debe otorgar los puntos determinados dependiendo el resultado del partido (3 puntos por victoria, 1 punto por empate y 0 por derrota) a los 2 clubes.
- El sistema debe calcular y actualizar automaticamente la tabla de posiciones (puntos y diferencia de goles) inmediatamente despues de que un partido de liga finalice.
- El sistema debe permitir instanciar un partido amistoso sin requerir la asociacion a una liga existente.
- El motor de juego debe invertir el movimiento de la pelota al detectar que sus coordenadas chocan con los limites definidos del campo.
- El sistema debe procesar las ordenes de sustitucion unicamente si el estado actual del partido esta detenido (cooling break o entretiempo).
- El sistema debe descontar y eliminar la posibilidad de sustitucion de un equipo para una pausa especifica si el usuario no envia la orden antes de reanudar el reloj.

## Restricciones del Sistema:

- El servidor del sistema debe implementarse obligatoriamente utilizando FastAPI y SQLAlchemy. 
- La interfaz de usuario (aplicacion web) debe desarrollarse exclusivamente en React.
- Queda estrictamente prohibida la utilizacion de tecnicas de polling para la comunicacion cliente-servidor y la actualizacion de los partidos en vivo. La actualizacion en tiempo real de los partidos debe implementarse mediante WebSockets. 
- El sistema debe contar con un entorno de ejecucion seguro y aislado (sandbox) que restrinja los recursos y bloquee operaciones de sistema en el codigo Python subido por los usuarios.
- El sistema debe persistir el estado de jugadores, comportamientos, ligas y resultados de partidos en la base de datos, de forma que no se pierda informacion ante una caida del servidor.

## Comportamiento del Sistema Autenticacion y Panel Principal: 

- Registro e Inicio de Sesion: Un nuevo usuario debe registrarse en la plataforma antes de poder acceder a cualquier funcionalidad (ver Requisitos Funcionales para las condiciones de validez del registro). Una vez autenticado, el sistema lo redirige a su panel principal, donde encuentra un resumen de su club, su historial de partidos y las ligas en las que participa activamente.

- Creacion de Club y Jugadores: Una vez registrado, el sistema le permite al usuario crear un club personalizable. A partir de ese club, el usuario puede crear jugadores (con sus atributos y comportamiento asignado, ver Requisitos Funcionales) y armar el plantel necesario para unirse a una liga o disputar amistosos.

- Dinamica del Partido: Iniciado un partido, la simulacion avanza de forma continua hasta alcanzar el final de cada uno de los 4 tiempos. Al llegar a una pausa, el sistema habilita las acciones de sustitucion para ambos equipos (ver Requisitos Funcionales para las condiciones de estas acciones); al finalizar la pausa, dichas acciones se deshabilitan y la simulacion se reanuda automaticamente. Este ciclo se repite hasta completar los 4 tiempos, momento en el cual el partido finaliza.

- Ligas y Amistosos: El usuario puede crear una liga definiendo sus parametros (nombre, duracion de partido, cantidad de equipos), o unirse a una liga existente que cumpla con las condiciones de ingreso. De forma independiente a las ligas, el usuario tambien puede disputar partidos amistosos con los jugadores que tenga disponibles en ese momento.

- Cierre de Partido, Tabla de Posiciones y Ranking Global: Al finalizar un partido, el sistema comunica el resultado a los usuarios y espectadores involucrados. Si el partido pertenece a una liga, el sistema actualiza la tabla de posiciones correspondiente (ver Requisitos Funcionales para el cálculo de puntos) y refleja el resultado en el ranking global, que acumula el desempeño de cada usuario a traves de todas las ligas en las que participa.





          

        
