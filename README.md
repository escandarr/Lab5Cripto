# Informe Laboratorio 5 - Seguridad en SSH

Este proyecto detalla el desarrollo y análisis del tráfico SSH generado entre diferentes versiones de Ubuntu utilizando contenedores Docker. Se realizaron configuraciones personalizadas tanto en clientes como en servidores SSH para identificar patrones de tráfico, ajustar configuraciones de seguridad y analizar el protocolo OpenSSH.

## Objetivo

El objetivo del laboratorio es garantizar un medio de comunicación seguro mediante SSH, analizando las diferencias entre versiones de clientes y servidores, personalizando la versión del cliente y optimizando el tamaño de los paquetes en el intercambio de claves.

## Estructura del Proyecto

### Contenedores

Se configuraron 4 contenedores Docker con las siguientes versiones de Ubuntu:
- **C1**: Ubuntu 16.10
- **C2**: Ubuntu 18.10
- **C3**: Ubuntu 20.10
- **C4** (Servidor S1): Ubuntu 22.10

### Funcionalidades implementadas

1. **Configuración de contenedores Docker:**
   - Cada contenedor cuenta con un cliente SSH instalado.
   - El contenedor C4 también actúa como servidor SSH con el usuario `prueba` (contraseña `prueba`).

2. **Personalización de cliente SSH:**
   - Se modificó la versión de OpenSSH en C3 para identificarla como `SSH-2.0-OpenSSH_?`.

3. **Captura de tráfico SSH:**
   - Se utilizó `tcpdump` para capturar el tráfico generado por cada cliente durante su conexión al servidor.

4. **Optimización del servidor:**
   - El paquete `Key Exchange Init` del servidor fue ajustado para ser menor a 300 bytes mediante la selección de algoritmos ligeros.

5. **Análisis de tráfico y HASSH:**
   - Se analizaron los patrones de tráfico generados utilizando Wireshark.
   - Se obtuvieron los HASSH correspondientes para validar las versiones del cliente.

## Configuración

### Requisitos previos

- Docker instalado en el sistema.
- Wireshark o tcpdump para capturar y analizar el tráfico.
- Repositorios de Ubuntu configurados para versiones antiguas.

### Instrucciones

1. **Construcción de contenedores Docker:**
   - Cada versión de Ubuntu tiene un `Dockerfile` en su respectiva carpeta.
   - Para construir un contenedor, ejecuta:
     ```bash
     docker build -t <nombre_imagen> <ruta_dockerfile>
     ```

2. **Ejecución de contenedores:**
   - Para iniciar un contenedor, ejecuta:
     ```bash
     docker run -d --name <nombre_contenedor> -p <puerto>:22 <nombre_imagen>
     ```

3. **Conexión SSH:**
   - Desde cualquier cliente, conecta al servidor usando:
     ```bash
     ssh prueba@<IP_SERVIDOR>
     ```

4. **Captura de tráfico:**
   - Utiliza `tcpdump` para capturar el tráfico SSH:
     ```bash
     sudo tcpdump -i docker0 port 22 -w traffic.pcap
     ```

5. **Análisis con Wireshark:**
   - Abre el archivo `.pcap` en Wireshark para analizar el tráfico.

## Resultados

- **Tráfico generado:** Se capturaron los patrones de tráfico para cada cliente, mostrando diferencias en algoritmos de intercambio de claves, cifrado y autenticación.
- **Cliente personalizado:** La versión del cliente en C3 se personalizó exitosamente y se verificó en el tráfico capturado.
- **Optimización del servidor:** El paquete `Key Exchange Init` del servidor se redujo a 274 bytes.

## Contribuciones

- **Desarrollador:** Benjamín Escandar  
  Email: [benjamin.escandar@mail.udp.cl](mailto:benjamin.escandar@mail.udp.cl)

## Licencia

Este proyecto es para fines educativos y pertenece al curso de Seguridad en Redes y Criptografía de la Universidad Diego Portales.

---

**Nota:** Para más detalles, consulta el informe completo en este repositorio.
