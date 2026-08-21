# Práctica 1 — QuetzalDev S.A.
### Diseño de Infraestructura Física de Red (Capa 1 del Modelo OSI)

**Universidad de San Carlos de Guatemala**
Facultad de Ingeniería — Escuela de Ingeniería en Ciencias y Sistemas
**Curso:** Redes de Computadoras 1 — Segundo Semestre 2026

| | |
|---|---|
| **Estudiante** | Carlos Javier Pérez Pocón |
| **Carné** | 202206425 |
| **Plano asignado** | Terminación 4, 5 |
| **Fecha de entrega** | 21/08/2026 |

---

## Descripción

Diseño de la infraestructura de cableado estructurado para el edificio corporativo de
un nivel de **QuetzalDev S.A.** (28 m × 21 m), abarcando la ubicación del cuarto de
telecomunicaciones (MDF), la definición de puntos de red y tipos de toma, la selección
y justificación de topologías físicas por departamento, la diferenciación entre cableado
troncal y horizontal, el dimensionamiento de equipo activo y pasivo, y la documentación
técnica conforme a los estándares TIA/EIA-568, 569 y 606.

> Este es un trabajo de **diseño**, no de configuración ni simulación de dispositivos.

---

## Contenido del repositorio

| Archivo | Descripción |
|---|---|
| [`Practica1/ManualTecnico.md`](Practica1/ManualTecnico.md) | Documentación técnica completa: inventario, justificaciones, tablas de cálculo, disposición de pines, etiquetado y presupuesto. |
| [`Practica1/InformeDesarrollo.md`](Practica1/InformeDesarrollo.md) | Proceso de diseño, criterios de decisión y retos encontrados. |
| [`Practica1/img/plano_base.png`](Practica1/img/plano_base.png) | Plano arquitectónico base proporcionado por la cátedra. |
| [`Practica1/img/diagrama_fisico.png`](Practica1/img/diagrama_fisico.png) | Diagrama de diseño físico elaborado sobre el plano base. |
| `Practica1/img/diagrama_fisico.drawio` | Archivo editable del diagrama. |

---

## Resumen del diseño

| Dato | Valor |
|---|---|
| Área del edificio | 588 m² (28 m × 21 m) |
| Departamentos con equipo de red | 8 |
| Hosts totales | 42 equipos de cómputo + 6 servidores |
| Puntos de red | 48 |
| Ubicación del MDF | Data Center (4 m × 7 m) |
| Topología general | Árbol (estrella extendida) con redundancia parcial en segmentos críticos |

---

## Herramientas utilizadas

- **Diagramación:** draw.io / diagrams.net
- **Documentación:** Markdown
- **Control de versiones:** Git / GitHub