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

### 2.2 Justificación de cada elemento de equipo activo

- **Switch de departamento (x8):** concentra las conexiones de los hosts (PCs, laptops, servidores) de cada área en un único punto, para que compartan un solo enlace troncal hacia el MDF en lugar de que cada host tuviera su propio cable individual atravesando todo el edificio. Esto reduce la cantidad de cable troncal necesario y facilita el mantenimiento por segmento.
- **Switch Central (MDF):** agrega el tráfico que viene de los 8 switches de departamento y sirve como punto único de interconexión de toda la red interna antes de salir hacia el router/firewall perimetral. Es el equipo que permite que todos los departamentos se comuniquen entre si y con el exterior.
- **Patch Panel (48 puertos):** actúa como punto de terminación fijo y organizado para todos los cables troncales que llegan al MDF, separando físicamente el cableado de infraestructura (fijo, hacia las paredes) de las conexiones activas (patch cords cortos hacia el switch central), lo que facilita el mantenimiento y evita manipular directamente el cableado empotrado cada vez que se reconfigura una conexión.

### 2.3 Dispositivos Finales (Hosts)

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

---

## 5. Cableado Horizontal: Tipo y Categoría de Cable

Se eligió **cable UTP Categoría 6 (Cat6)** para todo el cableado horizontal del edificio (switch de departamento ↔ hosts), por las siguientes razones:

- Soporta hasta 1 Gbps (Gigabit Ethernet) en distancias de hasta 100 metros, y hasta 10 Gbps en distancias cortas (55m), muy por encima de las distancias máximas encontradas en cualquier punto del edificio.
- Mejor blindaje contra interferencia (diafonía/crosstalk) que categorías inferiores (Cat5e), lo cual es importante en departamentos con alta densidad de puestos como RRHH y Capacitación.
- Costo moderado y ampliamente disponible en el mercado local, con suficiente margen de escalabilidad para futuras necesidades de ancho de banda sin requerir recableado a corto/mediano plazo.

No se necesita fibra óptica en el cableado horizontal ya que las distancias dentro de cada departamento (máximo 10m por punto) están por debajo del límite de 100m del cobre UTP, y no existe justificación de costo/beneficio para introducir fibra en enlaces tan cortos.

---

## 6. Estimación de Distancias y Cálculo de Bobinas

Las distancias se estimaron con base en la escala del plano arquitectónico (28mx21m), considerando trayectos ortogonales (siguiendo pasillos y paredes y no líneas rectas en diagonal), más un margen del 10% por holgura en terminaciones y curvas de instalación.

### 6.1 Cableado Horizontal (switch de departamento ↔ hosts)

| Departamento | Puntos de red | Distancia total estimada |
|---|---|---|
| Recepción | 4 | 28 m |
| Recursos Humanos | 8 | 48 m |
| Legal | 4 | 24 m |
| Sala de Capacitación | 10 | 80 m |
| Diseño e Innovación | 8 | 48 m |
| Dirección General | 4 | 20 m |
| Backend | 7 | 42 m |
| Data Center | 3 | 12 m |
| **Subtotal horizontal** | **48** | **302 m** |

### 6.2 Cableado Troncal (MDF ↔ switch de cada departamento)

| Enlace | Distancia estimada (incluye margen vertical y 10% de holgura) |
|---|---|
| MDF-Recepción | 31 m |
| MDF-RRHH | 23 m |
| MDF-Legal | 17 m |
| MDF-Capacitación | 9 m |
| MDF-Diseño | 25 m |
| MDF-Dirección | 19 m |
| MDF-Backend | 12 m |
| MDF-DataCenter | 8 m |
| **Subtotal troncal** | **144 m** |

### 6.3 Cálculo de Bobinas

- Total general de cable requerido: 302 m (horizontal) + 144 m (troncal) = **446 metros**
- Bobina estándar: 305 metros
- Bobinas necesarias: 446 ÷ 305 = 1.46 → **se requieren 2 bobinas** (610 m disponibles), dejando un remanente de 164m aproximadamente, como contingencia para pérdidas por corte, pruebas de certificación y posibles reinstalaciones.

---

## 7. Cableado Troncal: Medio de Transmisión

Se eligió **cable UTP Categoría 6 (Cat6)** también para el cableado troncal (MDF ↔ switches de departamento), con la misma justificación de categoría que en el cableado horizontal.

La distancia troncal más larga estimada (MDF-Recepción tiene 31 m) sigue estando por debajo del límite de 100 m del estándar UTP, por lo que **no es necesario utilizar fibra óptica** para ningún enlace troncal ya que:
- Todos los switches de departamento se conectan a velocidades de uplink de 1 Gbps, y un cable UTP Cat6 puede soportar perfectamente esas distancias.
- El costo de instalar fibra (transceptores SFP, empalmes, personal certificado) no se justifica para un edificio de un solo nivel con distancias cortas.
- Como consideración futura: en el caso de que el edificio se expanda a más niveles o que las distancias troncales aumentaran por encima de 90-100 m, se recomendaría migrar a fibra óptica multimodo para los enlaces troncales.

---

## 8. Estándares T568A/T568B: Straight-Through y Crossover

### 8.1 Straight-Through (cableado horizontal)

Todos los enlaces de cableado horizontal (hosts ↔ switch de departamento) utilizan cables **straight-through**, ponchados bajo el estándar **T568B en ambos extremos** (toma de red y patch panel/switch), ya que los dispositivos conectados son de tipos diferentes (host y switch), que es el caso de uso estándar para este tipo de cable.

### 8.2 Enlaces Troncal (switch de departamento ↔ switch central)

Los enlaces troncales conectan switch con switch (mismo tipo), lo cual tradicionalmente requeriría un cable **crossover**. Pero los switches modernos cuentan con la función **Auto-MDI/MDIX**, que detecta automáticamente el tipo de conexión y ajusta la señal internamente. Por eso **se utilizan cables straight-through también en los enlaces troncales**, lo que simplifica el inventario de cableado teniendo un solo tipo de cable para todo el edificio sin sacrificar funcionalidad.

### 8.2.1 Tabla de tipo de cable por enlace

| Enlace | Tipo de cable | Justificación |
|---|---|---|
| MDF-Recepción | Straight-through | Switch-switch con Auto-MDIX |
| MDF-RRHH | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Legal | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Capacitación | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Diseño | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Dirección | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Backend | Straight-through | Switch-switch con Auto-MDIX |
| MDF-Data Center | Straight-through | Switch-switch con Auto-MDIX |
| Todos los enlaces horizontales (host ↔ switch) | Straight-through | Host-switch, uso estándar del cable |

La disposición de pines de ambos tipos de cable es la siguiente:

### 8.3 Disposición de Pines — Cable Straight-Through (T568B en ambos extremos)

| Pin | Color (Extremo A) | Color (Extremo B) |
|---|---|---|
| 1 | Blanco/Naranja | Blanco/Naranja |
| 2 | Naranja | Naranja |
| 3 | Blanco/Verde | Blanco/Verde |
| 4 | Azul | Azul |
| 5 | Blanco/Azul | Blanco/Azul |
| 6 | Verde | Verde |
| 7 | Blanco/Café | Blanco/Café |
| 8 | Café | Café |

### 8.4 Disposición de Pines — Cable Crossover (T568A en un extremo, T568B en el otro)

| Pin | Color (Extremo A – T568A) | Color (Extremo B – T568B) |
|---|---|---|
| 1 | Blanco/Verde | Blanco/Naranja |
| 2 | Verde | Naranja |
| 3 | Blanco/Naranja | Blanco/Verde |
| 4 | Azul | Azul |
| 5 | Blanco/Azul | Blanco/Azul |
| 6 | Naranja | Verde |
| 7 | Blanco/Café | Blanco/Café |
| 8 | Café | Café |

### 8.5 Estándar aplicado al cableado horizontal

Todo el cableado horizontal se poncha bajo el **mismo estándar (T568B)** en ambos extremos: en la toma de red del punto final y en el patch panel del switch de cada departamento.

---

## 9. Ruta de Cableado (Canalización)

Se propone el uso de **escalerilla metálica cerrada (canaleta tipo bandeja con tapa)**, tendida sobre el cielo falso del edificio, siguiendo la ruta de la pared divisoria central (entre la fila superior de departamentos y la fila inferior), con perforaciones controladas en los muros para el paso de la canalización entre segmentos ("paso de muro").

Se eligió canalización **cerrada** en lugar de abierta ya que es un edificio corporativo con oficinas administrativas y espacios de atención al público, donde se prioriza la estética y la protección del cableado contra polvo y manipulación accidental, sobre la mayor facilidad de acceso que ofrece una escalerilla abierta.

---

## 10. Gabinete o Rack para el MDF

Se propone un **gabinete de pared de 12 UR** para alojar el equipo del MDF, en lugar de un rack de piso, por las siguientes razones:

- El Data Center (4x7m) es un espacio reducido y un rack de piso ocuparía espacio innecesario para la cantidad de equipo a alojar.
- El equipo activo a instalar es sencillo: 1 switch central (1-2 UR), 1 patch panel de 48 puertos (2 UR), 1 UPS de montaje en rack (2 UR), dejando un margen aplio de expansión dentro de las 12 UR disponibles.
- Los gabinetes de pared tienen buena ventilación, puerta con cerradura para seguridad, y son más económicos que un rack de piso de 42U, que estaría sobredimensionado para este caso.

---

## 11. Respaldo de Energía (UPS)

### 11.1 Estimación de consumo eléctrico

| Equipo | Cantidad | Consumo aprox. unitario | Subtotal |
|---|---|---|---|
| Switch 8 puertos (no administrado) | 7 | 8 W | 56 W |
| Switch 16 puertos (Capacitación) | 1 | 15 W | 15 W |
| Switch Central 48 puertos (administrado) | 1 | 45 W | 45 W |
| **Total estimado** | | | **116 W** |

### 11.2 Capacidad de UPS recomendada

Con un consumo activo estimado de 116 W aproximadamente y considerando margen de crecimiento futuro, se recomienda un **UPS de 1000 VA / 600 W** lo cual:
- Ofrece amplio margen sobre el consumo actual (factor de carga ~20%), permitiendo escalabilidad sin necesidad de reemplazo a corto plazo.
- Brinda autonomía suficiente (20-30 minutos con la carga actual) para un apagado controlado del equipo activo en caso de corte eléctrico o para cubrir cortes breves sin interrupción del servicio.

---

## 12. Etiquetado de Cableado y Comparación con TIA/EIA-606

### 12.1 Formato de etiquetado utilizado

- **Cableado horizontal:** `[Departamento]-[Número de Punto de Red]` — Ejemplo: `Recepcion-PR01`, `RRHH-PR05`.
- **Cableado troncal:** `MDF-[Departamento]` — Ejemplo: `MDF-Backend`, `MDF-Capacitacion`.

### 12.2 Tabla de etiquetado de cables

| Departamento | Etiquetas de cableado horizontal | Etiqueta de cableado troncal |
|---|---|---|
| Recepción | Recepcion-PR01 a Recepcion-PR04 | MDF-Recepcion |
| Recursos Humanos | RRHH-PR01 a RRHH-PR08 | MDF-RRHH |
| Legal | Legal-PR01 a Legal-PR04 | MDF-Legal |
| Sala de Capacitación | Capacitacion-PR01 a Capacitacion-PR10 | MDF-Capacitacion |
| Diseño e Innovación | Diseno-PR01 a Diseno-PR08 | MDF-Diseno |
| Dirección General | Direccion-PR01 a Direccion-PR04 | MDF-Direccion |
| Backend | Backend-PR01 a Backend-PR07 | MDF-Backend |
| Data Center | DataCenter-PR01 a DataCenter-PR03 | MDF-DataCenter |

### 12.3 Comparación con el estándar TIA/EIA-606

El estándar **TIA/EIA-606** (Administración de Infraestructura de Telecomunicaciones) exige un sistema de identificación más completo que el esquema simplificado usado en esta práctica. Tiene 2 diferencias concretas:

1. **Identificadores únicos jerárquicos:** TIA/EIA-606 exige que cada elemento (cable, panel, puerto, espacio, ruta) tenga un identificador único que incluya información de ubicación jerárquica (edificio, piso, cuarto de telecomunicaciones, rack, posición), mientras que en esta práctica solo se identifica departamento y número de punto, sin codificación de piso o edificio.
2. **Documentación de registros y administración:** El estándar exige el mantenimiento formal de registros (tablas de administración, planos "as-built", historial de cambios) para cada elemento etiquetado, incluyendo codificación por colores para distinguir los tipos de circuito. En esta práctica solamente fue una etiqueta visual sobre el diagrama sin un sistema de registro documentado independiente.

En un entorno real se optaría por el estándar completo TIA/EIA-606 en lugar de este esquema simplificado, ya que a medida que la infraestructura crece (más pisos, más racks y personal de mantenimiento rotativo), la trazabilidad completa de cada cable es crítica para minimizar tiempos de diagnóstico de fallas y evitar errores de reconexión, algo que un esquema simplificado no puede garantizar a gran escala.

---

## 13. Flujo de Conexión End-to-End

Por ejemplo, al describir el flujo completo de conexión del punto **RRHH-PR01** (una PC de escritorio en Recursos Humanos) hasta la salida a Internet/red externa:

1. La PC se conecta mediante un cable UTP Cat6 straight-through a la toma de red **RRHH-PR01**.
2. Desde la toma de red, el cableado horizontal recorre la canalización interna del departamento hasta el **SW RRHH**.
3. El **SW RRHH** se conecta mediante un cable UTP Cat6 troncal (**MDF-RRHH**) a través de la canalización de escalerilla cerrada cruzando el "paso de muro" hacia la pared divisoria central.
4. El cable troncal llega al **Patch Panel** dentro del gabinete de pared en el MDF en Data Center, donde se poncha bajo el mismo estándar T568B.
5. Desde el Patch Panel, un cable de parcheo (patch cord) corto conecta al puerto correspondiente del **Switch Central**.
6. El Switch Central agrega el tráfico de los 8 switches de departamento y lo entrega hacia el router/firewall perimetral (Capa 3), desde donde se conecta al proveedor de Internet.

---

## 14. Presupuesto Estimado

| Concepto | Cantidad | Precio unitario (Q) | Subtotal (Q) |
|---|---|---|---|
| Switch 8 puertos (no administrado) | 7 | Q 450.00 | Q 3,150.00 |
| Switch 16 puertos (Capacitación) | 1 | Q 900.00 | Q 900.00 |
| Switch Central 48 puertos (administrado) | 1 | Q 4,500.00 | Q 4,500.00 |
| Patch Panel 48 puertos | 1 | Q 650.00 | Q 650.00 |
| Bobina de cable UTP Cat6 (305 m) | 2 | Q 750.00 | Q 1,500.00 |
| Conectores RJ45 Cat6 (paquete de 100) | 2 | Q 150.00 | Q 300.00 |
| Tomas/jacks de red Cat6 | 48 | Q 35.00 | Q 1,680.00 |
| Placas/faceplates (unitaria/doble/N puertos) | — | — | Q 500.00 |
| Gabinete de pared 12 UR | 1 | Q 1,800.00 | Q 1,800.00 |
| UPS 1000 VA / 600 W | 1 | Q 1,400.00 | Q 1,400.00 |
| Escalerilla metálica cerrada (~50 m de recorrido troncal) | 50 m | Q 80.00 | Q 4,000.00 |
| Herramientas de instalación (ponchadora, tester de cable) | — | — | Q 800.00 |
| Mano de obra de instalación (estimado) | — | — | Q 3,000.00 |
| **TOTAL ESTIMADO** | | | **Q 24,180.00** |


### 14.1 Compra individual vs. proveedor externo

Se recomienda trabajar con un **proveedor/integrador externo especializado** en cableado estructurado para la instalación física (canalización, ponchado, certificación de enlaces), en lugar de la compra individual de materiales por parte de QuetzalDev S.A. ya que:
- Garantiza certificación de los enlaces conforme a estándares TIA/EIA (prueba de continuidad, diafonía, atenuación).
- Reduce el riesgo de errores de instalación que podrían no detectarse hasta la puesta en producción.
- El monto total estimado (~Q24,000) es razonable para contratar un servicio llave en mano con garantía, en lugar de asumir el riesgo de una instalación no certificada.

---

## 15. Consideraciones de Escalabilidad Futura

- El Switch Central (48 puertos) y el Patch Panel (48 puertos) cubren exactamente los 48 puntos de red actuales, sin puertos libres, así que se recomienda considerar equipo con 4-8 puertos adicionales en una revisión futura del presupuesto para permitir crecimiento sin reemplazo inmediato de equipo.
- El gabinete de pared de 12 UR deja espacio disponible (ocupado: 5-6 UR entre switch central, patch panel y UPS) para incorporar equipo adicional como un segundo patch panel o un organizador de cables.
- La Sala de Capacitación al usar tomas de N puertos por mesa, permite agregar más laptops en el futuro sin necesidad de recableado, solo ampliando el switch local si se supera la capacidad de 16 puertos.
