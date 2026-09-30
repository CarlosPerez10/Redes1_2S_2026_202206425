# Práctica 2 — Red de la Ciudad Comercial Cayalá

**Redes de Computadoras 1 · Segundo Semestre 2026 · Carnet 202206425**

Red conmutada para cinco zonas reales de Ciudad Cayalá, segmentada con VLANs y configurada con VTP, Rapid PVST+, EtherChannel PAgP y enrutamiento inter-VLAN en un switch multicapa 3560. Implementada en Cisco Packet Tracer.

![Topología](Documentacion/img/02_topologia_packet_tracer.png)

## Contenido

```
Practica2/
├── README.md
├── PacketTracer/
│   └── Practica2_202206425.pkt
├── Scripts/
│   ├── 00_Script_Completo_Todos_los_Switches.txt
│   ├── 01_SW-Z1-CORE.txt
│   ├── 02_SW-Z1-ACC1.txt
│   ├── 03_SW-Z1-ACC2.txt
│   ├── 04_SW-Z2-MAIN.txt
│   ├── 05_SW-Z2-ACC1.txt
│   ├── 06_SW-Z3-MAIN.txt
│   ├── 07_SW-Z4-MAIN.txt
│   ├── 08_SW-Z4-ACC1.txt
│   ├── 09_SW-Z5-MAIN.txt
│   ├── 10_Hosts_IP.txt
│   └── 11_Comandos_Verificacion.txt
└── Documentacion/
    ├── Manual_Tecnico.md
    ├── Informe_Desarrollo.md
    └── img/
```

## Documentación
- [Manual Técnico](Documentacion/Manual_Tecnico.md)
- [Informe de Desarrollo](Documentacion/Informe_Desarrollo.md)
- [Scripts CLI](Scripts/)
- [Archivo de Packet Tracer](PacketTracer/Practica2_202206425.pkt)

## Zonas

| Zona | Nombre | VLAN | Red | Criticidad | Esquema |
|---|---|---|---|---|---|
| 1 | Distrito Empresarial – Administración Central | 15 | 192.168.5.0/26 | Alta | Núcleo + anillo |
| 2 | Residencial – Lirios de Cayalá | 25 | 192.168.5.128/27 | Media | Bus |
| 3 | Gastronomía y Entretenimiento | 35 | 192.168.5.160/28 | Baja | Estrella |
| 4 | Paseo Cayalá – Comercial | 45 | 192.168.5.64/26 | Alta | Malla parcial + EtherChannel |
| 5 | Centro de Monitoreo y Seguridad | 55 | 192.168.5.176/28 | Alta | Malla parcial |

## Parámetros

| Parámetro | Valor |
|---|---|
| VLAN nativa | 99 |
| VLAN Blackhole | 999 |
| Dominio VTP | 202206425 (v2) |
| STP | Rapid PVST+ |
| EtherChannel | Po1 · PAgP desirable · 2 × 1 Gbps |
| Equipos | 1 × Cisco 3560-24PS · 8 × Cisco 2960-24TT · 14 PCs |
