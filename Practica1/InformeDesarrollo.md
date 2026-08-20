# Informe de Desarrollo — Práctica 1
## QuetzalDev S.A.

---

## 1. Proceso de Diseño

El diseño de la infraestructura física de red para QuetzalDev S.A. se hizo siguiendo las siguientes decisiones: primero la ubicación del cuarto de telecomunicaciones (MDF), después la distribución de los 48 puntos de red entre los 8 departamentos del edificio, luego la topología y tipo de toma por cada segmento, y finalmente el trazado del cableado horizontal y troncal sobre el plano proporcionado.

Se trabajó primero identificando, sobre el plano base, qué espacio del edificio ofrecía las mejores condiciones para alojar el cuarto de telecomunicaciones, considerando la centralidad geométrica y la disponibilidad de un espacio ya destinado a uso técnico (el Data Center). A partir de esa decisión se realizó la ruta de canalización troncal siguiendo la pared divisoria central del edificio ya que esta conecta ambas filas de departamentos directamente y ofrece el recorrido más corto y ordenado hacia el MDF.

Luego se distribuyeron las 30 PCs de escritorio, 12 laptops y 6 servidores entre los 8 departamentos, respetando los totales obligatorios que especifica el enunciado por cada switch, pero se decidió libremente qué proporción de cada departamento sería PC de escritorio o laptop, según el tipo de trabajo de cada área (por ejemplo más laptops en la sala de capacitación por su naturaleza de uso temporal y movible, y más PCs de escritorio en departamentos de trabajo fijo como Recursos Humanos y Backend).

## 2. Criterios Considerados para la Selección de Topología

Para todos los departamentos se seleccionó la **topología física en estrella**, priorizando la tolerancia a fallos sobre otras alternativas como bus o árbol.En una topología en estrella la falla de un cable individual no afecta la conectividad de los demás dispositivos del mismo segmento, lo cual es mejor para un entorno de oficina donde la continuidad operativa es importante.

Dentro de esa topología general, se diferenció el **tipo de toma de red** según la distribución física de los puestos observados en el plano:
- Puestos individuales o dispersos (Recepción, Legal) → tomas unitarias, priorizando independencia de cada enlace.
- Escritorios pareados (RRHH, Dirección General) → tomas dobles, aprovechando que los módulos ya comparten infraestructura física.
- Mesas de trabajo colaborativo de 3 puestos (Capacitación, Diseño, Backend) → tomas de N puertos (triples), reduciendo la cantidad de tomas individuales sin comprometer la independencia lógica de cada conexión.
- Data Center → tomas unitarias ya que se trata del segmento más crítico de la red, donde no es recomendable que varios servidores compartan infraestructura de toma.

## 3. Criterios para la Selección de Medios de Transmisión y Equipo Activo

Se seleccionó el cable **UTP Categoría 6** para el cableado horizontal y troncal, ya que ninguna distancia estimada en el edificio (máximo 31 metros en el enlace troncal más largo) se acerca al límite de 100 metros del cobre UTP. Esto descartó la necesidad de utilizar fibra óptica, que tiene un costo adicional (transceptores, empalmes, personal certificado) y no se justificaba un gasto innecesario para un edificio de un solo nivel con distancias tan cortas.

Para el equipo activo, se dimensionó cada switch de departamento según la cantidad exacta de puntos de red de esa área, y se definió un switch central y patch panel de 48 puertos para cubrir el total exacto de puntos de red del edificio, siguiendo el criterio que exige el enunciado de que el switch tenga capacidad igual o mayor a la del patch panel.

## 4. Retos de Planificación Física al Interpretar el Plano Base

El principal reto fue decidir cómo representar el cableado troncal de forma realista sin caer en simplificaciones incorrectas. Inicialmente se consideró trazar líneas rectas en diagonal entre cada switch y el MDF, pero eso no reflejaba cómo se instala cableado real ya que el cable no puede atravesar paredes libremente sin una ruta de canalización definida. Esto llevó a rediseñar el trazado troncal como una única ruta continua (backbone) corriendo sobre la pared divisoria central del edificio, con puntos de perforación controlados (paso de muro) donde la canalización cruza de una habitación a otra lo cual es la solución que representa de forma más sencilla una instalación de escalerilla metálica real.

Otro reto fue decidir la ubicación óptima del MDF: el punto geométricamente más central del edificio no coincidía con ningún espacio técnico apropiado (caía dentro de la oficina de Dirección General). Así que se optó por priorizar el Data Center como ubicación del MDF, aceptando una distancia troncal mayor hacia los departamentos más alejados (Recepción y Recursos Humanos), pero ganando en seguridad y disponibilidad de un espacio ya destinado a uso técnico, y con esta decisión se prioriza practicidad sobre optimización matemática pura.

Finalmente distribuir las 30 PCs y 12 laptops entre los departamentos, cuando el plano original no mostraba mobiliario suficiente para todos los puestos requeridos, implicó adaptar el plano agregando escritorios y equipo donde no había mobiliario dibujado, manteniendo la estructura de las habitaciones intacta.

## 5. Justificación del Medio Utilizado en el Cableado Troncal

Se utilizó cable **UTP Categoría 6** para todos los enlaces troncales (MDF ↔ switch de cada departamento), en lugar de fibra óptica, por las siguientes razones:

- La distancia troncal máxima estimada (31m hacia Recepción) está por debajo del límite de 100 metros que soporta el cobre UTP para Gigabit Ethernet.
- Todos los switches de departamento requieren una velocidad de uplink de 1 Gbps, y el Cat6 soporta perfectamente esas distancias.
- El costo de introducir fibra óptica (transceptores SFP, herramientas de fusión, personal certificado) no se justifica económicamente para un edificio de un solo nivel con recorridos tan cortos.
- Usar la misma categoría de cable (Cat6) tanto para horizontal como para troncal simplifica el inventario de materiales y la instalación, sin sacrificar el rendimiento.

Como consideración a futuro es en caso de que el edificio llegara a expandirse a más niveles o si las distancias troncales aumentaran significativamente, se recomendaría evaluar la migración a fibra óptica multimodo para los enlaces troncales, aunque para el alcance de esta práctica no es necesario.