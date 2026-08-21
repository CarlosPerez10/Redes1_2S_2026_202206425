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

> TODO — Contexto del problema: qué se pidió, bajo qué rol (ingeniero de
> telecomunicaciones), y qué alcance tuvo el trabajo. 1–2 párrafos.

---

## 2. Proceso de diseño

> TODO — Narrar el orden en que se trabajó. Sugerencia de estructura:

### 2.1 Interpretación del plano arquitectónico

> TODO — Cómo se leyeron las dimensiones y la distribución de áreas; qué escala se
> asumió; cómo se identificaron muros, puertas y el eje de circulación.

### 2.2 Distribución de hosts por departamento

> TODO — Cómo se repartieron las 30 PCs y 12 laptops entre los departamentos y bajo
> qué lógica (perfil de trabajo de cada área, movilidad requerida, etc.).

### 2.3 Determinación de la ubicación del MDF

> TODO — Explicar el cálculo del centroide ponderado, por qué el resultado teórico no
> coincidió con la ubicación final elegida, y qué criterios pesaron más.

### 2.4 Trazado de rutas de canalización

> TODO — Cómo se definió el recorrido troncal y las bajadas horizontales.

### 2.5 Dimensionamiento de equipo

> TODO — De puntos de red a patch panels, de patch panels a switches, y de ahí al
> rack y la UPS.

---

## 3. Criterios para la selección de topologías

> TODO — Explicar los tres criterios que exige el enunciado y cómo se aplicaron:

| Criterio | Cómo se aplicó |
|---|---|
| Número de hosts | TODO |
| Criticidad del segmento | TODO |
| Balance costo / escalabilidad / tolerancia a fallos | TODO |

> TODO — Párrafo explicando por qué **no** todos los departamentos recibieron el mismo
> tratamiento, y qué segmentos se consideraron críticos.

---

## 4. Criterios para la selección de medios de transmisión

> TODO — Distancia, ancho de banda requerido, inmunidad a interferencia, costo por
> metro, disponibilidad en el mercado local y facilidad de terminación.

---

## 5. Criterios para la selección de equipo activo

> TODO — Cantidad de puertos, capacidad de uplink, formato (rack vs. escritorio),
> soporte PoE si aplica, y previsión de crecimiento.

---

## 6. Justificación del medio utilizado en el cableado troncal

> TODO — Sección explícitamente solicitada en el enunciado. Comparar las alternativas
> evaluadas (cobre de categoría superior vs. fibra óptica), contrastando distancia real
> hacia cada departamento, velocidad de uplink de los switches, costo del enlace
> completo (incluyendo transceivers/ODF si aplica) y escalabilidad. Cerrar con la
> decisión tomada y su razón.

---

## 7. Retos de planificación física encontrados

> TODO — Sección explícitamente solicitada. Enumerar dificultades reales del proceso.
> Posibles ejes:

1. **TODO** — descripción del reto y cómo se resolvió.
2. **TODO**
3. **TODO**

---

## 8. Conclusiones

> TODO — 3 a 5 conclusiones sobre lo aprendido del diseño de Capa 1.
