
FlexARWAN

Herramienta De Automatización De Redes

Actividad: Subnetting, VLAN, configuración multifabricante, 
ciberdefensa y pruebas de failover


Alexander Reina

Correos: alexanderreina@ucompensar.edu.co


Fundación Universitaria Ucompensar
Facultad de Ingeniería, Telecomunicaciones


John Harold Pérez Calderón

Interconexión De Redes WAN

01 Octubre 2026
Bogotá, Colombia


Tabla de contenido
1. Introducción	3
1.1 Objetivos	3
1.2 Alcance y limitaciones	3
2. Requerimientos de software	3
2.1 Requerimientos funcionales	3
2.2 Requerimientos no funcionales	4
3. Desarrollo del laboratorio	4
3.1 Arquitectura física y medios de conexión	4
3.2 Servicios de dispositivos	4
3.3 ISP, ancho de banda y redundancia	5
3.4 Protocolos por capa	5
3.5 Plan de direccionamiento y VLAN	5
3.6 Subnetting: ejemplo resuelto	6
3.7 Dispositivos recomendados por piso	6
3.8 Configuración por fabricante	6
3.9 Ciberdefensa y VPN	7
3.10 Panel IA local	7
3.11 Pruebas de failover	7
3.12 Entrenamiento supervisado	7
3.13 Control de versiones y repositorio	7
4. Conclusiones	8
5. Bibliografía	8

 
1. Introducción
Un edificio de 20 pisos con servicios de datos, voz, video, WiFi y videovigilancia (CCTV) necesita una red ordenada, segura y con respaldo. Configurar a mano switches, routers, puntos de acceso y firewalls de varios fabricantes genera errores y diferencias entre pisos. FlexARWAN es una herramienta web, autónoma y de ejecución local, que calcula subredes, planifica el direccionamiento por piso, importa el inventario, genera configuraciones iniciales y simula fallos de enlace.
1.1 Objetivos
•	Calcular subredes IPv4 correctamente (red, broadcast y hosts útiles) y planificar IPv4 e IPv6 por piso y por servicio.
•	Generar configuraciones iniciales para Cisco, Huawei, Fortinet, MikroTik y HP a partir de un único plan.
•	Detectar configuraciones inseguras con reglas locales, sin enviar datos fuera del equipo.
•	Simular la caída de enlaces y proveedores (ISP) para evaluar la redundancia por piso.
1.2 Alcance y limitaciones
La versión V0.0.0 entrega una herramienta funcional como página HTML. Los scripts son una base inicial por familia de fabricante: deben validarse en laboratorio contra el modelo y el firmware reales antes de producción. Ningún sistema puede garantizar una confiabilidad del 100 %; el diseño reduce el riesgo mediante redundancia, validación de entradas y pruebas, pero no lo elimina. El agente con IA, el acceso SSH remoto automatizado, la VPN configurable y la vista por capas de protocolos están planificados para versiones posteriores.
2. Requerimientos de software
2.1 Requerimientos funcionales
Código	Descripción	Prioridad	Estado V0.0.0
RF-01	Calcular subredes IPv4 (red, máscara, broadcast, primer y último host, hosts útiles).	Alta	Implementado
RF-02	Plan IP por piso y servicio (datos, voz, video, WiFi, CCTV, gestión) en IPv4 e IPv6.	Alta	Implementado
RF-03	Editor de VLAN, tercer octeto y prefijo, con guardado local.	Alta	Implementado
RF-04	Importar inventario desde CSV (piso, tipo, fabricante, modelo, hostname, IP, puertos).	Alta	Implementado
RF-05	Generar configuración para Cisco, Huawei, Fortinet, MikroTik y HP, individual o conjunta.	Alta	Implementado
RF-06	Panel de auditoría con reglas locales de seguridad.	Alta	Implementado
RF-07	Generar usuario y contraseña seguros.	Media	Implementado
RF-08	Simulador de caída de ISP, core y uplinks por piso.	Alta	Implementado
RF-09	Agente con IA y conexión SSH a los equipos.	Media	Planificado
RF-10	Configuración de VPN IPsec/IKEv2 desde la herramienta.	Media	Planificado
RF-11	Vista de protocolos por capa asociada a cada equipo.	Baja	Planificado

2.2 Requerimientos no funcionales
•	Seguridad: sin envío de datos a terceros; contraseñas generadas con un generador aleatorio criptográfico del navegador.
•	Usabilidad: interfaz simple, consistente, de claridad visual y con retroalimentación inmediata mediante mensajes.
•	Portabilidad y responsividad: un solo archivo HTML, con tema claro y oscuro y adaptación a pantallas pequeñas.
•	Mantenibilidad: código comentado en español y control de versiones (V0.0.0).
•	Escalabilidad: parámetros editables (VLAN, prefijos) y soporte para el inventario de los 20 pisos.
3. Desarrollo del laboratorio
3.1 Arquitectura física y medios de conexión
El diseño usa una topología jerárquica de tres capas simplificada: acceso en cada piso, distribución en el piso de equipos y core redundante (Core A y Core B). Cada piso se conecta con dos enlaces de fibra, uno a cada core.
Medio	Uso en el edificio	Observación
Ethernet RJ-45 (UTP Cat6/6A)	Puestos de trabajo, teléfonos IP, cámaras y puntos de acceso (PoE)	Máximo 100 m por enlace de cobre
Fibra óptica	Troncales entre pisos, core y ISP	Multimodo para el edificio, monomodo para el ISP
Cable coaxial / cable	Acometida de algunos ISP y distribución de video	Se convierte a Ethernet en el equipo del ISP
Wi-Fi	Acceso inalámbrico de usuarios e invitados	Mínimo 2 puntos de acceso por piso, ajustable por área
Puertos de dispositivos	Consola, gestión, SFP/SFP+ y puertos PoE	Documentados en el inventario CSV

3.2 Servicios de dispositivos
Los equipos físicos (routers, switches, puntos de acceso, firewalls, teléfonos y cámaras IP) y lógicos (DHCP, DNS, NAT, VPN, ACL, QoS, monitoreo y registros) conectan, gestionan y protegen la comunicación entre computadoras y sistemas. El router enruta entre redes y hacia el ISP; el switch conmuta tramas y segmenta con VLAN; el punto de acceso extiende la red de forma inalámbrica; el firewall filtra y aplica políticas; el teléfono IP y la cámara IP son terminales que exigen priorización (voz) o segmentación estricta (CCTV).
3.3 ISP, ancho de banda y redundancia
Se recomienda contratar dos ISP con acometidas físicas distintas y, si es posible, con ancho de banda simétrico garantizado por contrato (SLA). El dimensionamiento inicial debe medirse con tráfico real; como referencia de diseño se calcula el consumo por servicio (datos, voz, video, CCTV) y se agrega un margen de crecimiento. Los firewalls operan en alta disponibilidad y el simulador muestra el efecto de perder un ISP: de «redundante» a «sin redundancia» y, con ambos caídos, «sin Internet».
3.4 Protocolos por capa
Capa	Protocolos	Función
Aplicación	DHCP, DNS, SSH, HTTPS, SNMPv3, SIP, RTSP, NTP, Syslog	Servicios, gestión remota segura, voz y video
Transporte	TCP, UDP	Entrega confiable (TCP) o en tiempo real (UDP)
Red / Internet	IPv4, IPv6, ICMP, OSPF, BGP, IPsec	Direccionamiento, enrutamiento y VPN
Enlace	Ethernet (IEEE 802.3), 802.1Q, LACP, RSTP, 802.1X	VLAN, agregación, prevención de bucles y control de acceso

3.5 Plan de direccionamiento y VLAN
Cada servicio usa el esquema 10.<piso>.<tercer octeto>.0/<prefijo>. En IPv6 se usa 2001:db8:<piso>:<VLAN>::/64; ese prefijo está reservado para documentación (RFC 3849) y debe reemplazarse por el entregado por el ISP.
Servicio	VLAN	IPv4 (piso 5)	Gateway	IPv6 (piso 5)
Datos	10	10.5.10.0/24	10.5.10.1	2001:db8:5:10::/64
Voz	20	10.5.20.0/24	10.5.20.1	2001:db8:5:20::/64
Video	30	10.5.30.0/24	10.5.30.1	2001:db8:5:30::/64
WiFi	40	10.5.40.0/23	10.5.40.1	2001:db8:5:40::/64
CCTV	50	10.5.50.0/24	10.5.50.1	2001:db8:5:50::/64
Gestión	99	10.5.99.0/26	10.5.99.1	2001:db8:5:99::/64
Nota: un /23 abarca dos terceros octetos (10.5.40.0 a 10.5.41.255), por eso el siguiente servicio no debe usar el octeto 41.

3.6 Subnetting: ejemplo resuelto
Problema: calcular la subred de 192.168.10.0/26. Un prefijo /26 deja 6 bits para hosts, por lo que hay 2^6 = 64 direcciones, de las cuales 62 son utilizables (se resta la dirección de red y la de broadcast).
Dato	Resultado
Máscara	255.255.255.192
Dirección de red	192.168.10.0
Dirección de broadcast	192.168.10.63
Primer host	192.168.10.1
Último host	192.168.10.62
Hosts útiles	62
Siguiente subred	192.168.10.64/26

Verificación: el incremento es 256 − 192 = 64, así que las subredes comienzan en 0, 64, 128 y 192. Esto coincide con la salida de la calculadora de FlexARWAN.
3.7 Dispositivos recomendados por piso
•	2 switches de acceso PoE de 48 puertos (principal y respaldo) con 2 uplinks de fibra de 10 G.
•	Puntos de acceso Wi-Fi: 2 por cada 500 m² de área útil.
•	Teléfonos IP: uno por puesto de trabajo con voz; cámaras IP: aproximadamente 1 por cada 80 m².
•	Piso de equipos: 2 routers WAN, 2 firewalls en alta disponibilidad y 2 switches core.
Estas cifras son una guía inicial y deben ajustarse al levantamiento real de cada piso.
3.8 Configuración por fabricante
Fabricante	Sistema operativo	Comandos iniciales (ejemplos)	Seguridad base
Cisco	IOS / IOS-XE	hostname, vlan, interface Vlan, ip ssh version 2, spanning-tree mode rapid-pvst	line vty con transport input ssh; secret
Huawei	VRP	sysname, vlan batch, interface Vlanif, stelnet server enable, Eth-Trunk	aaa con local-user; vty solo ssh
Fortinet	FortiOS	config system global/interface/firewall policy	admin-telnet disable; UTM en políticas
MikroTik	RouterOS	/interface vlan add, /ip address add, /ip service disable	Desactivar telnet, ftp, www; usuario propio
HP	ArubaOS-Switch / ProCurve	hostname, vlan, tagged, trunk, ip ssh	no telnet-server; password manager

La herramienta distingue el fabricante seleccionado y genera la sintaxis correspondiente. Los puertos de uplink (por ejemplo TenGigabitEthernet1/1/1-2 en Cisco o puertos 49–52 en HP) son valores de ejemplo y deben ajustarse al modelo exacto con ayuda del inventario CSV.
3.9 Ciberdefensa y VPN
•	Acceso administrativo solo por SSH versión 2; Telnet desactivado en producción por enviar datos en texto claro.
•	VLAN de gestión aislada (99) y listas de control de acceso entre VLAN con el principio de mínimo privilegio.
•	802.1X en puertos de acceso, DHCP snooping y Dynamic ARP Inspection contra suplantación.
•	VPN IPsec con IKEv2, cifrado AES-256-GCM e integridad SHA-384, siguiendo las guías del NIST.
•	Contra ataques asistidos por IA (por ejemplo, phishing o fuerza bruta automatizada): MFA, bloqueo temporal por intentos, segmentación de CCTV y VoIP, registros centralizados y detección de anomalías.
3.10 Panel IA local
El panel aplica reglas locales (expresiones regulares) sobre la configuración pegada por el usuario. Detecta Telnet activo, comunidades SNMP por defecto, contraseñas débiles, servicios HTTP/FTP sin cifrar, reglas «permitir todo» y ausencia de SSH o de protecciones de capa 2. No usa un modelo remoto, por lo que ningún dato sale del equipo. No sustituye una auditoría profesional.
3.11 Pruebas de failover
Escenario	Resultado esperado	Estado por piso
Todo activo	Salida y uplinks redundantes	Redundante
Cae un ISP	Tráfico por el ISP restante	Sin redundancia WAN
Cae Core A	Todos los pisos usan Core B	Sin respaldo
Cae el enlace A de un piso	Ese piso opera por el enlace B	Sin respaldo
Caen ambos enlaces de un piso	Piso sin conectividad al core	Aislado

3.12 Entrenamiento supervisado
Para el entrenamiento supervisado de las reglas de auditoría, se propone un conjunto de configuraciones etiquetadas por un especialista (seguras e inseguras). Cada regla se evalúa contra ese conjunto midiendo falsos positivos y falsos negativos, y el especialista revisa y corrige las reglas antes de publicar una nueva versión. Cuando se incorpore un modelo de IA, este flujo de revisión humana se mantiene como requisito.
3.13 Control de versiones y repositorio
La versión actual es V0.0.0. Se propone un repositorio público en GitHub con README, una rama de trabajo (develop) además de main y al menos cinco commits con mensajes claros, por ejemplo: «Agrega calculadora de subredes IPv4», «Agrega editor de VLAN y plan por piso», «Agrega generador de configuración multifabricante», «Agrega panel de auditoría local» y «Agrega simulador de failover».
4. Conclusiones
•	La automatización del plan IP y de las configuraciones reduce errores manuales y mantiene la coherencia entre los 20 pisos.
•	Un único plan de VLAN y direccionamiento permite generar sintaxis para varios fabricantes, aunque cada modelo requiere validación en laboratorio.
•	La redundancia debe probarse: el simulador muestra que un solo core o ISP caído deja el servicio, pero sin respaldo.
•	La auditoría local mejora la privacidad al no enviar configuraciones a terceros, pero sus reglas deben ampliarse y supervisarse.
•	Trabajo futuro: agente con IA y SSH, VPN configurable, vista por capas de protocolos y pruebas con equipos reales.
5. Bibliografía
Cisco Systems. (s. f.). Documentación oficial de Cisco IOS XE y Catalyst. Cisco Documentation. https://www.cisco.com/c/en/us/support/index.html
Deering, S., & Hinden, R. (2017). Internet Protocol, Version 6 (IPv6) Specification (RFC 8200). IETF.
Fortinet. (s. f.). FortiOS Administration Guide. Fortinet Document Library. https://docs.fortinet.com
Fuller, V., & Li, T. (2006). Classless Inter-domain Routing (CIDR) (RFC 4632). IETF.
Huawei Technologies. (s. f.). Documentación de productos de redes empresariales (VRP). Huawei Support. https://support.huawei.com
IEEE. (2018). IEEE Std 802.1Q-2018: Bridges and Bridged Networks. IEEE.
IEEE. (2020). IEEE Std 802.1X-2020: Port-Based Network Access Control. IEEE.
Kaufman, C., Hoffman, P., Nir, Y., Eronen, P., & Kivinen, T. (2014). Internet Key Exchange Protocol Version 2 (IKEv2) (RFC 7296). IETF.
Kurose, J. F., & Ross, K. W. (2021). Computer Networking: A Top-Down Approach (8.ª ed.). Pearson.
MikroTik. (s. f.). RouterOS Manual. MikroTik Documentation. https://help.mikrotik.com/docs
National Institute of Standards and Technology. (2020). Security and Privacy Controls for Information Systems and Organizations (SP 800-53, Rev. 5). NIST.
National Institute of Standards and Technology. (2020). Guide to IPsec VPNs (SP 800-77, Rev. 1). NIST.
Rekhter, Y., Moskowitz, B., Karrenberg, D., de Groot, G. J., & Lear, E. (1996). Address Allocation for Private Internets (RFC 1918). IETF.
Huston, G., Lord, A., & Smith, P. (2004). IPv6 Address Prefix Reserved for Documentation (RFC 3849). IETF.
Ylonen, T., & Lonvick, C. (2006). The Secure Shell (SSH) Protocol Architecture (RF.


OPCION 1:

<img width="1365" height="593" alt="Captura de pantalla 2026-10-02 010031" src="https://github.com/user-attachments/assets/c1702dfc-0d77-4788-8265-b71097ea6d35" />


<img width="1359" height="595" alt="Captura de pantalla 2026-10-02 010053" src="https://github.com/user-attachments/assets/eb97d569-4d46-4e5a-a9e6-f766d3fe0a60" />


OPCION 2:


<img width="1364" height="595" alt="Captura de pantalla 2026-10-02 010125" src="https://github.com/user-attachments/assets/09eabe57-8316-453d-86d7-01f6dbbccd60" />


<img width="1352" height="594" alt="Captura de pantalla 2026-10-02 010150" src="https://github.com/user-attachments/assets/67ccaa34-5cb0-472c-9a3b-79cdc6f87f74" />


<img width="1350" height="550" alt="Captura de pantalla 2026-10-02 010222" src="https://github.com/user-attachments/assets/6c3ad495-1973-4e92-b6aa-9ad65d0e9e88" />


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
