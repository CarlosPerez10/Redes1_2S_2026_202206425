# Informe de Desarrollo
## Práctica 1 — Diseño de Infraestructura Física de Red · QuetzalDev S.A.

**Curso:** Redes de Computadoras 1 — Segundo Semestre 2026
**Estudiante:** Carlos Javier Pérez Pocón
**Carné:** 202206425

---

## Índice

1. [Introducción](#1-introducción)
2. [Proceso de diseño](#2-proceso-de-diseño)
3. [Criterios para la selección de topologías](#3-criterios-para-la-selección-de-topologías)
4. [Criterios para la selección de medios de transmisión](#4-criterios-para-la-selección-de-medios-de-transmisión)
5. [Criterios para la selección de equipo activo](#5-criterios-para-la-selección-de-equipo-activo)
6. [Justificación del medio utilizado en el cableado troncal](#6-justificación-del-medio-utilizado-en-el-cableado-troncal)
7. [Retos de planificación física encontrados](#7-retos-de-planificación-física-encontrados)
8. [Conclusiones](#8-conclusiones)

---

## 1. Introducción

La empresa QuetzalDev S.A. inauguró un edificio corporativo de un solo nivel destinado a albergar sus equipos de ingeniería, diseño y administración. Antes de adquirir equipo de red, la empresa requiere la planificación de la distribución física, el cableado estructurado y la topología más adecuada para cada área.

Este informe documenta el proceso seguido para diseñar la infraestructura de Capa 1 del modelo OSI a partir del plano arquitectónico base, asumiendo el rol de ingeniero de telecomunicaciones responsable del planteamiento físico de la red. El alcance comprende la ubicación del cuarto de telecomunicaciones, la definición de puntos de red y tipos de toma, la selección y justificación de topologías por segmento, la diferenciación entre cableado troncal y horizontal, el dimensionamiento del equipo activo y pasivo, y la documentación técnica conforme a los estándares TIA/EIA.

El trabajo es de **diseño**, no de configuración ni de simulación. No se define direccionamiento IP, VLANs ni protocolos de enrutamiento: esos elementos corresponden a capas superiores y a prácticas posteriores. Todo lo aquí planteado se limita a la infraestructura física que debe existir antes de que cualquier configuración lógica sea posible.

---

## 2. Proceso de diseño

El diseño se desarrolló en cinco etapas secuenciales, donde cada una dependía de los resultados de la anterior.

### 2.1 Interpretación del plano arquitectónico

La primera etapa consistió en extraer del plano la información relevante para el diseño de red, que no es la misma que resulta relevante para un arquitecto.

Se identificaron las dimensiones generales (28 m × 21 m) y la organización en tres franjas: una superior de 9 m de fondo con cuatro departamentos, una media de 7 m con cinco espacios, y una franja inferior de 5 m correspondiente al Área Abierta de circulación general.

El hallazgo determinante de esta etapa fue reconocer que **el Área Abierta constituye el único eje continuo del edificio**. Recorre los 28 m de ancho y comunica mediante puertas con todos los departamentos de la franja media. Esta observación condicionó todo el diseño posterior: definió la ruta de canalización troncal, influyó en la ubicación del MDF y determinó la posición de los switches dentro de cada departamento.

Se identificó también una asimetría relevante: los departamentos de la franja superior **no tienen acceso directo al Área Abierta**. Sus enlaces troncales requieren un tramo adicional de descenso por el muro divisorio entre franjas, lo que se incorporó posteriormente al cálculo de distancias.

Finalmente se descartaron del diseño el Vestíbulo de Ingreso y el Baño, que no albergan equipo de cómputo.

### 2.2 Distribución de hosts por departamento

El enunciado fija el total de equipos por departamento pero deja a criterio del diseñador el reparto entre las 30 PCs y las 12 laptops. La decisión inicial fue **no repartir de forma uniforme**.

Un reparto proporcional habría sido más rápido, pero no responde a ninguna lógica operativa. Se optó en cambio por un criterio de **perfil de movilidad del puesto de trabajo**: cada departamento recibe la mezcla que corresponde a la forma en que efectivamente trabaja.

Esto llevó a decisiones deliberadamente asimétricas. El Departamento Legal recibió 3 laptops y 1 PC, invirtiendo la proporción del resto del edificio, porque el trabajo jurídico implica audiencias, notarías y visitas a cliente. Dirección General recibió la misma mezcla por razones análogas: agenda externa frecuente. En el extremo opuesto, Backend y Recepción no recibieron ninguna laptop: la compilación de código y la ejecución de contenedores locales exigen estaciones de escritorio, y la atención presencial en recepción es por definición un puesto fijo.

La Sala de Capacitación se resolvió leyendo el mobiliario del plano: tres filas de tres puestos cada una determinaron 9 estaciones fijas, más una laptop para el instructor.

Esta etapa tuvo una consecuencia que no era evidente al iniciarla. Al concentrarse 6 de las 12 laptops en Legal y Dirección General, quedó expuesta una contradicción: se había asignado equipo portátil a los puestos más móviles, pero un diseño exclusivamente cableado los obligaría a operar anclados a un punto fijo. De ahí surgió la decisión de incorporar puntos de red destinados a acceso inalámbrico, descrita en la sección 2.6.

### 2.3 Determinación de la ubicación del MDF

Esta fue la decisión más discutida del proceso, y la que mejor ilustra que un criterio cuantitativo aislado no basta para resolver un problema de diseño.

Se calculó primero el **centroide del edificio ponderado por cantidad de puertos de red** de cada área, para identificar el punto que minimiza la distancia promedio hacia todos los puntos de red. El resultado fue aproximadamente (16.0 m, 8.2 m) desde la esquina superior izquierda: un punto situado cerca del límite entre el Departamento Legal y Dirección General.

Ese punto no era viable. Cae dentro de oficinas activas, donde instalar un cuarto de telecomunicaciones implicaría sacrificar área productiva, exponer el equipo a manipulación no autorizada y contravenir el requisito de ANSI/TIA-569 de que el cuarto de telecomunicaciones sea un espacio exclusivo con acceso controlado.

Se seleccionó entonces el **Data Center** (4 m × 7 m), pese a situarse a unos 10 m del punto teórico. La justificación se construyó sobre cuatro pilares: es el único espacio técnico dedicado del plano, ya concentra los tres servidores principales, es contiguo al eje de canalización del Área Abierta, y —el argumento decisivo— la desviación no genera restricción técnica alguna.

Este último punto se demostró numéricamente. Se calculó el recorrido más desfavorable del edificio, correspondiente a Recepción, sumando tramo por tramo la bajada del punto, el recorrido interno, el descenso al Área Abierta, el trayecto longitudinal y el ascenso al rack. El resultado fue de aproximadamente 45 m con holgura incluida, frente al límite de 90 m que establece ANSI/TIA-568 para el enlace permanente. El enlace más largo del edificio consume la mitad del presupuesto de distancia disponible.

La lección de esta etapa fue que **la optimización de distancia solo importa cuando la distancia es una restricción activa**. En un edificio de 28 m × 21 m no lo es, y por tanto otros criterios —cumplimiento normativo, seguridad física, eficiencia de canalización— pasan a ser determinantes.

### 2.4 Trazado de las rutas de canalización

Definida la ubicación del MDF, se trazó la ruta troncal haciendo converger los siete enlaces en un **eje único a lo largo del Área Abierta**.

La alternativa habría sido trazar rutas independientes por el camino más corto de cada departamento hacia el MDF. Se descartó porque implicaría atravesar oficinas, requerir perforaciones adicionales en muros divisorios y multiplicar los tramos de canalización a mantener.

El eje único concentra la canalización en un solo elemento, permite inspección y mantenimiento sin ingresar a áreas de trabajo, y admite ampliaciones futuras sin nueva obra civil. El costo es un mayor consumo de cable en los departamentos más alejados —Recepción recorre 24 m cuando en línea recta serían unos 20 m— pero ese diferencial es marginal frente a las ventajas operativas.

Esta decisión determinó a su vez la **posición de los switches dentro de cada departamento**: todos se ubicaron contra la pared que da al Área Abierta, minimizando el tramo de troncal.

Para el cableado horizontal se adoptó un trazado perimetral por canaleta a media altura, con bajadas verticales hasta cada toma. Las tomas se ubicaron **en muro y no sobre mobiliario**, criterio que corresponde a la instalación real y que además hace las distancias predecibles y medibles.

### 2.5 Dimensionamiento del equipo

El dimensionamiento siguió una cadena de dependencias donde cada elemento se deriva del anterior:

```
Hosts por departamento
   → Puertos requeridos
      → Tipo y cantidad de tomas
         → Dimensión del patch panel
            → Dimensión del switch
               → Unidades de rack ocupadas
                  → Tamaño del rack
                     → Consumo eléctrico
                        → Capacidad de la UPS
```

El criterio general para las tomas fue **doble en todo puesto de trabajo**, reservando un puerto para expansión. Se establecieron tres excepciones justificadas: toma cuádruple en el puesto del instructor de Capacitación, que debe atender laptop, proyector de red, punto de acceso y equipo de demostración; y tomas unitarias tanto en el Data Center —donde los servidores terminan directamente en el patch panel del rack— como en los puntos destinados a acceso inalámbrico.

El resultado fue de 28 tomas físicas que suman 53 puertos instalados para 51 requeridos.

En el dimensionamiento del patch panel del MDF se encontró una ambigüedad en el enunciado, descrita en la sección 7.3.

### 2.6 Decisiones que emergieron durante el proceso

Dos decisiones no estaban previstas al inicio y surgieron como consecuencia de otras.

**Los puntos de acceso inalámbrico.** Como se describió, la distribución de laptops por perfil de movilidad expuso una contradicción con un diseño exclusivamente cableado. Se resolvió incorporando tres puntos de red destinados a AP: uno en el Área Abierta, uno en el muro entre Legal y Dirección General —donde se concentran 6 de las 12 laptops— y uno en el puesto del instructor de Capacitación. Se mantuvieron dentro del alcance de la práctica al tratarse de **puntos de red cableados** dimensionados para PoE+, no de equipo inalámbrico configurado.

**La supresión del switch del Data Center.** El diseño inicial contemplaba un switch de acceso en el Data Center, por simetría con los demás departamentos. Al revisar la disposición se advirtió que resultaba redundante: el switch core está alojado en el mismo rack, a menos de un metro de los servidores. Mantener un switch intermedio habría añadido un salto de red innecesario, un punto de falla adicional y el costo de un equipo, sin beneficio alguno.

Se suprimió, conectando los tres servidores directamente al core. Esta decisión tuvo una consecuencia en cadena que obligó a revisar el diseño de redundancia: el enlace de fibra hacia el Data Center perdía sentido, porque ya no existía enlace troncal que proteger. La redundancia quedó entonces limitada a Backend, el único segmento crítico que sí depende de un troncal.

---

## 3. Criterios para la selección de topologías

El enunciado establece tres criterios: número de hosts, criticidad del segmento y balance entre costo, escalabilidad y tolerancia a fallos. Se aplicaron de la siguiente manera:

| Criterio | Aplicación en el diseño |
|---|---|
| **Número de hosts** | Determinó la dimensión del switch de cada departamento (8 o 24 puertos) y la cantidad de tomas, pero **no** la topología: los ocho segmentos son estrella independientemente de tener 3 o 12 puertos |
| **Criticidad del segmento** | Determinó el tratamiento del enlace troncal. Se clasificó cada departamento según el impacto que su indisponibilidad tendría sobre la operación productiva de una empresa de desarrollo de software |
| **Balance costo / escalabilidad / tolerancia a fallos** | Determinó que la redundancia fuera **selectiva y no generalizada**: se redunda únicamente el segmento cuya caída detiene la actividad central del negocio |

### 3.1 Por qué todos los segmentos son estrella

Existía la tentación de introducir variedad topológica entre departamentos para que el diseño pareciera más elaborado. Se descartó por incorrecta.

Bus y anillo están obsoletos en cableado estructurado, y ANSI/TIA-568 establece la estrella como topología requerida para el cableado horizontal en edificios comerciales. Además, los switches Ethernet operan nativamente en estrella: implementar otra topología exigiría hardware fuera del mercado comercial actual.

La estrella se seleccionó por aislamiento de fallos —un cable dañado afecta a un solo host—, diagnóstico puerto a puerto, escalabilidad incremental, cumplimiento normativo y compatibilidad con el equipo activo. Su desventaja, un mayor consumo de cable, se cuantificó en aproximadamente 496 m de UTP Cat 6, sobrecosto marginal frente al costo de un rediseño posterior.

### 3.2 Dónde está realmente la diferenciación

La diferenciación entre departamentos **no está en la topología horizontal sino en el nivel troncal**. Este fue el punto conceptual más importante del proceso.

Se clasificaron los ocho segmentos por criticidad operativa. Backend resultó el de mayor criticidad: aloja el servidor de desarrollo y las estaciones de compilación, y su interrupción detiene la actividad productiva central de una empresa de software. En el extremo opuesto, la Sala de Capacitación —paradójicamente el segmento con mayor densidad de puertos del edificio, 12— resultó de criticidad baja, porque su uso es intermitente y no vinculado a la operación productiva.

Esa clasificación determinó la asignación de enlaces: simple en seis segmentos, doble con diversidad de medio en Backend, y conexión directa al core en el Data Center.

El resultado es una **topología en árbol con malla parcial**. Redundar los siete troncales habría duplicado el costo del cableado vertical, los puertos del core y la complejidad de administración, sin beneficio proporcional. Se aceptó deliberadamente un punto único de falla en los segmentos de baja criticidad para concentrar la inversión donde efectivamente se requiere.

---

## 4. Criterios para la selección de medios de transmisión

Se evaluaron seis variables en cada segmento:

**Distancia real del tendido.** Determina qué categorías son viables. Ningún tendido horizontal del edificio supera los 28 m, lo que sitúa todos los enlaces holgadamente dentro del alcance de cualquier categoría de cobre.

**Ancho de banda requerido y previsible.** No solo el actual sino el de la vida útil del cableado, que supera con amplitud la del equipo activo. El cableado es lo más costoso de sustituir y por tanto debe dimensionarse con mayor horizonte.

**Inmunidad a la interferencia.** Determinó la selección de cable blindado en el Data Center, donde la alta densidad de cables agrupados en rack genera diafonía exógena.

**Requerimientos de alimentación PoE.** Los puntos destinados a AP deben alimentar el equipo por el mismo cable, lo que motivó especificar calibre 23 AWG para reducir la caída de tensión y la elevación térmica.

**Tipo de conductor según el uso.** Conductor sólido en tendidos fijos dentro de canalización, terminados por desplazamiento de aislamiento; conductor flexible únicamente en patch cords, que toleran flexión cíclica sin fracturarse.

**Relación costo-beneficio.** El caso más claro fue la elección de Cat 6 sobre Cat 6A en el cableado horizontal: Cat 6A representa un sobrecosto aproximado del 40 % y su ventaja —10 Gbps a 100 m frente a 55 m— no es aprovechable en un edificio donde ningún tendido supera los 28 m.

La decisión más significativa fue **Cat 6 en lugar de Cat 5e**. El diferencial de precio es reducido, pero Cat 6 admite la migración futura a 10 Gbps sin recablear. Recablear es el componente más costoso de una ampliación, no por el material sino por la mano de obra y la interrupción del servicio.

---

## 5. Criterios para la selección de equipo activo

**Cantidad de puertos con reserva.** Se aplicó la regla del enunciado —el switch debe tener puertos iguales o mayores al patch panel— y se seleccionó por encima del mínimo cuando el diferencial de precio era reducido. Un switch de 24 puertos en un departamento de 8 deja 62 % de reserva a un costo marginalmente superior al de uno de 16.

**Proporcionalidad al segmento.** El criterio anterior no se aplicó de forma indiscriminada. En Recepción, Legal y Dirección General —4 puertos cada uno— se seleccionaron switches de 8 puertos: uno de 24 dejaría el 79 % de la capacidad ociosa sin justificación, con mayor costo y mayor consumo eléctrico.

**Capacidad de uplink.** El switch de Backend incorpora un puerto SFP+ para el enlace redundante de fibra, y el core incorpora dos: uno para ese enlace y otro de reserva para una futura conexión de fibra sin sustituir el equipo.

**Formato.** Se especificó formato rack para el equipo del MDF, permitiendo su integración ordenada en el bastidor.

**Consumo eléctrico.** Se estimó por equipo para dimensionar la UPS, considerando además que los tres puntos AP requerirán alimentación PoE+ de 30 W cada uno.

En cuanto al equipo pasivo, la justificación de los **patch panels** merece mención. Su función es desacoplar el cableado fijo —tendido en canalización, no destinado a manipulación— de los patch cords, que sí se manipulan con frecuencia. Sin patch panel, cada cambio de configuración implicaría manipular directamente el cableado estructural, degradándolo de forma progresiva e irreversible.

---

## 6. Justificación del medio utilizado en el cableado troncal

Esta fue la decisión que más contradijo la intuición inicial.

La asociación automática es "troncal igual a fibra óptica". Al evaluarla contra las condiciones reales del edificio, resultó **incorrecta para los enlaces principales**.

### 6.1 Evaluación de fibra para la totalidad del troncal

La distancia máxima entre el MDF y el switch más lejano —Recepción— es de aproximadamente 24 m, muy por debajo del límite de 100 m del cobre balanceado. En ese rango, un enlace de fibra exige adicionalmente dos transceptores SFP+ por enlace, puertos SFP+ disponibles en ambos switches, un ODF de terminación en cada extremo y mano de obra especializada.

El costo total por enlace resulta entre tres y cuatro veces superior al de un enlace equivalente en Cat 6A, **sin ganancia alguna en velocidad, latencia ni capacidad**: ambos medios entregan 10 Gbps en las distancias involucradas.

Se descartó fibra para los siete enlaces troncales principales, que se implementan en **UTP Cat 6A U/FTP**. Esta categoría entrega 10 Gbps de uplink a 100 m, incorpora blindaje contra diafonía exógena —relevante donde los cables corren agrupados por la escalerilla— y utiliza los puertos RJ45 nativos de los switches sin hardware adicional.

### 6.2 Por qué el enlace redundante sí es fibra

El enlace redundante hacia Backend sí se implementa en fibra OM3, y **la razón no es la distancia sino la diversidad de medio**.

Un enlace redundante tendido en el mismo medio y por la misma canalización que el principal comparte modos de falla. Una sobretensión inducida, una fuente de interferencia electromagnética o un daño físico a la escalerilla afectarían simultáneamente a ambos enlaces, anulando el propósito de la redundancia.

La fibra es un medio dieléctrico: no conduce corriente, es inmune a la interferencia electromagnética y no propaga sobretensiones. Un evento eléctrico que degrade el enlace de cobre no afecta al de fibra. Adicionalmente, ambos enlaces se tienden por rutas físicas separadas.

### 6.3 Lo que esta decisión enseñó

La conclusión que se extrae es que **la selección de un medio de transmisión no responde a una jerarquía de calidad sino a las condiciones del enlace concreto**. La fibra no es "mejor" que el cobre en términos absolutos: es superior en distancia, en inmunidad electromagnética y en aislamiento eléctrico, y esas ventajas solo justifican su costo cuando alguna de ellas responde a una necesidad real.

En este edificio, la distancia no lo justifica. La diversidad de medio en el único segmento crítico, sí.

---

## 7. Retos de planificación física encontrados

### 7.1 Conflicto entre el óptimo matemático y el óptimo normativo

El centroide ponderado indicaba una ubicación del MDF que las normas de infraestructura desaconsejan. El reto no fue calcular el centroide sino **decidir cuánto peso otorgarle frente a criterios cualitativos** que no admiten expresión numérica: seguridad física, cumplimiento normativo, eficiencia de mantenimiento.

Se resolvió invirtiendo el planteamiento: en lugar de preguntar cuál es la ubicación óptima, se preguntó si la desviación respecto al óptimo genera alguna restricción técnica. Al demostrar que el enlace más largo consume la mitad del presupuesto de distancia disponible, la desviación dejó de ser un problema y los criterios cualitativos pasaron a ser determinantes.

### 7.2 La franja superior sin acceso al eje de canalización

Los cuatro departamentos de la franja superior no comunican directamente con el Área Abierta. Sus troncales requieren un tramo de descenso por el muro divisorio, lo que introduce un segmento vertical adicional en cada enlace.

Se resolvió especificando tubería EMT para los descensos —que requieren protección mecánica en área transitada— y ubicando los switches de esos departamentos contra la pared que da a la franja media, minimizando el recorrido hasta el punto de descenso. El tramo vertical se incorporó al cálculo de distancias con una holgura del 20 %, superior al 15 % aplicado en el cableado horizontal.

### 7.3 Ambigüedad en el dimensionamiento del patch panel del MDF

El enunciado indica que el patch panel del edificio debe dimensionarse según la cantidad total de puntos de red, lo que llevaría a un patch panel de 48 puertos para los 51 puntos del edificio. Pero también define el cableado horizontal como el tramo entre el switch de departamento y los hosts, lo que implica que al MDF solo llegan físicamente los siete enlaces troncales y los tres servidores del Data Center: diez puertos, que un patch panel de 24 cubriría sobradamente.

Ambas lecturas son defendibles y conducen a diseños distintos. Se optó por la lectura literal —patch panel de 48 puertos en el MDF— por ser la que el enunciado enuncia de forma explícita, dejando constancia de la ambigüedad en el manual técnico y de la modificación puntual que requeriría la interpretación alternativa.

El reto aquí no fue técnico sino de **manejo de un requerimiento ambiguo**: documentar la interpretación adoptada y su alternativa, en lugar de elegir en silencio.

### 7.4 Contradicción entre la asignación de equipos y el diseño exclusivamente cableado

Al distribuir las laptops por perfil de movilidad se generó una inconsistencia: se asignó equipo portátil a los puestos que más se desplazan, pero un diseño exclusivamente cableado los obligaría a operar anclados.

Se resolvió incorporando tres puntos de red destinados a acceso inalámbrico, manteniéndolos dentro del alcance de Capa 1 al tratarse de puntos cableados dimensionados para PoE+ y no de equipo inalámbrico configurado. La decisión obligó a recalcular el total de puertos —de 48 a 51— y a ajustar el dimensionamiento aguas abajo.

### 7.5 Detección de un elemento redundante avanzado el diseño

El switch de acceso del Data Center se había incorporado por simetría con los demás departamentos, sin examinar si cumplía alguna función. Al revisar la disposición del rack se advirtió que el switch core estaba a menos de un metro de los servidores.

Corregirlo obligó a revisar decisiones ya tomadas: el enlace de fibra hacia el Data Center perdía sentido, el conteo de switches cambiaba, el presupuesto se modificaba y el diagrama debía ajustarse. El reto fue **aceptar rehacer trabajo ya completado** en lugar de conservar un elemento injustificado por evitar la corrección.

### 7.6 Traducción de la escala del plano a distancias de cable

Las distancias no se miden en línea recta: el cable desciende del punto, recorre canalización perimetral y asciende al gabinete. Un punto que en el plano está a 8 m del switch puede requerir 15 m de cable.

Se resolvió estableciendo un método explícito de suma por tramos —bajada, recorrido horizontal, subida, holgura— y aplicando promedios por departamento en lugar de medir los 53 puntos individualmente, criterio que el enunciado admite al solicitar un cálculo aproximado.

---

## 8. Conclusiones

**1. El diseño de Capa 1 condiciona todo lo que se construye encima.**
Las decisiones tomadas en esta práctica —ubicación del MDF, categoría de cable, dimensionamiento de puertos, rutas de canalización— determinan el rango de lo posible en las capas superiores. Una VLAN mal configurada se corrige en minutos; un cableado subdimensionado exige reintervenir la obra civil. Esta asimetría entre el costo de corregir una decisión lógica y una física justifica el rigor que el diseño físico demanda.

**2. La optimización de una variable aislada no produce el mejor diseño.**
El caso del MDF lo ilustra: el punto matemáticamente óptimo era inviable por razones normativas y de seguridad. El diseño correcto no fue el que minimizaba la distancia promedio sino el que satisfacía simultáneamente varias restricciones, verificando que la variable optimizada no fuera una restricción activa.

**3. La selección de medios responde a condiciones concretas, no a jerarquías de calidad.**
Descartar fibra en el troncal principal fue contraintuitivo pero correcto: sus ventajas —alcance, inmunidad electromagnética, aislamiento eléctrico— no resolvían ninguna necesidad real en enlaces de 24 m. La misma fibra resultó plenamente justificada en el enlace redundante, donde la diversidad de medio sí aporta valor. El medio no es mejor o peor en abstracto; lo es respecto a las condiciones del enlace.

**4. La redundancia debe ser selectiva y sustentada en criticidad.**
Redundar todo habría duplicado costos sin beneficio proporcional. Clasificar los segmentos por impacto operativo permitió concentrar la inversión donde una interrupción detiene la actividad productiva, aceptando de forma deliberada y documentada un punto único de falla donde no lo hace.

**5. La documentación es parte de la infraestructura, no un anexo administrativo.**
La comparación con TIA/EIA-606 mostró que un esquema de etiquetado simplificado funciona mientras quien lo diseñó permanece disponible y la instalación no crece —dos condiciones que dejan de cumplirse con el tiempo. El estándar completo optimiza la operación y el mantenimiento a lo largo de una vida útil que supera en uno o dos órdenes de magnitud al tiempo invertido en el diseño.

**6. Un buen diseño documenta también lo que descartó y por qué.**
Las decisiones más valiosas de este trabajo no fueron las que se adoptaron sino las que se examinaron y rechazaron: fibra en todo el troncal, redundancia generalizada, el switch del Data Center, la variedad topológica artificial. Registrar las alternativas evaluadas y las razones de su descarte es lo que permite que otro ingeniero comprenda el diseño y lo modifique con criterio.
