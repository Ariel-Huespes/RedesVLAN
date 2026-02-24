
<img width="1233" height="701" alt="TopologiaVLANS" src="https://github.com/user-attachments/assets/9451346b-d28f-4493-9d07-dcd45be9bc36" />
<img width="1233" height="701" alt="TopologiaVLANS" src="https://github.com/user-attachments/assets/9451346b-d28f-4493-9d07-dcd45be9bc36" />


Proyecto Cisco Packet Tracer – Red con DHCP Centralizado y VLANs
📌 Descripción General

Este proyecto consiste en el diseño e implementación de una red segmentada mediante VLANs, con un Servidor DHCP centralizado encargado de asignar automáticamente direcciones IP a todos los dispositivos de la red.

La topología incluye:

1 Servidor DHCP

1 Router

4 Switches interconectados

2 Access Points

PCs cableadas

PCs inalámbricas

Segmentación lógica mediante VLAN A, B y C

🔹 Estructura lógica

El Servidor DHCP está conectado al Router.

El Router está conectado a un Switch principal.

Los 4 Switches están interconectados mediante enlaces troncales (Trunk).

Cada Switch tiene 4 PCs:

2 PCs → VLAN A

2 PCs → VLAN B

Los Access Points asignan VLAN C a los dispositivos inalámbricos.

🌐 Esquema de Direccionamiento IP

Servidor DHCP -> 10.20.30.128 /27
VLAN A -> 10.20.30.160 /27
VLAN B -> 10.20.30.192 /27
VLAN C -> 10.20.30.224 /27

🧩 Configuración Implementada

1️⃣ Configuración de VLANs en los Switches

Se crearon las siguientes VLANs:

VLAN 10 → VLAN A

VLAN 20 → VLAN B

VLAN 30 → VLAN C

Ejemplo de configuración:

vlan 10
name VLAN A

vlan 20
name VLAN B

vlan 30
name VLAN C

🔹 Asignación de puertos

Ejemplo: 
interface fa0/1
switchport mode access
switchport access vlan 10

2️⃣ Configuración de Enlaces Troncales (Trunk)

Los enlaces entre switches se configuraron como troncales para permitir el tráfico de todas las VLANs:

interface fa0/2
switchport mode trunk

3️⃣ Configuración Router-on-a-Stick

Se configuró el router para realizar el enrutamiento inter-VLAN mediante subinterfaces.

Ejemplo:

interface g0/0.10
encapsulation dot1Q 10
ip address 10.20.30.161 255.255.255.224

interface g0/0.20
encapsulation dot1Q 20
ip address 10.20.30.193 255.255.255.224

interface g0/0.30
encapsulation dot1Q 30
ip address 10.20.30.225 255.255.255.224

Estas IPs funcionan como Gateway para cada VLAN.

4️⃣ Configuración del Servidor DHCP

El servidor DHCP fue configurado con pools para cada VLAN:

🔹 Pool VLAN A

Network: 10.20.30.160 /27

Default Gateway: 10.20.30.161

🔹 Pool VLAN B

Network: 10.20.30.192 /27

Default Gateway: 10.20.30.193

🔹 Pool VLAN C

Network: 10.20.30.224 /27

Default Gateway: 10.20.30.225

El servidor asigna automáticamente:

Dirección IP

Máscara de subred

Puerta de enlace

5️⃣ Configuración de DHCP Relay (ip helper-address)

Como el servidor DHCP se encuentra en otra red, se configuró en el router:

interface g0/0.10
ip helper-address 10.20.30.130

interface g0/0.20
ip helper-address 10.20.30.130

interface g0/0.30
ip helper-address 10.20.30.130

(10.20.30.130 corresponde a la IP del servidor DHCP)

📶 Configuración de Access Points

Los Access Points:

Fueron conectados a puertos configurados en VLAN 30.

Asignan conectividad inalámbrica a los clientes.

Los dispositivos Wireless reciben IP automáticamente del DHCP.

🔎 Pruebas Realizadas

✔ Obtención automática de IP en todas las PCs
✔ Comunicación entre dispositivos de la misma VLAN
✔ Comunicación entre VLANs mediante Router-on-a-Stick
✔ Conectividad entre dispositivos cableados e inalámbricos
✔ Verificación con ping entre distintas VLANs

📚 Conceptos Aplicados

VLAN

Trunking (802.1Q)

Router-on-a-Stick

DHCP

DHCP Relay (ip helper-address)

Subnetting /27

Segmentación de red

Broadcast Domains
