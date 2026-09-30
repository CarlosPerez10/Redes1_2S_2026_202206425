# Práctica 2 — Red de la Ciudad Comercial Cayalá

**Redes de Computadoras 1 · 2S2026 · Carnet 202206425**

Red conmutada para 5 zonas reales de Ciudad Cayalá con VLANs por zona, VTP, Rapid PVST+ y EtherChannel PAgP, implementada en Cisco Packet Tracer.

![Topología](Documentacion/img/01_topologia_drawio.png)

## Estructura

```
Practica2/
├── README.md
├── PacketTracer/
│   └── Practica2_202206425.pkt
├── Topologia/
│   └── Topologia_Cayala_202206425.drawio
├── Scripts/
│   ├── 00_Script_Completo_Todos_los_Switches.txt
│   ├── 01_SW-Z1-CORE.txt … 09_SW-Z5-MAIN.txt
│   ├── 10_Hosts_IP.txt
│   └── 11_Comandos_Verificacion.txt
└── Documentacion/
    ├── Manual_Tecnico.md
    ├── Informe_Desarrollo.md
    └── img/
```

## Documentos
- [Manual Técnico](Documentacion/Manual_Tecnico.md)
- [Informe de Desarrollo](Documentacion/Informe_Desarrollo.md)
- [Scripts CLI](Scripts/)

## Resumen

| Zona | Nombre | VLAN | Red | Criticidad | Esquema |
|---|---|---|---|---|---|
| 1 | Distrito Empresarial – Administración | 15 | 192.168.5.0/26 | Alta | Core + anillo |
| 2 | Residencial – Lirios de Cayalá | 25 | 192.168.5.128/27 | Media | Bus / lineal |
| 3 | Gastronomía y Entretenimiento | 35 | 192.168.5.160/28 | Baja | Estrella |
| 4 | Paseo Cayalá – Comercial | 45 | 192.168.5.64/26 | Alta | Malla parcial + EtherChannel |
| 5 | Centro de Monitoreo y Seguridad | 55 | 192.168.5.176/28 | Alta | Malla parcial |

VLAN nativa 99 · Blackhole 999 · Dominio VTP `202206425` · EtherChannel PAgP.
