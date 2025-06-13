# Sistema de monitorización multinodo - Ansible
Despliegue automatizado con Ansible de un sistema de monitorización de nodos fisicos con Prometheus y Grafana (alojado en Docker).

## Tabla de Contenidos

- [Descripción](#descripción)
- [Características](#características)
- [Requisitos](#requisitos)
- [Adaptación](#adaptación)
- [Instalación](#instalación)

## Descripción
Este proyecto automatiza el despliegue de un sistema de monitorización de nodos físicos utilizando el recolector de métricas Prometheus y la plataforma de visualización de datos Grafana (ambos alojados en Docker). Tanto la fuente de datos de Grafana como el dashboard con los datos que se monitorizarán se crearán automáticamente en la ejeucción del Playbook.

## Características

- Instalación y configuración automatizada con Ansible.
- Sistema de monitorización.
- Compatible con nodos que no se encuentran en la misma red física que Prometheus y Grafana. 
- Fácil adaptación a distintos entornos y nodos.
- Servicio altamente escalable.

## Requisitos

- Ansible versión 2.17 o superior
- Docker instalado y funcionando en el nodo donde se implementarán Prometheus y Grafana.
- Acceso SSH configurado y permisos necesarios para Ansible en todos los nodos. Se recomienda el uso de autentificación con par de claves para evitar indicar los usuarios y contraseñas de los nodos en texto plano en el fichero de inventario.

## Adaptación
- Modifica el archivo [intentory/hosts.ini](inventory/hosts.ini) para definir los nodos que se monitorizarán (grupo nodos_clientes) y el nodo que alojará los contenedores de Docker (grupo servidores).
- El despliegue está diseñado para que solo sea necesario indicar los nodos clientes en el fichero de inventario. No es necesario modificar ningún otro fichero para que el despliegue diseñado funcione, pero sientete con libertad de modificarlo según tus necesidades.

> ⚠️ **Advertencia:** Este proyecto contiene la figura de un nodo router (Ubuntu Server) que en el diseño original reenviaba los puertos desde una red externa hacia la topología permtiendo así que el servicio web sea accesible desde redes ajenas a la que pertenecian los nodos participantes al despliegue. Si en tu topoligía no dispones de esta figura **y por lo tanto, por defecto, este despliegue solo funcionará en tu red local**, no debes ejeuctar la tarea que crearía la regla de reenvío de puertos en el nodo router. Para ello, en el fichero principal de tareas [main.yaml](main.yaml), elimina la tarea llamada  *Configurar NAT en el router*.

## Instalación

Clona este repositorio y ejecuta el playbook con el fichero de inventario personalizado según tu topología:

```bash
git clone https://github.com/antonioleonsilva/ansibe-sistemaMonitorizacionMultinodo.git
cd ansible-sistemaMonitorizacionMultinodo
ansible-playbook -i inventory/hosts.ini main.yaml
```

Para acceder a la interfaz de Prometheus, en un navegador, introduce la dirección IP del servidor Docker con el puerto 9090
```bash
http://IP_DEL_SERVIDOR:9090
```

De la misma manera, para acceder a la interfaz Grafana utiliza la dirección IP del servidor Docker. Accede a través del puerto 3000. El usuario y contraseña por defecto es **admin**
```bash
http://IP_DEL_SERVIDOR:3000
```



