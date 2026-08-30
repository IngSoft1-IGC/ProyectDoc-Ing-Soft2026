### USE CASE 24: VER RANKING

### PRIMARY ACTOR: 
Usuario

### PRECONDITION:
1. El Usuario esta logueado

### Succesful scenarios:
1. El Usuario hace click en el panel de Ranking 
2. El Sistema responde con una lista de ranking de los equipos
3. El Usuario elije si quiere ver ranking de equipos o ligas, y hace click en el panel correspondiente

### ALTERNATIVE SCENARIOS:
3. El Usuario no hace click en panel
Flow: El Sistema no despliega la lista de rankings

### Execeptional scenarios: 
1. **Error de conexion a Internet**
Flow: Muestra un mensaje de error

### Poscondition:
1. El Usuario puede ver los rankings de los equipos o las ligas

