# Herramienta-de-Redes-WAN
Redes WAN
[README.md](https://github.com/user-attachments/files/32944443/README.md)
[FlexARWAN V0.0.0.md](https://github.com/user-attachments/files/32944473/FlexARWAN.V0.0.0.md)# FlexARWAN

Edificio de 20 pisos · Versión V0.0.0 · funciona sin conexión, los datos no salen de este navegador

## Calculadora de subredes IPv4

Red en notación CIDR

**Ejemplo resuelto:** 192.168.10.0/26 → máscara 255.255.255.192, red 192.168.10.0, broadcast 192.168.10.63, hosts .1 a .62 (62 útiles = 26 − 2).

## Plan IP por piso (editable)

Cada servicio usa 10.\<piso>.\<tercer octeto>.0/\<prefijo> en IPv4 y 2001:db8:\<piso>:\<VLAN>::/64 en IPv6 (prefijo de documentación: reemplácelo por el de su proveedor).

Ver pools del piso

## Topología e inventario (CSV)

Columnas: `piso,tipo,fabricante,modelo,hostname,ip,puertos`. Tipos: router, switch, ap, firewall, telefono, camara.

## Dispositivos recomendados por piso

1 switch de acceso PoE (48 puertos) + 1 de respaldo, 2 puntos de acceso Wi-Fi por cada 500 m², teléfonos IP según puestos, cámaras IP según cobertura (≈1 por 80 m²). En el piso de equipos (core): 2 routers WAN, 2 firewalls en alta disponibilidad, 2 switches core. Cada piso se conecta con 2 enlaces de fibra a core A y core B.

## Generador de configuraciones

Fabricante

Piso

Hostname

```
Pulse «Generar».
```

Revise cada script en laboratorio antes de aplicarlo en producción: la sintaxis varía según modelo y versión de firmware.

## Panel IA local (reglas, sin enviar datos)

Pegue una configuración para auditar

## Credenciales seguras

Usuario

```
—
```

## Ciberdefensa base

SSH v2 con claves, Telnet apagado en producción, VPN IPsec/IKEv2 con AES-256-GCM y SHA-384, VLAN de gestión aislada (99), 802.1X en puertos de acceso, DHCP snooping + DAI, ACL entre VLAN, syslog central, MFA en firewall. Contra ataques apoyados en IA: límites de intentos y bloqueo temporal, detección de anomalías por registros, segmentación estricta de CCTV y VoIP, y parches al día.

## Simulador de caída de uplink

git init && git branch -M main
git add README.md && git commit -m "Agrega README con descripción y uso de FlexARWAN"
git checkout -b develop
git add FlexARWAN_V0.0.0.html && git commit -m "Agrega herramienta FlexARWAN V0.0.0 con calculadora de subredes"
git add FlexARWAN_Informe_V0.0.0.docx && git commit -m "Agrega informe de laboratorio en Word"

git remote add origin <URL-de-su-repositorio>
git push -u origin main develop
