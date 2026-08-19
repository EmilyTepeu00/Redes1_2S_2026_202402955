# Manual Técnico — Práctica 1
## QuetzalDev S.A.

---

## 1. Introducción

Este manual documenta el diseño de la infraestructura física de Capa 1 del Modelo OSI para el edificio corporativo de QuetzalDev S.A., empresa de desarrollo de software. El diseño muestra la distribución de cableado estructurado, selección de topología física, medios de transmisión, ubicación del cuarto de telecomunicaciones (MDF) y más elementos requeridos para garantizar orden, escalabilidad y cumplimiento de estándares TIA/EIA.

---

## 2. Inventario de Equipos

### 2.1 Equipo Activo

| Cantidad | Equipo | Ubicación | Especificación |
|---|---|---|---|
| 8 | Switch de departamento | Recepción, RRHH, Legal, Capacitación, Diseño e Innovación, Dirección General, Backend, Data Center | 8 puertos (16 puertos en Capacitación, por sus 10 puntos de red) |
| 1 | Switch Central | MDF (Data Center) | 48 puertos o más |
| 1 | Patch Panel | MDF (Data Center) | 48 puertos |
| 1 | Rack/Gabinete | MDF (Data Center) | Dimensionado según equipo a alojar |
| 1 | UPS | MDF (Data Center) | Capacidad por definir |

### 2.2 Dispositivos Finales (Hosts)

| Departamento | PCs Escritorio | Laptops | Servidores | Total Puntos de Red |
|---|---|---|---|---|
| Recepción | 2 | 1 | 1 | 4 |
| Recursos Humanos | 8 | 0 | 0 | 8 |
| Legal | 2 | 2 | 0 | 4 |
| Sala de Capacitación | 1 | 9 | 0 | 10 |
| Diseño e Innovación | 7 | 0 | 1 | 8 |
| Dirección General | 4 | 0 | 0 | 4 |
| Backend | 6 | 0 | 1 | 7 |
| Data Center | 0 | 0 | 3 | 3 |
| **TOTAL** | **30** | **12** | **6** | **48** |

---

## 3. Ubicación del Cuarto de Telecomunicaciones (MDF)

El Cuarto de Telecomunicaciones (MDF) es la sala central del edificio, aquí es donde se reciben todas las líneas externas de los proveedores de Internet y telefonía, y desde aquí se distribuye la señal hacia otros pisos o cuartos secundarios llamados IDF. Se decidió ubicarlo en la sala designada como **Data Center** (4m x 7m), en la esquina inferior derecha del edificio.

Este espacio no representa el punto geométricamente más central del edificio respecto a todos los puntos de red, pero se prioriza esta ubicación por ser el único espacio del plano arquitectónico ya destinado a uso técnico, con las condiciones de seguridad y control de acceso adecuadas para alojar equipo crítico de red. Adicionalmente aloja los 3 servidores principales de la organización, lo cual reduce la necesidad de cableado adicional entre el MDF y el Data Center.

La canalización troncal se diseñó siguiendo la pared divisoria central del edificio, minimizando así la distancia efectiva de recorrido hacia los departamentos más alejados.

---

## 4. Topología Física y Tipo de Toma de Red por Departamento

### 4.1 Consideración general sobre la representación gráfica

En el diagrama de diseño físico, las líneas de cableado horizontal se agrupan visualmente antes de llegar al switch de cada departamento (para garantizar la claridad y representar la canalización física conjunta). Sin embargo, cada punto de red mantiene un enlace individual y dedicado hacia su switch correspondiente por lo que **la topología física real es en estrella en todos los departamentos** y no en bus como se puede visualizar gráficamente. La agrupación visual de las líneas representa únicamente que los cables comparten el mismo conducto/canaleta durante su recorrido, no que compartan la señal eléctrica.

### 4.2 Departamento de Recepción

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento de Recepción ya ¿ que cada uno de los 4 puntos de red (3 PCs/laptop y 1 servidor) requiere un enlace dedicado e independiente hacia el switch del departamento. Esta topología ofrece mayor tolerancia a fallos frente a una topología de bus, ya que la falla de un cable individual no afecta la conectividad de los demás dispositivos, además de facilitar el diagnóstico y mantenimiento al aislar cada segmento.

**Tipo de toma de red:** Unitaria (4 puntos individuales).

### 4.3 Departamento de Recursos Humanos

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento de Recursos Humanos, con 8 puntos de red organizados en 4 módulos de trabajo compartido (2 estaciones por módulo). Dado el alto número de hosts en un espacio relativamente compacto, se recomienda el uso de tomas de red dobles en cada módulo, optimizando la instalación sin comprometer la independencia de cada enlace hacia el switch.

**Tipo de toma de red:** Doble (4 módulos de 2 estaciones).

### 4.4 Departamento Legal

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento Legal, con 4 puntos de red distribuidos de forma individual (2 PCs de escritorio en zonas separadas, 2 laptops en área de trabajo colaborativo). Dada la distribución dispersa de los puestos, se recomienda el uso de tomas de red unitarias en cada punto, para garantizar la independencia total de cada enlace sin necesidad de compartir infraestructura de toma.

**Tipo de toma de red:** Unitaria (4 puntos individuales).

### 4.5 Sala de Capacitación

**Topología:** Estrella

Se seleccionó topología en estrella para la Sala de Capacitación que cuenta con 10 puntos de red: 9 laptops distribuidas en 3 mesas de trabajo (3 puestos por mesa) y 1 PC de escritorio independiente. Para las mesas de trabajo se recomienda el uso de tomas de red de 3 puertos (N puertos) por mesa, optimizando la instalación en un espacio de uso compartido y facilitando la reconfiguración de la sala según las necesidades de capacitación. El punto de la PC de escritorio (PR10) utiliza una toma unitaria independiente.

**Tipo de toma de red:** Triple/N puertos (3 mesas de 3) + unitaria (PC10).

### 4.6 Departamento de Diseño e Innovación

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento de Diseño e Innovación que cuanta con 8 puntos de red: 6 PCs de escritorio distribuidas en 2 mesas de trabajo colaborativo (3 puestos por mesa), 1 PC individual (PR07) y 1 servidor local (PR08). Para las mesas de trabajo se recomienda el uso de tomas de red de 3 puertos (N puertos) por mesa, dado que este departamento integra funciones de Diseño UI/UX, Data Analytics y Laboratorio de Pruebas QA, que suelen requerir configuraciones de trabajo colaborativo. El punto individual (PR07) y el del servidor (PR08) utilizan tomas unitarias independientes.

**Tipo de toma de red:** Triple/N puertos (2 mesas de 3) + unitaria (PR07, PR08).

### 4.7 Dirección General

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento de Dirección General, con 4 puntos de red organizados en 2 módulos de 2 estaciones cada uno. Se recomienda el uso de tomas de red dobles por módulo, consistente con el criterio aplicado en departamentos con distribución de escritorios pareados, para optimizar la instalación sin comprometer la independencia de cada enlace hacia el switch.

**Tipo de toma de red:** Doble (2 módulos de 2 estaciones).

### 4.8 Departamento de Backend

**Topología:** Estrella

Se seleccionó topología en estrella para el Departamento de Backend, con 7 puntos de red: 6 PCs de escritorio distribuidas en 2 mesas de trabajo (3 puestos por mesa) y 1 servidor local (PR07). Para las mesas de trabajo se recomienda el uso de tomas de red de 3 puertos (N puertos) por mesa, dado el trabajo colaborativo típico de un equipo de desarrollo backend. El punto del servidor (PR07) utiliza una toma unitaria independiente.

**Tipo de toma de red:** Triple/N puertos (2 mesas de 3) + unitaria (servidor).

### 4.9 Data Center

**Topología:** Estrella

Se seleccionó topología en estrella para el Data Center, con 3 puntos de red correspondientes a los servidores principales de la organización. A diferencia de otros departamentos, se recomienda el uso de tomas de red unitarias e independientes para cada servidor, sin agrupación en tomas múltiples, dado el alto nivel de criticidad de este segmento: una falla física en una toma compartida podría afectar múltiples servidores simultáneamente, lo cual es inaceptable para infraestructura crítica. Esta decisión prioriza la tolerancia a fallos sobre la optimización de costos de instalación ya que este segmento aloja los servicios centrales de la empresa.

**Tipo de toma de red:** Unitaria (por criticidad).

### 4.10 Resumen de tomas de red por departamento

| Departamento | Tipo de toma |
|---|---|
| Recepción | Unitaria (4 puntos individuales) |
| Recursos Humanos | Doble (4 módulos de 2) |
| Legal | Unitaria (4 puntos individuales) |
| Sala de Capacitación | Triple/N puertos (3 mesas de 3) + unitaria (PC10) |
| Diseño e Innovación | Triple/N puertos (2 mesas de 3) + unitaria (PR07, PR08) |
| Dirección General | Doble (2 módulos de 2) |
| Backend | Triple/N puertos (2 mesas de 3) + unitaria (servidor) |
| Data Center | Unitaria (por criticidad) |
