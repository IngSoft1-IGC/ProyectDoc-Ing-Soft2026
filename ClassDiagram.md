```mermaid
classDiagram
    class Usuario {
        +int id
        +string nickname
        +string email
        +string password_ruido
        +string avatar

        +registrarse()
        +iniciarSesion()
        +gestionarPerfil()
    }

    class Equipo {
        +int id
        +String nombre
        +List~(Jugador,Comportamiento)~ JugadoresTitulares
        +List~(Jugador,Comportamiento)~ JugadoresSuplentes
        

        +crearEquipo()
        +unirseALiga()
        +editarEquipo()
        +eliminarEquipo()
    }
    
    class Historial{
        +int ligasGanadas
        +int puntosTotales
        +int partidosGanados
        +int partidosPerdidos
        +int partidosEmpatadas
    }

    class Jugador {
        +int id
        +int dorsal
        +String nombre
        +int power
        +int agility
        +int control
        +int speed
        +int strength

        +validarPACSS()
    }

    class Comportamiento {
        +int id
        +string nombre
        +String codigoPython

        +crearcomportamiento()
        +validarSintaxis()
        +validarSeguridad()
        +editarComportamiento()
        +eliminarComportamiento()
    }

    class Liga {
        +int id
        +String nombre
        +bool esPrivada
        +String contraseña
        +int minEquipos
        +int maxEquipos
        +int duracionPartido
        +int fechaInicio
        +List~Equipos~ inscritos

        +crearLiga()
        +iniciarLiga()
        +cancelarLiga()
    }

    class TablaPosiciones {
        +List~Equipos~ inscritos
        +string orden

        +actualizar()
        +resetear()
    }

    class RankingGlobal {
        +List~Usuario~ usuarios
        +String orden

        +actualizarRanking()

    }

    class Partido {
        +int id
        +bool estado 
        +string resultado

        +iniciarPartido()
        +procesarSustitucion()
        +finalizarPartido()
    }

    class Resultado {
        +int golesEquipo1
        +int golesEquipo2

        +quienGano()
        +esEmpate()
    }

    Usuario "1" -- "1" Equipo : posee
    Usuario "1" --> "*" Comportamiento : crea
    Usuario "*" --> "1" Partido: Observa
    Usuario "1" --> "*" Jugador: crea

    Equipo "1" o-- "6" Jugador : conforma
    Equipo --> Historial
    Jugador "*" --> "1" Comportamiento : ejecuta

    Liga "1" o-- "*" Equipo : inscribe
    Liga "1" *-- "1" TablaPosiciones : gestiona
    Liga "1" *-- "*" Partido : programa

    Partido "*" --> "2" Equipo : disputan

    Partido --> Resultado: tendra

    Usuario --> RankingGlobal: compuesta
    
```
