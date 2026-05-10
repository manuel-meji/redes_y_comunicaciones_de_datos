# Laboratorio: Conectividad Inter-VLAN y Gestión de Dispositivos

## Objetivo
Configurar una topología de red con dos subredes distintas, asegurar la comunicación entre hosts de diferentes redes (Inter-VLAN) y habilitar una interfaz de gestión en el Switch para permitir su administración mediante IP.

## Topología

- **Router0**: Actúa como gateway de ambas redes (Router-on-a-Stick básico).
- **Switch0 (LAN 1)**: Conecta los hosts PC0 y PC2.
- **Switch1**: Conecta el host PC1.
- **PC0 y PC2**: Hosts en la subred `192.168.1.0/24`.
- **PC1**: Host en la subred `192.168.2.0/24`.

## Tareas a Realizar

### 1. Configuración del Router0 (Gateway)

Configure las interfaces del router para que funcionen como puerta de enlace de cada red:

- Interfaz **GigabitEthernet0/0**: IP `192.168.1.1/24`
- Interfaz **GigabitEthernet0/1**: IP `192.168.2.1/24`
- Activar ambas interfaces (`no shutdown`)

### 2. Configuración de Hosts

Asigne las siguientes direcciones IP respetando la Puerta de Enlace Predeterminada:

| Dispositivo | Dirección IP     | Máscara de Subred    | Default Gateway  |
|-------------|------------------|----------------------|------------------|
| PC0         | 192.168.1.11     | 255.255.255.0        | 192.168.1.1      |
| PC2         | 192.168.1.12     | 255.255.255.0        | 192.168.1.1      |
| PC1         | 192.168.2.11     | 255.255.255.0        | 192.168.2.1      |

### 3. Configuración de la Interfaz de Gestión (SVI) en Switch0

Configure la interfaz virtual VLAN 1 para permitir la administración del Switch0:

- Asignar IP: `192.168.1.5/24`
- Activar la interfaz
- Configurar **default gateway** (`ip default-gateway 192.168.1.1`)

> **Importante**: El comando `ip default-gateway` es necesario para que el Switch pueda responder a pings provenientes de la otra subred (PC1).

### 4. Verificación de Conectividad

Desde la terminal de **PC0** realice las siguientes pruebas:

```bash
ping 192.168.1.1     # Router0
ping 192.168.1.5     # Switch0 (gestión)
ping 192.168.2.11    # PC1 (Inter-VLAN)
ping 192.168.1.12    #PC2 (Dentro del mismo router)
