
# Informe de Auditoría de Seguridad del Sistema

**Fecha de la Auditoría**: 27 de septiembre, 19:42  
**Auditor**: Jose Daniel Moreno Lopez  
**Herramienta utilizada**: Lynis  
**Objetivo**: Identificar vulnerabilidades de seguridad y proporcionar recomendaciones de mitigación para mejorar la seguridad del sistema.

## 1. Introducción

Este informe detalla los hallazgos de la auditoría de seguridad realizada utilizando la herramienta `lynis`. La auditoría tiene como objetivo evaluar la configuración del sistema, identificar áreas de riesgo y proponer acciones correctivas. Las vulnerabilidades identificadas a continuación deben abordarse con prontitud para garantizar la integridad y la seguridad del entorno.

## 2. Hallazgos Críticos

### 2.1. Permisos Inseguros en el Archivo Sudoers  
**Gravedad**: Alto  
**Descripción**: El directorio `/etc/sudoers.d` tiene permisos excesivos (`drwxr-xr-x`), lo que podría permitir acceso no autorizado a archivos sensibles.  
**Solución**: Se recomienda restringir los permisos del directorio. Ejecutar el siguiente comando para cambiar los permisos:
```bash
sudo chmod 750 /etc/sudoers.d
```
Esto asegurará que solo el propietario y el grupo tengan acceso a este directorio, mientras que otros usuarios no podrán visualizar su contenido.  
**Permisos recomendados**: `drwxr-x---`

### 2.2. Interfaces de Red en Modo Promiscuo  
**Gravedad**: Alto  
**Descripción**: Se ha detectado que una o más interfaces de red están configuradas en modo promiscuo, lo que puede permitir la interceptación de tráfico no destinado a esa máquina.  
**Solución**:
1. Verificar las interfaces que están en modo promiscuo con el siguiente comando:
```bash
ip a
```
2. Desactivar el modo promiscuo en las interfaces afectadas ejecutando:
```bash
sudo ip link set <interface> promisc off
```
**Nota**: Si alguna aplicación, como `VirtualBox`, está utilizando el modo promiscuo (por ejemplo, con configuraciones de red en `Bridge` o `Internal Network`), puede que no sea posible deshabilitarlo inmediatamente. En este caso, revise la necesidad de estas configuraciones y deshabilite el modo promiscuo si no es estrictamente necesario.

### 2.3. Paquetes Vulnerables (PKGS-7392)  
**Gravedad**: Media  
**Descripción**: Se ha detectado que algunos paquetes instalados en el sistema no están actualizados y contienen vulnerabilidades conocidas.  
**Solución**: Actualizar todos los paquetes vulnerables ejecutando:
```bash
sudo apt update && sudo apt upgrade
```
Mantener el sistema actualizado es esencial para protegerse contra exploits que aprovechan vulnerabilidades en software antiguo.

### 2.4. Método de Hash de Contraseñas Anticuado (AUTH-9230)  
**Gravedad**: Media  
**Descripción**: Se está utilizando el algoritmo de hash de contraseñas SHA-512, que aunque es seguro, se recomienda usar SHA-256 por ser más eficiente y ampliamente adoptado.  
**Solución**: Configurar el sistema para utilizar SHA-256 como método de hash. Modificar el archivo `/etc/login.defs`:
1. Abrir el archivo:
```bash
sudo nano /etc/login.defs
```
2. Cambiar el método de hash:
```
ENCRYPT_METHOD SHA256
```

## 3. Recomendaciones Adicionales

- **Monitoreo Continuo**: Se recomienda realizar auditorías periódicas utilizando herramientas como `lynis` para detectar nuevas vulnerabilidades.
- **Hardening del Sistema**: Aplicar mejores prácticas de seguridad, como la desactivación de servicios no necesarios, y el uso de cortafuegos para filtrar tráfico de red.
- **Cifrado de Contraseñas**: Además de cambiar el método de hash a SHA-256, implementar autenticación multifactor (MFA) para aumentar la seguridad en el acceso al sistema.
- **Backup Regular**: Asegurarse de tener un plan de copias de seguridad en caso de que se detecten fallas críticas en el sistema.

## 4. Conclusión

Esta auditoría ha identificado varias áreas críticas y medianamente críticas que deben ser abordadas. La implementación de las soluciones recomendadas mejorará significativamente la seguridad del sistema. Se recomienda revisar estas configuraciones de forma regular y aplicar parches de seguridad en cuanto estén disponibles.
