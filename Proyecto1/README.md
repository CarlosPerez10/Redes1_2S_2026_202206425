# Proyecto 1 — SmartCity Tech Park

**Redes de Computadoras 1 · Segundo Semestre 2026**
Universidad de San Carlos de Guatemala · Facultad de Ingeniería · Ingeniería en Ciencias y Sistemas

| Campo | Valor |
|---|---|
| Nombre | Carlos Javier Pérez Pocón |
| Carné | 202206425 |
| Archivo de simulación | `Proyecto1_202206425.pkt` |
| Simulador | Cisco Packet Tracer 8.x |
| Fecha de entrega | 17/09/2026 |

---

## 1. Planteamiento

### 1.1 El problema

El campus de SmartCity Tech Park opera hoy como una **red plana de Capa 2**. Esto produce tres fallas estructurales, cada una con una consecuencia medible:

| Falla observada | Consecuencia técnica |
|---|---|
| Un único dominio de broadcast para todo el campus | Cada trama de broadcast (ARP, DHCP Discover) generada por un visitante llega a los servidores críticos y a la maquinaria industrial. Se consume ancho de banda útil y el tráfico administrativo queda expuesto a usuarios no confiables. |
| Dominio de colisión compartido en la Planta de Producción | La maquinaria legacy comparte medio. Cada colisión dispara el algoritmo de backoff de CSMA/CD; el throughput efectivo cae de forma no lineal conforme se agregan equipos. |
| Dependencia de enlaces físicos únicos | La caída de un solo cable o switch aísla un área completa. No existe tolerancia a fallos ni capacidad agregada en los enlaces de mayor demanda. |

### 1.2 La solución propuesta

Reestructuración completa de Capa 1 y Capa 2 sobre un modelo jerárquico de tres niveles (núcleo, distribución y acceso), aplicando:

| Tecnología | Problema que ataca |
|---|---|
| Segmentación con VLANs | Fragmenta el dominio de broadcast único en seis dominios independientes. |
| VTP | Centraliza la administración de VLANs en un único punto y evita divergencia de bases de datos entre switches. |
| EtherChannel (PAgP) | Agrega ancho de banda y elimina el punto único de falla en los enlaces de mayor demanda. |
| Rapid-PVST | Administra los bucles introducidos deliberadamente por la redundancia, sin tormentas de broadcast y con convergencia rápida. |
| Aislamiento de segmento legacy | Contiene el dominio de colisión heredado en un solo puerto de acceso, sin obligar a migrar la maquinaria. |

---

## 2. Parámetros derivados del carné

Todos los identificadores del proyecto se derivan del carné **202206425**.

| Parámetro | Regla del enunciado | Valor aplicado |
|---|---|---|
| Último dígito (X) | — | **5** |
| Penúltimo dígito (#) | — | **2** |
| Dominio VTP | `Smart_#` | `Smart_2` |
| Contraseña VTP | fija | `proyecto12S2026` |
| Paridad del carné | — | **Impar** |
| Protocolo de agregación | LACP si par, PAgP si impar | **PAgP** |
| Protocolo de Spanning Tree | PVST si par, Rapid-PVST si impar | **Rapid-PVST** |
| VLAN nativa de troncales | `9X` | **95** |
| Banner MOTD | `Acceso Restringido - TechPark_[Carné]` | `Acceso Restringido - TechPark_202206425` |

---

## 3. Topología general

_[Descripción del modelo jerárquico implementado: núcleo en el Centro de Datos, un switch de distribución por edificio, switches de acceso por segmento.]_

### 3.1 Inventario de dispositivos

| Nombre | Modelo | Rol jerárquico | Área | Modo VTP |
|---|---|---|---|---|
| `SW-CORE` | | Núcleo | Centro de Datos | Server |
| `SW-SRV` | | Acceso | Centro de Datos | Client |
| `SW-IDD-DIST` | | Distribución | Centro de I+D | Client |
| `SW-IDD-A` | | Acceso | Centro de I+D | Client |
| `SW-IDD-B` | | Acceso | Centro de I+D | Client |
| `SW-CORP-DIST` | | Distribución | Edificio Corporativo | Client |
| `SW-ALA-A` | | Acceso | Edificio Corporativo | Client |
| `SW-ALA-B` | | Acceso | Edificio Corporativo | Client |
| `SW-VISITAS` | | Acceso | Áreas Comunes | Transparent |
| `SW-PROD-DIST` | | Distribución | Planta de Producción | Client |
| `SW-PROD-ACC` | | Acceso | Planta de Producción | Client |
| `HUB-LEGACY` | Hub-PT | Capa 1 | Planta de Producción | N/A |

---

## 4. Diseño por área

### 4.1 Centro de Datos (núcleo)

**Requisito del enunciado:** administración VTP centralizada, troncales hacia los 3 edificios, granja de al menos 4 servidores sin depender de una única conexión física.

**Cómo se resolvió:**
- _[Elección del switch de núcleo]_
- _[Cómo se conecta la granja de servidores y por qué se usó EtherChannel en lugar de un enlace simple]_

### 4.2 Centro de I+D

**Requisito del enunciado:** al menos 3 switches interconectados de forma que la caída de uno no aísle a los demás; troncal hacia el núcleo con mayor ancho de banda que el resto del campus; mínimo 8 estaciones de trabajo.

**Cómo se resolvió:**
- _[Esquema de interconexión entre los 3 switches y qué pasa ante la caída de cada uno]_
- _[Por qué este enlace es el de mayor capacidad del campus]_

**Prueba de alta disponibilidad:** _[Escenario probado: apagar un switch y verificar que los otros dos siguen comunicados]_

### 4.3 Edificio Corporativo

**Requisito del enunciado:** dos alas con switch de acceso propio, conectividad entre alas activa aun si falla la ruta hacia la distribución; segmento de Áreas Comunes con aislamiento total de tráfico y de administración de VLANs, con servicio inalámbrico por Access Point.

**Cómo se resolvió:**
- _[Enlace directo entre alas y cómo STP lo mantiene en bloqueo hasta que se necesita]_
- _[Modo VTP elegido para el switch de Áreas Comunes y por qué eso cumple con "aislar la administración de VLANs"]_
- _[Configuración del Access Point y la VLAN de visitantes]_

**Prueba de redundancia entre alas:** _[Escenario: derribar el uplink de un ala y verificar que la conectividad se mantiene]_

### 4.4 Planta de Producción

**Requisito del enunciado:** segmento legacy con dominio de colisión compartido en Capa 1, integrado mediante un switch de acceso; documentar el impacto y las medidas de contención.

**Cómo se resolvió:**
- _[Descripción del hub y los equipos conectados a él]_

**Impacto del dominio de colisión compartido:**
_[Medio compartido, CSMA/CD, half-duplex forzado, degradación conforme crecen los equipos, y por qué el broadcast del hub se replica a todos sus puertos]_

**Medidas de contención aplicadas en el switch de acceso:**

| Medida | Comando | Qué mitiga |
|---|---|---|
| Control de tormentas | `storm-control broadcast level ...` | Impide que una tormenta originada en el segmento legacy inunde el resto del campus. |
| Seguridad de puerto | `switchport port-security ...` | Limita las MAC aprendidas por el puerto del hub. |
| Confinamiento a un puerto | Diseño | El dominio de colisión no se propaga más allá del puerto del switch, porque el switch sí segmenta colisiones. |

_Nota: estas medidas mitigan parcialmente el impacto. No lo eliminan, porque el medio compartido sigue existiendo dentro del hub._

---

## 5. Medios de transmisión

| Enlace | Medio seleccionado | Distancia estimada | Justificación |
|---|---|---|---|
| `SW-CORE` ↔ `SW-IDD-DIST` | | | |
| `SW-CORE` ↔ `SW-CORP-DIST` | | | |
| `SW-CORE` ↔ `SW-PROD-DIST` | | | |
| `SW-CORE` ↔ `SW-SRV` | | | |
| Enlaces intra-edificio | | | |
| Enlaces a dispositivos finales | | | |

**Criterio general aplicado:** _[Distancia máxima del estándar, ancho de banda requerido, inmunidad a interferencia electromagnética —especialmente relevante en la Planta de Producción— y costo]_

---

## 6. Dominios de colisión

Cada puerto activo de un switch constituye un dominio de colisión independiente, porque el switch conmuta por MAC y opera en full-duplex. Un hub, en cambio, repite la señal por todos sus puertos y por lo tanto todos sus equipos comparten un solo dominio.

| Dispositivo | Puertos activos | Dominios de colisión que genera | Observación |
|---|---|---|---|
| `SW-CORE` | | | |
| `SW-SRV` | | | |
| `SW-IDD-DIST` | | | |
| `SW-IDD-A` | | | |
| `SW-IDD-B` | | | |
| `SW-CORP-DIST` | | | |
| `SW-ALA-A` | | | |
| `SW-ALA-B` | | | |
| `SW-VISITAS` | | | |
| `SW-PROD-DIST` | | | |
| `SW-PROD-ACC` | | | |
| `HUB-LEGACY` | | **1 (compartido)** | Dominio de colisión compartido del segmento Legacy. |
| **Total** | | | |

**Nota sobre EtherChannel:** los puertos agrupados en un `Port-channel` siguen siendo enlaces físicos independientes, por lo que cada uno conserva su propio dominio de colisión.

---

## 7. Dominios de broadcast

Cada VLAN activa constituye un dominio de broadcast independiente.

| VLAN ID | Nombre | Dominio de broadcast | Área donde está presente | Dispositivos finales |
|---|---|---|---|---|
| 15 | GERENCIA | 1 | Edificio Corporativo | |
| 25 | INVESTIGACION | 1 | Centro de I+D | |
| 35 | PRODUCCION | 1 | Planta de Producción | |
| 45 | SERVIDORES | 1 | Centro de Datos | |
| 55 | VISITANTES | 1 | Áreas Comunes | |
| 95 | NATIVA | 1 | Troncales del campus | Sin hosts asignados |
| **Total** | | **6** | | |

---

## 8. Tabla de VLANs

| VLAN ID | Nombre configurado | Propósito | Origen |
|---|---|---|---|
| 15 | `GERENCIA` | Personal administrativo | Creada en `SW-CORE`, propagada por VTP |
| 25 | `INVESTIGACION` | Estaciones de I+D | Creada en `SW-CORE`, propagada por VTP |
| 35 | `PRODUCCION` | Maquinaria y equipos de planta | Creada en `SW-CORE`, propagada por VTP |
| 45 | `SERVIDORES` | Granja de servidores críticos | Creada en `SW-CORE`, propagada por VTP |
| 55 | `VISITANTES` | Invitados externos vía Access Point | Creada en `SW-CORE` y localmente en `SW-VISITAS` |
| 95 | `NATIVA` | VLAN nativa de todos los troncales | Creada en `SW-CORE`, propagada por VTP |

---

## 9. Asignación de puertos por switch

### 9.1 `SW-CORE`

| Interfaz | Modo | VLAN / Port-channel | Conecta con |
|---|---|---|---|
| | | | |

### 9.2 `SW-SRV`

| Interfaz | Modo | VLAN / Port-channel | Conecta con |
|---|---|---|---|
| | | | |

_[Repetir la tabla para cada switch del inventario]_

---

## 10. Decisiones de diseño y su justificación

### 10.1 Selección del switch servidor de VTP

**Decisión:** `SW-CORE` opera en modo **Server**; `SW-VISITAS` en modo **Transparent**; el resto en modo **Client**.

**Justificación:** _[Por qué el núcleo es el punto natural de administración, qué riesgo se evita al dejar a los demás como clientes —revisiones de configuración sobrescribiendo la base de datos— y por qué el switch de visitantes queda fuera del dominio administrativo]_

### 10.2 Selección del Root Bridge por VLAN

| VLAN | Root Bridge primario | Root Bridge secundario | Prioridad configurada |
|---|---|---|---|
| 15 | | | |
| 25 | | | |
| 35 | | | |
| 45 | | | |
| 55 | | | |
| 95 | | | |

**Justificación:** _[Criterio: dónde converge el tráfico, por qué la raíz debe estar ahí, y por qué se define un secundario]_

### 10.3 EtherChannel implementados

| Port-channel | Switches | Enlaces agrupados | Ancho de banda resultante | Por qué aquí |
|---|---|---|---|---|
| `Po1` | | | | |
| `Po2` | | | | |
| `Po3` | | | | |

**Justificación general:** _[Los dos beneficios simultáneos: agregación de ancho de banda y tolerancia a fallos; y por qué STP ve el bundle como un solo enlace lógico y no lo bloquea]_

**Protocolo:** PAgP, por carné impar. Modo utilizado: _[`desirable` / `auto` y por qué]_

### 10.4 Seguridad básica

| Medida | Alcance | Valor |
|---|---|---|
| Banner MOTD | Switches de distribución | `Acceso Restringido - TechPark_202206425` |
| VLAN nativa | Todos los troncales del campus | 95 |
| Port-security | _[Puertos definidos]_ | |
| Storm-control | _[Puertos definidos]_ | |

**Por qué se cambia la VLAN nativa:** _[Riesgo de double tagging / VLAN hopping cuando la nativa queda en VLAN 1]_

---

## 11. Comandos utilizados por dispositivo

### 11.1 `SW-CORE`

```
enable
configure terminal
hostname SW-CORE
!
```

### 11.2 `SW-SRV`

```
```

_[Repetir por cada dispositivo. Guardar además el `running-config` completo de cada switch en `configs/`]_

---

## 12. Evidencia de pruebas

### 12.1 `show interfaces trunk`

_[Troncales activos, VLAN nativa 95 confirmada, VLANs permitidas]_

### 12.2 `show spanning-tree`

_[Root bridge por VLAN, puertos en estado de bloqueo y por qué están ahí]_

### 12.3 `show etherchannel summary`

_[Estado `SU`, puertos en `P`, protocolo PAgP]_

### 12.4 Pruebas de conectividad

| Origen | Destino | VLAN origen | VLAN destino | Resultado esperado | Resultado obtenido |
|---|---|---|---|---|---|
| | | | | Éxito | |
| | | | | Fallo (aislamiento) | |

_[Debe demostrarse 100% de conectividad intra-VLAN y 0% de conectividad inter-VLAN]_

### 12.5 Pruebas de tolerancia a fallos

| Escenario simulado | Comportamiento esperado | Resultado |
|---|---|---|
| Caída de un switch del anillo de I+D | | |
| Caída del uplink de un ala del Edificio Corporativo | | |
| Caída de un enlace físico de un EtherChannel | | |

---

## 13. Presupuesto estimado

| Equipo / Material | Modelo | Cantidad | Costo unitario (Q) | Subtotal (Q) |
|---|---|---|---|---|
| Switch de núcleo | | | | |
| Switches de distribución | | | | |
| Switches de acceso | | | | |
| Hub | | | | |
| Access Point | | | | |
| Módulos de fibra | | | | |
| Cable UTP Cat 6 (m) | | | | |
| Fibra óptica (m) | | | | |
| **Total** | | | | |

_[Indicar la fuente de los precios utilizados]_

---

## 14. Anexo — Inspección de PDUs (alcance opcional)

### 14.1 BPDU de STP

_[Ubicación del Root ID, el Bridge ID y el costo del enlace dentro del encabezado]_

### 14.2 PDU de VTP

_[Ubicación del VTP Domain Name y el Configuration Revision Number]_

---

## 15. Matriz de trazabilidad de requisitos

| # | Requisito del enunciado | Sección donde se cumple | Estado |
|---|---|---|---|
| 1 | Topología jerárquica con núcleo y 3 distribuciones | §3, §4 | |
| 2 | Justificación del medio de transmisión por enlace | §5 | |
| 3 | Granja de ≥4 servidores sin conexión física única | §4.1 | |
| 4 | I+D con ≥3 switches y tolerancia a caída de uno | §4.2 | |
| 5 | I+D con el troncal de mayor ancho de banda | §4.2, §10.3 | |
| 6 | I+D con ≥8 estaciones de trabajo | §4.2 | |
| 7 | Corporativo con 2 alas y ruta alterna entre ellas | §4.3 | |
| 8 | Áreas Comunes aisladas en tráfico y administración | §4.3, §10.1 | |
| 9 | Access Point para laptops de invitados | §4.3 | |
| 10 | Hub legacy con dominio de colisión compartido | §4.4 | |
| 11 | Impacto y contención del segmento legacy documentados | §4.4 | |
| 12 | Dominio VTP `Smart_2` con contraseña | §10.1 | |
| 13 | VLANs 15/25/35/45/55 con nombres exactos | §8 | |
| 14 | VLAN nativa 95 en todos los troncales | §10.4 | |
| 15 | EtherChannel con PAgP | §10.3 | |
| 16 | Rapid-PVST con root bridge justificado | §10.2 | |
| 17 | Banner MOTD en switches de distribución | §10.4 | |
| 18 | Tabla de dominios de colisión | §6 | |
| 19 | Tabla de dominios de broadcast | §7 | |
| 20 | Tabla de asignación de puertos | §9 | |
| 21 | Comandos por dispositivo | §11 | |
| 22 | Evidencia `show spanning-tree` | §12.2 | |
| 23 | Evidencia `show etherchannel summary` | §12.3 | |
| 24 | Evidencia `show interfaces trunk` | §12.1 | |
| 25 | Etiquetado de medios en Packet Tracer | §5 | |
| 26 | Presupuesto de equipos | §13 | |
| 27 | Inspección de PDUs (opcional) | §14 | |

---

## 16. Conclusiones

_[Qué cambió en el comportamiento de la red antes y después de la segmentación, qué compromisos de diseño hubo que aceptar, y qué limitaciones quedan pendientes —por ejemplo, que sin Capa 3 no hay comunicación entre VLANs]_

---

## Estructura del repositorio

```
Proyecto1/
├── README.md
├── Proyecto1_202206425.pkt
├── img/
└── configs/
```